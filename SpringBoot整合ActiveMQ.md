# SpringBoot整合ActiveMQ

### 一、添加依赖

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-activemq</artifactId>
</dependency>
```

### 二、修改配置文件

```yaml
spring:
  activemq:
    # broker-url是activeMQ服务器地址
    # 端口61001是在/config/activmq.xml中name="openwire"的配置对应的端口
    broker-url: tcp://localhost:61001
    #user和password是在activeMQ服务器的/config/user.properites中的配置
    #用户名
    user: admin
    #密码
    password: admin
    #是否启用内存模式（就是不安装MQ，项目启动时同时启动一个MQ实例）
    in-memory: false
    packages:
      #信任所有的包
      trust-all: true
    pool:
      #是否替换默认的连接池，使用ActiveMQ的连接池需引入的依赖
      enabled: false
```

### 三、编写配置类

```java
package com.hym.springbootactivemq.demo.config;

import org.apache.activemq.command.ActiveMQQueue;
import org.apache.activemq.command.ActiveMQTopic;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.jms.config.DefaultJmsListenerContainerFactory;
import org.springframework.jms.config.JmsListenerContainerFactory;
import javax.jms.ConnectionFactory;
import javax.jms.Queue;
import javax.jms.Topic;

@Configuration
public class ActiveMQConfig {
    
    @Bean
    public Queue queue(){
        return new ActiveMQQueue("springboot.queue");
    }
    
    //springboot默认只配置queue类型消息，如果要使用topic类型的消息，则需要配置该bean
    @Bean
    public JmsListenerContainerFactory jmsTopicListenerContainerFactory(ConnectionFactory connectionFactory){
        DefaultJmsListenerContainerFactory factory = new DefaultJmsListenerContainerFactory();
        factory.setConnectionFactory(connectionFactory);
        //这里必须设置为true，false则表示是queue类型
        factory.setPubSubDomain(true);
        return factory;
    }

    @Bean
    public Topic topic(){
        return new ActiveMQTopic("springboot.topic");
    }

}
```

### 四、编写生产者类

```java
package com.hym.springbootactivemq.demo.producer;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.jms.core.JmsMessagingTemplate;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;
import javax.jms.Queue;
import javax.jms.Topic;

@RestController
public class Producer {

    @Autowired
    private JmsMessagingTemplate jmsTemplate;
    @Autowired
    private Queue queue;
    @Autowired
    private Topic topic;

    //发送queue类型消息
    @GetMapping("queue")
    public void sendQueueMsg(String msg){
        jmsTemplate.convertAndSend(queue,msg);
    }
    
	//发送topic类型消息
    @GetMapping("topic")
    public void sendTopicMsg(String msg){
        jmsTemplate.convertAndSend(topic,msg);
    }
}

```

### 五、编写消费者

```java
package com.hym.springbootactivemq.demo.consumer;

import org.springframework.jms.annotation.JmsListener;
import org.springframework.stereotype.Component;

@Component
public class Consumer {

    //接收queue类型的消息
    @JmsListener(destination = "springboot.queue")
    public void ListenQueue(String msg){
        System.out.println("接收到queue消息:"+msg);
    }

    //接收topic类型的消息
    @JmsListener(destination = "springboot.topic",containerFactory = "jmsTopicListenerContainerFactory")
    public void ListenTopic(String msg){
        System.out.println("接收到topic消息:"+msg);
    }

}
```

