<center> <h1>A la découverte de SpringBoot</h1> </center>

![A la découverte de SpringBoot](aventure.jpg)

# Introduction
Spring est un framework Java open source qui facilite et structure le développement d'applications java. Créé par Rod Jonhson, il est aujourd'hui porté par la société Pivotal.
 
Initialement dénommé Spring Framework, il existe et est utilisé dans de nombreux projets depuis de nombreuses années. 
Au fils du temps, il a évolué pour fournir toujours plus de services et permettre aux développeurs de travailler plus vite, plus facilement et de manière plus structurée. 
Spring Framework est composé de sept modules :
 * Spring Core
 * Spring AOP
 * Spring DAO
 * Spring ORM
 * Spring Web
 * Spring Context

Bien qu'ils soient prévus pour être indépendants, il est fréquent et souvent intéressant d'utiliser au maximum les services qu'ils proposent.
Spring est un framework extrement puissant, proposant des services très intéressants mais il un a un gros défaut : il est assez difficile à configurer.

C'est pour pallier ce défaut qu'a été créé SpringBoot. Bien que l'objectif principal soit toujours le même (permettre de développer plus facilement, plus rapidement et de manière structurée des applications java), 
SpringBoot facilite la mise en place d'un projet qui utilisera les fonctionnalités proposées par Spring en fournissant :
 * une configuration par défaut pour les projets Spring
 * un site qui centralise les ressources : https://start.spring.io/
 * un assistant intégré dans Eclipse : STS (Spring Tool Suite)
 * une intégration forte du serveur d'application (Tomcat version embarquée dans les projets SpringBoot)
 * un recours massif aux annotations (pour éviter les fichiers de configurations)
 * l'encapsulation d'un grand nombre de librairie java.
 * un cadre de bonnes pratiques : test, MVC...

Par ailleurs, SpringBoot est particulièrement efficace dans la création d'API Rest (ou RestFull). 

### Les Concepts fondateurs
Spring s'appuie sur 3 concepts :
* l'inversion de controle (IOC)
* la programmation orienté aspect (POA)
* une couche d'abstraction  

L'IOC gère les dépendances entre les classes par une déclaration.  
La POA sépare la couche métier (coeur du logiciel), de la couche technique.  
La couche d'abstraction permet d'intégrer facilement d'autres frameworks ou bibliothèques. On utilise pour cela l'outil Maven ou Gradle.

# Outils à disposition

Pour la suite de ce tutoriel, en plus d'Eclipse et Mysql que nous avons déjà installés et configurés, nous vous conseillons d'utiliser les outis présentés dans ce chapitre.

## STS
STS est un plugin Eclipse qui ajoute une perspective dédiée à la création et la gestion d'application SpringBoot. 
Pour l'installer, rendez-vous sur le marketplace et rechercher STS et suivez les instructions.

![STS in Eclipse MarketPlace](sts-marketplace.png)

# A la découverte de SpringBoot

Dans ce chapitre, nous allons créer une applications web avec SpringBoot from scratch (ie en repartant de zéro).

## Création d'un projet nouveau SpringBoot

### Création du projet
Dans la perspective Spring, créer un nouveau projet SpringBoot en passant par New \ Spring Boot Starter Project.

Dans la fenêtre de création qui s'affiche, renseignez les informations de votre projet en veillant à conserver comme Service URL "http://start.spring.io", comme type "Maven" et Java Version "8" et cliquez sur Next.

![Create Spring Project 1](new-spring-project-1.png)

Ensuite, selectionnez Spring Boot Version "1.5.10.BUILD-SNAPSHOT" (Attention : les versions 1.5.9 et 2.0.0 génèrent des erreurs par la suite) et ajouter "Web" dans les dépendances du projet. Cliquez sur Finish.

![Create Spring Project 2](new-spring-project-2.png)

Félicitation, votre projet SpringBoot est créé.

### Behind the scene
A cette étape, notre projet est déja bien avancé et STS a construit beaucoup de choses :
 * Le projet java a été créé avec la structure de projet SpringBoot
 * Un fichier de configuration Maven avec les dépendences "Web" a été créé
 * Les librairies du fichier de configuration sont téléchargées et ajoutées au classpath (rendez-vous dans le répertoire "Maven Dependencies" pour vous en assurer)
 * La classe de lancement à été créée

On entrevoit ici la puissance de SpringBoot qui, en quelques secondes, a créé un ensemble cohérent et structurant pour notre application en y incluant les librairies et TOUTES leurs dépendences.

### La structure d'un projet Spring Boot 
SpringBoot apporte aux développeurs une structuration forte pour leurs projets. En plus d'être une bonne pratique, ceci est important pour Spring qui parcourt ces repertoires pour gérer le code sources (identifications des classes "spéciales", injection de classes...), les ressources...

En parcourant notre projet SpringBoot, on peut déjà observer que les répertoires suivants ont été créés :
 * **src/main/java** : répertoire racine pour le code source de notre application
 * **src/main/resources** : répertoire racine pour les ressources pour notre application
 * **src/main/resources/static** : répertoire pour les ressources statiques à déployer(fichiers html, css...)
 * **src/test/java** : repertoire destiné à recevoir les tests (junit...)
 * **Maven Dependencies** : repertoire dans lequel Maven charge les dépendances du projet

En plus de ces répertoires, des fichiers de configurations essentiel au bon fonctionnement de notre projet ont été générés.

#### pom.xml
Ce fichier présent à la racine du projet est un fichier xml utilisé pour le pilotage de Maven. On y trouve :
 * des élements d'identifications de notre projet (méta informations)
 * la configuration Spring Boot (package, version...)
 * les propriétés pour le projet (version java, encodage...)
 * les dépendances vers les librairies utiles au projet
 
et d'autres informations qu'il est plus rare d'avoir à configurer.


**Bloc d'identification**
```xml
	<groupId>co.simplon.example</groupId>
	<artifactId>springboot-helloworld</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<packaging>jar</packaging>

	<name>springboot-helloworld</name>
	<description>Demo project for Spring Boot</description>
```

**Bloc SpringBoot**
```xml
	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>1.5.10.BUILD-SNAPSHOT</version>
		<relativePath/>
	</parent>
```

**Bloc de propriétés du projet**
```xml
	<properties>
		<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
		<project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
		<java.version>1.8</java.version>
	</properties>
```

**Bloc de dépendances**
```xml
	<dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>
	</dependencies>
```	

C'est gràce à ce fichier que le projet java sera configuré et que l'ensemble des librairies utiles seront téléchargées et intégrées au projet Java.

#### application.properties
Il s'agit d'un fichier de propriété java classique (clés/valeurs).
Ce fichier a vocation à contenir **TOUTES** les informations de configuration des composants utilisés par le projet (composants encapsulés par SpingBoot).

Pour plus de détails sur les mots clefs utilisables, cliquez [ici](https://docs.spring.io/spring-boot/docs/current/reference/html/common-application-properties.html).

#### la classe de lancement SpringBoot
Il s'agit de la classe java qui servira de point d'entrée pour notre application. 
Dans un projet Java classique, on pourrait la comparer avec la classe portant la méthode main.

```	 java
package co.simplon.springboot.helloworld;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class SpringbootHelloworldApplication {

	public static void main(String[] args) {
		SpringApplication.run(SpringbootHelloworldApplication.class, args);
	}
}
```	

On remarque que la classe qui a été générée est surmontée d'une annotation **@SpringBootApplication**. 
Cette annotation va permettre à Spring de savoir que c'est cette classe qu'il faut utiliser comme point d'entrée lors du lancement de notre application.

### Codons un peu

#### Helloworld
Pour la suite de notre projet, nous allons ajouter une nouvelle classe nommée HelloworldController.<br>
Dans le monde Spring, le mot 'Controller' designe la classe qui fournit les services web (équivalent des Servlet en Java Web).<br>
Pour définir un 'Controller' au sens Spring, on va ajouter à la classe précédemment créée l'annotations **@Controller**.

Grâce à cette annotation, Spring identifiera cette classe comme une classe 'spéciale' sur laquelle il travaillera.

Créez ensuite une fonction hello qui renvoie la chaîne de caractère "Helloworld".<br>
Pour relier cette méthode à une URI, on va ajouter à la déclaration de la méthode l'annotation **@RequestMapping(***chemin***).** (chemin étant la chaîne de caractère qui sera ajoutée à la fin de l'URL).<br>
Comme notre méthode retourne un variable (ici une chaîne de caractères), il faut aussi ajouter l'annotation **@ResponseBody**.

Ainsi, le code final de notre classe HelloworldController.java sera :

```	 java
package co.simplon.springboot.helloworld;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller
public class HelloworldController {
	
	@RequestMapping("/hello")
	@ResponseBody
	public String hello()
	{
		return "Helloworld";
	}
}
```	
#### Lancement d'un projet SpringBoot
Les développeurs de SpringBoot ont eu la bonne idée d'intégrer un serveur Tomcat à SpringBoot. Pour la phase de développement, il n'est donc plus nécessaire d'installer un serveur d'application, de le configurer, de l'intégrer dans Eclipse...

Pour lancer une application SpringBoot, il suffit simplement de faire un clique droit sur le projet, et lancer **Run as... \ Spring Boot App** (ou Debug as ... \Spring Boot App).

On peut suivre le lancement du serveur dont le log s'affiche automatiquement dans la console :

```console
2018-01-02 12:10:15.754  INFO 12280 --- [           main] c.s.s.h.SpringbootHelloworldApplication  : Starting SpringbootHelloworldApplication on SIMFOR094 with PID 12280 (C:\data\workspace-s\springboot-helloworld\target\classes started by Utilisateur in C:\data\workspace-s\springboot-helloworld)
2018-01-02 12:10:15.756  INFO 12280 --- [           main] c.s.s.h.SpringbootHelloworldApplication  : No active profile set, falling back to default profiles: default
2018-01-02 12:10:15.789  INFO 12280 --- [           main] ationConfigEmbeddedWebApplicationContext : Refreshing org.springframework.boot.context.embedded.AnnotationConfigEmbeddedWebApplicationContext@1445d7f: startup date [Tue Jan 02 12:10:15 CET 2018]; root of context hierarchy
2018-01-02 12:10:16.479  INFO 12280 --- [           main] s.b.c.e.t.TomcatEmbeddedServletContainer : Tomcat initialized with port(s): 8080 (http)
2018-01-02 12:10:16.485  INFO 12280 --- [           main] o.apache.catalina.core.StandardService   : Starting service [Tomcat]
2018-01-02 12:10:16.486  INFO 12280 --- [           main] org.apache.catalina.core.StandardEngine  : Starting Servlet Engine: Apache Tomcat/8.5.23
2018-01-02 12:10:16.538  INFO 12280 --- [ost-startStop-1] o.a.c.c.C.[Tomcat].[localhost].[/]       : Initializing Spring embedded WebApplicationContext
2018-01-02 12:10:16.538  INFO 12280 --- [ost-startStop-1] o.s.web.context.ContextLoader            : Root WebApplicationContext: initialization completed in 752 ms
2018-01-02 12:10:16.626  INFO 12280 --- [ost-startStop-1] o.s.b.w.servlet.ServletRegistrationBean  : Mapping servlet: 'dispatcherServlet' to [/]
2018-01-02 12:10:16.628  INFO 12280 --- [ost-startStop-1] o.s.b.w.servlet.FilterRegistrationBean   : Mapping filter: 'characterEncodingFilter' to: [/*]
2018-01-02 12:10:16.629  INFO 12280 --- [ost-startStop-1] o.s.b.w.servlet.FilterRegistrationBean   : Mapping filter: 'hiddenHttpMethodFilter' to: [/*]
2018-01-02 12:10:16.629  INFO 12280 --- [ost-startStop-1] o.s.b.w.servlet.FilterRegistrationBean   : Mapping filter: 'httpPutFormContentFilter' to: [/*]
2018-01-02 12:10:16.629  INFO 12280 --- [ost-startStop-1] o.s.b.w.servlet.FilterRegistrationBean   : Mapping filter: 'requestContextFilter' to: [/*]
2018-01-02 12:10:16.823  INFO 12280 --- [           main] s.w.s.m.m.a.RequestMappingHandlerAdapter : Looking for @ControllerAdvice: org.springframework.boot.context.embedded.AnnotationConfigEmbeddedWebApplicationContext@1445d7f: startup date [Tue Jan 02 12:10:15 CET 2018]; root of context hierarchy
2018-01-02 12:10:16.863  INFO 12280 --- [           main] s.w.s.m.m.a.RequestMappingHandlerMapping : Mapped "{[/hello]}" onto public java.lang.String co.simplon.springboot.helloworld.HelloworldController.hello()
2018-01-02 12:10:16.866  INFO 12280 --- [           main] s.w.s.m.m.a.RequestMappingHandlerMapping : Mapped "{[/error]}" onto public org.springframework.http.ResponseEntity<java.util.Map<java.lang.String, java.lang.Object>> org.springframework.boot.autoconfigure.web.BasicErrorController.error(javax.servlet.http.HttpServletRequest)
2018-01-02 12:10:16.866  INFO 12280 --- [           main] s.w.s.m.m.a.RequestMappingHandlerMapping : Mapped "{[/error],produces=[text/html]}" onto public org.springframework.web.servlet.ModelAndView org.springframework.boot.autoconfigure.web.BasicErrorController.errorHtml(javax.servlet.http.HttpServletRequest,javax.servlet.http.HttpServletResponse)
2018-01-02 12:10:16.885  INFO 12280 --- [           main] o.s.w.s.handler.SimpleUrlHandlerMapping  : Mapped URL path [/webjars/**] onto handler of type [class org.springframework.web.servlet.resource.ResourceHttpRequestHandler]
2018-01-02 12:10:16.885  INFO 12280 --- [           main] o.s.w.s.handler.SimpleUrlHandlerMapping  : Mapped URL path [/**] onto handler of type [class org.springframework.web.servlet.resource.ResourceHttpRequestHandler]
2018-01-02 12:10:16.907  INFO 12280 --- [           main] o.s.w.s.handler.SimpleUrlHandlerMapping  : Mapped URL path [/**/favicon.ico] onto handler of type [class org.springframework.web.servlet.resource.ResourceHttpRequestHandler]
2018-01-02 12:10:16.999  INFO 12280 --- [           main] o.s.j.e.a.AnnotationMBeanExporter        : Registering beans for JMX exposure on startup
2018-01-02 12:10:17.035  INFO 12280 --- [           main] s.b.c.e.t.TomcatEmbeddedServletContainer : Tomcat started on port(s): 8080 (http)
2018-01-02 12:10:17.039  INFO 12280 --- [           main] c.s.s.h.SpringbootHelloworldApplication  : Started SpringbootHelloworldApplication in 1.439 seconds (JVM running for 1.887)
2018-01-02 12:10:38.034  INFO 12280 --- [nio-8080-exec-1] o.a.c.c.C.[Tomcat].[localhost].[/]       : Initializing Spring FrameworkServlet 'dispatcherServlet'
2018-01-02 12:10:38.034  INFO 12280 --- [nio-8080-exec-1] o.s.web.servlet.DispatcherServlet        : FrameworkServlet 'dispatcherServlet': initialization started
2018-01-02 12:10:38.049  INFO 12280 --- [nio-8080-exec-1] o.s.web.servlet.DispatcherServlet        : FrameworkServlet 'dispatcherServlet': initialization completed in 15 ms
```

On y observe alors le lancement du serveur Tomcat :

```console
2018-01-02 12:10:15.754  INFO 12280 --- [           main] c.s.s.h.SpringbootHelloworldApplication  : Starting SpringbootHelloworldApplication on SIMFOR094 with PID 12280 (C:\data\workspace-s\springboot-helloworld\target\classes started by Utilisateur in C:\data\workspace-s\springboot-helloworld)
2018-01-02 12:10:15.756  INFO 12280 --- [           main] c.s.s.h.SpringbootHelloworldApplication  : No active profile set, falling back to default profiles: default
2018-01-02 12:10:15.789  INFO 12280 --- [           main] ationConfigEmbeddedWebApplicationContext : Refreshing org.springframework.boot.context.embedded.AnnotationConfigEmbeddedWebApplicationContext@1445d7f: startup date [Tue Jan 02 12:10:15 CET 2018]; root of context hierarchy
2018-01-02 12:10:16.479  INFO 12280 --- [           main] s.b.c.e.t.TomcatEmbeddedServletContainer : Tomcat initialized with port(s): 8080 (http)
2018-01-02 12:10:16.485  INFO 12280 --- [           main] o.apache.catalina.core.StandardService   : Starting service [Tomcat]
2018-01-02 12:10:16.486  INFO 12280 --- [           main] org.apache.catalina.core.StandardEngine  : Starting Servlet Engine: Apache Tomcat/8.5.23
```

le mapping des URL vers les méthodes java associées (notamment notre méthode hello) :

```console
2018-01-02 12:10:16.863  INFO 12280 --- [           main] s.w.s.m.m.a.RequestMappingHandlerMapping : Mapped "{[/hello]}" onto public java.lang.String co.simplon.springboot.helloworld.HelloworldController.hello()
2018-01-02 12:10:16.866  INFO 12280 --- [           main] s.w.s.m.m.a.RequestMappingHandlerMapping : Mapped "{[/error]}" onto public org.springframework.http.ResponseEntity<java.util.Map<java.lang.String, java.lang.Object>> org.springframework.boot.autoconfigure.web.BasicErrorController.error(javax.servlet.http.HttpServletRequest)
2018-01-02 12:10:16.866  INFO 12280 --- [           main] s.w.s.m.m.a.RequestMappingHandlerMapping : Mapped "{[/error],produces=[text/html]}" onto public org.springframework.web.servlet.ModelAndView org.springframework.boot.autoconfigure.web.BasicErrorController.errorHtml(javax.servlet.http.HttpServletRequest,javax.servlet.http.HttpServletResponse)
2018-01-02 12:10:16.885  INFO 12280 --- [           main] o.s.w.s.handler.SimpleUrlHandlerMapping  : Mapped URL path [/webjars/**] onto handler of type [class org.springframework.web.servlet.resource.ResourceHttpRequestHandler]
2018-01-02 12:10:16.885  INFO 12280 --- [           main] o.s.w.s.handler.SimpleUrlHandlerMapping  : Mapped URL path [/**] onto handler of type [class org.springframework.web.servlet.resource.ResourceHttpRequestHandler]
2018-01-02 12:10:16.907  INFO 12280 --- [           main] o.s.w.s.handler.SimpleUrlHandlerMapping  : Mapped URL path [/**/favicon.ico] onto handler of type [class org.springframework.web.servlet.resource.ResourceHttpRequestHandler]
```

...

Enfin, rendez-vous dans votre navigateur préféré à l'adresse : http://localhost:8080/hello.
Celui-ci vous affichera alors "Helloworld".

#### Observations
Avec ce projet d'introduction, vous avez pu découvrir que :
* SpringBoot accélère la mise en place d'un projet
* SpringBoot utilise Maven (ou Gradle) pour gérer les dépendances du projet
* SpringBoot nécessite peu de configuration
* Cette configuration se fait dans les classes java à l'aide d'annotations (dont beaucoup restent à découvrir)
* SpringBoot fournit une encapsulation forte de serveur d'application (avec une configuration par défaut)
 * SpringBoot c'est cool ?
