#### Node.js Cluster와 Amazon ECS

팀에서 AWS BatchLambda를 사용한 적이 있다. 

```ts
const numCPUs = os.cpus().length`
```

AWS BatchLambda에서 numCPUs를 출력했는데 2가 나왔다.

회사에서 사용하는 대부분의 서버들이 ECS에서 Fargate를 사용하고 있었다. Node.js를 사용하고 있었기 때문에 ECS Fargate에서도 궁금해서 출력해보았는데 2가 나왔다.

v0.25 CPU, v0.5 CPU, v1 CPU에서 동일하게 결과값이 나왔다.

나의 짧은 지식으로는 Node.js의 [cluster](https://nodejs.org/api/cluster.html#cluster)를 사용해야 최대한의 성능이 나올 것 같았다.

스택오버플로우에도 왜 2가 나온 지부터 해서 Cluster를 사용해야 하는지 등 나와 비슷한 생각을 가진 사람들의 질문들이 있긴 했었다.

[How does a Node.js process behave in AWS Fargate?](https://stackoverflow.com/questions/63107690/how-does-a-node-js-process-behave-in-aws-fargate)

[What vCPUs in Fargate really mean?](https://stackoverflow.com/questions/52090968/what-vcpus-in-fargate-really-mean)

[Docker containers and Node.js clusters](https://stackoverflow.com/questions/28547993/docker-containers-and-node-js-clusters)

[https://stackoverflow.com/questions/43213250/nodejs-cluster-on-aws-lambda](https://stackoverflow.com/questions/43213250/nodejs-cluster-on-aws-lambda)

하지만 예측하지 말고 직접 테스트하기로 했다.

테스트 툴로는 [loadtest](https://github.com/alexfernandez/loadtest)를 사용했다.

DB를 사용하는 API, Redis를 사용하는 API, Node.js 애플리케이션에서 계산만 해서 단순 응답값만 반환하는 API 등을 통해서 테스트를 했다. 간단하거나 Redis만 사용하는 API들은 대부분 속도가 비슷했다. 대신 여러 테이블들을 호출해서 애플리케이션에서 JOIN을 해주고 응답값을 가공해주고 응답값이 큰 API에서 속도차이가 났다.

v1CPU/2GB Memory의 ECS Fargate 환경에서는 Node.js 클러스터를 사용하지 않는게 더 빨랐다.

여러 개의 vCPU를 사용하거나 ECS EC2를 사용한다면 다른 결과값이 나올 수 있다.

**예측하는 것보다 직접 테스트하고 측정하는 것이 가장 정확한 것 같다.**
