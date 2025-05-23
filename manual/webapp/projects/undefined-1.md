# 화이트리스트

#### &#x20;화이트리스트(Whitelisting) 활성화(선택사항)

* FossID 20.2 버전부터 기본적으로 화이트리스트 기능은 비활성화 상태입니다.
* 활성화하려면 `fossid.conf` 파일 내 `webapp_cli_command` 파라미터에 `--fields +mid` 옵션을 추가하세요. 예시:

```ini
webapp_cli_command=/fossid/bin/fossid-cli --config /fossid/etc/fossid.conf --fields +mid
```

* 주의: 화이트리스트 기능 활성화 이전에 스캔했던 파일은 재스캔이 필요합니다. 그래야 해당 파일에서 화이트리스트 규칙을 추가할 수 있습니다.

\


***





화이트리스트 규칙을 관리하여 자동 생성 코드 또는 매우 일반적인 코드와 같이 검증 시 예외시키고자 하는 매칭 결과에 대해 향후 Scan에 표시되지 않도록 할 수 있습니다.

### 전제 조건

**fossid.conf** 파일의 webapp\_cli\_command에 `'--fields +mid'`를 추가해야 합니다.



## 화이트리스트 규칙 생성

매치율이 '**partial**'인 매칭 결과를 대상으로 'Add whitelisting rule'을 통해 화이트리스트 규칙을 생성할 수 있습니다.

<figure><img src="../../../.gitbook/assets/image (167).png" alt=""><figcaption></figcaption></figure>

* 화이트리스트 규칙은 매치율이 'partial'인 매칭 결과에 대해서만 추가할 수 있습니다.
* 화이트리스트는 Project 단위로 적용 가능하며, 특정 Project 내의 모든 Scan에 영향을 미칩니다.
* 화이트리스트는 FossID WebApp 서버 이름(fossid.conf의 webapp\_server\_name)과 연결됩니다. 서버 이름이 변경되면 모든 화이트리스트가 삭제됩니다.

## 화이트리스트 규칙 확인

'Whitelist Administration'을 클릭하여 Project 내 화이트리스트 규칙을 확인할 수 있습니다.

<figure><img src="../../../.gitbook/assets/image (112).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (149).png" alt=""><figcaption></figcaption></figure>
