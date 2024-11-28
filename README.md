# Introducción Project Jigsaw

El proyecto Jigsaw (JSR 376 – Java Platform Module System) básicamente nos da el poder de romper nuestra
aplicación en trozos (siguiendo el principio de modularidad), que llamaremos módulos, donde cada uno de estos módulos
tiene perfectamente definidas sus dependencias con otros módulos.

## Principio de modularidad

La modularidad es un principio de diseño que nos ayuda a lograr:

* Un acoplamiento flexible entre componentes
* Contratos y dependencias claros entre componentes
* Implementación oculta mediante un fuerte encapsulamiento

## module-info.java

Para definir un módulo debemos añadir un fichero module-info.java en el directorio raíz de nuestro código fuente.

Además tener las siguiente keywords para definir un módulo:

```java
 // nombre del módulo. open permite la reflexión en todo el módulo
  open module com.example 
  {
      // exporta un paquete para que otros módulos accedan a sus paquetes públicos
      exports com.motorcycle;
  
      // indica una dependencia con el módulo com.orange
      requires com.vehicule;
  
      // indica una dependencia con com.ktm. el 'static' hace que la dependencia 
      // sea obligatoria durante compilación pero opcional durante ejecución
      requires static com.ktm;
  
      // indica una dependencia al módulo com.bmw y sus dependencias
      requires transitive com.bmw;
  
      // permite reflexión en el módulo com.yamaha
      opens com.yamaha;
  
      // permite reflexión en el paquete com.lemon pero solo desde el módulo com.mango
      opens com.honda to com.navi;
  
      // expone el tipo MyImplementation que implementa el servicio MyService
      provides com.service.MyService with com.consumer.MyImplementation;
  
      // usa el servicio com.service.MyService
      uses com.service.MyService;
  }
```

## Compilar

* `mkdir -p mods/org.astro mods/com.greetings`

* `javac -d mods/org.astro \
astro-module/src/main/java/module-info.java astro-module/src/main/java/org/astro/World.java`

* `javac --module-path mods -d mods/com.greetings \
greetings-module/src/main/java/module-info.java greetings-module/src/main/java/com/greetings/Main.java`

## Ejecutar

`java --module-path mods -m com.greetings/com.greetings.Main`

--module-path es la ruta del módulo, su valor es uno o más directorios que contienen módulos. La opción -m especifica el módulo principal, el valor después de la barra es el nombre de la clase principal en el módulo.

[Referencia](https://openjdk.org/projects/jigsaw/quick-start#multimodulecompile)
