Spring Boot Security with JDBC Authentication
Step-1 ) Setup Database tables with required data

CREATE TABLE `users` (
  `username` VARCHAR(50) NOT NULL,
  `password` VARCHAR(120) NOT NULL,
  `enabled` TINYINT(1) NOT NULL,
  PRIMARY KEY (`username`)
);

CREATE TABLE `authorities` (
  `username` VARCHAR(50) NOT NULL,
  `authority` VARCHAR(50) NOT NULL,
  KEY `username` (`username`),
  CONSTRAINT `authorities_ibfk_1` FOREIGN KEY (`username`)
  REFERENCES `users` (`username`)
);

Step 2 :- we will encript password using https://bcrypt-generator.com/ website.

Step 3 : -  insert records into table users & authorities.

insert into users values ('admin', '$2a$12$tw1vxO2Phtba2gjkMU44euk9rsG6fg3/O5sZfHwBZqDTG9..Vkjry',  1);
insert into users values ('user', '$2a$12$cDgq/OPn7tyRYQWwft5ptu/8Lh55TQYC/CyYYQCqK4YdQz.wkg5cK',  1);

insert into authorities values ('admin', 'ROLE_ADMIN');
insert into authorities values ('admin', 'ROLE_USER');
insert into authorities values ('user', 'ROLE_USER');

Steps 4 :-  Create Boot application with below dependencies

		a) web-starter
		b) security-starter
		c) data-jdbc
		d) mysql-connector
		e) lombok
		f) devtools

  Steps 5 : - 

spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    password: root
    url: jdbc:mysql://localhost:3306/rolebasedlogin
    username: root
  jpa:
    show-sql: true

    Steps : -

    @Bean
	public SecurityFilterChain securityConfig(HttpSecurity http) throws Exception {
			
		http.authorizeHttpRequests( (req) -> req
				.requestMatchers("/admin").hasRole(ADMIN)  
				.requestMatchers("/user").hasAnyRole(ADMIN,USER)
				.requestMatchers("/").permitAll()
				.anyRequest().authenticated()
		).formLogin();
		
		return http.build();
	}
![everyon](https://github.com/JavaDevKKT/spring-security-jdbc-app/assets/147974177/22073828-7526-4042-95fa-af7e2260a9e3)

![admin](https://github.com/JavaDevKKT/spring-security-jdbc-app/assets/147974177/0e243e5f-e3cc-427a-842e-7e333f8b403f)




  

