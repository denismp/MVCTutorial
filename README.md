# MVCTutorial

I created a Spring MVC Project.

Do this with the File->New->Legacy Spring Project->Spring MVC Project, then make the changes below.

I whacked the home.jsp and the HomeController.java
that comes with the project setup.

I then mixed in stuff from the web page
http://scottsdigitalcommunity.blogspot.com/2013/05/developing-spring-mvc-project-using.html

I used the domain name, com.spring.mvc, the same as the above web page.

I added META-INF/persistence.xml

Look for my hacks
	Adding to default
in
	pom.xml
	servlet-context.xml

The other files
	users.jsp
	User.java
	UserController.java
	UserRepository.java
are from the web page

This was added to the servlet-context.xml file.
<!-- Adding to default 
Added properties to beans:beans
	xmlns:tx="http://www.springframework.org/schema/tx"
	xmlns:jpa="http://www.springframework.org/schema/data/jpa"
Added to beans:beans property xsi:schemaLocation
	http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx.xsd
	http://www.springframework.org/schema/data/jpa http://www.springframework.org/schema/data/jpa/spring-jpa.xsd
-->
	<jpa:repositories base-package="com.springapp.mvc"/>
	<beans:bean id="entityManagerFactory" class="org.springframework.orm.jpa.LocalEntityManagerFactoryBean">
		<beans:property name="persistenceUnitName" value="defaultPersistenceUnit"/>
	</beans:bean>
	<beans:bean id="transactionManager" class="org.springframework.orm.jpa.JpaTransactionManager">
		<beans:property name="entityManagerFactory" ref="entityManagerFactory" />
	</beans:bean>
	<tx:annotation-driven transaction-manager="transactionManager"/>
<!-- Done adding to default -->

This is the src/main/resources/META-INF/persitence.xml file:

<?xml version="1.0" encoding="UTF-8"?>
<persistence version="2.0"
	xmlns="http://java.sun.com/xml/ns/persistence" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://java.sun.com/xml/ns/persistence http://java.sun.com/xml/ns/persistence/persistence_2_0.xsd">
	<persistence-unit name="defaultPersistenceUnit"
		transaction-type="RESOURCE_LOCAL">
		<provider>org.hibernate.ejb.HibernatePersistence</provider>
		<properties>
			<property name="hibernate.dialect" value="org.hibernate.dialect.HSQLDialect" />
			<property name="hibernate.connection.url" value="jdbc:hsqldb:mem:spring" />
			<property name="hibernate.connection.driver_class" value="org.hsqldb.jdbcDriver" />
			<property name="hibernate.connection.username" value="sa" />
			<property name="hibernate.connection.password" value="" />
			<property name="hibernate.hbm2ddl.auto" value="create-drop" />
		</properties>
	</persistence-unit>
</persistence>

Apparently placing the above code into the src/main/resources/META-INF/persistence.xml file is found by spring for the database.  I suspect
that this is from the 
	<!-- Handles HTTP GET requests for /resources/** by efficiently serving up static resources in the ${webappRoot}/resources directory -->
	<resources mapping="/resources/**" location="/resources/" />
in the generated servlet-context.xml file.
