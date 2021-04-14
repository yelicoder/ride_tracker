# Notes and code for pluralsight training course "Building Applications Using JDBC" by Bryan Hansen

### Module 3
* mysql datasource xml configuration
```
<bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
		<property name="driverClassName" value="com.mysql.jdbc.Driver"/>
		<property name="url" value="jdbc:mysql://localhost:3306/ride_tracker"/>
		<property name="username" value="root"/>
		<property name="password" value="pass"/>
</bean>
```
* JdbcTemplate bean xml configuration
```
<bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
	<property name="dataSource" ref="dataSource" />
</bean>
```
* Inject JdbcTemplate bean into the respository bean
* JDBC dependency
```
<dependency>
	<groupId>org.springframework</groupId>
	<artifactId>spring-jdbc</artifactId>
	<version>4.3.6.RELEASE</version>
</dependency>
```
