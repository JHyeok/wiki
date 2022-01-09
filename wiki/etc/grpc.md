# gRPC란?

분산 시스템간 서로 통신을 해야하는데 시스템들간 모듈들을 보니 언어가 다양해서 데이터를 주고 받기 쉽지 않다. RESTful API를 개발하여 통신을 주고받으려고 하니, 각 모듈별로 웹서버를 띄우는 것도 부담스러운 환경이다.

좀 더 가볍고, 좀 더 빠르고, 언어에 종속적이지 않고 여러가지 Application이 쉽게 메시지를 주고 받을 수 있는 방법이 없을까? 라고 고민되면 gRPC를 한 번 사용해보자.

RPC(Remote Procedure Call)이란?
- 프로세스간 통신을 위한 기법 중 하나
- 다른 컴퓨터에 있는 함수를 호출했을 때, 마치 같은 컴퓨터에 있는 것처럼 호출할 수 있는 구조
- 클라이언트와 서버간에 각자가 일반 로컬 메서드를 호출하는 것처럼 사용하게 하는 것
- 다양한 언어환경에 제약없이, 플랫폼 제약없이 사용할 수 있게 하는 것

RPC의 구성요소
- Caller/Callee는 IDL(Interface Define Language)를 통하여 서로의 인터페이스를 명세한다. 
- Stub은 StubComplier를 통해 IDL을 기반으로 각각의 플랫폼, 언어에 맞춰 호출할 수 있는 Code를 생성한다. 
- Client와 Server간에 서로를 식별할 수 있는 Communication Layer가 있다. (transport Layer)

gRPC는
- google 에서 만든 RPC Framework
- 저용량 메시지
- 고성능 RPC 

다른 특징들은 RPC 개념 그대로인데 고성능 RPC라는 것을 특징으로 표현한다.

gRPC는 프로토콜 버퍼로 데이터를 주고 받는다.

#### 구글 프로토콜 버퍼(Google Protocol Buffer)란?

XML, JSON등과 같이 데이터 교환을 위해 만들어진 포맷으로 구글에서 제작하였다. JSON과는 다르게 바이너리 형태로 포맷이 이루어져 있어서 JSON 보다는 속도가 빠르다, 하지만 별도의 컴파일러를 이용해서 스키마(데이터 정의) 파일을 컴파일 해서 사용해야 하기 때문에, 데이터 포맷을 변경하기가 JSON 보다는 까다롭다는 단점이 있다.

가볍고, 빠르고, 사용하기에 쉽다고 한다.

사용법은 최초에 우리가 사용하고자 하는 데이터를 구조화하고, 사용하는 언어의 코드로 컴파일링을 하면 자동으로 코드가 생산되며, 자동으로 생산된 코드는 파일을 쓰고/읽는데 사용하면 된다. 구글 프로토콜 버퍼는 Java, Python, 그리고 C++을 지원하고 있으며, Proto3 부터는 Go, JavaNono, Ruby, 그리고 C#까지 지원이 가능하다고 한다.

또한, 구글에서 제공하는 RPC(Remote Procedure, 주로 서비스간 통신을 주고받을때 많이 사용하며 비슷한 서비스로는 apache thrift가 있다) 프레임워크인 gRPC와 같이 쓰이는 경우가 많다.

protobuf는 데이터의 타입을 정의하는 schema파일인 .proto 파일과 이름 컴파일해서 사용하는 각 언어별 protobuf 라이브러리로 구성되어 있다.

---
#### 참고

- [gRPC 란?](https://blog.naver.com/phh0606c/221718739856)
- [구글 프로토콜 버퍼란?](https://ourcstory.tistory.com/47)
- [Node.js로 Google Protobuf 사용하기](https://semtax.tistory.com/27)