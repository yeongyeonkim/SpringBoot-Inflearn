SpringBoot에서 Redis를 적용시키기 위한 모듈로
Jedis나 Lettuce를 사용한다고 한다.

하지만, Jedis는 업데이트가 더이상 되고 있지 않을 뿐더러 Lettuce의 장점 때문에 Lettuce를 사용하는 추세

---

### Lettuce

- Synchronous, Asynchronous 및 React 사용을 위한 확장 가능한 thread-safe Redis 클라이언트.

---

### Properties

- 각 application-{env}.properties 에 환경에 맞는 redis host와 port를 구성한다.

```java
# aws redis를 사용하는 예제
aws.redis.host={endpoint}
aws.redis.host={port}
```

---

### Redis Configuration

- 1. RedisClient 생성
    - @Value 어노테이션을 사용해서 해당 host와 port를 사용하여 redisURI를 생성후 해당 URI로 RedisClient를 생성
- 2. StatefulRedisPubSubConnection 생성
    - Redis Cluster 내에서 Pub/Sub 메시지를 게시(pub)하고 구독(sub)할 수 있다.</br>
    Redis 클러스터 특성으로 인해 메시지는 클러스터 전체에 브로드캐스트되어 클라이언트는 임의의 노드에 연결하여 구독에 참여할 수 있다.
```java
@Configuration
public class RedisConfiguration{
	@Value("${aws.redis.host}")
	private String awsRedisHost;
	@Value("${aws.redis.port}")
	private Integer awsRedisPort;
	@Bean(name = "redisClient")
	public RedisClient redisClient() {
        RedisURI redisURI = RedisURI.builder().withHost(awsRedisHost).withPort(awsRedisPort).build();

        return RedisClient.create(redisURI);
	}
    @Bean
    public StatefulRedisPubSubConnection<String, String> connect(RedisClient redisClient){
        return redisClient.connectPubSub();
    }
}
```

---

### Redis Listener

RedisPubSubListener


### Redis Channel

* Redis에서 Message를 주고받을 채널

1. 이벤트(메시지)를 발행하는 Publisher가 존재하며, Publisher는 특정 Channel에 이벤트를 전송한다.
2. 특정 Channel을 구독하는 Subscriber가 존재하며, Publisher에 관계없이 받을 수 있다.











