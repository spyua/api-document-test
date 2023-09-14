---
title: Login API v1
language_tabs:
  - shell: Shell
  - http: HTTP
  - javascript: JavaScript
  - ruby: Ruby
  - python: Python
  - php: PHP
  - java: Java
  - go: Go
toc_footers: []
includes: []
search: true
highlight_theme: darkula
headingLevel: 2

---

<!-- Generator: Widdershins v4.0.1 -->

<h1 id="login-api">Login API v1</h1>

> Scroll down for code samples, example requests and responses. Select a language for code samples from the tabs above or the mobile navigation menu.

# Authentication

* API Key (Bearer)
    - Parameter Name: **Authorization**, in: header. JWT Authorization header using the Bearer scheme (Example: 'Bearer 12345abcdef')

<h1 id="login-api-account">Account</h1>

## post__Account_Create

`POST /Account/Create`

> Body parameter

```json
{
  "userName": "string",
  "password": "string"
}
```

<h3 id="post__account_create-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|[AccountCreate](#schemaaccountcreate)|false|none|

> Example responses

> 200 Response

```
{"message":"string","data":{"userName":"string","password":"string","token":"string"}}
```

```json
{
  "message": "string",
  "data": {
    "userName": "string",
    "password": "string",
    "token": "string"
  }
}
```

<h3 id="post__account_create-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Success|[AccountDtoApiResponse](#schemaaccountdtoapiresponse)|

<aside class="warning">
To perform this operation, you must be authenticated by means of one of the following methods:
Bearer
</aside>

## post__Account_Login

`POST /Account/Login`

> Body parameter

```json
{
  "userName": "string",
  "password": "string"
}
```

<h3 id="post__account_login-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|[AccountLogin](#schemaaccountlogin)|false|none|

> Example responses

> 200 Response

```
{"message":"string","data":{"userName":"string","password":"string","token":"string"}}
```

```json
{
  "message": "string",
  "data": {
    "userName": "string",
    "password": "string",
    "token": "string"
  }
}
```

<h3 id="post__account_login-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Success|[AccountDtoApiResponse](#schemaaccountdtoapiresponse)|

<aside class="warning">
To perform this operation, you must be authenticated by means of one of the following methods:
Bearer
</aside>

# Schemas

<h2 id="tocS_AccountCreate">AccountCreate</h2>
<!-- backwards compatibility -->
<a id="schemaaccountcreate"></a>
<a id="schema_AccountCreate"></a>
<a id="tocSaccountcreate"></a>
<a id="tocsaccountcreate"></a>

```json
{
  "userName": "string",
  "password": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|userName|string¦null|false|none|none|
|password|string¦null|false|none|none|

<h2 id="tocS_AccountDto">AccountDto</h2>
<!-- backwards compatibility -->
<a id="schemaaccountdto"></a>
<a id="schema_AccountDto"></a>
<a id="tocSaccountdto"></a>
<a id="tocsaccountdto"></a>

```json
{
  "userName": "string",
  "password": "string",
  "token": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|userName|string¦null|false|none|none|
|password|string¦null|false|none|none|
|token|string¦null|false|none|none|

<h2 id="tocS_AccountDtoApiResponse">AccountDtoApiResponse</h2>
<!-- backwards compatibility -->
<a id="schemaaccountdtoapiresponse"></a>
<a id="schema_AccountDtoApiResponse"></a>
<a id="tocSaccountdtoapiresponse"></a>
<a id="tocsaccountdtoapiresponse"></a>

```json
{
  "message": "string",
  "data": {
    "userName": "string",
    "password": "string",
    "token": "string"
  }
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|message|string¦null|false|none|none|
|data|[AccountDto](#schemaaccountdto)|false|none|none|

<h2 id="tocS_AccountLogin">AccountLogin</h2>
<!-- backwards compatibility -->
<a id="schemaaccountlogin"></a>
<a id="schema_AccountLogin"></a>
<a id="tocSaccountlogin"></a>
<a id="tocsaccountlogin"></a>

```json
{
  "userName": "string",
  "password": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|userName|string¦null|false|none|none|
|password|string¦null|false|none|none|

