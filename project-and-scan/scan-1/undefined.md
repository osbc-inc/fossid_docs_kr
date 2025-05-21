# 파일 단위 검증

검증 작업은 파일별 혹은 폴더별로 식별할 수 있으며, 파일 단위 검증 방법에 대해 알아보도록 하겠습니다.



코드트리 내에서 하나의 파일을 클릭하면 다음과 같이 나타납니다.

<figure><img src="../../.gitbook/assets/화면 캡처 2025-05-21 152903.png" alt=""><figcaption></figcaption></figure>

## 업로드한 파일 정보

User가 업로드한 파일의 정보를 보여줍니다.

<figure><img src="../../.gitbook/assets/화면 캡처 2025-05-21 153201.png" alt=""><figcaption></figcaption></figure>

* Assign component : Component를 검색하고 생성할 수 있습니다. \
  또는 파일에 할당된 Component 정보가 나타납니다.
* File License : 파일에서 감지된 License 정보가 나타납니다.
* Copyright : 파일에서 감지된 저작자의 정보가 나타납니다.
* Add Not Distribute Flag : 배포되거나 배포되지 않을 파일로 설정할 수 있습니다.
* Add comment to file : 해당 파일과 관련된 설명을 추가할 수 있습니다.
* Identified : 파일을 'Identified' 상태로 변경합니다. (검증 완료)





<figure><img src="../../.gitbook/assets/화면 캡처 2025-05-21 154353.png" alt=""><figcaption></figcaption></figure>

* View file information : 파일의 상세 정보를 확인합니다.
* Rescan Selected File : 파일을 재스캔 합니다.
* Identifications have been found for this item in other scans \
  : 선택한 파일에 대한 기존 식별 기록이 하나 이상 있는 경우, 지문모양 아이콘이 표시됩니다. \
  재사용할 식별 결과를 선택할 수 있습니다.
* Mark as Identified : 파일을 'Identified' 상태로 변경합니다. (검증 완료)





## 오픈소스 매칭 목록



파일과 매칭된 오픈소스의 목록이 나타납니다. FossID의 알고리즘에 의해 매칭율이 높은 것을 기준으로 상위 Limit 개수까지 보여줍니다.

<figure><img src="../../.gitbook/assets/화면 캡처 2025-05-21 155021.png" alt=""><figcaption></figcaption></figure>

* MATCH : 업로드한 파일과 오픈소스 파일이 모두 일치할 경우 'full', \
  부분적으로 일치할 경우 'partial'이 나타납니다.
* ARTIFACT : 매칭된 오픈소스 컴포넌트의 이름입니다.
* VERSION : 매칭된 오픈소스 컴포넌트의 버전입니다.
* COMPONENT LICENSE : 매칭된 오픈소스 컴포넌트의 대표 License입니다.
* FILE LICENSE : 매칭된 오픈소스 컴포넌트의 파일에서 감지된 License입니다.
* RELEASE DATE : 매칭된 오픈소스 컴포넌트의 릴리즈 날짜입니다.
* FILE : 매칭된 오픈소스 컴포넌트의 해당 파일 경로입니다.
* SIZE : 매칭된 오픈소스 컴포넌트의 해당 파일 사이즈입니다.
* URL : Github 혹은 다운로드 가능한 링크 정보가 나타납니다.
* HITS : 업로드한 파일과 오픈소스 파일이 모두 일치할 경우 all, \
  부분적으로 일치할 경우 매칭된 라인의 수와 일치한 부분의 백분율이 나타납니다.







### 매칭 결과 비교

매칭 목록 중 하나의 오픈소스를 클릭하면 우측 하단에 오픈소스 코드가 나타나며, 매칭된 부분은 노란색으로 음영 표시됩니다.

<figure><img src="../../.gitbook/assets/화면 캡처 2025-05-21 155405.png" alt=""><figcaption></figcaption></figure>

* Copy To Clipboard : 파일의 전체 내용을 복사합니다.
* Download File From FossID Mirror \
  : FossID Mirror에서 로컬 또는 오픈소스 파일을 다운로드할 수 있습니다.
* Go To Next Matching Block (partial, 부분 매칭된 경우) : 매칭된 부분으로 이동합니다.
* Use As Identification : 해당 오픈소스 정보가 'Assign Components'로 이동합니다.







## 파일 단위 검증 절차

1.  하나의 Component 선택 시 우측 하단에 매칭되는 오픈소스 소스코드가 나타나며, 'Use As Identification' 버튼을 클릭합니다.\


    <figure><img src="../../.gitbook/assets/화면 캡처 2025-05-21 160045.png" alt=""><figcaption></figcaption></figure>
2.  'Assign Components' 창에 선택한 오픈소스 Component가 나타나며, 'Identified' 클릭 시 검증이 저장됩니다. (기존 검증 내용이 있을 시) 'Identifications have been found for this item in other scans' 버튼 클릭 시에는 다른 Scan에서 진행된 검증 결과를 사용할 수 있습니다.



    <figure><img src="../../.gitbook/assets/화면 캡처 2025-05-21 160510.png" alt=""><figcaption></figcaption></figure>
3. 오픈소스 매칭 목록 내 원하는 Component가 없는 경우, \
   'Assign component'에 Component를 검색하거나 생성할 수 있습니다.\

   1.  Component 검색 : 오픈소스의 Name, Version을 입력하여 기존에 존재하는 Component를 검색합니다.\


       <figure><img src="../../.gitbook/assets/image (220).png" alt=""><figcaption></figcaption></figure>
   2.  Component 생성 : 오픈소스의 Name, Version, License를 입력하여 Component 생성과 동시에 Assign 합니다.\


       <figure><img src="../../.gitbook/assets/image (232).png" alt=""><figcaption></figcaption></figure>
4.  파일 단위 검증을 완료한 후 'Identified' 탭에서 검증된 내역을 확인할 수 있습니다.



    <figure><img src="../../.gitbook/assets/화면 캡처 2025-05-21 160642.png" alt=""><figcaption></figcaption></figure>

### 검증 결과 되돌리기

파일 단위 검증을 완료한 상태에서 검증 결과를 수정하거나 상태를 변경할 수 있습니다.

1.  'Remove Component Identification'을 클릭하여 할당된 Component를 삭제하고 다른 Component를 할당할 수 있습니다.



    <figure><img src="../../.gitbook/assets/화면 캡처 2025-05-21 160850.png" alt=""><figcaption></figcaption></figure>
2.  'Identified' 상태인 파일을 다시 'Pending id' 상태로 되돌리고자 할 경우, 'Identified'를 체크를 해제 합니다.\


    <figure><img src="../../.gitbook/assets/화면 캡처 2025-05-21 161020.png" alt=""><figcaption></figcaption></figure>
