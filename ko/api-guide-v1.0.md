
## Security > Secure Key Manager > API v1.0 가이드

Secure Key Manager는 사용자 데이터에 접근할 수 있는 다양한 API를 제공합니다. 클라이언트는 키 저장소에 설정한 인증을 통과한 후 Secure Key Manager에 저장한 데이터를 사용할 수 있습니다.

[API 목록]

| Method | URI | 설명 |
|---|---|---|
| GET | /keymanager/v1.0/appkey/{appkey}/confirm | API를 호출한 클라이언트 정보를 제공합니다. |
| GET | /keymanager/v1.0/appkey/{appkey}/secrets/{keyid} | Secure Key Manager에 저장한 기밀 데이터를 조회합니다. |
| POST | /keymanager/v1.0/appkey/{appkey}/symmetric-keys/{keyid}/encrypt | Secure Key Manager에 저장한 대칭키로 데이터를 암호화합니다. |
| POST | /keymanager/v1.0/appkey/{appkey}/symmetric-keys/{keyid}/decrypt | Secure Key Manager에 저장한 대칭키로 데이터를 복호화합니다. |
| POST | /keymanager/v1.0/appkey/{appkey}/symmetric-keys/{keyid}/create-local-key | 클라이언트가 로컬 환경에서 데이터 암/복호화에 사용할 수 있는 AES-256 대칭키를 생성합니다. |
| GET | /keymanager/{version}/appkey/{appkey}/symmetric-keys/{keyid}/symmetric-key | Secure Key Manager에 저장한 대칭키를 조회합니다. |
| POST | /keymanager/v1.0/appkey/{appkey}/asymmetric-keys/{keyid}/sign | Secure Key Manager에 저장한 비대칭키로 데이터를 서명합니다. |
| POST | /keymanager/v1.0/appkey/{appkey}/asymmetric-keys/{keyid}/verify | Secure Key Manager에 저장한 비대칭키로 데이터와 서명을 검증합니다. |

[API 요청의 HTTP 헤더]

Secure Key Manager의 MAC 주소 인증을 사용하려면 HTTP 헤더에 클라이언트 MAC 주소를 설정해서 요청해야 합니다.
```
X-TOAST-CLIENT-MAC-ADDR: {MAC 주소}
```

[API 요청의 경로 변수]

| 이름 | 타입 | 설명 |
|---|---|---|
| appkey | String | 사용하려는 데이터를 저장하고 있는 NHN Cloud 프로젝트의 앱키 |
| keyid | String | 사용하려는 데이터의 식별자 |

[API 응답의 데이터 공통 헤더]
```
{
    "header": {
        "resultCode": 0,
        "resultMessage": "success",
        "isSuccessful": true
    },
    "body": {
        ...
    }
}
```
| 이름 | 타입 | 설명 |
|---|---|---|
| resultCode | Number | API 호출 결과 코드값 |
| resultMessage | String | API 호출 결과 메시지 |
| isSuccessful | Boolean | API 호출 성공 여부 |

## 클라이언트 정보 조회
API를 호출한 클라이언트 정보를 조회할 때 사용합니다.
```
GET https://api-keymanager.cloud.toast.com/keymanager/v1.0/appkey/{appkey}/confirm
```
[Response Body]

```
{
    "header": {
        ...
    },
    "body": {
        "clientIp": "0.0.0.0",
        "clientMacHeader": "00:00:00:00:00:00",
        "clientSentCerfificate": false
    }
}
```
| 이름 | 타입 | 설명 |
|---|---|---|
| clientIp | String | API를 호출한 클라이언트의 IP 주소 |
| clientMacHeader | String |API를 호출한 클라이언트의 MAC 주소 헤더값 |
| clientSentCertificate | Boolean | API를 호출한 클라이언트가 인증서를 사용하고 있는지 여부 |

## 기밀 데이터

### 기밀 데이터 조회
Secure Key Manager에 저장한 기밀 데이터를 조회할 때 사용합니다.
```
GET https://api-keymanager.cloud.toast.com/keymanager/v1.0/appkey/{appkey}/secrets/{keyid}
```

[Response Body]
```
{
    "header": {
        ...
    },
    "body": {
        "secret": "data"
    }
}
```
| 이름 | 타입 | 설명 |
|---|---|---|
| secret | String | 기밀 데이터 조회 결과 |

## 대칭키

### 대칭키 암호화
Secure Key Manager에 생성한 대칭키로 데이터를 암호화할 때 사용합니다. 사용자는 32KB 이하의 텍스트 데이터를 전달해서 Secure Key Manager에 저장한 대칭키로 암호화할 수 있습니다.
```
POST https://api-keymanager.cloud.toast.com/keymanager/v1.0/appkey/{appkey}/symmetric-keys/{keyid}/encrypt
```

[Request Body]

```
{
    "plaintext": "data"
}
```
| 이름 | 타입 | 설명 |
|---|---|---|
| plaintext | String | 대칭키로 암호화할 데이터 |

[Response Body]
```
{
    "header": {
        ...
    },
    "body": {
        "ciphertext": "AAAAABzGwQniNneKXmcOLhWnxEqC1rNY+UdVb3lyeX/4wSrP",
        "keyVersion": 1
    }
}
```
| 이름 | 타입 | 설명 |
|---|---|---|
| ciphertext | String | 대칭키로 데이터를 암호화한 결과 |
| keyVersion | Number | API 요청 처리에 사용한 대칭키 버전 |

### 대칭키 복호화
Secure Key Manager에 생성한 대칭키로 데이터를 복호화할 때 사용합니다. 사용자는 암호화된 텍스트를 전달해서 Secure Key Manager에 저장한 대칭키로 복호화할 수 있습니다.
```
POST https://api-keymanager.cloud.toast.com/keymanager/v1.0/appkey/{appkey}/symmetric-keys/{keyid}/decrypt
```

[Request Body]
```
{
    "ciphertext": "AAAAABzGwQniNneKXmcOLhWnxEqC1rNY+UdVb3lyeX/4wSrP"
}
```
| 이름 | 타입 | 설명 |
|---|---|---|
| ciphertext | String | 대칭키로 복호화할 데이터 |

[Response Body]
```
{
    "header": {
        ...
    },
    "body": {
        "plaintext": "data",
        "keyVersion": 1
    }
}
```
| 이름 | 타입 | 설명 |
|---|---|---|
| plaintext | String | 대칭키로 데이터를 복호화한 결과 |
| keyVersion | Number | API 요청 처리에 사용한 대칭키 버전 |

### 대칭키로 암호화한 로컬 대칭키 생성
클라이언트가 로컬 환경에서 사용할 수 있는 AES-256 대칭키를 생성할 때 사용합니다. localKeyPlaintext는 생성한 대칭키를 Base64 인코딩한 형태이며 Base64 디코딩 후 바로 사용할 수 있습니다. localKeyCiphertext는 생성한 대칭키를 Secure Key Manager에 저장한 대칭키로 암호화한 후 Base64 인코딩한 형태이며 스토리지에 저장할 때 사용합니다. 스토리지에 저장한 대칭키는 복호화 API를 사용해서 복호화한 후 사용할 수 있습니다.
```
POST https://api-keymanager.cloud.toast.com/keymanager/v1.0/appkey/{appkey}/symmetric-keys/{keyid}/create-local-key
```

[Response Body]
```
{
    "header": {
        ...
    },
    "body": {
        "localKeyPlaintext": "srV7MWkYIfYBknkASzwSEK1Z1y9Nx0f/RMZ3MSVIjm8=",
        "localKeyCiphertext": "v1s1WkiIj3KR+AafnupNv9xcX/JhL4GUzUr8mzLRpjbGuoAwU/GgboM/6QdRRY24",
        "keyVersion": 1
    }
}
```
| 이름 | 타입 | 설명 |
|---|---|---|
| localKeyPlaintext | String | Base64 인코딩한 AES-256 대칭키 |
| localKeyCiphertext | String | Secure Key Manager에 저장한 대칭키로 암호화한 후 Base64 인코딩한 AES-256 대칭키 |
| keyVersion | Number | API 요청 처리에 사용한 대칭키 버전 |

### 대칭키 조회

Secure Key Manager에 저장한 대칭키(AES-256)를 조회할 수 있습니다.

#### v1.0

```
GET https://api-keymanager.cloud.toast.com/keymanager/v1.0/appkey/{appkey}/symmetric-keys/{keyid}/symmetric-key
```

[Response Body]
```
{
    "header": {
        ...
    },
    "body": {
        "symmetricKey": "0x00, 0x20, 0x00, 0x41, 0x00, 0x20, 0x00, 0x73, 0x00, 0x69, 0x00, 0x6d, 0x00, 0x70, 0x00, 0x6c, 0x00, 0x65, 0x00, 0x20, 0x00, 0x4a, 0x00, 0x61, 0x00, 0x76, 0x00, 0x61, 0x00, 0x2e, 0x00, 0x20"
    }
}
```
| 이름 | 타입 | 설명 |
|---|---|---|
| symmetricKey | String | 대칭키 데이터(Hex String 형태) |

#### v1.1

```
GET https://api-keymanager.cloud.toast.com/keymanager/v1.1/appkey/{appkey}/symmetric-keys/{keyid}/symmetric-key?keyVersion={keyVersion}
```

[Request Parameter]

| 이름 | 타입 | 설명 |
|---|---|---|
| keyVersion | Number | 조회하려는 대칭키 버전 |

[Response Body]
```
{
    "header": {
        ...
    },
    "body": {
        "symmetricKey": "0x00, 0x20, 0x00, 0x41, 0x00, 0x20, 0x00, 0x73, 0x00, 0x69, 0x00, 0x6d, 0x00, 0x70, 0x00, 0x6c, 0x00, 0x65, 0x00, 0x20, 0x00, 0x4a, 0x00, 0x61, 0x00, 0x76, 0x00, 0x61, 0x00, 0x2e, 0x00, 0x20",
        "keyVersion": 1
    }
}
```
| 이름 | 타입 | 설명 |
|---|---|---|
| symmetricKey | String | 대칭키 데이터(Hex String 형태) |
| keyVersion | Number | API 요청 처리에 사용한 대칭키 버전 |

## 비대칭키

### 비대칭키로 서명
Secure Key Manager에 생성한 비대칭키로 데이터를 서명할 때 사용합니다. 사용자는 245 Byte 이하의 텍스트 데이터를 전달해서 Secure Key Manager에 저장한 비대칭키로 서명할 수 있습니다.
```
POST https://api-keymanager.cloud.toast.com/keymanager/v1.0/appkey/{appkey}/asymmetric-keys/{keyid}/sign
```

[Request Body]
```
{
    "plaintext": "data"
}
```
| 이름 | 타입 | 설명 |
|---|---|---|
| plaintext | String | 비대칭키로 서명할 데이터 |

[Response Body]
```
{
    "header": {
        ...
    },
    "body": {
        "signature": "AAAAAGI9zf831DX...",
        "keyVersion": 1
    }
}
```
| 이름 | 타입 | 설명 |
|---|---|---|
| signature | String | 비대칭키로 데이터를 서명한 서명값 |
| keyVersion | Number | API 요청 처리에 사용한 비대칭키 버전 |

### 비대칭키로 데이터 검증
Secure Key Manager에 생성한 비대칭키로 데이터를 검증할 때 사용합니다. 사용자는 데이터와 서명값을 전달해서 Secure Key Manager에 저장한 비대칭키로 데이터가 위변조되지 않았음을 검증할 수 있습니다.
```
POST https://api-keymanager.cloud.toast.com/keymanager/v1.0/appkey/{appkey}/asymmetric-keys/{keyid}/verify
```

[Request Body]

```
{
    "plaintext": "data",
    "signature": "AAAAAGI9zf831DX..."
}
```
| 이름 | 타입 | 설명 |
|---|---|---|
| plaintext | String | 비대칭키로 검증할 데이터 |
| signature | String | 비대칭키로 데이터를 서명한 서명값 |

[Response Body]

```
{
    "header": {
        ...
    },
    "body": {
        "result": true,
        "keyVersion": 1
    }
}
```
| 이름 | 타입 | 설명 |
|---|---|---|
| result | Boolean | 비대칭키로 데이터와 서명값을 검증한 결과 |
| keyVersion | Number | API 요청 처리에 사용한 비대칭키 버전 |
