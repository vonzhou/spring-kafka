# Spring Kafka

在使用spring-kafka的时候，需要注解`EnableKafka`，这个注解的目的就是引入了相关的启动配置。

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Import(KafkaBootstrapConfiguration.class) // 看过来
public @interface EnableKafka {
}
```

这个启动配置类引入了2个bean，`KafkaListenerAnnotationBeanPostProcessor`这个后置处理器用于发现处理有`KafkaListener`注解的方法，`KafkaListenerEndpointRegistry`用于注册Listener容器。

```java
@Configuration
public class KafkaBootstrapConfiguration {

	@SuppressWarnings("rawtypes")
	@Bean(name = KafkaListenerConfigUtils.KAFKA_LISTENER_ANNOTATION_PROCESSOR_BEAN_NAME)
	@Role(BeanDefinition.ROLE_INFRASTRUCTURE)
	public KafkaListenerAnnotationBeanPostProcessor kafkaListenerAnnotationProcessor() {
		return new KafkaListenerAnnotationBeanPostProcessor();
	}

	@Bean(name = KafkaListenerConfigUtils.KAFKA_LISTENER_ENDPOINT_REGISTRY_BEAN_NAME)
	public KafkaListenerEndpointRegistry defaultKafkaListenerEndpointRegistry() {
		return new KafkaListenerEndpointRegistry();
	}

}
```

需要再说，Spring的可扩展性值得学习。

## Link

Kafka 0.9.0

http://kafka.apache.org/090/documentation.html