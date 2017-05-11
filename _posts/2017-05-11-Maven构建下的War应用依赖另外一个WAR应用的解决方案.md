---
title: Maven构建下的War应用依赖另外一个WAR应用的解决方案
date: 2017-05-11 14:37:30
tags:
---

应用JEECG 3.3.6的过程中，碰到自己的web项目需要依赖JEECG的整个class的问题。解决办法如下:
(1)在JEECG 3.3.6项目的pom文件中添加如下内容：
			<!-- 把war包中的内容再打成一份 jar包 -->
			<plugin>
      			<groupId>org.apache.maven.plugins</groupId>
      			<artifactId>maven-war-plugin</artifactId>
      			<version>2.3</version>
      			<configuration>
       				<attachClasses>true</attachClasses><!-- 把class打包jar作为附件 -->
          			<archiveClasses>true</archiveClasses><!-- 把class打包jar -->
      			</configuration>
  			</plugin>

(2)在自己的web项目的pom文件中添加如下内容：
	<dependency>
        <groupId>org.jeecgframework</groupId>
        <artifactId>jeecgos</artifactId>
        <version>3.6.6</version>
        <type>war</type>
    </dependency>
    <dependency>
        <groupId>org.jeecgframework</groupId>
        <artifactId>jeecgos</artifactId>
        <version>3.6.6</version>
        <classifier>classes</classifier>
        <scope>provided</scope>
    </dependency>



 	或者添加如下内容：
    <dependencies>
		<dependency>
			<groupId>com.xxx</groupId>
			<artifactId>A-web</artifactId>
			<version>0.0.1-SNAPSHOT</version>
			<type>jar</type>
			<scope>compile</scope>
			<classifier>api</classifier>
		</dependency>
	</dependencies>

    注意以上的依赖中的type属性为jar,而不是默认的war。classifier的值api与A中的classesClassifier的值api要一致。