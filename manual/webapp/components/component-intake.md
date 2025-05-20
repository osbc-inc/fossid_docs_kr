# Component intake 기능

Component intake 기능은 파일을 Component와 쉽게 연결할 수 있도록 합니다. \
연결된 파일이 Scan 인터페이스에서 결과를 생성하여 신속하게 식별을 수행합니다.

### 전제 조건

1. User는 ‘Components - Access component intake interface’ 권한을 통해 intake 기능을 사용할 수 있습니다.
2. fossid.conf 파일의 webapp\_intake\_repository에 연결된 패키지가 상주할 스토리지 대상을 가리킵니다.\
   (fossid.conf 파일에서 'webapp\_intake\_repository' 매개변수를 구성해야 합니다. 이 매개변수는 관련 패키지가 저장될 저장소 위치를 가리킵니다. 웹 서버는 해당 경로에 접근하고 쓰기 권한을 가질 수 있는 적절한 권한이 있어야 합니다.)

## Component intake 인터페이스

적절한 권한이 있는 User는 해당 인터페이스에 액세스할 수 있습니다.

'Show component hashing interface'를 통해 현재 상태를 확인할 수 있으며, 해당 인터페이스를 통해 바이너리, 파일 또는 패키지를 Component와 연관시킬 수 있습니다.

<figure><img src="../../../.gitbook/assets/화면 캡처 2025-05-20 141540.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/화면 캡처 2025-05-20 141629.png" alt=""><figcaption></figcaption></figure>

'Download from url' 옵션을 통해 다운로드할 수 있는 URL을 지정하여 파일을 다운로드하거나 'Upload' 옵션을 통해 파일을 서버에 업로드할 수 있습니다.

해싱 작업을 수행하기 위해 모든 패키지 파일의 압축이 해제됩니다. 비패키지 파일이 지정된 경우 해당 파일과의 단순 연결이 생성됩니다. 바이너리를 연결할 때 유용합니다.

URL을 지정하거나 WebServer에 파일을 업로드하는 프로세스를 시작하면 해당 파일과 Component에 대한 연결 항목이 생성됩니다.

<figure><img src="../../../.gitbook/assets/화면 캡처 2025-05-20 141925.png" alt=""><figcaption></figcaption></figure>

## Scan 인터페이스

프로세스가 성공적으로 완료되면 User는 Components에서 상태를 확인할 수 있습니다.

<figure><img src="../../../.gitbook/assets/화면 캡처 2025-05-20 141840.png" alt=""><figcaption></figcaption></figure>

FossID Workbench의 Components에 추가된 intake package는 intake 인터페이스에서 직접 다운로드 할 수 있습니다.

<figure><img src="../../../.gitbook/assets/화면 캡처 2025-05-20 142012.png" alt=""><figcaption></figcaption></figure>

Scan의 일부에 intake 기능으로 Component와 연결된 파일이 있는 경우, 매칭 목록의 첫 번째 줄에서 intake 매칭 결과를 확인할 수 있습니다.

<figure><img src="../../../.gitbook/assets/화면 캡처 2025-05-20 142132.png" alt=""><figcaption></figcaption></figure>
