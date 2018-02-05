# 在Spring Boot中使用 @ConfigurationProperties 注解

开始创建一个`@ConfigurationProperties` bean:

```java
@ConfigurationProperties(locations = "classpath:mail.properties", 
                         ignoreUnknownFields = false, 
                         prefix = "mail")
public class MailProperties { 
	  public static class Smtp {  
	    private boolean auth;  
	    private boolean starttlsEnable;  
	    // ... getters and setters 
	  }
	  @NotBlank private String host;
	  private int port;  
	  private String from; 
	  private String username;
	  private String password; 
	  @NotNull private Smtp smtp; 
	  // ... getters and setters
}
```
…从如下属性中创建(`mail.properties`):

```java
mail.host=localhost
mail.port=25
mail.smtp.auth=false
mail.smtp.starttls-enable=false
mail.from=me@localhost
mail.username=
mail.password=
```

## 方案一
```java
@Configuration
@ConfigurationProperties(locations = "classpath:mail.properties", 
                         prefix = "mail")
public class MailConfiguration { 
	 public static class Smtp {
	    private boolean auth;
	    private boolean starttlsEnable;
	    // ... getters and setters
	 }

	 @NotBlank private String host; 
	 private int port;
	 private String from; 
	 private String username;
	 private String password; 
	
	 @NotNull private Smtp smtp; 
	 // ... getters and setters  
	 
	 @Bean public JavaMailSender javaMailSender() {
	  // omitted for readability
	 }
}
```

## 方案二

```java
@Configuration
@EnableConfigurationProperties(MailProperties.class)
 public class MailConfiguration { 
    @Autowired private MailProperties mailProperties; 

    @Bean public JavaMailSender javaMailSender() {
      // omitted for readability
    }
 }
```

**注意:** `@EnableConfigurationProperties`这个注解告诉Spring Boot能支持指定特定类型的`@ConfigurationProperties`。如果不指定会看到如下异常:

```
org.springframework.beans.factory.NoSuchBeanDefinitionException: No qualifying bean of type [demo.mail.MailProperties] found for dependency: expected at least 1 bean which qualifies as autowire candidate for this dependency. Dependency annotations: {@org.springframework.beans.factory.annotation.Autowired(required=true)}
```

* 方案一不使用`@EnableConfigurationProperties`注解，使用`@Configuration`或者`@Component`注解使其被component scan发现。
* 方案二使用`@EnableConfigurationProperties`注解，使其指定的配置类被`EnableConfigurationPropertiesImportSelector`注册到应用上下文。
