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

    <figure><img src="../.gitbook/assets/화면 캡처 2025-05-21 134216.png" alt=""><figcaption></figcaption></figure>
3.  업로드가 끝난 후 'Done' 버튼을 클릭합니다.\


    <figure><img src="../.gitbook/assets/image (177).png" alt=""><figcaption></figcaption></figure>
4.  다음 단계는 Scan을 수행하기 전 Scan 매개변수를 선택하는 것입니다. (선택사항)



    <figure><img src="../.gitbook/assets/화면 캡처 2025-05-21 134455.png" alt=""><figcaption></figcaption></figure>

\[Scan Prarmeters]

* Limit : 파일당 매칭되는 최대 Component의 수입니다.
* Sensitivity : Snippet이 오픈소스와 매칭되는 데 필요한 최소 라인 수입니다.



\[ID Assist]

* Scroing : KB에서 제공되는 매치된 오픈소스 리스트는 사용자 코드와 매칭률이 높은 순서(또는 많이 사용되는 오픈소스 순서)로 스코어가 할당되며, 스코어가 높은 순으로 리스트가 제공되고 있습니다.\
  향상된 Scoring을 통해 오픈소스 후보군 리스트의 정확도를 높일 수 있으며, 이를 통해 식별 시 보다 정확하게 진행할 수 있도록 해주는 옵션입니다.
* Filtering : 무의미한 부분이 매칭될 때 자동 Ignore 처리하는 옵션입니다.



\[File-Level License and Copyright]

* Automatically detect license declarations and copyright statements inside file\
  : 파일 내 license 선언 및 copyright 문구 자동 감지하는 옵션입니다.



\[Auto-Identification (Pending id tab)]

* Reuse previous identifications from \
  : 이전 검증 결과를 재사용하여 자동 식별하는 옵션입니다.
  * any previous identification : 가장 최근에 식별된 내용 재사용
  * only identifications made by me : 본인이 식별했던 컴포넌트에 대해서만 재사용
  * identifications from a specific project \
    : 특정 프로젝트를 지정하여 해당 프로젝트 내 모든 스캔에서 식별된 파일들 재사용
  * identifications form a specific scan : 특정 스캔을 지정하여 해당 스캔 내 식별된 파일들 재사용



* Automatically resolve pending identifications\
  : 사용자 파일과 매치된 오픈소스 후보군 리스트에서 최상위 컴포넌트로 자동식별하는 FossID 자체 자동 검증 옵션입니다.



\[Declared dependencies (Dependencies tab)]

* Identify direct and transitive dependencies \
  : 매니페스트 파일들을 FossID-DA 도구를 통해 디펜던시를 자동 식별하는 옵션



* Edit Ignore Rules : Ignore Rules 설정 옵션
* Edit String Match Rules : String Match 설정 옵션







5. Scan 버튼을 클릭하면 다음과 같이 Scan을 진행합니다.

*   'Continue scan in background' 버튼을 클릭하면 Scan 중에 다른 작업이 가능합니다.\


    <figure><img src="../.gitbook/assets/화면 캡처 2025-05-21 143548.png" alt=""><figcaption></figcaption></figure>

6. Scan이 완료되면 다음과 같이 나타납니다.

*   'Start Identification' 버튼을 클릭하면 Scan 인터페이스로 이동합니다.\


    <figure><img src="../.gitbook/assets/화면 캡처 2025-05-21 143630.png" alt=""><figcaption></figcaption></figure>
