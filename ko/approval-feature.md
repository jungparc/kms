## Security > Secure Key Manager > 콘솔 사용 가이드 > 승인 기능

국내·외 보안 인증 심사(ISMS-P, ISO 등)에서 요구하는 안전한 암호화 키 관리 요구 사항을 충족하기 위해 사용하는 Secure Key Manager의 승인 기능에 대해 설명합니다.

## 승인 기능 활성화
승인 기능을 사용 하기 위해서는 먼저 승인 기능을 활성화 해야합니다.
조직 관리 화면의 거버넌스 설정에서 승인 프로세스 관리 설정을 통해 Secure Key Manager의 승인 기능을 활성화합니다.

![console-guide-29](http://static.toastoven.net/prod_kms/2023-03-28-ko/console-guide-29.png)

### 승인 기능 역할 설정
Secure Key Manager의 멤버 관리를 통해 승인자(APPROVAL ADMIN), 요청자(APPROVAL MEMBER) 역할을 획득하여 승인 절차를 진행합니다.

![console-guide-30](http://static.toastoven.net/prod_kms/2023-03-28-ko/console-guide-30.png)

### 승인 기능 활성화 여부에 따른 차이점
승인 기능을 활성화하고 승인자 또는 요청자 역할을 획득하면 Secure Key Manager에 **승인리스트**와 **키 저장소 관리** 탭이 추가됩니다. 두 탭은 승인자, 요청자만 접근할 수 있습니다.

![console-guide-31](http://static.toastoven.net/prod_kms/2023-03-28-ko/console-guide-31.png)

승인 기능을 활성화하면 더 이상 키 저장소에서 데이터를 추가, 수정, 삭제할 수 없으며, 변경 요청 시 **키 저장소 관리** 탭으로 이동합니다.

![console-guide-32](http://static.toastoven.net/prod_kms/2023-03-28-ko/console-guide-32.png)

## 승인 과정

### 승인 요청 작성
승인자와 요청자는 **키 저장소 관리** 탭에서 키 저장소별로 변경 내용을 승인 요청할 수 있습니다. 기존의 키 저장소와 유사한 동작을 통해 추가, 수정, 삭제를 진행합니다. 키, 인증 정보의 변경 상태에 대해서는 다음과 같이 상태에 표시됩니다.

![console-guide-33](http://static.toastoven.net/prod_kms/2023-03-28-ko/console-guide-33.png)

![console-guide-34](http://static.toastoven.net/prod_kms/2023-03-28-ko/console-guide-34.png)

키 저장소의 **승인 요청** 버튼으로 승인을 요청하고 해당 프로젝트의 승인 요청들은 **승인리스트** 탭에서 확인할 수 있습니다.

![console-guide-35](http://static.toastoven.net/prod_kms/2023-03-28-ko/console-guide-35.png)

### 승인 요청 반영
승인자는 **승인리스트**에서 키 저장소의 변경 승인 요청을 확인하고 **승인** 또는 **거절**을 선택해 반영 여부를 결정합니다.

![console-guide-36](http://static.toastoven.net/prod_kms/2023-03-28-ko/console-guide-36.png)

승인을 누르는 즉시 키 저장소에 반영됩니다. **키 저장소** 또는 **키 저장소 관리** 탭에서 변경 내용을 확인할 수 있습니다.

![console-guide-37](http://static.toastoven.net/prod_kms/2023-03-28-ko/console-guide-37.png)