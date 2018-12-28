Maven Environment specific Build
========================================

Assume we have two different environments staging and production. Every time we want to  build war for these environment, it is expected that configuration file(here it is application.properties) for the target environment is comes with war file.

Prerequisite
============
Maven


Our configuration file is present 

<!-- config directory structure -->

    src/main/resources/
        appliocation.properties
        dev/
        	 appliocation.properties
        production/
        	 appliocation.properties
        staging/
        	application.properties
 
        
 
 We will see how we can use profile to achieve goal.  We will define three profiles
* dev
* staging
* production

dev
=====
This is a default profile. This is used in eclipse

staging
========
This profile is used to build war for staging

production
==========
This profile is used to build war for production


```xml
<profiles>
	<profile>
		<id>local</id>
		<activation>
			<activeByDefault>true</activeByDefault>
		</activation>
	</profile>
	<profile>
		<id>staging</id>
		<properties>
			<env>staging</env>
		</properties>
	</profile>
	<profile>
		<id>production</id>
		<properties>
			<env>production</env>
		</properties>
	</profile>
</profiles>
```

While building war, we will filter our configuration file in resource section  as shown below

```xml
<filters>
	<filter>src/main/resources/${env}/application.properties</filter>
</filters>
<resources>
	<resource>
		<directory>src/main/resources/${env}</directory>
		<filtering>true</filtering>
		<includes>
			<include>application.properties</include>
		</includes>
	</resource>
	<resource>
		<directory>src/main/resources</directory>
		<filtering>true</filtering>
		<excludes>
			<exclude>**/application.properties</exclude>
		</excludes>
	</resource>
</resources>
```

And  we have to use profile option of maven  to build war file for different environment.
For staging it is
```console
mvn clean install -P staging
```

For production it is,
```console
mvn clean install -P  production
```




