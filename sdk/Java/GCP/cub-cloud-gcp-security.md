# Security

- [專案說明](#project-description)
- [1.GCP KMS](#gcp-kms)
  - [a.注入使用](#gcp-kms-di)
  - [b.建立金鑰設定](#gcp-kms-keySetting)
  - [c.方法使用說明](#gcp-kms-methodDoc)
    - [I.對明文進行對稱加密](#gcp-kms-se)
    - [II.對密文進行對稱解密](#gcp-kms-sd)
    - [III.對明文進行非對稱加密](#gcp-kms-ase)
    - [IV.非對稱解密](#gcp-kms-asd)
    - [V.使用HMAC加密明文](#gcp-kms-hmace)

<a id="project-description"></a>
## 專案說明
1. 實作GCP KMS相關功能。
2. 實作GCP Security Manager相關功能。

<a id="gcp-kms"></a>
## 1.GCP KMS
KMSServiceImpl 是一個用於密鑰管理和加解密操作的Java服務類。它實現了 KMSService<KeyInfoDto> 介面，提供
1. **對稱加密和解密**
2. **非對稱加密和解密**
3. **HMAC加密**

<a id="gcp-kms-di"></a>
### a.注入使用
``` java
  @Autowired
  KMSService<KeyInfoDto> kmsService;
```

<a id="gcp-kms-keySetting"></a>
### b.建立金鑰設定

``` java
  KeyInfoDto key = new KeyInfoDto();
  key.setProjectId("your-project-id");
  key.setLocationId("key-location-name");
  key.setKeyRingId("key-ring-id");   
  key.setKeyId("key-id");
  key.setKeyVersion("key-version");
```

<a id="gcp-kms-methodDoc"></a>
### c.方法使用說明

<a id="gcp-kms-se"></a>
#### I. 對明文進行對稱加密 : symmetricEncrypt(KeyInfoDto keyInfo, String plaintext) 
- 參數
  - keyInfo : 密鑰信息DTO。
  - plaintext : 需要加密的明文。
- 返回值 : 加密後的Base64字串。

- 範例 :
  ``` java
  KeyInfoDto keyInfo = new KeyInfoDto();
 
  // 設置您的keyInfo參數...
 
  String encrypt = kmsService.symmetricEncrypt(keyInfo,"明文");
  ```
<br/>

<a id="gcp-kms-sd"></a>
#### II. 對密文進行對稱解密 : symmetricDecrypt(KeyInfoDto keyInfo, String ciphertext)
- 參數
  - keyInfo : 密鑰信息DTO。
  - ciphertext : 需要解密的密文。
- 返回值 : 解密後的明文。

- 範例 :
  ``` java
  KeyInfoDto keyInfo = new KeyInfoDto();
 
  // 設置您的keyInfo參數...
 
  String decrypt = kmsService.symmetricDecrypt(key,"密文");
  ```
<br/>

<a id="gcp-kms-ase"></a>
#### III. 對明文進行非對稱加密 : asymmetricEncrypt(KeyInfoDto keyInfo, String publicKeyPem, String plaintext)
- 參數
  - keyInfo: 密鑰信息DTO。
  - publicKeyPem: 公鑰（PEM格式）。
  - plaintext: 需要加密的明文。
- 返回值: 加密後的Base64字串。

- 範例:
  ``` java
  KeyInfoDto keyInfo = new KeyInfoDto();
 
  // 設置您的keyInfo參數...

  // 從檔案讀取pem
  String publicKeyPath = Paths.get("pub file path");
  String publicKeyPem = new String(Files.readAllBytes(publicKeyPath), StandardCharsets.UTF_8);
  
  // 非對稱加密
  String asyncEncrypt = kmsService.asymmetricEncrypt(key,publicKeyPem,testPlaintext);
  ```
<br/>

<a id="gcp-kms-asd"></a>
#### IV. 非對稱解密 : asymmetricDecrypt(KeyInfoDto keyInfo, String ciphertext)
- 參數
  - keyInfo: 密鑰信息DTO。
  - ciphertext: 需要解密的密文。
- 返回值: 解密後的明文。

- 範例:
  ``` java
  KeyInfoDto keyInfo = new KeyInfoDto();
 
  // 設置您的keyInfo參數...
 
  String decryptedText = kmsServiceImpl.asymmetricDecrypt(keyInfo, "密文");
  ```
  <br/>

<a id="gcp-kms-hmace"></a>
#### V. 使用HMAC加密明文 : hmacEncrypt(KeyInfoDto keyInfo, String plaintext)
- 參數
  - keyInfo: 密鑰信息DTO。
  - plaintext: 需要加密的明文。
- 返回值: 加密後的Base64字串。

- 範例:
  ``` java
  KeyInfoDto keyInfo = new KeyInfoDto();
 
  // 設置您的keyInfo參數...
 
  String hmacEncryptedText = kmsServiceImpl.hmacEncrypt(keyInfo, "明文");
  ```