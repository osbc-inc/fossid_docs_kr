# Permission

사용 가능한 Permission 목록은 다음과 같습니다.

로그인한 User의 Permission이나 Role이 변경된 경우 이 변경 사항을 적용하려면 User가 로그아웃 후 다시 로그인해야 합니다.

| Code                                   | Name                                                            | Description                                                |
| -------------------------------------- | --------------------------------------------------------------- | ---------------------------------------------------------- |
| APPROVAL\_POLICY\_GLOBAL               | Policies - Change global approval policies                      | 전체 Component 승인 정책을 설정 및 업데이트할 수 있습니다.                     |
| COMPONENT\_HASH\_ACCESS                | Components - Access component intake interface                  | Component intake/hashing 인터페이스에 액세스할 수 있습니다.               |
| COMPONENTS\_CREATE                     | Components - Create New Components                              | Component를 수동으로 생성하고, 수동으로 생성한 Component 정보를 업데이트할 수 있습니다. |
| COMPONENTS\_EDIT\_ANY                  | Components - Edit any existing components                       | 기존 Component 정보를 업데이트할 수 있습니다.                             |
| EDIT\_OWN\_KB\_COMPONENTS              | Components - Edit own components created from KB identification | KB 식별의 결과로 User가 생성한 Component를 업데이트할 수 있습니다.              |
| VIEW\_DEBUG\_INFORMATION               | FossID Webapp Debug - View System Debug Information             | 인터페이스에서 시스템 디버그 정보를 확인할 수 있습니다.                            |
| IGNORE\_RULES\_SET\_GLOBAL             | Ignore rules - Set global ignore rules                          | 새로운 global ignore 규칙을 추가할 수 있습니다.                          |
| JIRA\_CREATE\_TICKETS                  | Jira - Allow creation of tickets in Jira                        | Jira 티켓을 생성할 수 있습니다.                                       |
| LICENSES\_ADMINISTRATE                 | Licenses - Administrate licenses                                | 새로운 License를 생성하고 기존 License를 삭제 및 업데이트할 수 있습니다.           |
| LOG\_ACCESS                            | Log - Access to System Log View                                 | 시스템 로그에 액세스할 수 있습니다.                                       |
| LOG\_DELETE                            | Log - Deletes existing Log entries                              | 존재하는 로그 항목을 삭제할 수 있습니다.                                    |
| MESSAGES\_BROADCAST\_ALL               | Messages - Broadcast message to all active users                | 모든 User에게 Broadcast 메시지를 보낼 수 있습니다.                        |
| PROJECT\_ACCESS\_ANY                   | Projects - Access & Search any project                          | 기존 Project를 검색하고 액세스할 수 있습니다.                              |
| PROJECTS\_MANAGE\_STRING\_MATCH\_RULES | Projects - Add and remove string match rules                    | Project에 대한 String Match 규칙을 관리할 수 있습니다.                   |
| PROJECTS\_CREATE                       | Projects - Create New Projects                                  | 새 Project를 생성할 수 있습니다.                                     |
| PROJECT\_DELETE\_ANY                   | Projects - Delete any project                                   | Project를 삭제할 수 있습니다.                                       |
| PROJECT\_LIST\_ALL                     | Projects - List all projects                                    | 모든 Project 목록을 확인할 수 있습니다.                                 |
| PROJECTS\_COMPONENT\_APPROVER          | Projects - Projects components approver                         | Project 내 Component를 승인 또는 거부할 수 있습니다.                     |
| PROJECT\_UPDATE\_ANY                   | Projects - Update any project                                   | 모든 Project 정보를 업데이트할 수 있습니다.                               |
| QUICK\_VIEW\_ACCESS                    | Quick View - Access Quick View Interface                        | Quick View 인터페이스에 액세스할 수 있습니다.                             |
| ROLES\_PERMISSIONS\_ADMINISTRATE       | Roles & Permissions - Administrate Roles & Permissions          | 시스템 관리자의 Role 및 Permission을 갖습니다.                          |
| SCAN\_ACCESS\_ANY                      | Scans - Access & Search any scan                                | 모든 Scan을 검색하고 액세스할 수 있습니다.                                 |
| SCAN\_ACCESS                           | Scans - Access Scans Interface                                  | Scans 인터페이스에 액세스할 수 있습니다.                                  |
| GLOBAL\_MANAGE\_STRING\_MATCH\_RULES   | Scans - Add and remove Global String Match Rules                | 전체 String Match 규칙을 관리할 수 있습니다.                            |
| SCANS\_MANAGE\_STRING\_MATCH\_RULES    | Scans - Add and remove String Match Rules                       | Scan에 대한 String Match 규칙을 관리할 수 있습니다.                      |
| SCAN\_CREATE                           | Scans - Create New Scans                                        | 새 Scan을 생성할 수 있습니다.                                        |
| SCAN\_DELETE\_ANY                      | Scans - Delete any scan                                         | 모든 Scan을 삭제할 수 있습니다.                                       |
| SCANS\_LIST\_ALL                       | Scans - List all scans                                          | 모든 Scan 목록을 확인할 수 있습니다.                                    |
| REFRESH\_FILES                         | Scans - Look for file changes in file system                    | “Look for file changes in file system” 기능을 사용할 수 있습니다.     |
| SCAN\_UPDATE\_ANY                      | Scans - Update any scan                                         | 모든 Scan 정보를 업데이트할 수 있습니다.                                  |
| SNIPPET\_SEARCH\_ACCESS                | Snippet Search - Access Snippet Search Interface                | Snippet Search 인터페이스에 액세스할 수 있습니다.                         |
| MANAGE\_BACKUPS                        | System - Manage database backups                                | WebApp에서 백업 생성, 삭제 및 복원이 가능합니다.                            |
| SYSTEM\_ACCESS                         | System - View System Information                                | System Information 및 Update CPE List 인터페이스에 액세스할 수 있습니다.   |
| EDIT\_FOSSID\_CONFIGURATION            | System - Edit FossID configuration file                         | FossID configuration을 편집할 수 있습니다.                          |
| USERS\_DELETE\_ANY                     | Users - Delete any user                                         | 모든 User를 삭제할 수 있습니다.                                       |
| USERS\_EDIT\_ANY                       | Users - Edit any User                                           | 모든 User 정보를 업데이트할 수 있습니다.                                  |
| USERS\_ASSIGN\_PERMISSIONS             | Users - Users assign Roles & Permissions                        | User에게 Role 및 Permission을 할당할 수 있습니다.                      |
| VIEW\_SECURITY\_INFORMATION            | Vulnerabilities - View security information                     | 보안 정보를 업데이트할 수 있습니다.                                       |
| VSF\_ACCESS                            | VSF - Access VSF Interface                                      | VSF 인터페이스에 액세스할 수 있습니다.                                    |
| WHITE\_LIST\_ADMIN                     | Whitelisting - Access to whitelist administration               | 화이트리스트 관리 인터페이스에 액세스할 수 있습니다.                              |
