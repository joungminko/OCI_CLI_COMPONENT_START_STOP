# OCI CLI로 클라우드 자원 stop, start

### 1. OCI CLI 설치
#### 1.1. free tier의 경우 OCI CLI 설치가 필요 (참고: [link](https://docs.cloud.oracle.com/en-us/iaas/Content/API/SDKDocs/cliinstall.htm))
#### 1.2. OCI CLI 생성(로컬 환경에서 CLI 접속 session 정보 생성 후 free tier 콘솔로 복사, session authentication은 web browser 인증이기 때문)
#### 1.2.1. session 생성 (참고: [link](https://docs.cloud.oracle.com/en-us/iaas/tools/oci-cli/2.14.0/oci_cli_docs/cmdref/session/authenticate.html))
#### 1.2.2. tar 로 만들어 scp 명령어로 전송
```
tar -cvf oci-cli-session.tar ~/.oci/
scp -i ~/.ssh/id_rsa ~/oci-cli-session.tar opc@퍼블릭IP:~/
```
#### 1.2.3. ssh 로 instance로 접속 후 home 폴더에 tar 해제 ~/.oci/config 파일 내용에서 path 를 변경(/home/opc/로)

#### 2.1. ADB instance command
#### 2.1.1. ADB stop command (참고: [link](https://docs.cloud.oracle.com/en-us/iaas/tools/oci-cli/2.14.0/oci_cli_docs/cmdref/db/autonomous-data-warehouse/stop.html))
#### 2.1.2. ADB start command (참고: [link](https://docs.cloud.oracle.com/en-us/iaas/tools/oci-cli/2.14.0/oci_cli_docs/cmdref/db/autonomous-data-warehouse/start.html))
#### 2.1.3. OAC stop command (참고: [link](https://docs.cloud.oracle.com/en-us/iaas/tools/oci-cli/2.14.0/oci_cli_docs/cmdref/analytics/analytics-instance/stop.html))
#### 2.1.4. OAC start command (참고: [link](https://docs.cloud.oracle.com/en-us/iaas/tools/oci-cli/2.14.0/oci_cli_docs/cmdref/analytics/analytics-instance/start.html))

#### 3.1. linux script (예제: shell script로 만들어진 cloud 자원을 내리거나 올리는 작업을 crontab에 등록해서 스케줄에 맞게 동작하도록 한다)
#### 3.1.1. start compute
```
#!/bin/bash

export CUR_DATE=$(date +\%Y\%m\%d_\%H%M%S)
echo '#'$CUR_DATE' start compute instance'

oci compute instance action --action START \
--config-file /home/opc/.oci/config --profile DEFAULT \
--auth security_token \
--instance-id ocid1.instance.oc1.iad.anuwcljrwpphycic47simcha2vmakbmc6zmi2lgnaxot2kmm5zcplluaisma
```
#### 3.1.2. crontab 작성(예제: 8시 30분에 동작)
```
30 8 * * * /home/opc/start-compute.sh | tee -a /home/opc/start-compute.log
```
