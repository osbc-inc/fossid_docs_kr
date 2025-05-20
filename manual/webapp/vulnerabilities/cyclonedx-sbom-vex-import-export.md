# CycloneDX SBOM에서 VEX import/export

Workbench에서 생성된 CycloneDX 보고서에 취약점 악용 가능성 데이터가 추가되었습니다. 다음 옵션의 선택을 해제하여 CycloneDX 보고서에서 VEX 정보를 내보내지 않도록 설정할 수 있습니다.

<figure><img src="../../../.gitbook/assets/화면 캡처 2025-05-20 161245.png" alt=""><figcaption></figcaption></figure>

* CycloneDX 레포트를 import할때 VEX 정보가 자동으로 포함됩니다.
* 레포트에 포함된 Component는 Scan내의 dependency에서 추가되므로, VEX 정보를  첨부할 수 있습니다.
  * Component는  Workbench내에 이미 생성되어 있어야하고, CPE가 할당되어있어야 합니다.
  * Component는 import 시 생성되며 CycloneDX 리포트에 CPE 정보가 포함되어 있어야 합니다.
  * Component의 이름과 버전을 기반으로 데이터베이스에서 일치하는 CPE를 찾을 수 있어야 합니다.
  *
* CPE가 요구되는 이유는 취약점 정보와 VEX 정보는 컴포넌트에 CPE가 할당되어 있어야만 연결될 수 있기 때문입니다.

