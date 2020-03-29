# Node 애플리케이션 AWS Elastic Beanstalk에 배포

AWS Elastic Beanstalk에 Node 환경으로 만들어진 애플리케이션을 배포할 때 마주한 문제들과 해결방법들이다.

AWS Elastic Beanstalk는 기본적으로 NPM을 사용하도록 되어있는데 내가 만든 프로젝트는 Yarn을 사용하고 있었다.

그래서 그 부분을 고쳐줄 필요가 있었다. 열심히 검색을 해본 결과 관련 config들이 공유가 되고 있었다.

NPM을 disable하는 config과 yarn을 사용하도록 하는 config들이 필요했다. 그리고 이 config 파일들은 `.ebextensions` 디렉토리에 있어야지 AWS Elastic Beanstalk가 config 파일들을 인식하고 사용할 수 있다.

`.ebextensions/disable-npm.config`

```
files:
  "/opt/elasticbeanstalk/hooks/appdeploy/pre/50npm.sh":
    mode: "000755"
    owner: root
    group: users
    content: |
      #!/usr/bin/env bash
      #
      # Prevent installing or rebuilding like Elastic Beanstalk tries to do by
      # default.
      #
      # Note that this *overwrites* Elastic Beanstalk's default 50npm.sh script
      # (https://gist.github.com/wearhere/de51bb799f5099cec0ed28b9d0eb3663).

  "/opt/elasticbeanstalk/hooks/configdeploy/pre/50npm.sh":
    mode: "000755"
    owner: root
    group: users
    content: |
      #!/usr/bin/env bash
      #
      # Prevent installing or rebuilding like Elastic Beanstalk tries to do by
      # default.
      #
      # Note that this *overwrites* Elastic Beanstalk's default 50npm.sh script.
      # But their default script actually doesn't work at all, since the app
      # staging dir, where they try to run `npm install`, doesn't exist during
      # config deploys, so ebnode.py just aborts:
      # https://gist.github.com/wearhere/de51bb799f5099cec0ed28b9d0eb3663#file-ebnode-py-L140
```

`.ebextensions/yarn.config`

```
files:
    # Found at https://stackoverflow.com/a/42096244/9074640
    # Runs right before `npm install` in '.../50npm.sh'
    "/opt/elasticbeanstalk/hooks/appdeploy/pre/49_yarn.sh" :
        mode: "000775"
        owner: root
        group: root
        content: |
            #!/bin/bash

            # install node
            curl --silent --location https://rpm.nodesource.com/setup_12.x | bash -;

            # install yarn
            curl --silent --location https://dl.yarnpkg.com/rpm/yarn.repo | tee /etc/yum.repos.d/yarn.repo;
            yum -y install yarn;

            # install node_modules with yarn
            app="$(/opt/elasticbeanstalk/bin/get-config container -k app_staging_dir)";
            cd "${app}";
            echo "Inside ${app}, about to run yarn."
            yarn install;
```

그리고 소스 번들을 git으로 쉽게 만드는 방법이 있는데 이 명령을 사용하는 이유는 git에 저장된 파일들만 포함시켜서 소스 번들을 생성하기 때문에 소스번들을 최대한 작게 유지할 수 있는 장점이 있다.

```
$ git archive -v -o myapp.zip --format=zip HEAD
```

AWS Elastic Beanstalk 환경에서 환경 변수를 생성할 수 있는데 `NODE_ENV`를 `production`으로 설정하고 소스 번들을 업로드했다. 역시나 한 번에 되는 경우가 없다.

502 오류를 마주쳤는데 AWS Elastic Beanstalk에서는 로그를 확인할 수 있기 때문에 로그 요청에서 전체 로그를 다운받아서 확인했다.

```
 FATAL  Cannot find module '@nuxt/typescript-build'
Require stack:
- /var/app/current/node_modules/@nuxt/core/dist/core.js
- /var/app/current/node_modules/@nuxt/cli/dist/cli-command.js
- /var/app/current/node_modules/@nuxt/cli/dist/cli.js
- /var/app/current/node_modules/@nuxt/typescript-runtime/bin/nuxt-ts.js
```

로그를 확인해본 결과 프로젝트의 devDependencies에 있는 `@nuxt/typescript-build`가 `node_modules`에 없어서 오류가 생긴것을 확인했다.

TypeScript 기반이기 때문에 종속성을 설치할 때, Production으로 `dependencies`에 있는 종속성만 설치하면 안된다는 것을 알았고 `.ebextensions/yarn.config`에서 해당 부분을 수정했다.

`yarn --production;`을 `yarn install;`로 변경했다.

그리고 다시 소스 번들을 올려보았는데 이번에도 똑같이 502 오류가 뜨고 있었다. 로그를 다시 확인해본 결과, 이번엔 nginx에서 오류가 발생하고 있었다.

```
-------------------------------------
/var/log/nginx/error.log
-------------------------------------
connect() failed (111: Connection refused) while connecting to upstream, client: xx.xx.xx.xx, server: , request: "GET / HTTP/1.0", upstream: "http://127.0.0.1:8081/"
```

오류의 일부분인데 AWS Elastic Beanstalk는 프록시 서버로 Nginx를 사용하고 있는데 기본적으로 트래픽을 8081로 리디렉션하려고 시도하기 때문이다. 그래서 Nuxt.js의 포트를 8081로 수신하도록 변경하였더니 정상적으로 진행되었다.

---
#### 참고

https://www.trytape.com/2019/10/28/using-yarn-on-elastic-beanstalk/

https://docs.aws.amazon.com/ko_kr/elasticbeanstalk/latest/dg/applications-sourcebundle.html

