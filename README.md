# Configuración inicial de Spring MVC por XML

**_Proyecto con Maven y el servidor Jetty Embebido_**

---

### Pasos:

1. Creamos un **proyecto Maven sin arquetipo**.

2. Establecemos la **estructura mínima de carpetas** de nuestra aplicación.

   > **src/main/java**
   >
   > **src/main/webapp/WEB-INF/web.xml**
   
   
   
3. Añadimos las **dependencias necesarias** en el ``pom.xml``.
   
   ```xml
   <project xmlns="http://maven.apache.org/POM/4.0.0"
   	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
   	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
     
   	<modelVersion>4.0.0</modelVersion>
   	<groupId>com.codenaiten.spring.mvc</groupId>
   	<artifactId>spring-web-mvc</artifactId>
   	<version>1.0.0-SNAPSHOT</version>
   	<packaging>war</packaging>
   	<name>spring-web</name>
   	<description>Configuración XML de un proyecto web con Spring</description>
     
     
     
   	<properties>
       	
       	<spring.version>5.2.0.RELEASE</spring.version>
       	<servlet.api.version>2.5</servlet.api.version>
       	<jstl.version>1.2</jstl.version>
       	
       	<built.name>spring-web-mvc</built.name>
       	<source.directory>src/main/java</source.directory>
       	
       	<maven.compiler.plugin.version>3.8.0</maven.compiler.plugin.version>
       	<jdk.version>1.8</jdk.version>
       	<maven.war.plugin.version>3.2.1</maven.war.plugin.version>
       	<jetty-maven-plugin.version>9.4.15.v20190215</jetty-maven-plugin.version>
       	
       	<war.source.directory>src/main/webapp</war.source.directory>
       	
       </properties>
       
       
       
       <dependencies>
   		
   		<!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
   		<dependency>
   		    <groupId>org.springframework</groupId>
   		    <artifactId>spring-webmvc</artifactId>
   		    <version>${spring.version}</version>
   		</dependency>
   
   		<!-- https://mvnrepository.com/artifact/javax.servlet/servlet-api -->
   		<dependency>
   		    <groupId>javax.servlet</groupId>
   		    <artifactId>servlet-api</artifactId>
   		    <version>${servlet.api.version}</version>
   		    <scope>provided</scope>
   		</dependency>
   		
   		<!-- https://mvnrepository.com/artifact/jstl/jstl -->
   		<dependency>
   		    <groupId>jstl</groupId>
   		    <artifactId>jstl</artifactId>
   		    <version>${jstl.version}</version>
   		</dependency>
   		
       </dependencies>
       
       
       
       <build>
       	
       	<finalName>${built.name}</finalName>
       	
           <sourceDirectory>${source.directory}</sourceDirectory>
           
           <plugins>
           
               <plugin>
                   <artifactId>maven-compiler-plugin</artifactId>
                   <version>${maven.compiler.plugin.version}</version>
                   <configuration>
                     <source>${jdk.version}</source>
                     <target>${jdk.version}</target>
                   </configuration>
               </plugin>
               
               
               <plugin>
                   <artifactId>maven-war-plugin</artifactId>
                   <version>${maven.war.plugin.version}</version>
                   <configuration>
                       <warSourceDirectory>${war.source.directory}</warSourceDirectory>
                   </configuration>
               </plugin>
               
               <plugin>
   	            <groupId>org.eclipse.jetty</groupId>
   	            <artifactId>jetty-maven-plugin</artifactId>
   	            <version>${jetty-maven-plugin.version}</version>
   	            
   	            <configuration>
   	                <scanIntervalSeconds>10</scanIntervalSeconds>
   	                <webApp>
   	                	<contextPath>/</contextPath>
   	                </webApp>
   	            </configuration>
   	        </plugin>
               
           </plugins>
           
       </build>
       
   </project>
   ```
   
   
   
4. Establecemos la **configuración de spring-mvc mediante un archivo XML** que crearemos dentro de la carpeta ``src/main/webapp/WEB-INF``.

   ```xml
   <beans xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
   	xmlns:mvc="http://www.springframework.org/schema/mvc"
   	xmlns="http://www.springframework.org/schema/beans"
   	xsi:schemaLocation="http://www.springframework.org/schema/mvc
   		http://www.springframework.org/schema/mvc/spring-mvc.xsd
   		http://www.springframework.org/schema/beans
   		http://www.springframework.org/schema/beans/spring-beans.xsd">
   
       <!-- ACTIVACIÓN DE LAS ANOTACIONES DE SPRING -->
       <mvc:annotation-driven/>
       
       <!-- RUTA VIRTUAL DE LA CARPETA DE LOS RECURSOS WEB -->
       <mvc:resources mapping="/resources/**" location="/WEB-INF/web/resources/"/>
   
   	<!-- ESTABLECEMOS LA UBICACIÓN Y LA EXTENSIÓN DE NUESTRAS VISTAS -->
       <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
           <property name="prefix">
               <value>/WEB-INF/web/view/</value>
           </property>
           <property name="suffix">
               <value>.jsp</value>
           </property>
       </bean>
       
   </beans>
   ```

   

5. Establecemos el **contexto de Spring mediante un archivo XML** que crearemos dentro de la carpeta ``src/main/webapp/WEB-INF``.

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
   	xmlns:context="http://www.springframework.org/schema/context"
   	xmlns="http://www.springframework.org/schema/beans"
   	xsi:schemaLocation="http://www.springframework.org/schema/beans
   		http://www.springframework.org/schema/beans/spring-beans.xsd
   		http://www.springframework.org/schema/context
   		http://www.springframework.org/schema/context/spring-context.xsd">
       
       <!-- IMPORTAMOS LA CONFIGURACIÓN DE SPRING-MVC -->
       <import resource="config-mvc.xml"/>
       
       <!-- ESTABLECEMOS EL PAQUETE RAÍZ DONDE SE ENCONTRARÁN NUESTROS COMPONENTES DE SPRING -->
       <context:component-scan base-package="com.codenaiten.spring.mvc"/>
       
   </beans>
   ```

   

6. Establecemos la **configuración de Spring en el archivo ``web.xml``** que se encuentra en ``src/main/webapp/WEB-INF``.

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
   	xmlns="http://java.sun.com/xml/ns/javaee"
   	xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd"
   	id="WebApp_ID"
   	version="3.0">
       
       <display-name>spring-web-mvc</display-name>
   
   	<!-- ESTABLECEMOS LA UBICACIÓN DEL CONTEXTO DE SPRING -->
       <context-param>
           <param-name>contextConfigLocation</param-name>
           <param-value>/WEB-INF/spring/context.xml</param-value>
       </context-param>
   	
   	<!-- ESTABLECEMOS EL SERVLET DE SPRING -->
       <servlet>
           <servlet-name>dispatcher</servlet-name>
           <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
           <init-param>
               <param-name>contextConfigLocation</param-name>
               <param-value></param-value>
           </init-param>
           <load-on-startup>1</load-on-startup>
       </servlet>
   	
   	<!-- MAPEAMOS EL SERVLET DE SPRING -->
       <servlet-mapping>
           <servlet-name>dispatcher</servlet-name>
           <url-pattern>/</url-pattern>
       </servlet-mapping>
   	
   	<!-- ESTABLECEMOS EL LISTTENER PARA LA CARGA DEL CONTEXTO DE SPRING -->
       <listener>
           <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
       </listener>
   
   </web-app>
   ```



7. Creamos nuestro controlador dentro de``src/main/java`` y nuestras vistas donde le hayamos especificado a Spring que estarán.

8. **Arrancamos la aplicación con Jetty** con el comando ``mvn jetty:run``.