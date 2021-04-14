# Notes and code for pluralsight training course "Building Applications Using JDBC" by Bryan Hansen

### Module 3
* mysql datasource xml configuration
```
<bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
	<property name="driverClassName" value="com.mysql.jdbc.Driver"/>
	<property name="url" value="jdbc:mysql://localhost:3306/ride_tracker?useSSL=false"/>
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
### Module 4
* JDBC template is using the perparedStatement
### Module 5
* Use RowMapper to get the query results
```
List<Ride> rides = jdbcTemplate.query("select * from ride", new RowMapper<Ride> () {
			
	public Ride mapRow(ResultSet rs, int rowNum) throws SQLException {
		Ride ride = new Ride();
		ride.setId(rs.getInt("id"));
		ride.setName(rs.getString("name"));
		ride.setDuration(rs.getInt("duration"));
		return ride;		
	}
});
```
* Post for object creation
```
ride = restTemplate.postForObject("http://localhost:8080/ride_tracker/ride", ride, Ride.class);	
```
* Use external RowMapper
```
List<Ride> rides = jdbcTemplate.query("select * from ride", new RideRowMapper());

public class RideRowMapper implements RowMapper<Ride> {
	
	@Override
	public Ride mapRow(ResultSet rs, int rowNum) throws SQLException {
		Ride ride = new Ride();
		ride.setId(rs.getInt("id"));
		ride.setName(rs.getString("name"));
		ride.setDuration(rs.getInt("duration"));
		return ride;		
	}

}

```
* If you want to retrieve the record just created in the database	
```
// User Post
ride = restTemplate.postForObject("http://localhost:8080/ride_tracker/ride", ride, Ride.class);

// Use a KeyHolder when creating record in database


KeyHolder keyHolder = new GeneratedKeyHolder();
		jdbcTemplate.update(new PreparedStatementCreator() {
			
			@Override
			public PreparedStatement createPreparedStatement(Connection con) throws SQLException {
				PreparedStatement ps = con.prepareStatement("insert into ride (name,duration) values (?,?)", 
						new String[] {"id"});
				ps.setString(1, ride.getName());
				ps.setInt(2, ride.getDuration());
				return ps;
			}
		}, keyHolder);
		
		Number id=keyHolder.getKey();
		
		return getRide(id.intValue());
		
// Use the key in the KeyHolder to retrieve the record
public Ride getRide(Integer id) {
		Ride ride=jdbcTemplate.queryForObject("select * from ride where id = ?", new RideRowMapper(), id);
		return ride;
	}
```
### Module 6
