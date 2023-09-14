# Storage

### 專案說明
1. 實作GCP Storage相關功能

### Sample
前置作業：設定檔
需在 application.properties 中設定 project-id
```properties
 # Storage
 spring.cloud.gcp.project-id=[PROJECT_ID]
```

前置作業：引入Storage
``` java
  @Autowired
  private StorageImpl storage;
```

<br/>

- 上傳檔案
  ```java
  try {
      storage.upload([UPLOAD_PATH], [STREAM], [APPEND]);
  } catch (Exception e) {
      throw new RuntimeException(e);
  }
  ```
  參數說明
 
  | 參數          | 格式          | 說明                                                                                                                            |
  |-------------|-------------|-------------------------------------------------------------------------------------------------------------------------------|
  | UPLOAD_PATH | Path        | 上傳路徑： [bucket_name]/[folder]/[fileName.txt]                                                                                   |
  | STREAM      | InputStream | 檔案內容                                                                                                                          |
  | APPEND      | Boolean     | true: 新增檔案，會檢查檔案是否存在 <br/> false: 更新檔案，需存在相同檔名<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 直接複寫，若檔案修改前被異動則更新失敗 |

<br/>

- 讀取檔案（單筆）
  ```java
  InputStream stream = service.getStorageObject([PATH]);
  // print file content
  String content = new BufferedReader(new InputStreamReader(stream))
                .lines().collect(Collectors.joining("\n"));
  System.out.println(content);
  ```
  參數說明

  | 參數 | 格式    | 說明       |
  |-----|--------|----------|
  | PATH | Path  | 檔案路徑： [bucket_name]/[folder]/[fileName.txt] |

<br/>

- 取得檔案（多筆）
  ```java
    List<String> path = new ArrayList<String>();
    path.add([PATH_1]);
    path.add([PATH_2]);
    List<StorageObject<Blob>> files = storage.getStorageObjects(path);
  ```
  參數說明

  | 參數 | 格式    | 說明       |
  |-----|--------|----------|
  | PATH | Path  | 檔案路徑： [bucket_name]/[folder]/[fileName.txt] |

<br/>

 - 篩選檔案
    ```java
      StorageListOptions options = new StorageListOptions();
      options.setBucketName([BucketName]);
      options.setFolderPath([Folder]);
      options.setFilePrefix([FilePrefix]);
      options.setMaxResults(10);
      List<StorageObject<Blob>> blobs = storage.getStorageObjects(options);
    ```
    參數說明

    | 參數         | 格式      | 說明                                                |
    |------------|---------|---------------------------------------------------|
    | BucketName | String  | bucket名稱                                          |
    | FolderPath | Path    | 檔案資料夾位置                                           |
    | FilePrefix | String  | 檔名前綴，要使用前綴<font color=red>必須包含</font>FolderPath設定 |
    | MaxResults | Integer | 最大資料數                                             |

    [參考文件]( https://cloud.google.com/storage/docs/samples/storage-list-files-with-prefix)