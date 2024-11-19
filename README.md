HikariCP (Hikari Connection Pool) is a high-performance JDBC connection pool used in Spring Boot applications to manage database connections efficiently. It's lightweight, fast, and provides a straightforward way to handle database connections, making it a popular choice for Spring Boot applications. Spring Boot supports HikariCP out of the box as the default connection pool.

### **Configuration of HikariCP in Spring Boot**
By default, Spring Boot uses HikariCP if it's available on the classpath. To customize or configure HikariCP, you can define properties in the `application.properties` or `application.yml` file.

#### **Step-by-Step Setup**

1. **Dependencies**:
   Make sure you have the necessary dependencies in your `pom.xml` (if using Maven) or `build.gradle` (if using Gradle). Usually, it's enough to include the database and Spring Boot dependencies because Spring Boot will automatically include HikariCP.

   ```xml
   <!-- For Maven projects -->
   <dependency>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-starter-data-jpa</artifactId>
   </dependency>
   
   <!-- Database Dependency Example (PostgreSQL) -->
   <dependency>
       <groupId>org.postgresql</groupId>
       <artifactId>postgresql</artifactId>
   </dependency>
   ```

2. **Configuration in `application.properties`**:
   Here are some common configuration settings:

   ```properties
   # DataSource configuration
   spring.datasource.url=jdbc:postgresql://localhost:5432/your_database
   spring.datasource.username=your_username
   spring.datasource.password=your_password
   spring.datasource.driver-class-name=org.postgresql.Driver

   # HikariCP specific settings
   spring.datasource.hikari.connection-timeout=20000          # Maximum wait time for a connection from the pool
   spring.datasource.hikari.maximum-pool-size=10              # Maximum size of the pool
   spring.datasource.hikari.minimum-idle=5                    # Minimum idle connections in the pool
   spring.datasource.hikari.idle-timeout=300000               # Idle timeout for connections
   spring.datasource.hikari.max-lifetime=1800000              # Maximum lifetime of a connection
   spring.datasource.hikari.pool-name=MyHikariCPPool          # Pool name (for identification)
   ```

3. **Configuration in `application.yml`**:
   If you prefer YAML format:

   ```yaml
   spring:
     datasource:
       url: jdbc:postgresql://localhost:5432/your_database
       username: your_username
       password: your_password
       driver-class-name: org.postgresql.Driver
       hikari:
         connection-timeout: 20000
         maximum-pool-size: 10
         minimum-idle: 5
         idle-timeout: 300000
         max-lifetime: 1800000
         pool-name: MyHikariCPPool
   ```

### **HikariCP Properties Explanation**
- **`spring.datasource.hikari.connection-timeout`**: Time in milliseconds for which the pool will wait for a connection to become available. Default is 30000 (30 seconds).
- **`spring.datasource.hikari.maximum-pool-size`**: The maximum number of connections allowed in the pool.
- **`spring.datasource.hikari.minimum-idle`**: The minimum number of idle connections the pool maintains.
- **`spring.datasource.hikari.idle-timeout`**: The maximum time that a connection can stay idle in the pool before it’s eligible for eviction.
- **`spring.datasource.hikari.max-lifetime`**: The maximum time a connection can live before it’s retired from the pool. It's a good practice to keep this value less than the database's maximum connection timeout.

### **Advantages of Using HikariCP**
1. **High Performance**: HikariCP is known for its speed and efficiency, making it ideal for high-performance applications.
2. **Minimum Configuration**: It works efficiently with minimal configuration, and default settings often provide good performance.
3. **Advanced Features**: HikariCP offers features like connection timeout, leak detection, and maximum pool size which you can tune for optimal performance.

### **Example Usage in Java Class**
Here's a simple example of how to use the configured DataSource in your Java code:

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.stereotype.Repository;

@Repository
public class MyRepository {

    private final JdbcTemplate jdbcTemplate;

    @Autowired
    public MyRepository(JdbcTemplate jdbcTemplate) {
        this.jdbcTemplate = jdbcTemplate;
    }

    public List<MyEntity> findAll() {
        String sql = "SELECT * FROM my_table";
        return jdbcTemplate.query(sql, new MyEntityRowMapper());
    }
}
```

By default, `JdbcTemplate` in Spring Boot will use the DataSource configured with HikariCP.

### **Debugging & Monitoring**
To enable detailed logging for HikariCP, you can set:

```properties
logging.level.com.zaxxer.hikari=DEBUG
logging.level.org.springframework.jdbc.datasource=DEBUG
```

This will help you debug connection issues and see how the connection pool behaves during the application's runtime.

Let me know if you want a more advanced configuration or have any other questions!
