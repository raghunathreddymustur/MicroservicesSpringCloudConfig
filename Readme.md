Spring Cloud Configuration
--------------------------



Best Pratices
------------
1. application name is very important use if correctly
   `spring.application.name = "configserver"`


Setting up Spring Config
------------------------
1. ### Steps to Setup Microservice
   1. **_**Pom.xml**_** - Add the spring config client depedencies as following
      1. Make sure java version is compitable with spring cloud
      ```xml
           <dependency>
               <groupId>org.springframework.cloud</groupId>
               <artifactId>spring-cloud-starter-config</artifactId>
           </dependency>
         
         <spring-cloud.version>2022.0.5</spring-cloud.version>
      
            
      <dependencyManagement>
           <dependencies>
               <dependency>
                   <groupId>org.springframework.cloud</groupId>
                   <artifactId>spring-cloud-dependencies</artifactId>
                   <version>${spring-cloud.version}</version>
                   <type>pom</type>
                   <scope>import</scope>
               </dependency>
           </dependencies>
      </dependencyManagement>
      
      
       ```
      2. Application.properties 
         3. Keep only properties that does not differ by environment
         4. Add the config client properties
         ```yaml
         server:
         port: 8080
         spring:
         application:
         name: "accounts"
         profiles:
         active: "prod"
         datasource:
         url: jdbc:h2:mem:testdb
         driverClassName: org.h2.Driver
         username: sa
         password: ''
         h2:
         console:
         enabled: true
         jpa:
         database-platform: org.hibernate.dialect.H2Dialect
         hibernate:
         ddl-auto: update
         show-sql: true
         config:
         import: "optional:configserver:http://localhost:8071/"

         ```
2. ### Setting Up Config server
   1. pom.xml
      2. Add spring config server dependency
   2. **Properties files**
      2. Move all the microservices propertie file to config folder 
         3. Name the files by microservice names
         ![img.png](img.png)
   4. setup properties file
      ```yaml
      spring:
      application:
      name: "configserver"
      profiles:
      active: native
      cloud:
      config:
      server:
      native:
      search-locations: "classpath:/config"

      server:
      port: 8071

      ```
   5. main Method
      6. Add @EnableConfigServer to main class
      ```java
      @SpringBootApplication
      @EnableConfigServer
      public class ConfigserverApplication {
      public static void main(String[] args) {
      SpringApplication.run(ConfigserverApplication.class, args);
      }
      }
            
     ```
3. ### Run the Services
   4. Sping up the config server
   5. Spin up the microservice
      6. Change the enviroment through cmd or vm arguments
