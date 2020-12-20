# nGrinder 환경 AWS에서 구성

[가장 도움이 많이 된 글](https://velog.io/@windtrip/nGrinder-%ED%99%98%EA%B2%BD%EC%9D%84-AWS%EC%83%81%EC%97%90%EC%84%9C-%EA%B5%AC%EC%B6%95%ED%95%98%EA%B8%B0)

전체적인 내용은 위의 글대로 전부 따라 하면 되는데, Amazon Linux를 사용한다면 명령어들이 조금씩 다르다.

Ubuntu가 아닌 Amazon Linux를 사용하다 보니 위의 글에서 아래의 명령어들을 사용

```
# JAVA 환경변수 설정
/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.265.b01-1.amzn2.0.1.x86_64/bin/javac

JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.265.b01-1.amzn2.0.1.x86_64
PATH=$PATH:$JAVA_HOME/bin
CLASSPATH=$JAVA_HOME/jre/lib:$JAVA_HOME/lib/tools.jar

# nGrinder 컨트롤러 실행
docker run -d -v ~/ngrinder-controller:/opt/ngrinder-controller -p 80:80 -p 16001:16001 -p 12000-12009:12000-12009 ngrinder/controller

# nGrinder 에이전트 실행
에이전트 인스턴스에 접속한 이후에 아래 명령어 실행
sh ./ngrinder-agent/run_agent_bg.sh
```

한 번 설정하고 난 이후에는 AMI로 만들어서 에이전트가 더 필요하면 간편하게 추가할 수 있다.

---
#### 참고

[Amazon Linux JAVA 설치](https://bamdule.tistory.com/57)

https://multicloud.tistory.com/m/86

http://jmlim.github.io/ngrinder/2019/07/01/ngrinder-docker-setup/

https://brownbears.tistory.com/26

https://sg-choi.tistory.com/265

https://deveric.tistory.com/83

https://brownbears.tistory.com/27
