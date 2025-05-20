# 취약점 사용자 역할 및 권한

<figure><img src="../../../.gitbook/assets/화면 캡처 2025-05-20 161324.png" alt=""><figcaption></figcaption></figure>

\[권한]

* VULNERABILITIES\_VIEW\_ACCESS : 기본적으로 Workbench의 모든 기존 역할과 새로 생성된 역할인 "Security Officer"에 할당되며 Vulnerbilities 페이지와 메인 페이지에 대한 접근 권한을 제공합니다. \
  \
  이 권한은 사용자의 Project 및 Scan에 대한 취약점과 VEX 정보를 볼 수 있는 접근 권한도 제공합니다.
* VULNERABILITIES\_ACCESS\_ANY : API에서 모든 Project 및 Scan의 취약점 및 VEX 정보에 대한 보기 권한을 부여합니다. Vulnerbilities 페이지에서는 PROJECTS\_LIST\_ALL 및 SCANS\_LIST\_ALL 권한과 함께 사용해야 합니다.
* VEX\_EDIT : 자신의 모든 Project 및 Scan의 VEX를 생성/편집하고 “VEX 재사용”할 수 있습니다.(접근 권한이 없는 Scan에서 VEX를 재사용 할 수 없습니다.
* VEX\_EDIT\_ANY : 모든 Project 및 Scan에서 VEX를 생성, 편집, 재사용 모두할 수 있습니다.



\[역할]

* Security Officer : 이 역할에는 VULNERABILITIES\_VIEW\_ACCESS, VEX\_EDIT, VEX\_EDIT\_ANY, PROJECT\_LIST\_ALL, SCAN\_LIST\_ALL 의 모든 권한이 부여됩니다.
