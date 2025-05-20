# 취약점 악용 가능성

취약점 악용 가능성 (VEX)은 소프트웨어 제품의 취약점 악용 가능성에 대한 정보를 전달하는 데 \
사용되는 표준화된 형식입니다.&#x20;

\
Workbench에서 VEX 정보는 다음 세 가지 엔티티와 연결됩니다.

* CVE
* CVE가 할당된 Component
* 식별 또는 Dependency에 Component가 있는 Scan



VEX 필드 상태, 응답 및 정당화에서 사용 가능한 값은 CycloneDX 사양을 따릅니다: [CycloneDX VEX](https://cyclonedx.org/docs/1.6/json/#vulnerabilities_items_analysis_state)



### VEX 재사용

<figure><img src="../../../.gitbook/assets/화면 캡처 2025-05-20 152442.png" alt=""><figcaption></figcaption></figure>

scan에서 'Reuse vex FROM Scan'을 이용해서 한 scan에서 다른 scan으로 VEX 정보를 가져와서 \
대상 scan에서 소스 scan과 일치하는 컴포넌트 요소 및 CVE를 검색할 수 있습니다.





### &#x20;<a href="#vulnerabilities-user-roles-and-permissions" id="vulnerabilities-user-roles-and-permissions"></a>

