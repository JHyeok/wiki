# Linux 자주 사용하는 명령어

```sh
# login as: centos

# root 권한을 얻어서 접속한다
sudo su 

# 현재 경로 위치를 알 수 있다
pwd 

# 현재 디렉터리에 있는 프로젝트에 대한 종속성 및 도구를 복원
dotnet restore

# 프로젝트 게시
dotnet publish --output:/home/centos/curation/bin /p:EnvironmentName=Production

# 실행 시킬 때 &를 붙이면 백그라운드에서 실행
dotnet Sample.Web.dll &

# 모든 프로세스 중에 grep를 이용하여 dotnet이 포함된 것을 출력 (특정 프로세스를 찾을 때 주로 사용)
ps -ef | grep dotnet

# 디렉토리 삭제
rm -r -f 

# 프로세스 종료 (-9 option 강제 종료)
kill -9 {PID}
```

#### 로컬에서 AWS EC2로 파일 옮기기

```
# 서버 접속
ssh -i [pem 파일 경로] [EC2 계정]@[EC2 퍼블릭 주소]

# 파일 전송
scp -i [pem 파일 경로] [업로드할 파일 경로] [EC2 계정]@[EC2 퍼블릭 주소]:~/[받으려는 위치]

# 폴더 전송
scp -i [pem 파일 경로] -r [업로드할 폴더] [EC2 계정]@[EC2 퍼블릭 주소]:~/[받으려는 위치]
```

#### nohup과 & 명령어

리눅스/유닉스에서 쉘 스크립트파일을 데몬 형태로 실행시키는 명령어, nohup.out 파일이 로그 형태로 생성된다.

& 명령은 프로그램을 백그라운드에서 실행하도록 한다.

프로그램 종료는 `ps -ef` 명령어로 `PID`를 확인해서 `kill -9 {PID}` 명령어를 사용한다.

```
# nohup, 백그라운드(&)로 실행
nohup sh shell.sh &
```