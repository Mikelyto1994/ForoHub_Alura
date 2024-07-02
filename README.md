# FOROHUB - ALURA Oracle Next Education

Este es el último **challenge** de Alura para Backend donde se aprende a crear una API desde cero generando distintos end points para el CRUD de el foro de forohub de Alura.


El principal reto para este challege no es el challenge; por el contrario, el reto es hacerlo en IDX. Ya que al ser un editor de código en la web, hay que solucionar los siguientes problemas
* Configurar el entorno para que funcione java
* Configurar una base de datos en local y utilizarla en este proyecto
* Investigar cómo hacer todo esto ya que estamos 01/06/2024, así que al ser un editor de código relativamente nuevo no hay mucha información aparte de la que nos ofrece la documentación de google.
* Hacerlo en menos de 15 días (porque el reto de alura termina en un mes y tengo otro proyecto pendiente)


### Configuracion del proyecto

1. Primero es ir a a [Spring Initializer](https://start.spring.io/) y crear un nuevo proyecto con sus dependencias. Como normalmente se haría.
2. Después se descarga el proyecto haciendo **click** en Generate
3. Luego de esto debemos subir los cambios a github.
    - Creamos un repositorio
    - Seguimos las instrucciones de Github
    - Subimos nuestros archivos al repositorio creado
4. Una vez ya tenemos nuestros archivos entramos a [Project IDX](https://idx.google.com/) y hacemos lo siguiente:
    - Iniciamos sesión si aún no has iniciado.
    - Damos click a **Get Started**
    - Damos click a **Import repo** y seguimos las instrucciones para iniciar sesión con github si aún no lo ha hecho
    - Después de esto nos va a pedir la URL al repositorio, y lo único que tenemos que hacer es importarlo
5. IDX nos ofrece unos paquetes por defecto para instalar; que, por recomendación, deberian descargarlos
    - Si no quieres descargar estos paquetes puedes buscar el paquete **Extension Pack for Java Auto Config** escribiendo jdk en el buscador de extensiones de project IDX
    - Una vez descargamos todas las extensiones asociadas con programacion para java entramos al archivo dev.nix
6. En el archivo dev.nix tenemos que configurar tanto el jdk como las extensiones agregadas
    - Arriba de packages nos va a aparecer la instruccion Add packages y debemos buscar el jdk que necesitamos, por ejemplo, yo necesito la version 17, por lo que voy a usar la 17 de la siguiente manera
    ```nix
    packages = [
        pkgs.jdk17
    ];
    ```

    - Ya con esto configuramos el jdk en el workspace y lo que tendriamos que hacer es añadir las extensiones en el apartado extensions 
    ```nix
    extensions = [
        "redhat.java"
        "vscjava.vscode-java-debug"
        "vscjava.vscode-java-dependency"
        "vscjava.vscode-java-pack"
        "vscjava.vscode-java-test"
        "vscjava.vscode-maven"
    ];
    ```
    
7. Una vez terminamos de configurar nuestro dev.nix, solamente debemos dar click en **Rebuild environment**

Solamente con esto ya podemos empezar a programar Java con Spring en este editor de código. Sin absolutamente nada más. Pero nuestro proyecto requiere de algo muy importante.

## Base de datos

Tenemos otro problema, la base de datos no se puede conectar de la manera tradicional; por el contrario, se deben utilizar otros métodos y a continuación presento los siguientes:

1. Crear la base de datos en local y conectarla con un servicio de tunneling
    - **Explicación:** Como no podemos usar el localhost desde idx ya que se ejecuta desde internet. Tenemos que utilizar nuestra IP pública y puerto para poder conectarnos a la base de datos en IDX, de esta manera podemos conectar nuestro localhost a nuestro proyecto. 
    - **Problema:** **Si esta IP se llegara a filtrar** ya sea mediante github o por cualquier descuido en general, podemos sufrir un ataque de manera remota.
    - **Solución:** Podemos conectar esta base de datos a un servicio externo de tunneling que nos cifra nuestros datos para mantenerlos seguros.
    - Algunas plataformas que ofrecen estos servicios son: [LocalXpose](https://localxpose.io/) o [Ngrok](https://ngrok.com/); sin embargo, estos dos servicios piden targeta para poder utilizar el servicio con apache o mysql. Por lo que aquí la siguiente solución:
    - **Solución 2:** La siguiente solución requiere tener bastante cuidado, ya que vamos a exponer nuestra IP Pública, por lo que hay dos opciones: *Crear variables de entorno en el idx/dev.nix y eliminarlo agregarlo al .gitignore (recomendado)* o por el contrario *Poner la información de la conexión directo en el application.properties y agregarlo al gitignore*. Para configurar las variables de entorno

2. Crear una base de datos en la nube:
    - **Explicación:** Podemos crear una base de datos en la nube utilizando los multiples servicios que la nube nos ofrece; por ejemplo: [Amazon RDS](https://aws.amazon.com/rds/), [Google CLoud](https://cloud.google.com/), [Oracle Cloud](https://www.oracle.com/cloud/), y existen muchos otros.
    - **Problema:** Todos estos servicios son **caros** y los que son gratuitos ofrecen una capa muy reducida. No encontré servicios de mysql que dieran una base de datos gratuita. Normalmente las bases de datos gratuitas por tiempo indefinido, ofrecen una cantidad de **GB limitados** para no exederse.
    - **Solución:** No necesariamente necesitan usar MySQL, como bien sabemos, el curso pide utilizar JPA que es un ORM que se caracteriza porque, no importa el lenguaje que utilicemos, solo tendremos que **configurar el application.properties** y con esto ya podremos usar el lenguaje que necesitemos. Por lo que a continuación voy a escribir alternativas gratuitas que ofrecen bases de datos y se pueden conectar con nuestro proyecto.

* [Vercel](https://vercel.com/) : Nos ofrece 1GB de espacio para nuestra base de datos **PostgreSQL** y aunque es para usar en aplicaciones de vercel, se pueden usar en el exterior.
* [Supabase](https://supabase.com/) : Nos ofrece 500MB de almacenamiento para nuestra base de datos **PostgreSQL**
* ~~[Turso](https://turso.tech/)~~ : Nos ofrece 9GB de espacio para nuestras bases de datos **SQLite**. Se pueden crear 500 Bases de datos pero estos 9GB son compartidos, igualmente está muy bien para ser una capa gratuita. Lo intenté y creo que no se puede.
* ~~[AstroDB](https://astro.build/db/)~~ : Nos ofrece 1GB de almacenamiento para base de datos **LibSQL** / **SQLite**. Se necesita una invitación puesto que está en su version beta.

Voy a utilizar [XAMPP](https://www.apachefriends.org/download.html) para tener una base de datos en local, por lo que debemos ir a la URL adjunta y descargarlo. Esta es una plataforma que sirve para iniciar servicios de apache y mysql.
Entre las plataformas que pueden usar:
* [MAMP](https://www.mamp.info/en/downloads/) - No lo he probado aún
* [WAMP](https://www.wampserver.com/) - Solo está en inglés y francés
* [XAMPP](https://www.apachefriends.org/download.html) - Solo está en inglés y alemán y es más ligero que WAMP


### Configuración de la base de datos

Así que lo primero que voy a hacer es configurar el entorno de desarrollo con las dependencias necesarias en build.gradle.

Lo que pueden hacer es agregar las dependencias al build.gradle y luego ejecutar el instalador de gradle para que se instalen automáticamente. Para ello nos vamos a [Maven repository](https://mvnrepository.com/) y buscamos las dependencias necesarias. Aún siendo de maven, tiene soporte para dependencias de gradle igualmente, por lo que no es necesario ir a otra página.

> [!CAUTION]
> Si ya agregaron las dependencias que se encuentran abajo, no las tienen que volver a agregar.
> Y si están usando otra base de datos, recuerden usar el driver adecuado.

Y agregamos lo siguiente en nuestras dependencias de build.gradle, agregaré la equivalencia a **pom.xml** al final:

```build.gradle
dependencies {
	implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
	implementation 'org.springframework.boot:spring-boot-starter-security'
	implementation 'org.springframework.boot:spring-boot-starter-validation'
	implementation 'org.springframework.boot:spring-boot-starter-web'
	implementation 'org.flywaydb:flyway-core'
	compileOnly 'org.projectlombok:lombok'
	developmentOnly 'org.springframework.boot:spring-boot-devtools'
	annotationProcessor 'org.projectlombok:lombok'
	testImplementation 'org.springframework.boot:spring-boot-starter-test'
	testImplementation 'org.springframework.security:spring-security-test'
	testRuntimeOnly 'org.junit.platform:junit-platform-launcher'
	runtimeOnly 'com.mysql:mysql-connector-j'
}
```
> [!NOTE]
> Si les da error al iniciar el proyecto, posiblemente es porque la configuración de flyway no es correcta

> [!IMPORTANT]
> La última dependencia se trata del driver para MySQL.
> Sin embargo, tenemos que agregar el driver que se necesite para la base de datos que utilicemos.

Y antes de configurar el application.properties necesito configurar la contraseña y nombre de mi base de datos.

Por lo que entramos al Shell de Xampp y ejecutamos los siguientes comandos

* Iniciamos sesión
```bash
mysql -u root -p

```

* Creamos la base de datos de forohub.
* Creamos un usuario y le damos una contraseña para entrar en esta.
```sql
CREATE DATABASE forohub;

CREATE USER 'user'@'localhost' IDENTIFIED BY 'password';
GRANT ALL PRIVILEGES ON *.* TO 'user'@'localhost';
FLUSH PRIVILEGES;
```
En mi caso el usuario es memo entonces escribiría `'memo'@'localhost'`, dejando el localhost intacto, y la password pueden usar la que quieran siempre y cuando:
* NO USEN CARACTERES ESPECIALES
* TIENE QUE SER ALGO SENCILLO

> [!NOTE]
> Recuerden que nadie va a poder entrar porque su base de datos igual va a estar en local.
> Así que tampoco tiene que ser una contraseña super compleja.

Ahora tenemos que configurar nuestro dev.nix para agregar las variables de entorno.

Pero, si ya hicieron push de dev.nix recuerden eliminarlo haciendo lo siguiente
* Agregamos .idx/dev.nix al .gitignore
* Ejecutamos los siguientes comandos en la terminal

```bash
git rm --cached .idx/dev.nix
git add .idx/dev.nix
git commit -m "Elimina el archivo dev.nix del proyecto"
git push origin main
```
Con esto lo eliminaremos este archivo del proyecto en github sin sufrir problemas en local.

Ahora tenemos que configurar el dev.nix con las variables de entorno, así que insertamos lo siguiente en el campo env = {}
```nix
env = {
    PUBLIC_IP = "su ip pública";
    PASSWORD = "contraseña";
}
```
Y su ip pública la pueden obtener usando la siguiente página "[What is my IP address](https://whatismyipaddress.com/)"

Finalmente, podemos configurar nuestro application properties que lo pueden encontrar en [este enlace](src/main/resources/application.properties)

Con todo esto, podremos ejecutar nuestro proyecto

> [!WARNING]
> Saltarán un montón de advertencias las cuales son normales, 
> al final deberá decir en qué puerto sale, que no puede ser el puerto 8080, como verán en mi application.properties.

### Tecnologías principales

* Java - Spring
* MySQL
* XAMPP

### Tecnologías secundarias

* Graddle / Maven
* IDX de Google

### Dependencias

* Lombok
* Spring Web
* Spring Boot DevTools
* Spring Data JPA
* Flyway Migration
* MySQL Driver
* Validation
* Spring Security


Dependencias en pom.xml:
```xml

<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-jpa</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-security</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-validation</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    <dependency>
        <groupId>org.flywaydb</groupId>
        <artifactId>flyway-core</artifactId>
    </dependency>
    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <scope>compile</scope>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-devtools</artifactId>
        <scope>development-only</scope>
    </dependency>
    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <scope>annotation-processor</scope>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
    </dependency>
    <dependency>
        <groupId>org.springframework.security</groupId>
        <artifactId>spring-security-test</artifactId>
        <scope>test</scope>
    </dependency>
    <dependency>
        <groupId>org.junit.platform</groupId>
        <artifactId>junit-platform-launcher</artifactId>
        <scope>test</scope>
    </dependency>
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-j</artifactId>
        <scope>runtime</scope>
    </dependency>
</dependencies>

```