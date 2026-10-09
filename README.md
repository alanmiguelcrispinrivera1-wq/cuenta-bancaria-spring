# Cajero automático — Spring Core e inyección de dependencias

**Autor:** Alan Miguel Crispin Rivera

## Cómo correrlo

    ./correr.sh App final-app
    ./probar.sh final

## Las piezas

| Bean | Clase | Cómo lo declara Spring (`@Component` o `@Bean`) | Singleton o prototype |
|---|---|---|---|
| cajeroAutomatico | CajeroAutomatico | `@Component` | Singleton |
| sesionCajero | SesionCajero | `@Component` + `@Scope("prototype")` | Prototype |
| repositorioEnMemoria | RepositorioEnMemoria | `@Component` | Singleton |
| antifraudePorMonto | AntifraudePorMonto | `@Component` + `@Primary` | Singleton |
| antifraudeEstricto | AntifraudeEstricto | `@Component` | Singleton |
| notificadorConsola | NotificadorConsola | `@Component` | Singleton |
| reloj | Clock | `@Bean` | Singleton |

## Boleto de salida

1. ¿Qué es la inyección de dependencias? Explícalo con el cajero, en tus palabras.

La inyección de dependencias es un mecanismo mediante el cual Spring proporciona a una clase los objetos que necesita para funcionar, en lugar de que esa clase tenga que crearlos directamente.
En nuestro proyecto, el cajero automático necesita un componente antifraude para validar las operaciones. En lugar de instanciarlo directamente con new, Spring puede crear el componente correspondiente e inyectarlo en el cajero. Esto nos ayuda a reducir el acoplamiento entre las clases, facilita el cambiar las implementaciones y permite probar el cajero con diferentes componentes.

2. En la MP-1, ¿quién decidía qué antifraude usaba el cajero? ¿Y desde la MP-2?

En MP-1 la decisión recae en nosotros, que elegiamos y creabamos explícitamente la implementación antifraude que utilizaría el cajero, mas explicitamente esto sucedia en AppSinSpring, ahi escribimos new AntifraudePorMonto() y se lo pasamos al constructor del cajero. Por otro lado, en MP-2 la decisión pasa a la configuración del contenedor de Spring, que resuelve qué implementación debe inyectar según los beans registrados y las reglas de selección configuradas. Por decirlo de forma mas explicita con lo que realizamos dentro del MP-2, tenemos que Spring lo decide siguiendo ConfiguracionBanco, ahi el método @Bean antifraude() dice qué antifraude crear, y Spring se lo inyecta al cajero al llamar a cajero(...).

3. ¿Cuándo usarías `@Bean` en vez de `@Component`? Da el ejemplo de hoy.

Cuando la clase no es nuestra y no podemos ponerle @Component encima, o cuando crearla requiere una configuración especial. El ejemplo de hoy es el Clock: es una clase de Java (java.time), así que en ConfiguracionBanco lo declaras con @Bean y Clock.system(ZoneId.of("America/Mexico_City")). Nuestras propias clases (CajeroAutomatico, NotificadorConsola, los antifraudes) llevan @Component y Spring las encuentra con @ComponentScan.

4. ¿Qué gana: `@Primary` o `@Qualifier`? ¿Por qué tiene sentido?

Gana @Qualifier. En nuestro código, AntifraudePorMonto es @Primary, pero el cajero pide @Qualifier("antifraudeEstricto") y recibe AntifraudeEstricto. Tiene sentido porque @Primary es un valor por defecto general (“si nadie dice nada, usa este”), mientras que @Qualifier es una petición explícita de quien usa el bean en un punto concreto. Basicamente, lo específico le gana a lo general.

5. En tu proyecto de Empleados de la Semana 3 nunca escribiste `@ComponentScan`. ¿Quién lo hace? (Pista: abre la
   anotación `@SpringBootApplication` con `Ctrl+clic` y busca las anotaciones que tiene arriba.)

Lo hace @SpringBootApplication. Es una anotación compuesta que incluye @SpringBootConfiguration (que a su vez es un @Configuration), @EnableAutoConfiguration y @ComponentScan. Por eso Spring Boot escanea automáticamente el paquete de nuestra clase principal y sus subpaquetes, sin que nosotros lo tengamos que escribir.   
