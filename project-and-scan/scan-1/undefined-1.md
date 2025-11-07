# 폴더 단위 검증

검증 작업은 파일별 혹은 폴더별로 식별할 수 있으며, 폴더 단위 검증 방법에 대해 알아보도록 하겠습니다.



코드트리 내에서 특정 폴더를 클릭하면 다음과 같이 나타납니다.

<figure><img src="../../.gitbook/assets/화면 캡처 2025-05-21 161148.png" alt=""><figcaption></figcaption></figure>

## 폴더 단위 검증 절차

1.  폴더 단위로 검증하고자 하는 특정 폴더를 클릭합니다.\


    <figure><img src="../../.gitbook/assets/image (235).png" alt=""><figcaption></figcaption></figure>
2.  'Apply Folder Identification' 버튼을 클릭하여 선택한 폴더 내 파일에 식별을 적용할 수 있습니다. \
    또한 필터를 통해 원하는 파일에만 영향을 미치도록 할 수도 있습니다.\


    <figure><img src="../../.gitbook/assets/화면 캡처 2025-05-21 161642.png" alt=""><figcaption></figcaption></figure>



    <figure><img src="../../.gitbook/assets/화면 캡처 2025-05-21 161814.png" alt=""><figcaption></figcaption></figure>
3.  폴더 내 파일에 식별할 Component를 'Component'에 입력한 후 선택합니다.\


    <figure><img src="../../.gitbook/assets/image (203).png" alt=""><figcaption></figcaption></figure>
4. 'Apply' 버튼을 클릭하여 폴더 단위 검증을 완료합니다.

### 매칭되는 오픈소스 확인 및 검증

'> Top matched components'에서 >를 클릭하면 매칭된 오픈소스가 나타납니다.

우측의 ![](<../../.gitbook/assets/image (188).png>)아이콘을 클릭한 후 'Primary matches only', 'All matches'의 선택을 통해 확률이 높은 매칭 혹은 전체 매칭 내역을 확인하고 검증에 활용할 수 있습니다.

<figure><img src="../../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

매칭된 오픈소스 내역에서 > 모양을 클릭하면 매칭된 파일 경로를 확인할 수 있습니다.

파일 경로에서 우측에 있는 창 버튼을 클릭하면 새 창에서 파일 단위의 검증을 진행할 수 있습니다.

![](<../../.gitbook/assets/image (188).png>)아이콘을 클릭한 후 'Identify files matching'을 클릭하면 특정 오픈소스로 검증이 적용됩니다.

<figure><img src="../../.gitbook/assets/image (238).png" alt=""><figcaption></figcaption></figure>

### 검증 결과 되돌리기

폴더 단위로 검증된 내용을 되돌리고자 할 경우 다음과 같이 진행합니다.

1.  해당 폴더를 선택한 후, 'Apply Folder Identification' 버튼을 클릭합니다.\


    <figure><img src="../../.gitbook/assets/image (221).png" alt=""><figcaption></figcaption></figure>
2.  'Remove Component Identification ![](<../../.gitbook/assets/image (78).png>)' 버튼을 클릭하여 'Component'에 할당된 Component를 Remove 합니다.\


    <figure><img src="../../.gitbook/assets/image (230).png" alt=""><figcaption></figcaption></figure>
3.  ‘Override files with identifications’를 선택한 뒤 'Apply' 버튼을 클릭하면 Assign된 Component가 삭제됩니다.\


    <figure><img src="../../.gitbook/assets/image (227).png" alt=""><figcaption></figcaption></figure>
4.  이후 'Remove from Marked as Identified' 버튼을 클릭하면 선택된 폴더 하위의 파일 상태 값이 'Pending id'로 변경됩니다.\


    <figure><img src="../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>
