# System Utils

FossID Workbench가 제공하는 시스템 유틸리티에는 MySQL 데이터베이스의 로그, 시스템 수준 정보, 백업 및 복원에 대한 접근 권한이 포함되어 있습니다. 각 부분에 대한 접근 권한은 역할 또는 사용자 권한 설정을 통해 제어할 수 있습니다.

<figure><img src="../../.gitbook/assets/화면 캡처 2025-05-21 111354.png" alt=""><figcaption></figcaption></figure>

### Logs

* Workbench에서 기록된시스템 로그를 확인할 수 있습니다.
* 특정 Action을 지정하거나 날짜를 지정하여 조회할 수 있습니다.
* 로그는 사용자별로 분류되며, 스캔 및 프로젝트 작업, 사용자 관련 변경 사항 등에 대한 정보를 포함합니다.

<figure><img src="../../.gitbook/assets/화면 캡처 2025-05-21 111028.png" alt=""><figcaption></figcaption></figure>

### System Information

* Workbench가 실행 중인 시스템 정보를 확인할 수 있습니다. (디스크 정보, 메모리 정보, 평균 CPU 부하)
* fossid.conf 파일 내 정보를 보여줍니다.
* 시스템 정보 및 로그를 다운로드할 수 있습니다.
* 전반적인 시스템의 상태를 체크할 수 있습니다.

<figure><img src="../../.gitbook/assets/화면 캡처 2025-05-21 111208.png" alt=""><figcaption></figcaption></figure>

### Manage Backups

* DB 및 파일 백업을 진행할 수 있습니다.
* 백업 파일을 이용하여 DB 및 파일을 복원할 수 있습니다.
* 보관되지 않은 스캔에 대한 소스 코드에 대해서도 백업할 수 있습니다.

<figure><img src="../../.gitbook/assets/image (130).png" alt=""><figcaption></figcaption></figure>

* 데이터베이스와 파일 백업은 별개의 엔터티라는 점에 유의하세요.
  * **DB 백업은** 로컬 MySQL 데이터베이스의 스냅샷을 생성하여 SQL 파일에 저장합니다. 이 스냅샷에는 검사 결과, 구성 요소, 라이선스, 사용자 설정 및 권한 등의 정보가 포함됩니다.
  * **파일 백업은** 스캔에 업로드된 소스 파일의 스냅샷을 생성하여 .tar.gz 파일에 저장합니다



* Manage Backups 기능을 사용하기 전, fossid.conf에 지정된 웹 서버에서 액세스할 수 있는 유효한 백업 경로가 있는지 확인하십시오. 백업을 내보내거나 가져올 때 해당 경로에 스냅샷을 배치하거나 액세스합니다.

<figure><img src="../../.gitbook/assets/image (119).png" alt=""><figcaption></figcaption></figure>



* 생성된 백업이나 백업 위치에 추가된 백업은 "Available backups" 섹션에 나열되며, 여기서 백업을 복원하거나 삭제할 수 있습니다.
* `webapp_intake_repository` intake 인터페이스를 통해 업로드된 파일은 백업되지 않습니다. \
  필요한 경우 FossID 구성 파일의 매개변수가 가리키는 디렉터리를 수동으로 백업하세요 .\
  (참고: 백업에 액세스하고 백업을 생성하려면 역할 또는 사용자에 대해 'System - Manage database backups' permission이 설정되어 있어야 합니다.)

\


### Process List

* 최근 실행된 1000개의 프로세스를 보여줍니다.

<figure><img src="../../.gitbook/assets/화면 캡처 2025-05-21 112103 (1).png" alt=""><figcaption></figcaption></figure>

### Scheduled Tasks

* 정기적으로 실행해야 하는 예약된 작업을 보여줍니다.

<figure><img src="../../.gitbook/assets/화면 캡처 2025-05-21 112352.png" alt=""><figcaption></figcaption></figure>

### Edit FossID Configuration

* FossID 설정 파일인 fossid.conf 파일을 Workbench에서도 수정할 수 있도록 합니다.

<figure><img src="../../.gitbook/assets/image (162).png" alt=""><figcaption></figcaption></figure>

### Update CPE List

* CPE 목록을 업데이트하기 위한 Url 혹은 로컬 파일을 지정할 수 있습니다.

<figure><img src="../../.gitbook/assets/화면 캡처 2025-05-21 112545.png" alt=""><figcaption></figcaption></figure>
