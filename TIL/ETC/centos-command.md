# CentOS7 자주 사용하는 Command

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