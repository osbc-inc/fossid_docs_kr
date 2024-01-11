# Jira 연동

FossID는 개발 및 검증 프로세스 활성화를 위해, Jira 연동 기능을 기본 탑재하고 있습니다.

사용자는 FossID 웹 인터페이스를 통해 Jira 티켓 생성이 가능합니다.

<figure><img src="../../.gitbook/assets/image (163).png" alt=""><figcaption></figcaption></figure>

Jira 연동을 위해 /fossid/etc/fossid.conf 파일을 아래와 같이 수정해야 합니다.

<pre data-overflow="wrap"><code>; Path to your JIRA API endpoint, more specifically to issue. Example of a path:
;   https://your_company.atlassian.net/rest/api/2/issue/
<strong>webapp_jira=https://your_company.atlassian.net/rest/api/2/issue/
</strong></code></pre>

Jira Ticket 생성 절차는 다음과 같습니다.

1. 스캔 내 좌측 상단의 'Generate JIRA Ticket' 아이콘을 클릭합니다.
2. Project Key, Jira Email 등 정보를 입력한 후 'Create' 버튼을 클릭합니다.
3.  아래와 같이 Jira에 이슈가 생성된 것을 확인할 수 있습니다.\


    <figure><img src="../../.gitbook/assets/image (133).png" alt=""><figcaption></figcaption></figure>
