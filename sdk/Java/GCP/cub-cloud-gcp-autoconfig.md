# 自動化設定專案
> 例如ＤＢ連線我們會封裝設定方式起來，讓使用者透過參數檔即可連線ＤＢ且會仿造Spring Boot的功能將Connection Pool & transaction & DataSource自動配置好

# 如何連線資料庫

配置設定檔已達到資料庫的連線自動化配置，在目前SDK中最多可以達到同時配置兩個資料庫連線設定。
若要配置第三台需要自己實作相關設定配置。以下範例提供單一資料庫與雙資料庫連線配置的方法，

### 單一資料庫連線 - 使用Spring Boot 自動化設定

#### 資料庫連線設定
```properties
# 啟用Spring Cloud GCP 資料庫自動化配置設定 (Default 設定為 true 可以省略配置)
spring.cloud.gcp.sql.enabled=true
# 欲連線資料庫名稱
spring.cloud.gcp.sql.database-name=<DATA BASE NAME>
# GCP連線位置
spring.cloud.gcp.sql.instance-connection-name=<PROJECT ID>:<REGION ID>:<INSTANCE>
# 連線帳號
spring.datasource.username=<ACCOUNT>
# 連線密碼
spring.datasource.password=<PASSWORD>
```

#### 連線池配置 HikairCP
```properties
# 連線逾時時間建議設定為30秒
spring.datasource.hikari.connection-timeout=30000
# 閒置連線最小數量建議設定5個連線數
spring.datasource.hikari.minimum-idle=5
# 閒置池最大連線數量建議設定為32個連線數
spring.datasource.hikari.maximum-pool-size=32
# 閒置連線存活時間建議設定 5 分鐘
spring.datasource.hikari.idle-timeout=300000
# 連線數最長存活時間建議設定30分鐘
spring.datasource.hikari.max-lifetime=1800000
# 自動Commit 預設為 true (建議設為False)
spring.datasource.hikari.auto-commit=false
```
#### 啟用ORM JAP 配置
```properties
# 建議設定DDL指令不執行
spring.jpa.hibernate.ddl-auto=none
# 指定Jpa資料庫驅動種類
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.PostgreSQLDialect
```

### 雙資料庫連線 - 使用SDK自動化配置

#### 資料庫連線設定
```properties
# 關閉Spring GCP SQL 自動配置功能
spring.cloud.gcp.sql.enabled=false

# 第一組配置設定
# 設定一組連線設定啟用(預設 false)
cub.cloud.datasource.primary.enable= true
# 欲連線資料庫名稱
cub.cloud.datasource.primary.dataBaseName= <DATA BASE NAME>
# GCP 專案名稱
cub.cloud.datasource.primary.projectId=<PROJECT ID>
# GCP 資料庫實際位置
cub.cloud.datasource.primary.regionId=<REGION ID>
# GCP 資料庫實例名稱
cub.cloud.datasource.primary.instance=<INSTANCE>
# 連線帳號
cub.cloud.datasource.primary.username= <ACCOUNT>
# 連線密碼
cub.cloud.datasource.primary.password= <PASSWORD>

# 第二組配置設定
# 設定二組連線設定啟用(預設 false)
cub.cloud.datasource.secondary.enable= true
# 欲連線資料庫名稱
cub.cloud.datasource.secondary.dataBaseName= <DATA BASE NAME>
# GCP 專案名稱
cub.cloud.datasource.secondary.projectId=<PROJECT ID>
# GCP 資料庫實際位置
cub.cloud.datasource.secondary.regionId=<REGION ID>
# GCP 資料庫實例名稱
cub.cloud.datasource.secondary.instance=<INSTANCE>
# 連線帳號
cub.cloud.datasource.secondary.username= <ACCOUNT>
# 連線密碼
cub.cloud.datasource.secondary.password= <PASSWORD>
```

#### 連線池配置 HikairCP
```properties
#第一組連線池設定
# 連線逾時時間建議設定為30秒
cub.cloud.datasource.primary.hikari.connectionTimeout=30000
# 閒置連線最小數量建議設定5個連線數
cub.cloud.datasource.primary.hikari.minimumIdle=5
# 閒置池最大連線數量建議設定為32個連線數
cub.cloud.datasource.primary.hikari.maximumPoolSize= 32
# 閒置連線存活時間建議設定 6 分鐘
cub.cloud.datasource.primary.hikari.idleTimeout =300000
# 連線數最長存活時間建議設定30分鐘
cub.cloud.datasource.primary.hikari.maxLifeTime=1800000
# 自動Commit 預設為 true (建議設為False)
cub.cloud.datasource.primary.hikari.autoCommit= false

#第二組連線池設定
# 連線逾時時間建議設定為30秒
cub.cloud.datasource.secondary.hikari.connectionTimeout=30000
# 閒置連線最小數量建議設定5個連線數
cub.cloud.datasource.secondary.hikari.minimumIdle=5
# 閒置池最大連線數量建議設定為32個連線數
cub.cloud.datasource.secondary.hikari.maximumPoolSize= 32
# 閒置連線存活時間建議設定 5 分鐘
cub.cloud.datasource.secondary.hikari.idleTimeout =300000
# 連線數最長存活時間建議設定30分鐘
cub.cloud.datasource.secondary.hikari.maxLifeTime=1800000
# 自動Commit 預設為 true (建議設為False)
cub.cloud.datasource.secondary.hikari.autoCommit= false
```

#### 啟用ORM JAP 配置
```properties
# 建議設定DDL指令不執行
spring.jpa.hibernate.ddl-auto=none
# 指定Jpa資料庫驅動種類
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.PostgreSQLDialect

#第一組 Entity ＆ Repository配置
cub.cloud.jpa.primary.entity.path=<YOUR PACKAGE>
cub.cloud.jpa.primary.repository.path=<YOUR PACKAGE>

#第二組 Entity ＆ Repository配置
cub.cloud.jpa.secondary.entity.path=<YOUR PACKAGE>
cub.cloud.jpa.secondary.repository.path=<YOUR PACKAGE>

```


### 額外配置
```properties
# 顯示ORM執行時所編譯出來的SQL指令
spring.jpa.show-sql=true
# 開啟HikariCP Debug模式，可以檢視相關設定
logging.level.com.zaxxer.hikari.HikariConfig=DEBUG
logging.level.com.zaxxer.hikari=TRACE
```

# 如何使用JdbcTempalte

### 單一資料庫連線 - 使用Spring Boot自動配置的Singleton物件
#### 注入JdbcTemplate

```java
@Autowired
JdbcTemplate jdbcTemplate;
```

### 雙資料庫連線 - 使用SDK配置的Singleton物件

此處需要特別注意一下，因為配置的JdbcTemplate會有兩個，需要特別使用@Qualifier來篩選指定的物件，以免框架自動注入到同一個JdbcTemplate。

```java
//第一個連線配置物件
@Autowired
@Qualifier("primaryJdbcTemplate")
JdbcTemplate primaryJdbcTemplate;

//第二個連線配置物件
@Autowired
@Qualifier("primaryTransactionManager")
JdbcTemplate secondaryJdbcTemplate;
```

# 交易事務的使用

在事務方面的使用，要非常注意，目前的SDK不提供分散式事務管理的機制，因此針對每一個資料庫的操作，事務都是獨立的，不可以混用，否則是很有可能會造成資料異常的狀況。
因此在使用@Transactional需要特別告知目前是使用哪一個TransactionManager，如下圖。
```java
//第一個資料庫事務管理
@Transactional(transactionManager = "primaryTransactionManager",rollbackFor = Exception.class, propagation = Propagation.REQUIRED)
public void doSomething(){
    dosomething.....
  };

//第二個資料庫事務管理
@Transactional(transactionManager = "secondaryTransactionManager",rollbackFor = Exception.class, propagation = Propagation.REQUIRED)
public void doSomething(){
  dosomething.....
  };
```