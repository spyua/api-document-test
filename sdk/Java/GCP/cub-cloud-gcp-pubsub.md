# PubSub

## 專案說明
1. 推播訊息
2. 同步與非同步主動拉訊息
3. 訂閱頻道持續接收

## Version


| SDK | Version | Git |
| -------- | -------- | -------- |
| cub-cloud-gcp-pubsub     | 1.0.0     | 待鍵入連結     |


## 專案引入說明
### Maven Pom Setting

> 使用版本管理Pom，需繼承cub-cloud-general-parent

```xml
<parent>
    <groupId>cub.cloud</groupId>
    <artifactId>cub-cloud-general-parent</artifactId>
    <version>1.0.0</version>
</parent>
```

>在相依關係加入PubSub SDK專案

```xml
<dependency>
      <groupId>cub.cloud</groupId>
      <artifactId>cub-cloud-gcp-pubsub</artifactId>
</dependency>

```

>若不想要依賴版本管理Pom，則可直引用，差異在於需要指定版本。

```xml
<dependency>
      <groupId>cub.cloud</groupId>
      <artifactId>cub-cloud-gcp-pubsub</artifactId>
      <version>1.0.0</version>
</dependency>

```


## API 使用說明
### 如果您是在本機開發，請確認以下設定
1. 與GCP環境驗證設定
> 常用的方法有下列 - 參考官方文件 [Link](https://cloud.google.com/docs/authentication/provide-credentials-adc?hl=zh-cn)
> 1. GCP SDK - gcloud
> 2. Json Key - Json驗證檔案
> 3. Oauth2驗證

若您採用的不是GCP SDK驗證，需要在專案中的application.properties/.yml中設定，下列資訊：

>properties
```properties=
spring.cloud.gcp.project-id=my-gcp-project-id
```

>yml
```yaml
spring:
    cloud:
        gcp:
            project-id: my-gcp-project-id
```

## 注入SDK服務
在您需要用到PubSub功能的實作物件中，透過Spring Boot IOC進行注入。

```java
  // 透過Spring IOC Container注入服務
  @Autowired
  private Publisher publisher; //推播訊息

  @Autowired
  private Receiver receiver;  //主動收取訊息
```

## Publisher : PubSubPublisherImpl 推播訊息使用範例

### 單筆訊息推播
此方法為同步方式進行單筆推播

```java
  // PubSub訊息標籤
  Map<String, String> attributes = new HashMap<>();
  attributes.put("info", "demo");
  
  // 單筆推播訊息
  publisher.publishTopicMessage("TOPIC", "Message", attributes);
```

### 多筆訊息逐步推播
此方法為多筆逐步推播，與單筆相同為同步處理方式。

```java
   List<String> inputList = new ArrayList<>();
   inputList.add("Message 1");
   inputList.add("Message 2");
   inputList.add("Message 3");

   publisher.publishTopicMessages("TOPIC",inputList);
```

## Receiver<T,R,K> : PubSubReceiverImpl 主動接收訊息
### 同步接收，無自動進行Ack動作

```java
  //接收訊息
  List<AcknowledgeablePubsubMessage> messages = receiver.pullMessages("topic1-sub", 1);
  
  //建立處理成功要進行Ack的清單
  List<AcknowledgeablePubsubMessage> successProcessList = new ArrayList<>();

  messages.forEach( x -> {
          //將取回的訊息進行處理
          System.out.println(x.getPubsubMessage().getData());
            
          //處理成功加入ack清單
          successProcessList.add(x);
        });

  //進行ack
  receiver.acknowledges(successProcessList);
```

### 同步接收，自動進行Ack動作
```java
  //取回訊息，並且會自動Ack
  List<PubsubMessage> messages = receiver.pullMessagesAndAck("topic1-sub", 1);
  
  //訊息處理
  messages.forEach(x -> System.out.println(x.getData()));
```

### 非同步接收，無自動進行Ack動作
```java

  //呼叫非同步取回一個Future讓後續決定可以怎麼處理
  ListenableFuture<List<AcknowledgeablePubsubMessage>> future = (ListenableFuture<List<AcknowledgeablePubsubMessage>>) receiver.pullMessagesAsync("topic1-sub", 1);

  //定義Future成功處理的事和失敗處理的事
  future.addCallback(new ListenableFutureCallback<List<AcknowledgeablePubsubMessage>>() {
      
      @Override
      public void onSuccess(List<AcknowledgeablePubsubMessage> result) {
        //當成功作些什麼
        //做完之後記得要ack
        receiver.acknowledges(result);
      }

      @Override
      public void onFailure(Throwable ex) {
        //當失敗做些什麼
      }
   );
  // 等待非同步執行結果時可以做些別的事

  // 非同步執行結果取回;
  future.get();
```

### 非同步接收，自動進行Ack動作
```java
  //呼叫非同步取回一個Future讓後續決定可以怎麼處理
  ListenableFuture<List<PubsubMessage>> future = (ListenableFuture<List<PubsubMessage>>) receiver.pullMessagesAndAckAsync("topic1-sub", 1);
  
  //定義Future成功處理的事和失敗處理的事
  future.addCallback(new ListenableFutureCallback<List<PubsubMessage>>() {
      
      @Override
      public void onSuccess(List<PubsubMessage> result) {
        //當成功作些什麼
      }

      @Override
      public void onFailure(Throwable ex) {
        //當失敗做些什麼
      }
  });
  
  // 等待非同步執行結果時可以做些別的事

  // 非同步執行結果取回;
  future.get();
```


## Subscribe
在這邊展示如何透鍋監聽的方式收取訊息，這邊的方式與官方教學文件一樣，可以參考連結[Link](https://cloud.google.com/pubsub/docs/spring?hl=zh-cn)，下面為展示的程式碼。

```java
  @Configuration
  public class PubSubSubscriptionConfig {

  private static final Log LOGGER = LogFactory.getLog(PubSubSubscriptionConfig.class);

  // 讓Spring 去協助配置一個準備監聽的頻道
  @Bean
  public MessageChannel inputMessageChannel() {
    return new PublishSubscribeChannel();
  }

  // 建立一個Adapter監聽的渠道，並且將PubSubTemplate設定給他，後續他將會使用PubSubTemplate來訂閱消息
  @Bean
  public PubSubInboundChannelAdapter inboundChannelAdapter(
      @Qualifier("inputMessageChannel") MessageChannel messageChannel,
      PubSubTemplate pubSubTemplate) 
  {
    PubSubInboundChannelAdapter adapter =
        new PubSubInboundChannelAdapter(pubSubTemplate, "topic1-sub"); //第二個參數為訂閱的主題
    adapter.setOutputChannel(messageChannel);
    adapter.setAckMode(AckMode.MANUAL);
    adapter.setPayloadType(String.class);
    return adapter;
  }

  @ServiceActivator(inputChannel = "inputMessageChannel")
  public void messageReceiver(
      String payload,
      @Header(GcpPubSubHeaders.ORIGINAL_MESSAGE) BasicAcknowledgeablePubsubMessage message) {
    // 當收到訊息進行處理
    LOGGER.info("Message arrived via an inbound channel adapter from sub-one! Payload: " + payload);
    // 記得這邊要進行Ack
    message.ack();
  }
}
```