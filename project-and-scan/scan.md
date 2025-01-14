# Scan 진행

1. 생성되는 업로드 팝업 창에 소스코드를 Drag\&Drop 혹은 browse를 클릭하여 소스코드의 경로를 지정합니다.
   *   소스코드는 압축 파일 형태를 권장합니다.\


       <figure><img src="../.gitbook/assets/image (126).png" alt=""><figcaption></figcaption></figure>
2.  업로드 팝업 창에서 'Show configuration options'을 클릭하면 추가 업로드 옵션이 제공됩니다. (선택사항)

    * Extract archive files to subdirectories : 업로드된 파일의 하위 디렉토리에서 압축 파일을 추출합니다.
    * Recursively extract archive files : 업로드된 파일에 압축 파일이 포함될 경우 모든 압축 파일을 해제하여 분석합니다.
      * Do not extract .jar files : Jar 파일들을 압축 해제하지 않고 오픈소스와 체크섬으로 확인합니다.
      * Always extract .jar files : Jar 파일들을 압축 해제하여 분석합니다.
      * Extract .jar files if and only if there is not a full file match against it  : Jar 파일을 압축 해제하지 않고 분석을 실행하여 오픈소스와 체크섬으로 확인하고 오픈소스와 매치되는 정보가 확인되지 않을 경우 압축을 해제하여 분석을 실행합니다.

    <figure><img src="../.gitbook/assets/43 (1).PNG" alt=""><figcaption></figcaption></figure>
3.  업로드가 끝난 후 'Done' 버튼을 클릭합니다.\


    <figure><img src="../.gitbook/assets/image (177).png" alt=""><figcaption></figcaption></figure>
4. 다음 단계는 Scan을 수행하기 전 Scan 매개변수를 선택하는 것입니다. (선택사항)
   * Limit : 파일당 매칭되는 최대 Component의 수입니다.
   * Sensitivity : Snippet 이 오픈소스와 매칭되는 데 필요한 최소 라인 수입니다.
   * Auto-Identification options : 파일 내 License 및 Copyright 문구를 추출하고 보류 중인 식별을 자동으로 해결합니다.
   * Identification : 기존 검증 내역을 재사용할 수 있습니다.
   * Edit Ignore Rules : Scan을 제외할 디렉토리, 파일명, 확장자명을 설정하여 Scan 대상에서 제외할 수 있습니다.
   *   Edit String Match Rules : Scan 대상 코드에 포함된 String을 확인하기 위한 Rule을 생성할 수 있습니다.\


       <figure><img src="../.gitbook/assets/image (16).png" alt=""><figcaption></figcaption></figure>
5. Scan 버튼을 클릭하면 다음과 같이 Scan을 진행합니다.
   *   'Continue scan in background' 버튼을 클릭하면 Scan 중에 다른 작업이 가능합니다.\


       <figure><img src="../.gitbook/assets/image (140).png" alt=""><figcaption></figcaption></figure>
6. Scan이 완료되면 다음과 같이 나타납니다.
   *   'Start Identification' 버튼을 클릭하면 Scan 인터페이스로 이동합니다.\


       <figure><img src="../.gitbook/assets/image (56).png" alt=""><figcaption></figcaption></figure>
