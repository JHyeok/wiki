# Python에서 SQLAlchemy로 Session관리

python3에서 MySQLdb를 사용하기 위해 직접 설치를 하지 않고, pymysql 모듈을 이용해서 사용을 하였다.

```python
import pymysql
pymysql.install_as_MySQLdb()
```

데이터베이스를 사용할 관련 class를 만들고 그 안에 Session관리를 하기 위한 코드를 추가하였다.

Session관리를 간단하게 설명하면 데이터베이스 연결을 캐싱하고 관리하는 기법이라고 생각하면 된다. 매번 데이터베이스 연결을 생성하고 닫는 과정을 반복하면 비용이 크기 때문에 이 기법을 이용해서 연결 생성 과정을 줄일 수 있다.

SQLAlchemy에서는 session이 두 번째 수준의 캐시 작업이라고 정의한다. 또한 비동기 방식으로 사용되도록 고안되어있으며, 일반적으로 한 번에 하나의 스레드만 의미한다.

```python
from sqlalchemy import create_engine
from sqlalchemy.orm import scoped_session, sessionmaker

class SampleRepository(object):
    def __init__(self):
        # ... 생략
        self.engine = create_engine(connection_string)
        self.Session = scoped_session(sessionmaker(bind=self.engine))

    @contextmanager
    def session_scope(self):
        """
        일련의 작업에 대한 트랜잭션 범위를 제공한다.
        """
        _session = self.Session()
        try:
            yield _session
            _session.commit()
        except:
            _session.rollback()
            raise
        finally:
            _session.close()
```
`scoped_session`을 사용해서 session의 범위를 쓰레드 단위로 생성해준다. scoped_session을 사용하지 않고 아래와 같은 방법으로
session값을 찍어보면 중구난방하게 찍히는 것을 확인할 수 있다.

```python
with self.session_scope() as session:
    print(session)

# <sqlalchemy.orm.session.Session object at 0x00000201BD9478C8>
```

위의 코드로 session을 출력해서 database에 연결시에 session을 잘 사용하고 있는지, 세션이 닫힌 이후에 어떻게 관리가 되는지를 확인할 수 있다.

이후 session을 출력값을 확인해서 하나의 쓰레드에서 동일한 session을 사용하는 것을 확인하였고, 다른 터미널에서 해당 서비스를 사용하면
다시 새로운 session을 사용하는 것을 확인하였다. session이 혹시라도 충돌할까봐 테스트를 해보았다.

sqlalchemy와 pandas를 같이 사용할 때는 아래처럼 `session.bind`를 사용하면 된다.

```python
with self.session_scope() as session:
    sample = pd.read_sql_table('sample', session.bind)
```

raw query에서도 session을 사용해서 특정 어플리케이션이 데이터베이스에 연결할 때 해당 session만 사용하도록 하였다.

---
#### 참고

https://spoqa.github.io/2018/01/17/connection-pool-of-sqlalchemy.html

https://docs.sqlalchemy.org/en/13/orm/session_basics.html#what-does-the-session-do

https://yujuwon.tistory.com/entry/SQLALCHEMY-session-%EA%B4%80%EB%A6%AC

https://planbs.tistory.com/entry/Engine%EA%B3%BC-Session-Scoped-Session