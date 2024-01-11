# Scan 인터페이스

Scan이 완료되면 Scan 인터페이스가 제공됩니다.

Scan 인터페이스를 통해 파일 구조를 탐색하고 Scan 결과를 검사하고 적절하게 식별할 수 있습니다.

<figure><img src="../../.gitbook/assets/image (224).png" alt=""><figcaption></figcaption></figure>

### 검증 대상 파일 확인

아래 그림과 같이 4가지의 파일 트리 보기를 제공합니다.

각 탭의 숫자가 해당 탭에 속하는 파일의 수이며, 각 탭을 클릭하면 좌측 코드트리에 대상 파일이 필터링됩니다.

* All Files : Scan에 업로드된 모든 파일이 표시됩니다.
* Pending id : 오픈소스 컴포넌트와 일치하는 파일(Scan 시 사용된 sensitivity 설정 및 FossID KB에 따름)이 표시되며 식별 작업을 해야할 파일입니다.
* Identified : 이미 식별되었거나 무시된 파일을 보여줍니다.
*   No Matches : 오픈소스 컴포넌트와의 일치 항목이 없는 파일을 표시합니다.\


    <figure><img src="../../.gitbook/assets/image (218).png" alt=""><figcaption></figcaption></figure>

코드트리를 통해 해당 Project에 포함되어 있는 모든 파일의 경로를 파악할 수 있으며, 파일 또는 폴더별로 검증을 진행할 수 있습니다.

*   \>, ∨ 버튼 : 클릭하여 해당 폴더의 아래 경로를 펼치거나 접을 수 있습니다. 또한 더블클릭을 하면 하위 경로를 모두 펼치거나 접을 수 있습니다.\


    <figure><img src="../../.gitbook/assets/image (222).png" alt=""><figcaption></figcaption></figure>

### 기본 표시줄 내 메뉴 옵션

상단의 기본 표시줄에 있는 메뉴 옵션은 다음과 같습니다.

* Upload Code : 현재 Scan에 추가 소스코드를 업로드할 수 있습니다.
* Scan Code : 다시 Scan을 수행하거나 진행 중인 Scan의 현재 상태를 볼 수 있습니다.
* Look for changes in filesystem : 서버 내 소스코드를 Scan하는 경우 파일 시스템의 업데이트 사항을 확인할 수 있습니다.
* View Scan Log : 작업 중인 Scan과 관련된 모든 로그 항목을 확인할 수 있습니다.
* Generate Report : 검증 결과에 대한 보고서를 생성할 수 있습니다.
* Generate JIRA Ticket : 단일 파일 또는 전체 디렉토리와 관련하여 JIRA에서 Issue를 관리할 수 있습니다.
*   Generate approval requests : 식별된 모든 Component에 대한 승인 요청을 생성할 수 있습니다.\


    <figure><img src="../../.gitbook/assets/image (239).png" alt=""><figcaption></figcaption></figure>

### 파일 확장자별 예외 처리

파일 확장자별로 개수를 보여주며, 특정 확장자를 예외 처리할 수 있습니다.

검증 작업을 진행하지 않고자 하는 확장자를 대상으로 적용할 수 있습니다.

23.1 버전부터 해당 기능은 옵션이므로, fossid.conf 파일 내 하기 내용을 기입하여야 합니다.

```
; Display extensions bar in folder metrics view. Files can be marked as identified based on their extensions from this bar.
; By default hidden from 23.1 versions
webapp_display_extensions_in_folder_metrics=1
```

<figure><img src="../../.gitbook/assets/image (214).png" alt=""><figcaption></figcaption></figure>

특정 확장자를 클릭하면 아래와 같은 화면이 나타납니다.

Apply 버튼을 클릭하면 'Identified' 상태로 변경됩니다.

<figure><img src="../../.gitbook/assets/image (234).png" alt=""><figcaption></figcaption></figure>

'Identified' 탭을 클릭하여 예외 처리한 파일들의 상태를 확인할 수 있습니다.

<figure><img src="../../.gitbook/assets/image (226).png" alt=""><figcaption></figcaption></figure>

### 이 외 옵션

<figure><img src="../../.gitbook/assets/image (50).png" alt=""><figcaption></figcaption></figure>

* Add a comment : 해당 디렉터리와 관련된 설명을 추가할 수 있습니다.
* Download local directory : 로컬 또는 매칭된 파일을 다운로드할 수 있습니다.
* Rescan directory : 선택한 디렉터리를 다시 스캔할 수 있습니다.
* Apply Folder Identification : 폴더 단위로 검증할 수 있습니다.
* Marked as identified : Pending id 상태인 파일들을 Identified 상태로 변경합니다.
