# Cajero automático — Spring Core e inyección de dependencias

**Autor:** Tu Nombre Completo

## Cómo correrlo

    ./correr.sh App final-app
    ./probar.sh final

## Las piezas

| Bean | Clase | Cómo lo declara Spring (`@Component` o `@Bean`) | Singleton o prototype |
|---|---|---|---|
| cajeroAutomatico | CajeroAutomatico | … | … |
| … | … | … | … |

## Boleto de salida

1. ¿Qué es la inyección de dependencias? Explícalo con el cajero, en tus palabras.
2. En la MP-1, ¿quién decidía qué antifraude usaba el cajero? ¿Y desde la MP-2?
3. ¿Cuándo usarías `@Bean` en vez de `@Component`? Da el ejemplo de hoy.
4. ¿Qué gana: `@Primary` o `@Qualifier`? ¿Por qué tiene sentido?
5. En tu proyecto de Empleados de la Semana 3 nunca escribiste `@ComponentScan`. ¿Quién lo hace? (Pista: abre la
   anotación `@SpringBootApplication` con `Ctrl+clic` y busca las anotaciones que tiene arriba.)