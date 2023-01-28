# 🍹 DB 다중 커넥션

## application.yml

```yml
server:
port: 9999

spring:

# gov .127에 해당하는 DB

gov-datasource:
driver-class-name: com.mysql.cj.jdbc.Driver
jdbcUrl: jdbc:mysql:/주소:3306/voc_gov?useUnicode=true&characterEncoding=utf8&serverTimezone=Asia/Seoul&zeroDateTimeBehavior=convertToNull&rewriteBatchedStatements=true
username: ID
password: PW

# pub .131에 해당하는 DB

pub-datasource:
driver-class-name: com.mysql.cj.jdbc.Driver
jdbcUrl: jdbc:mysql://주소:3307/voc_pub?useUnicode=true&characterEncoding=utf8&serverTimezone=Asia/Seoul&zeroDateTimeBehavior=convertToNull&rewriteBatchedStatements=true
username: ID
password: PW!

jpa:
    hibernate:
        ddl-auto: update
        naming:
            physical-strategy: org.hibernate.boot.model.naming.PhysicalNamingStrategyStandardImpl
        properties:
        hibernate:
            dialect: org.hibernate.dialect.MySQL8Dialect
            show_sql: true
            format_sql: true
```

## 1번 설정

```java
package com.voc.backend.config;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.boot.jdbc.DataSourceBuilder;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Primary;
import org.springframework.context.annotation.PropertySource;
import org.springframework.data.jpa.repository.config.EnableJpaRepositories;
import org.springframework.orm.jpa.JpaTransactionManager;
import org.springframework.orm.jpa.LocalContainerEntityManagerFactoryBean;
import org.springframework.orm.jpa.vendor.HibernateJpaVendorAdapter;
import org.springframework.transaction.PlatformTransactionManager;

import javax.sql.DataSource;
import java.util.HashMap;

@Configuration
@EnableJpaRepositories(
        basePackages = "com.voc.backend.db.repository.gov", // master Repository 경로
        entityManagerFactoryRef = "govEntityManager",
        transactionManagerRef = "govTransactionManager"
)
@PropertySource("classpath:application.yml")
public class GovDatabaseConfig {
    @Value("${spring.jpa.hibernate.ddl-auto}")
    private String DDL_AUTO;

    @Value("${spring.jpa.hibernate.naming.physical-strategy}")
    private String PHYSICAL_STRATEGY;

    @Value("${spring.jpa.properties.hibernate.dialect}")
    private String DIALECT;

    @Value("${spring.jpa.properties.hibernate.show_sql}")
    private String SHOW_SQL;

    @Value("${spring.jpa.properties.hibernate.format_sql}")
    private String FORMAT_SQL;
    @Primary
    @Bean
    public LocalContainerEntityManagerFactoryBean govEntityManager() {
        LocalContainerEntityManagerFactoryBean em = new LocalContainerEntityManagerFactoryBean();
        em.setDataSource(govDataSource());
//      사용할 Entity 주소
        em.setPackagesToScan(new String[] {"com.voc.backend.db.entity.gov"});
        em.setJpaVendorAdapter(new HibernateJpaVendorAdapter());
        HibernateJpaVendorAdapter vendorAdapter = new HibernateJpaVendorAdapter();
        em.setJpaVendorAdapter(vendorAdapter);
        // EntityManger에서 사용할 고유한 이름
        em.setPersistenceUnitName("govEntityManager");
        HashMap<String, Object> properties = new HashMap<>();

        properties.put("hibernate.hbm2ddl.auto",DDL_AUTO);
        properties.put("hibernate.physical_naming_strategy", PHYSICAL_STRATEGY);
        properties.put("hibernate.dialect", DIALECT);
        properties.put("hibernate.show_sql", SHOW_SQL);
        properties.put("hibernate.format_sql",FORMAT_SQL);
        em.setJpaPropertyMap(properties);
        return em;
    }

    @Primary
    @Bean
    @ConfigurationProperties(prefix="spring.gov-datasource")
    public DataSource govDataSource() {
        return DataSourceBuilder.create().build();
    }

    @Primary
    @Bean
    public PlatformTransactionManager govTransactionManager() {
        JpaTransactionManager transactionManager = new JpaTransactionManager();
        transactionManager.setEntityManagerFactory(govEntityManager().getObject());
        return transactionManager;
    }
}

```

## 2번 설정

```java
package com.voc.backend.config;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.boot.jdbc.DataSourceBuilder;
import org.springframework.boot.orm.jpa.hibernate.SpringPhysicalNamingStrategy;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Primary;
import org.springframework.context.annotation.PropertySource;
import org.springframework.core.env.Environment;
import org.springframework.data.jpa.repository.config.EnableJpaRepositories;
import org.springframework.orm.jpa.JpaTransactionManager;
import org.springframework.orm.jpa.LocalContainerEntityManagerFactoryBean;
import org.springframework.orm.jpa.vendor.HibernateJpaVendorAdapter;
import org.springframework.transaction.PlatformTransactionManager;

import javax.sql.DataSource;
import java.util.HashMap;

@Configuration
@EnableJpaRepositories(
        basePackages = "com.voc.backend.db.repository.pub", // master Repository 경로
        entityManagerFactoryRef = "pubEntityManager",
        transactionManagerRef = "pubTransactionManager"
)
@PropertySource("classpath:application.yml")
public class PubDatabaseConfig {
    @Value("${spring.jpa.hibernate.ddl-auto}")
    private String DDL_AUTO;

    @Value("${spring.jpa.hibernate.naming.physical-strategy}")
    private String PHYSICAL_STRATEGY;

    @Value("${spring.jpa.properties.hibernate.dialect}")
    private String DIALECT;

    @Value("${spring.jpa.properties.hibernate.show_sql}")
    private String SHOW_SQL;

    @Value("${spring.jpa.properties.hibernate.format_sql}")
    private String FORMAT_SQL;

    @Bean
    public LocalContainerEntityManagerFactoryBean pubEntityManager() {
        LocalContainerEntityManagerFactoryBean em = new LocalContainerEntityManagerFactoryBean();
        em.setDataSource(pubDataSource());
//      사용할 Entity 주소
        em.setPackagesToScan(new String[] {"com.voc.backend.db.entity.pub"});
        em.setJpaVendorAdapter(new HibernateJpaVendorAdapter());
        HibernateJpaVendorAdapter vendorAdapter = new HibernateJpaVendorAdapter();
        em.setJpaVendorAdapter(vendorAdapter);
        // EntityManger에서 사용할 고유한 이름
        em.setPersistenceUnitName("pubEntityManager");
        HashMap<String, Object> properties = new HashMap<>();

        properties.put("hibernate.hbm2ddl.auto",DDL_AUTO);
        properties.put("hibernate.physical_naming_strategy", PHYSICAL_STRATEGY);
        properties.put("hibernate.dialect", DIALECT);
        properties.put("hibernate.show_sql", SHOW_SQL);
        properties.put("hibernate.format_sql",FORMAT_SQL);
        em.setJpaPropertyMap(properties);
        return em;
    }

    @Bean
    @ConfigurationProperties(prefix="spring.pub-datasource")
    public DataSource pubDataSource() {
        return DataSourceBuilder.create().build();
    }
    @Bean
    public PlatformTransactionManager pubTransactionManager() {
        JpaTransactionManager transactionManager = new JpaTransactionManager();
        transactionManager.setEntityManagerFactory(pubEntityManager().getObject());
        return transactionManager;
    }
}

```
