---
title: Image Upload API v1
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

<h1 id="image-upload-api">Image Upload API v1</h1>

> Scroll down for code samples, example requests and responses. Select a language for code samples from the tabs above or the mobile navigation menu.

# Authentication

* API Key (Bearer)
    - Parameter Name: **Authorization**, in: header. JWT Authorization header using the Bearer scheme (Example: 'Bearer 12345abcdef')

<h1 id="image-upload-api-image">Image</h1>

## post__UploadImage

`POST /UploadImage`

*上傳圖片*

> Body parameter

```yaml
file: string

```

<h3 id="post__uploadimage-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|object|false|none|
|» file|body|string(binary)|false|none|

> Example responses

> 200 Response

```
{"message":"string","data":{"fileName":"string","originalFileName":"string","fileLinkPath":"string","mediaLink":"string","size":0,"createTime":"2019-08-24T14:15:22Z","updateTime":"2019-08-24T14:15:22Z"}}
```

```json
{
  "message": "string",
  "data": {
    "fileName": "string",
    "originalFileName": "string",
    "fileLinkPath": "string",
    "mediaLink": "string",
    "size": 0,
    "createTime": "2019-08-24T14:15:22Z",
    "updateTime": "2019-08-24T14:15:22Z"
  }
}
```

<h3 id="post__uploadimage-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Success|[ImageInformationDtoApiResponse](#schemaimageinformationdtoapiresponse)|

<aside class="warning">
To perform this operation, you must be authenticated by means of one of the following methods:
Bearer
</aside>

## delete__DeleteImage

`DELETE /DeleteImage`

<h3 id="delete__deleteimage-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|FileName|query|string|false|Cloud上檔案名稱|

> Example responses

> 200 Response

```
{"message":"string","data":null}
```

```json
{
  "message": "string",
  "data": null
}
```

<h3 id="delete__deleteimage-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Success|[ObjectApiResponse](#schemaobjectapiresponse)|

<aside class="warning">
To perform this operation, you must be authenticated by means of one of the following methods:
Bearer
</aside>

## get__GetAllImages

`GET /GetAllImages`

> Example responses

> 200 Response

```
{"message":"string","data":[{"fileName":"string","originalFileName":"string","fileLinkPath":"string","mediaLink":"string","size":0,"createTime":"2019-08-24T14:15:22Z","updateTime":"2019-08-24T14:15:22Z"}]}
```

```json
{
  "message": "string",
  "data": [
    {
      "fileName": "string",
      "originalFileName": "string",
      "fileLinkPath": "string",
      "mediaLink": "string",
      "size": 0,
      "createTime": "2019-08-24T14:15:22Z",
      "updateTime": "2019-08-24T14:15:22Z"
    }
  ]
}
```

<h3 id="get__getallimages-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Success|[ImageInformationDtoListApiResponse](#schemaimageinformationdtolistapiresponse)|

<aside class="warning">
To perform this operation, you must be authenticated by means of one of the following methods:
Bearer
</aside>

# Schemas

<h2 id="tocS_ImageInformationDto">ImageInformationDto</h2>
<!-- backwards compatibility -->
<a id="schemaimageinformationdto"></a>
<a id="schema_ImageInformationDto"></a>
<a id="tocSimageinformationdto"></a>
<a id="tocsimageinformationdto"></a>

```json
{
  "fileName": "string",
  "originalFileName": "string",
  "fileLinkPath": "string",
  "mediaLink": "string",
  "size": 0,
  "createTime": "2019-08-24T14:15:22Z",
  "updateTime": "2019-08-24T14:15:22Z"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|fileName|string¦null|false|none|Cloud上檔案名稱|
|originalFileName|string¦null|false|none|原始檔案名稱|
|fileLinkPath|string¦null|false|none|Cloud Storage Path|
|mediaLink|string¦null|false|none|Download Url|
|size|integer(int64)¦null|false|none|檔案大小|
|createTime|string(date-time)|false|none|none|
|updateTime|string(date-time)|false|none|none|

<h2 id="tocS_ImageInformationDtoApiResponse">ImageInformationDtoApiResponse</h2>
<!-- backwards compatibility -->
<a id="schemaimageinformationdtoapiresponse"></a>
<a id="schema_ImageInformationDtoApiResponse"></a>
<a id="tocSimageinformationdtoapiresponse"></a>
<a id="tocsimageinformationdtoapiresponse"></a>

```json
{
  "message": "string",
  "data": {
    "fileName": "string",
    "originalFileName": "string",
    "fileLinkPath": "string",
    "mediaLink": "string",
    "size": 0,
    "createTime": "2019-08-24T14:15:22Z",
    "updateTime": "2019-08-24T14:15:22Z"
  }
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|message|string¦null|false|none|none|
|data|[ImageInformationDto](#schemaimageinformationdto)|false|none|none|

<h2 id="tocS_ImageInformationDtoListApiResponse">ImageInformationDtoListApiResponse</h2>
<!-- backwards compatibility -->
<a id="schemaimageinformationdtolistapiresponse"></a>
<a id="schema_ImageInformationDtoListApiResponse"></a>
<a id="tocSimageinformationdtolistapiresponse"></a>
<a id="tocsimageinformationdtolistapiresponse"></a>

```json
{
  "message": "string",
  "data": [
    {
      "fileName": "string",
      "originalFileName": "string",
      "fileLinkPath": "string",
      "mediaLink": "string",
      "size": 0,
      "createTime": "2019-08-24T14:15:22Z",
      "updateTime": "2019-08-24T14:15:22Z"
    }
  ]
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|message|string¦null|false|none|none|
|data|[[ImageInformationDto](#schemaimageinformationdto)]¦null|false|none|none|

<h2 id="tocS_ObjectApiResponse">ObjectApiResponse</h2>
<!-- backwards compatibility -->
<a id="schemaobjectapiresponse"></a>
<a id="schema_ObjectApiResponse"></a>
<a id="tocSobjectapiresponse"></a>
<a id="tocsobjectapiresponse"></a>

```json
{
  "message": "string",
  "data": null
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|message|string¦null|false|none|none|
|data|any|false|none|none|

