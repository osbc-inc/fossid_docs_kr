# VSF (VulnSnippet Finder)

VSF는 Snippet 코드 단위로 보안 취약점 확인이 가능합니다.

일반적인 프로세스에서 취약점을 감지할 경우, 스캔한 파일과 취약한 파일 간에 파일 match가 full인 항목을 찾습니다. VSF를 사용하면 한 단계 더 나아가 파일 match가 full이 아닌 경우에도 스캔한 코드에 포함된 취약한 코드 Snippet을 찾을 수 있습니다.

따라서 프로젝트를 취약하게 만드는 정확한 코드 라인을 찾을 수 있습니다.

## 작동 원리

VSF는 확장된 클라이언트 기능을 사용하여 취약한 Snippet 검사를 수행합니다. 이 기능은 클라이언트 및 WebApp에서 액세스할 수 있습니다. 코드는 FossID security volume과 비교됩니다. (security volume이 활성화되어야 함)



## WebApp 사용

WebApp은 코드를 스캔하고 취약한 코드 Snippet을 찾기 위한 테스트 인터페이스를 제공합니다. 이 인터페이스를 사용하여 코드를 업로드하고 볼 수 있습니다.

이 기능은 User에게 VSF\_ACCESS 권한을 추가하여 WebApp에서 액세스할 수 있습니다. 이 권한은 WebApp의 VSF 인터페이스에 대한 액세스 권한만 부여합니다. WebApp 또는 CLI를 사용하여 파일을 Scan할 때 실제로 VSF 결과를 얻으려면 VSF enabled cli token을 구성해야 합니다.

WebApp에서 VSF에 액세스할 때 Scan을 위해 소스코드를 업로드할 수 있는 초기 인터페이스가 제공됩니다.

<figure><img src="../../../.gitbook/assets/image (107).png" alt=""><figcaption></figcaption></figure>

스캔이 수행된 후 CVSS Base Score 심각도별로 그룹화된 개요 정보가 표시됩니다. (CVSS2 및 CVSS3 모두 고려됨)

각 CVE는 취약점 설명과 함께 나열됩니다. 항목을 확장하면 각 취약점이 존재하는 파일 목록을 볼 수 있습니다.

<figure><img src="../../../.gitbook/assets/image (172).png" alt=""><figcaption></figcaption></figure>

파일을 선택하면 해당 보안 메타데이터와 함께 발견된 모든 CVE에 대한 정보가 제공되고 security volume에서 발견된 코드와 로컬 코드를 강조 표시하는 매칭 항목이 표시됩니다.

<figure><img src="../../../.gitbook/assets/image (138).png" alt=""><figcaption></figcaption></figure>

## 클라이언트 사용

매개변수 `--vsf`를 사용하여 대상 코드의 취약점을 스캔할 수 있습니다. 이 작업을 수행하려면 시스템에 **jq**를 설치해야 합니다.

Debian 기반 시스템에 jq를 설치하려면 다음을 실행하십시오.

```
sudo apt install jq
```

RedHat 또는 CentOS에 jq를 설치하려면 다음을 실행하십시오.

```
sudo yum install jq
```

#### 사용 예시

```
./fossid-cli --vsf '/tmp/t1_lib.c'
```

#### 결과

하기는 가독성을 위해 축약된 결과입니다.

```
{
    "date": "2020-11-03T12:32:33Z",
    "file": {
        "available": true,
        "encoding": "UTF-8",
        "id": "c66dd54d05901afad2a2eaa900000000",
        "md5": "c66dd54d05901afad2a2eaa900000000",
        "path": "CVE-2016-2177",
        "size": 145142
    },
    "local_path": "/tmp/t1_lib.c",
    "snippet": {
        "id": "37b03c7c0df744a1659a44cd06f6b0a3",
        "local_coverage": 0.03,
        "local_highlight": {
            "blocks": [
                {
                    "byte_range": {
                        "begin": 39643,
                        "end": 39765
                    },
                    "char_range": {
                        "begin": 39643,
                        "end": 39765
                    },
                    "hash_range": {
                        "begin": 1017,
                        "end": 1022
                    },
                    "id": "12bff91f4092e0405ccbac07d92b155c"
                }
            ],
            "encoding": "UTF-8",
            "id": "37b03c7c0df744a1659a44cd06f6b0a3",
            "pfm_format": 2
        },
        "local_size": 54,
        "remote_coverage": 0.02,
        "remote_highlight": {
            "blocks": [
                {
                    "byte_range": {
                        "begin": 85581,
                        "end": 85732
                    },
                    "char_range": {
                        "begin": 85581,
                        "end": 85732
                    },
                    "hash_range": {
                        "begin": 2066,
                        "end": 2071
                    },
                    "id": "12bff91f4092e0405ccbac07d92b155c"
                }
            ],
            "encoding": "UTF-8",
            "id": "37b03c7c0df744a1659a44cd06f6b0a3",
            "pfm_format": 2
        },
        "remote_size": 54
    },
    "type": "vulnerability",
    "vulnerability": {
        "details": {
            "configurations": {
                "CVE_data_version": "4.0",
                "nodes": [
                    {
                        "cpe_match": [
                            {
                                "cpe23Uri": "cpe:2.3:a:hp:icewall_sso:10.0:*:*:*:dfw:*:*:*",
                                "vulnerable": true
                            }
                        ],
                        "operator": "OR"
                    }
                ]
            },
            "cve": {
                "CVE_data_meta": {
                    "ASSIGNER": "cve@mitre.org",
                    "ID": "CVE-2016-2177"
                },
                "data_format": "MITRE",
                "data_type": "CVE",
                "data_version": "4.0",
                "description": {
                    "description_data": [
                        {
                            "lang": "en",
                            "value": "OpenSSL through 1.0.2h incorrectly uses pointer arithmetic for heap-buffer boundary checks, which might allow remote attackers to cause a denial of service (integer overflow and application crash) or possibly have unspecified other impact by leveraging unexpected malloc behavior, related to s3_srvr.c, ssl_sess.c, and t1_lib.c."
                        }
                    ]
                },
                "problemtype": {
                    "problemtype_data": [
                        {
                            "description": [
                                {
                                    "lang": "en",
                                    "value": "CWE-190"
                                }
                            ]
                        }
                    ]
                },
                "references": {
                    "reference_data": [
                        {
                            "name": "https://git.openssl.org/?p=openssl.git;a=commit;h=a004e72b95835136d3f1ea90517f706c24c03da7",
                            "refsource": "CONFIRM",
                            "tags": [
                                "Issue Tracking",
                                "Patch",
                                "Third Party Advisory"
                            ],
                            "url": "https://git.openssl.org/?p=openssl.git;a=commit;h=a004e72b95835136d3f1ea90517f706c24c03da7"
                        }
                    ]
                }
            },
            "impact": {
                "baseMetricV2": {
                    "cvssV2": {
                        "accessComplexity": "LOW",
                        "accessVector": "NETWORK",
                        "authentication": "NONE",
                        "availabilityImpact": "PARTIAL",
                        "baseScore": 7.5,
                        "confidentialityImpact": "PARTIAL",
                        "integrityImpact": "PARTIAL",
                        "vectorString": "AV:N/AC:L/Au:N/C:P/I:P/A:P",
                        "version": "2.0"
                    },
                    "exploitabilityScore": 10.0,
                    "impactScore": 6.4,
                    "obtainAllPrivilege": false,
                    "obtainOtherPrivilege": false,
                    "obtainUserPrivilege": false,
                    "severity": "HIGH",
                    "userInteractionRequired": false
                },
                "baseMetricV3": {
                    "cvssV3": {
                        "attackComplexity": "LOW",
                        "attackVector": "NETWORK",
                        "availabilityImpact": "HIGH",
                        "baseScore": 9.8,
                        "baseSeverity": "CRITICAL",
                        "confidentialityImpact": "HIGH",
                        "integrityImpact": "HIGH",
                        "privilegesRequired": "NONE",
                        "scope": "UNCHANGED",
                        "userInteraction": "NONE",
                        "vectorString": "CVSS:3.0/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:H",
                        "version": "3.0"
                    },
                    "exploitabilityScore": 3.9,
                    "impactScore": 5.9
                }
            },
            "lastModifiedDate": "2019-12-27T16:08Z",
            "publishedDate": "2016-06-20T01:59Z"
        },
        "id": "CVE-2016-2177",
        "url": "https://nvd.nist.gov/vuln/detail/CVE-2016-2177"
    }
}
```

#### Snippet 및 하이라이팅 표시 가져오기

매치 결과에서 로컬 하이라이팅 또는 원격 하이라이팅 데이터를 가져와 CLI에서 하이라이팅하도록 할 수 있습니다. 먼저 다음 명령어를 사용하여 하이라이팅 정보를 추출합니다.

```
$ fossid-cli --vsf '/tmp/t1_lib.c' | head -1 | jq .snippet.local_highlight -rc

{"blocks":[{"byte_range":{"begin":39643,"end":39765},"char_range":{"begin":39643,"end":39765},"hash_range":{"begin":1017,"end":1022},"id":"12bff91f4092e0405ccbac07d92b155c"}],"encoding":"UTF-8","id":"37b03c7c0df744a1659a44cd06f6b0a3","pfm_format":2}
```

\--highlight-input 명령줄 인수를 사용하여 fossid-cli에 대한 입력으로 하이라이팅 정보(위의 출력)를 사용합니다.

```
$ fossid-cli --highlight '/tmp/t1_lib.c' --highlight-input '{"blocks":[{"byte_range":{"begin":39643,"end":39765},"char_range":{"begin":39643,"end":39765},"hash_range":{"begin":1017,"end":1022},"id":"12bff91f4092e0405ccbac07d92b155c"}],"encoding":"UTF-8","id":"37b03c7c0df744a1659a44cd06f6b0a3","pfm_format":2}'

[   ]#ifndef OPENSSL_NO_NEXTPROTONEG
[   ]   s->s3->next_proto_neg_seen = 0;
[   ]#endif
[   ]
[   ]#ifndef OPENSSL_NO_HEARTBEATS
[   ]   s->tlsext_heartbeat &= ~(SSL_TLSEXT_HB_ENABLED |
[   ]                          SSL_TLSEXT_HB_DONT_SEND_REQUESTS);
[===]#endif
[===]
[===]   if (data >= (d+n-2))
[===]      goto ri_check;
[===]
[===]   n2s(data,length);
[===]   if (data+length != d+n)
[===]      {
[===]      *al = SSL_AD_DECODE_ERROR;
[   ]      return 0;
[   ]      }
[   ]
[   ]   while(data <= (d+n-4))
[   ]   {

```
