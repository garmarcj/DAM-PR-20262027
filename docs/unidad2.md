# Sprint 2. Utilización de objetos y clases

---

## Semana 4. Fundamentos de POO, instanciación y gestión de memoria (Stack vs. Heap)
Esta semana marca la entrada formal en los fundamentos de la Programación Orientada a Objetos (POO). A lo largo de estas
sesiones dejaremos atrás la visión lineal del código para comprender cómo la Máquina Virtual de Java estructura y gestiona
la memoria real, diferenciando las referencias en la pila (Stack) de los objetos dinámicos alojados en el montón (Heap). 
Apoyándonos en clases predefinidas de la biblioteca estándar como Random para dotar de seguridad e impredecibilidad,
aprenderemos a dar vida a objetos mediante el operador new y sus constructores, elevando la complejidad técnica y la 
robustez tanto del caso guía como de los proyectos propios de cada equipo.
---

### Día 13 - 2 sesiones

---

#### 1. Caso guía en AzaharTech
Son las tres de la tarde del lunes 5 de octubre. En la sala técnica de **AzaharTech**, **Alba Torres** proyecta en grande el código de `ControlAccesoQR.java` (versión v1.0) que cerramos el viernes pasado:

> *«En el Sprint 1 logramos que nuestro software fuera un motor secuencial exacto. Pero el director del IES El Caminàs nos ha trasladado una vulnerabilidad de seguridad evidente: el token QR que mostramos en la pantalla del vestíbulo utiliza como identificador de registro un número correlativo simple (`#1043`, `#1044`...). Cualquier estudiante avispado puede predecir el siguiente código y falsificar su asistencia desde el móvil sin estar físicamente en el instituto.*
>
> *Para blindar el acceso necesitamos que el token contenga un **código criptográfico pseudoaleatorio impredecible** que cambie en cada emisión.*
>
> *No vamos a reinventar la rueda programando generadores matemáticos complejos desde cero. Vamos a dar el salto a la **Programación Orientada a Objetos (POO)**: aprenderemos a instanciar y utilizar clases predefinidas de la biblioteca de Java como **`Random`**, y entenderemos qué ocurre físicamente bajo el capó en la memoria RAM (*Stack* frente a *Heap*) cuando creamos un objeto con el operador `new`»*.

---

#### 2. Fundamento teórico: clases, objetos y el modelo de memoria física (*Stack* vs. *Heap*)

```
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                        ARQUITECTURA DE LA MEMORIA RAM EN JAVA                          │
├───────────────────────────────────┬────────────────────────────────────────────────────┤
│ MEMORIA STACK (Pila de ejecución) │ MEMORIA HEAP (Montón de objetos dinámicos)         │
├───────────────────────────────────┼────────────────────────────────────────────────────┤
│ • Rápida, estructurada y estricta.│ • Área de memoria dinámica y flexible.             │
│ • Almacena variables primitivas   │ • Aquí residen los OBJETOS REALES creados con new. │
│   (int, double, boolean).         │ • Viven los atributos internos y estructuras.     │
│ • Almacena las REFERENCIAS        │ • Gestionada automáticamente por el recolector de  │
│   (punteros/direcciones de memoria)│   basura (Garbage Collector).                     │
└───────────────────────────────────┴────────────────────────────────────────────────────┘
```

##### A. Clase (molde) frente a objeto (instancia)
* **Clase (`java.util.Random`):** Es el plano o diseño conceptual. Define qué datos puede almacenar y qué métodos sabe ejecutar, pero no ocupa memoria como entidad viva.
* **Objeto:** Es el ejemplar concreto que creamos en memoria a partir de esa clase utilizando el operador **`new`**.
  ```java
  Random generador = new Random(); // Instanciación
  ```

##### B. Qué ocurre exactamente en memoria al instanciar
Cuando ejecutamos la instrucción `Random generador = new Random();`:
1. `Random generador`: Se reserva en la memoria **Stack** una variable de referencia (un puntero que guardará una dirección física de memoria, por ejemplo `0x4f2a`).
2. `new`: Pide al sistema operativo que reserve un bloque de memoria en el **Heap** para alojar el objeto.
3. `Random()` (Constructor): Inicializa el estado interno del objeto en el Heap (configura la semilla del reloj).
4. Asignación (`=`): Guarda la dirección de memoria del Heap en la variable `generador` del Stack.

```text
    STACK (Pila)                             HEAP (Montón dinámico)
┌──────────────────┐                     ┌───────────────────────────────┐
│ generador: 0x4f2a│ ──────────────────► │ Objeto Random en 0x4f2a       │
│                  │ (apunta a)          │ [seed: 1728139482019L]        │
└──────────────────┘                     └───────────────────────────────┘
```

##### C. El valor `null` y el temido `NullPointerException`
Si declaramos una variable de referencia pero no creamos el objeto con `new`:
```java
Random generador = null; // No apunta a ninguna dirección de memoria
int numero = generador.nextInt(); // ¡ERROR FATAL!: NullPointerException
```
La excepción `NullPointerException` se produce cuando intentamos invocar un método a través de una variable de referencia que apunta a la nada (`null`).

---

#### 3. Evolución del caso guía a `ControlAccesoQR v1.1` (PSeInt y Java)

Ampliamos el código guía sustituyendo el código fijo por la generación de un token pseudoaleatorio de seguridad con la clase predefinida `Random`.

##### Paso A. Algoritmo en PSeInt (`pr/pseudocodigo/ControlAccesoQR.psc` — v1.1)

```psc
Algoritmo ControlAccesoQR
    // =========================================================================
    // SISTEMA DE CONTROL DE ASISTENCIA QR - IES EL CAMINAS (Castellon)
    // Version: 1.1 (Evolucion: generacion aleatoria de seguridad)
    // =========================================================================

    Definir NOMBRE_CENTRO, PREFIJO_CENTRO, RUTA_LOGS Como Cadena
    Definir MINUTOS_POR_HORA, SEGUNDOS_POR_HORA, SEGUNDOS_POR_MINUTO Como Entero
    Definir FACTOR_PORCENTAJE Como Real
    
    NOMBRE_CENTRO <- "IES El Caminas (Castellon)"
    PREFIJO_CENTRO <- "CAMINAS"
    RUTA_LOGS <- ".\caminas\terminal\logs"
    MINUTOS_POR_HORA <- 60
    SEGUNDOS_POR_HORA <- 3600
    SEGUNDOS_POR_MINUTO <- 60
    FACTOR_PORCENTAJE <- 100.0

    Definir terminalId Como Entero
    Definir tempVestibulo Como Real
    Definir nombrePersona, dniPersona Como Cadena
    Definir perfilPersona Como Caracter
    Definir esEntrada Como Logico
    
    Definir horaEntrada, minutoEntrada Como Entero
    Definir horaSalida, minutoSalida Como Entero
    Definir minutosTotalesEntrada, minutosTotalesSalida, minutosEstanciaTotal Como Entero
    Definir tokenResumen Como Cadena
    
    Definir personasEnCentro, aforoDisponible, idUltimoFichaje Como Entero
    personasEnCentro <- 1200
    aforoDisponible <- 800
    idUltimoFichaje <- 1042
    
    Definir segundosActividadTerminal Como Entero
    Definir horasUptime, minutosUptime, segundosUptime Como Entero
    Definir aforoTotal Como Entero
    Definir porcentajeOcupacionReal Como Real
    Definir porcentajeOcupacionEntero Como Entero

    // [NUEVO DÍA 13] Variable para el token de seguridad aleatorio
    Definir codigoSeguridadAleatorio Como Entero

    Escribir "AZAHARTECH: TERMINAL ", NOMBRE_CENTRO, " (v1.1 - Seguridad QR)"
    Escribir "ID del terminal:"
    Leer terminalId
    Escribir "Temperatura del sensor (ºC):"
    Leer tempVestibulo
    Escribir "Segundos de actividad del terminal (uptime):"
    Leer segundosActividadTerminal
    
    Escribir "DNI de la persona:"
    Leer dniPersona
    Escribir "Nombre completo:"
    Leer nombrePersona
    Escribir "Perfil de acceso (E = Estudiante, D = Docente, V = Visita):"
    Leer perfilPersona

    esEntrada <- Verdadero
    
    Escribir "Introduce hora y minuto de entrada (por ejemplo, 8 15):"
    Leer horaEntrada
    Leer minutoEntrada
    Escribir "Introduce hora y minuto de salida (por ejemplo, 14 10):"
    Leer horaSalida
    Leer minutoSalida
        
    minutosTotalesEntrada <- (horaEntrada * MINUTOS_POR_HORA) + minutoEntrada
    minutosTotalesSalida <- (horaSalida * MINUTOS_POR_HORA) + minutoSalida
    minutosEstanciaTotal <- minutosTotalesSalida - minutosTotalesEntrada
    
    personasEnCentro <- personasEnCentro + 1
    aforoDisponible <- aforoDisponible - 1
    idUltimoFichaje <- idUltimoFichaje + 1

    // [NUEVO DÍA 13] Generación de un número aleatorio de seguridad de 4 cifras (1000 a 9999)
    codigoSeguridadAleatorio <- Aleatorio(1000, 9999)
      
    tokenResumen <- PREFIJO_CENTRO + "-" + dniPersona
    tokenResumen <- tokenResumen + "-T" + ConvertirATexto(terminalId)
    tokenResumen <- tokenResumen + "-SEC#" + ConvertirATexto(codigoSeguridadAleatorio)
    tokenResumen <- tokenResumen + "-REG" + ConvertirATexto(idUltimoFichaje)
    
    horasUptime <- trunc(segundosActividadTerminal / SEGUNDOS_POR_HORA)
    minutosUptime <- trunc((segundosActividadTerminal MOD SEGUNDOS_POR_HORA) / SEGUNDOS_POR_MINUTO)
    segundosUptime <- segundosActividadTerminal MOD SEGUNDOS_POR_MINUTO
    
    aforoTotal <- personasEnCentro + aforoDisponible
    porcentajeOcupacionReal <- (personasEnCentro * FACTOR_PORCENTAJE) / aforoTotal
    porcentajeOcupacionEntero <- trunc(porcentajeOcupacionReal)
    
    Escribir ""
    Escribir "======================================================================"
    Escribir "             INFORME OFICIAL DE ACCESO EN VESTIBULO                   "
    Escribir "======================================================================"
    Escribir "CENTRO:        ", NOMBRE_CENTRO
    Escribir "REGISTRO N. :  #", idUltimoFichaje, " | TOKEN QR DINAMICO: ", tokenResumen
    Escribir "PERSONA:       ", nombrePersona, " | PERFIL: ", perfilPersona
    Escribir "IDENTIFICADOR: ", dniPersona, " | SENTIDO: Entrada (", esEntrada, ")"
    Escribir "----------------------------------------------------------------------"
    Escribir "HORARIO:       Entrada ", horaEntrada, ":", minutoEntrada, " | Salida ", horaSalida, ":", minutoSalida
    Escribir "PERMANENCIA:   ", minutosEstanciaTotal, " minutos en el centro."
    Escribir "AFORO (", aforoTotal, "):  ", redon(porcentajeOcupacionReal * 100) / 100, " % (Panel: ", porcentajeOcupacionEntero, " %)"
    Escribir "ACTIVO:        ", horasUptime, "h ", minutosUptime, "m ", segundosUptime, "s (Sensor: ", tempVestibulo, " C)"
    Escribir "REGISTRO LOG:  ", RUTA_LOGS
    Escribir "======================================================================"
FinAlgoritmo
```

##### Paso B. Implementación en Java (`pr/src/ControlAccesoQR.java` — v1.1)

```java
/**
 * SISTEMA DE CONTROL DE ASISTENCIA POR CÓDIGO QR
 * Cliente: IES El Caminàs (Castellón de la Plana)
 * Consultora: AzaharTech Software Consulting
 *
 * Versión 1.1:
 * Evolución a POO mediante la instanciación de clases predefinidas (Random) en el Heap.
 * Se incorpora un token criptográfico pseudoaleatorio para evitar la predicción de códigos.
 *
 * @author Equipo AzaharTech (Alba Torres, Pau Ferrer)
 * @version 1.1 (Octubre 2026)
 * @since JDK 21 LTS
 */

import java.util.Scanner;
import java.util.Random; // [NUEVO DÍA 13] Importación de la clase predefinida

public class ControlAccesoQR {
    public static void main(String[] args) {
        // ---------------------------------------------------------------------
        // 1. CONSTANTES INMUTABLES DEL SISTEMA (Configuración corporativa)
        // ---------------------------------------------------------------------
        final String NOMBRE_CENTRO = "IES El Caminàs (Castellón)";
        final String PREFIJO_CENTRO = "CAMINAS";
        final String RUTA_LOGS = ".\caminas\terminal\logs";
        final int MINUTOS_POR_HORA = 60;
        final int SEGUNDOS_POR_HORA = 3600;
        final int SEGUNDOS_POR_MINUTO = 60;
        final double FACTOR_PORCENTAJE = 100.0;
        final int BASE_TOKEN_SEGURIDAD = 1000;
        final int RANGO_TOKEN_SEGURIDAD = 9000;

        // ---------------------------------------------------------------------
        // 2. DECLARACIÓN DE VARIABLES Y RESERVA DE OBJETOS EN MEMORIA
        // ---------------------------------------------------------------------
        // Instanciación de objetos en el Heap mediante el operador 'new'
        Scanner teclado = new Scanner(System.in);
        Random generadorSeguridad = new Random(); // [NUEVO DÍA 13] Reserva de objeto en el Heap

        // Variables de hardware y terminal
        int terminalId;
        double tempVestibulo;

        // Variables de identidad y perfil de la persona
        String nombrePersona;
        String dniPersona;
        char perfilPersona;
        boolean esEntrada;

        // Variables de cómputo horario y estancia
        int horaEntrada, minutoEntrada;
        int horaSalida, minutoSalida;
        int minutosTotalesEntrada;
        int minutosTotalesSalida;
        int minutosEstanciaTotal;
        String tokenResumen;

        // Variables de aforo del recinto y numeración de ticket
        int personasEnCentro = 1200;
        int aforoDisponible = 800;
        int idUltimoFichaje = 1042;

        // Variables de diagnóstico y tiempo de servicio (uptime)
        int segundosActividadTerminal;
        int horasUptime;
        int minutosUptime;
        int segundosUptime;

        // Variables de aforo consolidado y porcentajes
        int aforoTotal;
        double porcentajeOcupacionReal;
        int porcentajeOcupacionEntero;

        // [NUEVO DÍA 13] Variable para almacenar el código generado por el objeto Random
        int codigoSeguridadAleatorio;

        // ---------------------------------------------------------------------
        // 3. CAPTURA INTERACTIVA DE DATOS
        // ---------------------------------------------------------------------
        System.out.println("=================================================");
        System.out.println("   AZAHARTECH - TERMINAL " + NOMBRE_CENTRO);
        System.out.println("   Versión 1.1 (Token Dinámico con Objeto Random)  ");
        System.out.println("=================================================");
        System.out.print("ID del terminal: ");
        terminalId = teclado.nextInt();

        System.out.print("Temperatura del sensor (ºC): ");
        tempVestibulo = teclado.nextDouble();

        System.out.print("Segundos de actividad del terminal (uptime): ");
        segundosActividadTerminal = teclado.nextInt();

        teclado.nextLine(); // Limpieza obligatoria del buffer de teclado

        System.out.print("DNI de la persona: ");
        dniPersona = teclado.nextLine();

        System.out.print("Nombre completo: ");
        nombrePersona = teclado.nextLine();

        System.out.print("Perfil de acceso (E = Estudiante, D = Docente, V = Visita): ");
        perfilPersona = teclado.next().charAt(0);

        esEntrada = true;

        System.out.print("Hora de entrada (0-23): ");
        horaEntrada = teclado.nextInt();

        System.out.print("Minuto de entrada (0-59): ");
        minutoEntrada = teclado.nextInt();

        System.out.print("Hora de salida (0-23): ");
        horaSalida = teclado.nextInt();

        System.out.print("Minuto de salida (0-59): ");
        minutoSalida = teclado.nextInt();

        // ---------------------------------------------------------------------
        // 4. PROCESAMIENTO SECUENCIAL Y OPERACIONES MATEMÁTICAS
        // ---------------------------------------------------------------------
        // Conversión horaria a minutos y cálculo de permanencia neta
        minutosTotalesEntrada = (horaEntrada * MINUTOS_POR_HORA) + minutoEntrada;
        minutosTotalesSalida = (horaSalida * MINUTOS_POR_HORA) + minutoSalida;
        minutosEstanciaTotal = minutosTotalesSalida - minutosTotalesEntrada;

        // Operadores unarios de incremento y decremento de estado
        personasEnCentro++;
        aforoDisponible--;
        idUltimoFichaje++;

        // [NUEVO DÍA 13] Invocación de método de instancia sobre el objeto Random
        // nextInt(9000) genera valores entre 0 y 8999; al sumar 1000 acota al rango [1000 - 9999]
        codigoSeguridadAleatorio = BASE_TOKEN_SEGURIDAD + generadorSeguridad.nextInt(RANGO_TOKEN_SEGURIDAD);

        // Composición acumulativa del token con operador += incorporando el código dinámico
        tokenResumen = PREFIJO_CENTRO + "-" + dniPersona;
        tokenResumen += "-T" + terminalId;
        tokenResumen += "-SEC#" + codigoSeguridadAleatorio;
        tokenResumen += "-ESTANCIA-" + minutosEstanciaTotal;
        tokenResumen += "-REG" + idUltimoFichaje;

        // Descomposición temporal exacta mediante división entera y módulo (%)
        horasUptime = segundosActividadTerminal / SEGUNDOS_POR_HORA;
        minutosUptime = (segundosActividadTerminal % SEGUNDOS_POR_HORA) / SEGUNDOS_POR_MINUTO;
        segundosUptime = segundosActividadTerminal % SEGUNDOS_POR_MINUTO;

        // Cálculo de porcentaje con casting explícito a (double) para evitar división a 0
        aforoTotal = personasEnCentro + aforoDisponible;
        porcentajeOcupacionReal = ((double) personasEnCentro / aforoTotal) * FACTOR_PORCENTAJE;
        porcentajeOcupacionEntero = (int) porcentajeOcupacionReal;

        // ---------------------------------------------------------------------
        // 5. SALIDA FORMATEADA PROFESIONAL (System.out.printf)
        // ---------------------------------------------------------------------
        System.out.println("\n======================================================================");
        System.out.println("             INFORME OFICIAL DE ACCESO EN VESTÍBULO                   ");
        System.out.println("======================================================================");
        System.out.printf("CENTRO:        %-30s%n", NOMBRE_CENTRO);
        System.out.printf("REGISTRO N.º:  #%05d | TOKEN DINÁMICO: %s%n", idUltimoFichaje, tokenResumen);
        System.out.printf("PERSONA:       %-30s | PERFIL: %c%n", nombrePersona, perfilPersona);
        System.out.printf("IDENTIFICADOR: %-12s | SENTIDO: Entrada (%b)%n", dniPersona, esEntrada);
        System.out.println("----------------------------------------------------------------------");
        System.out.printf("HORARIO:       Entrada %02d:%02d | Salida %02d:%02d%n", horaEntrada, minutoEntrada, horaSalida, minutoSalida);
        System.out.printf("PERMANENCIA:   %03d minutos en el centro.%n", minutosEstanciaTotal);
        System.out.printf("AFORO (%04d):  %6.2f %% (Panel: %03d %%)%n", aforoTotal, porcentajeOcupacionReal, porcentajeOcupacionEntero);
        System.out.printf("ACTIVO:        %02dh %02dm %02ds (Sensor: %.1f ºC)%n", horasUptime, minutosUptime, segundosUptime, tempVestibulo);
        System.out.printf("REGISTRO LOG:  %s%n", RUTA_LOGS);
        System.out.println("======================================================================");

        teclado.close();
    }
}
```

---

El término **«Kata»** (procedente del *Software Craftsmanship* y las artes marciales) es una elección excelente: transmite la idea de **entrenamiento deliberado, técnica depurada y repetición consciente** para interiorizar la mecánica del código, algo que encaja a la perfección con un grupo que ya tiene base de Grado Medio.

A continuación tienes el **apartado 4 del Día 13 (Día 1 del Sprint 2)** completamente adaptado a la terminología de **Katas de código**, estructuradas por niveles de maestría:

---

#### 4. Dojo de entrenamiento y katas de código

Durante esta segunda hora, el aula se transforma en un **dojo de programación**. El trabajo pasa a ser **100 % activo e individual**, estructurado en tres **katas de dificultad progresiva** para entrenar la memoria, la instanciación de objetos y la generación pseudoaleatoria:

---

### Kata 1 (Cinturón blanco / Nivel base). Adaptación al proyecto propio
* **Objetivo:** Instanciar la clase predefinida `Random` en el montículo (*Heap*) y utilizarla para generar una magnitud aleatoria propia del dominio de tu proyecto.
* **Instrucciones:** Abre tu archivo `MiProyecto.java` (en versión v1.0) e incorpora un generador aleatorio:
  * En **Aventura conversacional**. Instanciar `Random` para simular una tirada de iniciativa o factor de suerte entre 1 y 20.
  * En **Motor de recomendación**. Instanciar `Random` para simular la variación aleatoria de afinidad de un usuario invitado.
  * En **Simulador de físicas 2D**. Instanciar `Random` para generar la velocidad inicial aleatoria de la partícula entre 5.0 y 25.0 m/s.
  * En **Bóveda de contraseñas**. Instanciar `Random` para generar un PIN numérico aleatorio de seguridad de 6 dígitos.

---

### Kata 2 (Cinturón marrón / Nivel avanzado. Acotación matemática en rango `[MIN, MAX]`
* **Contexto técnico.** Cuando a `nextInt()` le pasamos un valor entre parentesis (`nextInt(tope)`), genera números enteros que van desde 0 hasta tope - 1. Por ejemplo, nextInt(10) genera valores del 0 al 9.
* **Desafío de la kata.** Diseña una fórmula matemática secuencial en una sola línea que utilice `random.nextInt()` para generar un número pseudoaleatorio acotado estrictamente dentro de un rango cerrado $[MIN, MAX]$ (donde ambos extremos están obligatoriamente incluidos), **sin utilizar estructuras condicionales `if`**:
  $$\text{Resultado} = MIN + \text{random.nextInt}((MAX - MIN) + 1)$$
* **Evidencia de maestría.** Prueba tu fórmula configurando $MIN = 50$ y $MAX = 75$. Ejecuta el programa varias veces consecutivas y certifica en la consola que el valor generado nunca desciende de 50 ni supera 75.

---

### Kata 3 (Cinturón negro / Kata «Hacker AzaharTech»). El misterio de la semilla (*Seed*)
* **Contexto de seguridad.** La clase `Random` de Java no genera aleatoriedad pura física, sino secuencias matemáticas deterministas basadas en una «semilla» (*seed*).
* **Misión de la kata:**
  1. Abre una clase de prueba temporal en tu proyecto.
  2. Instancia **dos objetos distintos** de la clase `Random` pasándoles exactamente la misma semilla numérica en su constructor:
     ```java
     Random generadorA = new Random(12345L);
     Random generadorB = new Random(12345L);
     ```
  3. Invoca `nextInt(100)` tres veces seguidas en cada uno de los dos objetos e imprime sus salidas. ¿Qué observas en la consola?
  4. Redacta dos líneas de comentario en tu código explicando **por qué un generador con semilla fija comprometería la seguridad de los tokens del IES El Caminàs** y cómo el constructor por defecto `new Random()` soluciona esta vulnerabilidad utilizando el reloj del sistema (`System.currentTimeMillis()`).

---

### Día 14 - 2 sesiones

---

#### 1. Caso guía en AzaharTech
Son las tres de la tarde del martes 6 de octubre. En la sala técnica de **AzaharTech**, **Pau Ferrer** muestra entusiasmado una modificación en el sistema de acceso para el **IES El Caminàs**.

La dirección del instituto ha trasladado un problema logístico evidente: en un centro educativo con una capacidad de 2000 personas, situar un único monitor emisor en el vestíbulo principal provocaría aglomeraciones a primera hora de la mañana. Por ello, el centro ha solicitado instalar un **segundo monitor de acceso en la Puerta Norte**.

Ambos monitores proyectarán códigos QR dinámicos en paralelo y deben operar de forma **completamente autónoma e independiente**, de modo que los escaneos que se produzcan en una puerta no consuman ni descompensen la secuencia de seguridad de la otra.

Pau ha escrito en el código:
```java
Random generadorVestibulo = new Random();
Random generadorPuertaNorte = generadorVestibulo; // Pau cree que ha duplicado el generador
```

Pau explica convencido al resto del equipo:
> *«Para que el monitor de la Puerta Norte tenga su propio generador sin tener que volver a escribir `new`, he copiado la variable. Así cada monitor tiene su motor QR independiente»*.

**Alba Torres** mira el código, niega con la cabeza y llama a Laia Claramunt y al estudiante:
> *«Pau, acabas de cometer el error conceptual más extendido al pasar de tipos primitivos a objetos. Con un entero, escribir `int b = a` duplica el número en una celda de memoria independiente. Pero con objetos, **el signo igual jamás crea un objeto nuevo**.*
>
> *Solo has utilizado el operador `new` una vez, por lo que en el Heap solo existe **un único generador físico**. Lo que has hecho ha sido crear dos variables en el Stack que contienen la misma dirección de memoria.*
>
> *Has creado un **alias**: dos pantallas conectadas al mismo motor. Cada vez que un estudiante escanea en la Puerta Norte, hace avanzar la semilla interna del generador y altera la secuencia del Vestíbulo. Si de verdad quieres dos monitores autónomos, **debes invocar a `new` dos veces**.*
>
> *Hoy aprenderemos qué ocurre físicamente en la memoria RAM al asignar referencias, qué es un alias y cómo gestiona la JVM el ciclo de vida de los objetos en el Heap»*.

---

#### 2. Fundamento teórico: variables primitivas frente a variables de referencia

```
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                        COPIA DE VALOR FRENTE A COPIA DE REFERENCIA                     │
├───────────────────────────────────┬────────────────────────────────────────────────────┤
│ 1. COPIA POR VALOR (Primitivos)   │ 2. COPIA DE REFERENCIA / ALIAS (Objetos)           │
├───────────────────────────────────┼────────────────────────────────────────────────────┤
│ int a = 5;                        │ Random r1 = new Random();                          │
│ int b = a;                        │ Random r2 = r1; // NO crea un nuevo objeto         │
│                                   │                                                    │
│ Se duplica el dato en el Stack.   │ Se copia el PUNTERO en el Stack. Ambas variables   │
│ Si modificas 'b', 'a' no cambia.  │ apuntan a la MISMA instancia física en el Heap.    │
└───────────────────────────────────┴────────────────────────────────────────────────────┘
```

##### A. El mecanismo de copia en memoria: valor frente a referencia (alias)
* **Tipos primitivos (`int`, `double`, `boolean`, `char`).**
  La variable contiene directamente el dato binario en la memoria de ejecución (**Stack**). Al hacer `b = a`, se clona el valor en una celda física distinta.
* **Tipos referencia (Objetos creados con `new`).**
  La variable en el Stack **no almacena el objeto, sino una dirección de memoria (puntero)** que indica dónde reside el objeto real dentro de la memoria dinámica (**Heap**).
    * Al hacer `generadorPuertaNorte = generadorVestibulo`, se copia únicamente la dirección de memoria de 64 bits.
    * Se genera un **alias**: dos variables locales que gobiernan la misma instancia física.
    * Para disponer de dos generadores independientes con secuencias pseudoaleatorias que no interfieran entre sí, es obligatorio realizar **dos reservas independientes en el Heap mediante dos llamadas a `new`**:
      ```java
      Random generadorVestibulo   = new Random(); // Instancia 1 en el Heap
      Random generadorPuertaNorte = new Random(); // Instancia 2 en el Heap
      ```

```text
CASO A: Copia de referencia (1 objeto en Heap, 2 punteros en Stack)
       STACK                                     HEAP
┌───────────────────────────┐               ┌───────────────────────────────┐
│ generadorVestibulo: 0x7ffe│ ──────────┐   │ Objeto Random en 0x7ffe       │
├───────────────────────────┤           └──►│ (Compartido por ambas)        │
│ generadorPuertaNorte:0x7ffe│ ──────────┘   └───────────────────────────────┘
└───────────────────────────┘

CASO B: Dos instancias independientes con new (2 objetos en Heap)
       STACK                                     HEAP
┌───────────────────────────┐               ┌───────────────────────────────┐
│ generadorVestibulo: 0x1111│ ─────────────►│ Objeto Random A en 0x1111     │
├───────────────────────────┤               ├───────────────────────────────┤
│ generadorPuertaNorte:0x2222│ ─────────────►│ Objeto Random B en 0x2222     │
└───────────────────────────┘               └───────────────────────────────┘
```

##### B. Objetos huérfanos y el recolector de basura (*Garbage Collector*)
En lenguajes como Java no existe una instrucción manual para destruir objetos. La gestión de memoria dinámica es automática:
1. Si una variable que apuntaba a un objeto pasa a apuntar a `null` o a otra instancia:
   ```java
   Random r = new Random(); // Se crea el Objeto 1 en el Heap
   r = new Random();        // Se crea el Objeto 2. La variable 'r' apunta al Objeto 2.
   ```
2. El Objeto 1 se convierte en un **objeto huérfano**: ningún puntero activo en el Stack conserva su dirección.
3. El **Garbage Collector (GC)** de la Máquina Virtual de Java rastrea periódicamente la memoria en segundo plano, detecta los objetos inalcanzables y **libera automáticamente sus bytes en el Heap**.

##### C. El valor `null` y la seguridad de referencias
El literal reservado `null` indica que una variable de referencia no está apuntando a ningún objeto en memoria.
* Intentar invocar un método sobre una variable que vale `null` (*por ejemplo, `generador = null; generador.nextInt();`*) desencadena un error crítico en tiempo de ejecución: **`NullPointerException`**.

---

#### 3. Evolución del caso guía a `ControlAccesoQR v1.2`

Ampliamos el programa guía modelando la infraestructura de los dos accesos del centro educativo: instanciamos **dos generadores independientes con `new`** para garantizar la autonomía de emisión entre el monitor del vestíbulo y el monitor de la Puerta Norte.

##### Paso A. Algoritmo en PSeInt (`pr/pseudocodigo/ControlAccesoQR.psc` — v1.2)

```psc
Algoritmo ControlAccesoQR
    // =========================================================================
    // SISTEMA DE CONTROL DE ASISTENCIA QR - IES EL CAMINAS (Castellon)
    // Version: 1.2 (Evolucion: monitores multiples y generadores independientes)
    // =========================================================================

    Definir NOMBRE_CENTRO, PREFIJO_CENTRO, RUTA_LOGS Como Cadena
    Definir MINUTOS_POR_HORA, SEGUNDOS_POR_HORA, SEGUNDOS_POR_MINUTO Como Entero
    Definir FACTOR_PORCENTAJE Como Real
    
    NOMBRE_CENTRO <- "IES El Caminas (Castellon)"
    PREFIJO_CENTRO <- "CAMINAS"
    RUTA_LOGS <- ".\caminas\terminal\logs"
    MINUTOS_POR_HORA <- 60
    SEGUNDOS_POR_HORA <- 3600
    SEGUNDOS_POR_MINUTO <- 60
    FACTOR_PORCENTAJE <- 1000

    Definir terminalId Como Entero
    Definir tempVestibulo Como Real
    Definir nombrePersona, dniPersona Como Cadena
    Definir perfilPersona Como Caracter
    Definir esEntrada Como Logico
    
    Definir horaEntrada, minutoEntrada Como Entero
    Definir horaSalida, minutoSalida Como Entero
    Definir minutosTotalesEntrada, minutosTotalesSalida, minutosEstanciaTotal Como Entero
    Definir tokenResumen Como Cadena
    
    Definir personasEnCentro, aforoDisponible, idUltimoFichaje Como Entero
    personasEnCentro <- 1200
    aforoDisponible <- 800
    idUltimoFichaje <- 1042
    
    Definir segundosActividadTerminal Como Entero
    Definir horasUptime, minutosUptime, segundosUptime Como Entero
    Definir aforoTotal Como Entero
    Definir porcentajeOcupacionReal Como Real
    Definir porcentajeOcupacionEntero Como Entero

    // [NUEVO DÍA 14] Dos tokens independientes para vestíbulo y puerta norte
    Definir codigoTokenVestibulo, codigoTokenPuertaNorte Como Entero

    Escribir "AZAHARTECH: TERMINAL ", NOMBRE_CENTRO, " (v1.2 - Monitores Multiples)"
    Escribir "ID del terminal principal (vestibulo):"
    Leer terminalId
    Escribir "Temperatura del sensor (ºC):"
    Leer tempVestibulo
    Escribir "Segundos de actividad del terminal (uptime):"
    Leer segundosActividadTerminal
    
    Escribir "DNI de la persona:"
    Leer dniPersona
    Escribir "Nombre completo:"
    Leer nombrePersona
    Escribir "Perfil de acceso (E = Estudiante, D = Docente, V = Visita):"
    Leer perfilPersona

    esEntrada <- Verdadero
    
    Escribir "Introduce hora y minuto de entrada (por ejemplo, 8 15):"
    Leer horaEntrada
    Leer minutoEntrada
    Escribir "Introduce hora y minuto de salida (por ejemplo, 14 10):"
    Leer horaSalida
    Leer minutoSalida
        
    minutosTotalesEntrada <- (horaEntrada * MINUTOS_POR_HORA) + minutoEntrada
    minutosTotalesSalida <- (horaSalida * MINUTOS_POR_HORA) + minutoSalida
    minutosEstanciaTotal <- minutosTotalesSalida - minutosTotalesEntrada
    
    personasEnCentro <- personasEnCentro + 1
    aforoDisponible <- aforoDisponible - 1
    idUltimoFichaje <- idUltimoFichaje + 1

    // [DÍA 14] Generación autónoma en cada punto de acceso
    codigoTokenVestibulo <- Aleatorio(1000, 9999)     // Motor monitor vestíbulo
    codigoTokenPuertaNorte <- Aleatorio(1000, 9999)   // Motor monitor puerta norte
      
    tokenResumen <- PREFIJO_CENTRO + "-" + dniPersona
    tokenResumen <- tokenResumen + "-T" + ConvertirATexto(terminalId)
    tokenResumen <- tokenResumen + "-SEC#" + ConvertirATexto(codigoTokenVestibulo)
    tokenResumen <- tokenResumen + "-REG" + ConvertirATexto(idUltimoFichaje)
    
    horasUptime <- trunc(segundosActividadTerminal / SEGUNDOS_POR_HORA)
    minutosUptime <- trunc((segundosActividadTerminal MOD SEGUNDOS_POR_HORA) / SEGUNDOS_POR_MINUTO)
    segundosUptime <- segundosActividadTerminal MOD SEGUNDOS_POR_MINUTO
    
    aforoTotal <- personasEnCentro + aforoDisponible // 1201 + 799 = 2000
    porcentajeOcupacionReal <- (personasEnCentro * FACTOR_PORCENTAJE) / aforoTotal
    porcentajeOcupacionEntero <- trunc(porcentajeOcupacionReal)
    
    Escribir ""
    Escribir "======================================================================"
    Escribir "             INFORME OFICIAL DE ACCESO EN VESTIBULO                   "
    Escribir "======================================================================"
    Escribir "CENTRO:             ", NOMBRE_CENTRO
    Escribir "REGISTRO N. :       #", idUltimoFichaje, " | TOKEN VESTIBULO: ", tokenResumen
    Escribir "TOKEN PUERTA NORTE: ", PREFIJO_CENTRO, "-", dniPersona, "-T102-SEC#", codigoTokenPuertaNorte
    Escribir "PERSONA:            ", nombrePersona, " | PERFIL: ", perfilPersona
    Escribir "IDENTIFICADOR:      ", dniPersona, " | SENTIDO: Entrada (", esEntrada, ")"
    Escribir "----------------------------------------------------------------------"
    Escribir "HORARIO:            Entrada ", horaEntrada, ":", minutoEntrada, " | Salida ", horaSalida, ":", minutoSalida
    Escribir "PERMANENCIA:        ", minutosEstanciaTotal, " minutos en el centro."
    Escribir "AFORO (", aforoTotal, "):       ", redon(porcentajeOcupacionReal * 100) / 100, " % (Panel: ", porcentajeOcupacionEntero, " %)"
    Escribir "ACTIVO:             ", horasUptime, "h ", minutosUptime, "m ", segundosUptime, "s (Sensor: ", tempVestibulo, " C)"
    Escribir "REGISTRO LOG:       ", RUTA_LOGS
    Escribir "======================================================================"
FinAlgoritmo
```

##### Paso B. Implementación en Java (`pr/src/ControlAccesoQR.java` — v1.2)

```java
/**
 * SISTEMA DE CONTROL DE ASISTENCIA POR CÓDIGO QR
 * Cliente: IES El Caminàs (Castellón de la Plana)
 * Consultora: AzaharTech Software Consulting
 *
 * Versión 1.2:
 * Múltiples instancias en el Heap con 'new' frente a copias de punteros en el Stack.
 * Soporte para dos monitores de acceso independientes (vestíbulo principal y puerta norte).
 *
 * @author Equipo AzaharTech (Alba Torres, Pau Ferrer)
 * @version 1.2 (Octubre 2026)
 * @since JDK 21 LTS
 * 
 */

import java.util.Scanner;
import java.util.Random;

public class ControlAccesoQR {
    public static void main(String[] args) {
        // ---------------------------------------------------------------------
        // 1. CONSTANTES INMUTABLES DEL SISTEMA (Configuración corporativa)
        // ---------------------------------------------------------------------
        final String NOMBRE_CENTRO = "IES El Caminàs (Castellón)";
        final String PREFIJO_CENTRO = "CAMINAS";
        final String RUTA_LOGS = ".\\caminas\\terminal\\logs";
        final int MINUTOS_POR_HORA = 60;
        final int SEGUNDOS_POR_HORA = 3600;
        final int SEGUNDOS_POR_MINUTO = 60;
        final double FACTOR_PORCENTAJE = 100.0;
        final int BASE_TOKEN_SEGURIDAD = 1000;
        final int RANGO_TOKEN_SEGURIDAD = 9000;


      // ---------------------------------------------------------------------
        // 2. DECLARACIÓN DE VARIABLES Y RESERVA DE OBJETOS EN MEMORIA
        // ---------------------------------------------------------------------
        Scanner teclado = new Scanner(System.in);

        // [NUEVO DÍA 14] Instanciamos DOS objetos independientes en el Heap mediante 'new'
        // Cada generador tiene su propia dirección de memoria y su propio estado interno
        Random generadorVestibulo   = new Random(); // Objeto 1 en el Heap (Monitor principal)
        Random generadorPuertaNorte = new Random(); // Objeto 2 en el Heap (Monitor puerta norte)

        // Variables de hardware y terminal
        int terminalId;
        double tempVestibulo;

        // Variables de identidad y perfil de la persona
        String nombrePersona;
        String dniPersona;
        char perfilPersona;
        boolean esEntrada;

        // Variables de cómputo horario y estancia
        int horaEntrada, minutoEntrada;
        int horaSalida, minutoSalida;
        int minutosTotalesEntrada;
        int minutosTotalesSalida;
        int minutosEstanciaTotal;

        // [NUEVO DÍA 14] Dos tokens independientes generados para cada monitor
        String tokenResumenVestibulo;
        String tokenResumenPuertaNorte;

        // Variables de aforo del recinto y numeración de ticket
        int personasEnCentro = 1200;
        int aforoDisponible = 800;
        int idUltimoFichaje = 1042;

        // Variables de diagnóstico y tiempo de servicio (uptime)
        int segundosActividadTerminal;
        int horasUptime;
        int minutosUptime;
        int segundosUptime;

        // Variables de aforo consolidado y porcentajes
        int aforoTotal;
        double porcentajeOcupacionReal;
        int porcentajeOcupacionEntero;

        // Códigos pseudoaleatorios independientes
        int codigoTokenVestibulo;
        int codigoTokenPuertaNorte;

        // ---------------------------------------------------------------------
        // 3. CAPTURA INTERACTIVA DE DATOS
        // ---------------------------------------------------------------------
        System.out.println("=================================================");
        System.out.println("   AZAHARTECH - TERMINAL " + NOMBRE_CENTRO);
        System.out.println("   Versión 1.2 (Soporte Multimonitor Independiente) ");
        System.out.println("=================================================");
        System.out.print("ID del terminal principal: ");
        terminalId = teclado.nextInt();

        System.out.print("Temperatura del sensor (ºC): ");
        tempVestibulo = teclado.nextDouble();

        System.out.print("Segundos de actividad del terminal (uptime): ");
        segundosActividadTerminal = teclado.nextInt();

        teclado.nextLine(); // Limpieza obligatoria del buffer de teclado

        System.out.print("DNI de la persona: ");
        dniPersona = teclado.nextLine();

        System.out.print("Nombre completo: ");
        nombrePersona = teclado.nextLine();

        System.out.print("Perfil de acceso (E = Estudiante, D = Docente, V = Visita): ");
        perfilPersona = teclado.next().charAt(0);

        esEntrada = true;

        System.out.print("Hora de entrada (0-23): ");
        horaEntrada = teclado.nextInt();

        System.out.print("Minuto de entrada (0-59): ");
        minutoEntrada = teclado.nextInt();

        System.out.print("Hora de salida (0-23): ");
        horaSalida = teclado.nextInt();

        System.out.print("Minuto de salida (0-59): ");
        minutoSalida = teclado.nextInt();

        // ---------------------------------------------------------------------
        // 4. PROCESAMIENTO SECUENCIAL Y OPERACIONES MATEMÁTICAS
        // ---------------------------------------------------------------------
        // Conversión horaria a minutos y cálculo de permanencia neta
        minutosTotalesEntrada = (horaEntrada * MINUTOS_POR_HORA) + minutoEntrada;
        minutosTotalesSalida = (horaSalida * MINUTOS_POR_HORA) + minutoSalida;
        minutosEstanciaTotal = minutosTotalesSalida - minutosTotalesEntrada;

        // Operadores unarios de incremento y decremento de estado
        personasEnCentro++;
        aforoDisponible--;
        idUltimoFichaje++;

        // [NUEVO DÍA 14] Cada monitor invoca su propio objeto en el Heap de forma autónoma
        codigoTokenVestibulo   = BASE_TOKEN_SEGURIDAD + generadorVestibulo.nextInt(RANGO_TOKEN_SEGURIDAD);   // Token vestíbulo [1000 - 9999]
        codigoTokenPuertaNorte = BASE_TOKEN_SEGURIDAD + generadorPuertaNorte.nextInt(RANGO_TOKEN_SEGURIDAD); // Token puerta norte [1000 - 9999]

        // Composición acumulativa de ambos tokens
        tokenResumenVestibulo = PREFIJO_CENTRO + "-" + dniPersona;
        tokenResumenVestibulo += "-T" + terminalId;
        tokenResumenVestibulo += "-SEC#" + codigoTokenVestibulo;
        tokenResumenVestibulo += "-ESTANCIA-" + minutosEstanciaTotal;
        tokenResumenVestibulo += "-REG" + idUltimoFichaje;

        tokenResumenPuertaNorte = PREFIJO_CENTRO + "-" + dniPersona;
        tokenResumenPuertaNorte += "-T102"; // ID fijo asignado al monitor norte
        tokenResumenPuertaNorte += "-SEC#" + codigoTokenPuertaNorte;
        tokenResumenPuertaNorte += "-ESTANCIA-" + minutosEstanciaTotal;
        tokenResumenPuertaNorte += "-REG" + idUltimoFichaje;

        // Descomposición temporal exacta mediante división entera y módulo (%)
        horasUptime = segundosActividadTerminal / SEGUNDOS_POR_HORA;
        minutosUptime = (segundosActividadTerminal % SEGUNDOS_POR_HORA) / SEGUNDOS_POR_MINUTO;
        segundosUptime = segundosActividadTerminal % SEGUNDOS_POR_MINUTO;

        // Cálculo de porcentaje con casting explícito a (double) para evitar división a 0
        aforoTotal = personasEnCentro + aforoDisponible;
        porcentajeOcupacionReal = ((double) personasEnCentro / aforoTotal) * FACTOR_PORCENTAJE;
        porcentajeOcupacionEntero = (int) porcentajeOcupacionReal;

        // ---------------------------------------------------------------------
        // 5. SALIDA FORMATEADA PROFESIONAL (System.out.printf)
        // ---------------------------------------------------------------------
        System.out.println("\n======================================================================");
        System.out.println("             INFORME OFICIAL DE ACCESO EN VESTÍBULO                   ");
        System.out.println("======================================================================");
        System.out.printf("CENTRO:               %-30s%n", NOMBRE_CENTRO);
        System.out.printf("REGISTRO N.º:         #%05d%n", idUltimoFichaje);
        System.out.printf("TOKEN QR VESTÍBULO:   %s%n", tokenResumenVestibulo);
        System.out.printf("TOKEN QR PUERTA NORTE:%s (Monitor autónomo)%n", tokenResumenPuertaNorte);
        System.out.printf("PERSONA:              %-30s | PERFIL: %c%n", nombrePersona, perfilPersona);
        System.out.printf("IDENTIFICADOR:        %-12s | SENTIDO: Entrada (%b)%n", dniPersona, esEntrada);
        System.out.println("----------------------------------------------------------------------");
        System.out.printf("HORARIO:              Entrada %02d:%02d | Salida %02d:%02d%n", horaEntrada, minutoEntrada, horaSalida, minutoSalida);
        System.out.printf("PERMANENCIA:          %03d minutos en el centro.%n", minutosEstanciaTotal);
        System.out.printf("AFORO (%04d):         %6.2f %% (Panel: %03d %%)%n", aforoTotal, porcentajeOcupacionReal, porcentajeOcupacionEntero);
        System.out.printf("ACTIVO:               %02dh %02dm %02ds (Sensor: %.1f ºC)%n", horasUptime, minutosUptime, segundosUptime, tempVestibulo);
        System.out.printf("REGISTRO LOG:         %s%n", RUTA_LOGS);
        System.out.println("======================================================================");

        teclado.close();
    }
}
```

---

#### 4. Segunda sesión: dojo de entrenamiento y katas de código

Pasamos al dojo de entrenamiento para programar, con tres **katas de dificultad progresiva** sobre el comportamiento de las referencias y la memoria física:

---

##### Kata 1 (Cinturón blanco / Nivel base). Dos generadores independientes en tu proyecto
* **Objetivo:** Instanciar **dos objetos independientes** con el operador `new` en el Heap para dos propósitos diferenciados de tu proyecto propio.
* **Instrucciones:** Abre tu archivo `MiProyecto.java` (en versión v1.1) e incorpora dos instancias de `Random`:
    * En **Aventura conversacional**: instanciar `randomJugador = new Random();` para la tirada de ataque y `randomEnemigo = new Random();` para la tirada de defensa del rival.
    * En **Motor de recomendación**: instanciar `randomAfinidad = new Random();` para simular gustos del usuario y `randomCatalogo = new Random();` para simular la popularidad del ítem en catálogo.
    * En **Simulador de físicas 2D**: instanciar `randomVelocidad = new Random();` para la velocidad inicial y `randomAngulo = new Random();` para la dirección de lanzamiento.
    * En **Bóveda de contraseñas**: instanciar `randomPin = new Random();` para generar el PIN de acceso y `randomSalt = new Random();` para generar el salt criptográfico de la clave.

---

##### Kata 2 (Cinturón marrón / Nivel avanzado). La trampa del alias de memoria
* **Contexto técnico:** Demostrar experimentalmente la diferencia entre copiar una referencia (crear un alias) y crear dos objetos reales con `new`.
* **Misión de la kata:**
    1. Crea una clase temporal de pruebas llamada `PruebaAlias.java`.
    2. Escribe y ejecuta el siguiente experimento:
       ```java
       // Experimento 1: Dos variables apuntando al MISMO objeto (Alias)
       Random g1 = new Random(500L);
       Random g2 = g1; // Copia de referencia en el Stack
  
       System.out.println("--- EXPERIMENTO 1 (Un solo objeto en Heap) ---");
       System.out.println("Número g1: " + g1.nextInt(100));
       System.out.println("Número g2: " + g2.nextInt(100));
  
       // Experimento 2: Dos objetos DISTINTOS en el Heap con new
       Random g3 = new Random(500L);
       Random g4 = new Random(500L); // Dos llamadas a new
  
       System.out.println("\n--- EXPERIMENTO 2 (Dos objetos independientes) ---");
       System.out.println("Número g3: " + g3.nextInt(100));
       System.out.println("Número g4: " + g4.nextInt(100));
       ```
    3. **Pregunta técnica para tu cuaderno:**  
       ¿Por qué en el Experimento 1 `g1` y `g2` muestran números **distintos**, mientras que en el Experimento 2 `g3` y `g4` muestran exactamente el **mismo** número?  
       *(Escribe la respuesta en dos líneas de comentario explicando qué ocurre con el estado interno del generador en el Heap).*

---

##### Kata 3 (Cinturón negro / «Hacker AzaharTech»). Telemetría de objetos huérfanos y Garbage Collector
* **Contexto de sistemas.** En sistemas que operan 24/7 (como los servidores de AzaharTech), no controlar los objetos huérfanos puede degradar el rendimiento de la JVM.
* **Misión de la kata:**
    1. Utiliza la clase `Runtime` para consultar cuánta memoria libre en bytes tiene la JVM:
       ```java
       Runtime runtime = Runtime.getRuntime();
       System.out.println("Memoria libre inicial: " + runtime.freeMemory() + " bytes");
  
       // Creamos 3 referencias en el Stack y 3 objetos en el Heap
       Random r1 = new Random();
       Random r2 = new Random();
       Random r3 = new Random();
  
       // Dejamos r1 y r2 huérfanos en el Heap
       r1 = null;
       r2 = null;
  
       System.gc(); // Solicitamos a la JVM que pase el Garbage Collector
  
       System.out.println("Memoria libre tras liberar referencias: " + runtime.freeMemory() + " bytes");
       ```
    2. Comprueba en la consola la variación de memoria e incluye una línea de comentario explicando qué ha hecho `System.gc()` con los objetos que apuntaban a `null`.

---

### Día 15 - 2 sesiones

---

#### 1. Caso guía en AzaharTech
Son las tres de la tarde del miércoles 7 de octubre. En el laboratorio de pruebas de **AzaharTech**, **Pau Ferrer** y **Alba Torres** revisan el funcionamiento del sistema multimonitor del **IES El Caminàs** que implementaron ayer.

El departamento de informática del instituto ha planteado una nueva necesidad técnica para el equipo:
> *«Antes de abrir las puertas a los estudiantes, el personal de mantenimiento necesita realizar un **test de calibración y diagnóstico matutino** de los sensores y terminales. Para comprobar que el lector óptico del vestíbulo lee bien, necesitan que el sistema emita un código de prueba con una **secuencia reproducible y predecible** (para contrastarlo con un patrón de referencia). Además, el sensor térmico del vestíbulo debe simular una pequeña fluctuación decimal ambiental y comprobar si el hardware de respaldo por radiofrecuencia (NFC) responde correctamente»*.

**Laia Claramunt** toma el mando en la pizarra:
> *«Hasta ahora solo hemos utilizado el constructor por defecto `new Random()` para pedir números enteros. Pero los objetos son mucho más versátiles:*
>
> *1. Las clases predefinidas pueden ofrecer **constructores con parámetros**, como `new Random(semilla)`, que nos permiten fijar un estado inicial concreto en el Heap.*  
> *2. Los objetos tienen métodos con diferentes **tipos de retorno**: no solo devuelven enteros con `nextInt()`, sino también números decimales con `nextDouble()` y estados lógicos con `nextBoolean()`.*
>
> *Hoy aprenderemos a invocar métodos pasando argumentos, a capturar diferentes tipos de retorno en variables de memoria y evolucionaremos `ControlAccesoQR` a la versión **v1.3**»*.

---

#### 2. Fundamento teórico: constructores con parámetros, firmas y tipos de retorno

```
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                        ANATOMÍA DE LA COMUNICACIÓN CON UN OBJETO                       │
├────────────────────────────────────────────────────────────────────────────────────────┤
│ tipoRetorno variable = objeto.nombreMetodo( argumento1, argumento2... );               │
│      ▲                     ▲         ▲                  ▲                              │
│      │                     │         │                  └─ Parámetros que ENTRAN       │
│      │                     │         └─ Acción solicitada                              │
│      │                     └─ Referencia en el Stack que apunta al objeto en Heap      │
│      └─ Dato que SALE del método tras su ejecución                                     │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

##### A. Constructores con parámetros frente a constructores vacíos
Un **constructor** es un método especial de la clase cuya misión exclusiva es inicializar el estado del objeto en el momento exacto de su creación con `new`.
* **Constructor por defecto (sin argumentos):**  
  `Random r = new Random();`  
  Inicializa la semilla interna utilizando el reloj del procesador (`System.currentTimeMillis()`). Cada ejecución produce secuencias numéricas distintas.
* **Constructor con parámetros:**  
  `Random rAuditoria = new Random(123456789L);`  
  Pasa un valor inicial (`long`) para fijar la semilla en el Heap. Permite que el generador sea **completamente determinista y reproducible**, ideal para bancos de pruebas de software, auditorías y calibraciones de hardware.

##### B. Tipos de retorno y métodos de la clase `Random`
Un método es un bloque de instrucciones asociado a un objeto que realiza una tarea y puede (o no) devolver un valor al punto del programa donde fue invocado:

```
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                     MÉTODOS PREDEFINIDOS DE INSTANCIA EN RANDOM                        │
├──────────────────────────┬───────────────────┬─────────────────────────────────────────┤
│ Firma del método         │ Tipo de retorno   │ Rango de datos que produce              │
├──────────────────────────┼───────────────────┼─────────────────────────────────────────┤
│ nextInt(int bound)       │ int               │ Entero en el rango [0, bound - 1]       │
├──────────────────────────┼───────────────────┼─────────────────────────────────────────┤
│ nextDouble()             │ double            │ Decimal en el rango [0.0, 1.0)          │
├──────────────────────────┼───────────────────┼─────────────────────────────────────────┤
│ nextBoolean()            │ boolean           │ Devuelve de forma pseudoaleatoria       │
│                          │                   │ true o false con probabilidad del 50 %  │
└──────────────────────────┴───────────────────┴─────────────────────────────────────────┘
```

> **Regla de asignación estricta:** la variable receptora debe coincidir en tipo con el valor devuelto por el método. Si un método devuelve `boolean`, no podemos asignarlo a un `int`; si devuelve `double`, podemos guardarlo en `double` o forzar un estrechamiento explícito con *casting* a `(int)`.

---

#### 3. Evolución del caso guía a `ControlAccesoQR v1.3`

Ampliamos el programa incorporando la calibración técnica matutina del IES El Caminàs:
* Instanciamos un generador de calibración con semilla fija (`SEMILLA_CALIBRACION`).
* Generamos una pequeña fluctuación decimal con `nextDouble()` sobre la temperatura del vestíbulo.
* Simulamos el estado del sensor de respaldo NFC mediante `nextBoolean()`.

##### Paso A. Algoritmo en PSeInt (`pr/pseudocodigo/ControlAccesoQR.psc` — v1.3)

```psc
Algoritmo ControlAccesoQR
    // =========================================================================
    // SISTEMA DE CONTROL DE ASISTENCIA QR - IES EL CAMINAS (Castellon)
    // Version: 1.3 (Evolucion: Constructores con Parametros y Tipos de Retorno)
    // =========================================================================

    Definir NOMBRE_CENTRO, PREFIJO_CENTRO, RUTA_LOGS Como Cadena
    Definir MINUTOS_POR_HORA, SEGUNDOS_POR_HORA, SEGUNDOS_POR_MINUTO Como Entero
    Definir FACTOR_PORCENTAJE Como Real
    Definir SEMILLA_CALIBRACION Como Entero
    
    NOMBRE_CENTRO <- "IES El Caminas (Castellon)"
    PREFIJO_CENTRO <- "CAMINAS"
    RUTA_LOGS <- ".\caminas\\terminal\\logs"
    MINUTOS_POR_HORA <- 60
    SEGUNDOS_POR_HORA <- 3600
    SEGUNDOS_POR_MINUTO <- 60
    FACTOR_PORCENTAJE <- 100.0
    SEMILLA_CALIBRACION <- 987654321

    Definir terminalId Como Entero
    Definir tempVestibulo, fluctuacionTermica, tempCalibrada Como Real
    Definir nombrePersona, dniPersona Como Cadena
    Definir perfilPersona Como Caracter
    Definir esEntrada Como Logico
    
    Definir horaEntrada, minutoEntrada Como Entero
    Definir horaSalida, minutoSalida Como Entero
    Definir minutosTotalesEntrada, minutosTotalesSalida, minutosEstanciaTotal Como Entero
    Definir tokenResumenVestibulo, tokenResumenPuertaNorte Como Cadena
    
    Definir personasEnCentro, aforoDisponible, idUltimoFichaje Como Entero
    personasEnCentro <- 1200
    aforoDisponible <- 800
    idUltimoFichaje <- 1042
    
    Definir segundosActividadTerminal Como Entero
    Definir horasUptime, minutosUptime, segundosUptime Como Entero
    Definir aforoTotal Como Entero
    Definir porcentajeOcupacionReal Como Real
    Definir porcentajeOcupacionEntero Como Entero

    Definir codigoTokenVestibulo, codigoTokenPuertaNorte Como Entero
    
    // [NUEVO DÍA 15] Variables para retornos de calibración (código determinista, real y lógico)
    Definir codigoTestCalibracion Como Entero
    Definir sensorNfcOperativo Como Logico

    Escribir "AZAHARTECH: TERMINAL ", NOMBRE_CENTRO, " (v1.3 - Diagnostico y Calibracion)"
    Escribir "ID del terminal principal:"
    Leer terminalId
    Escribir "Temperatura base del sensor (ºC):"
    Leer tempVestibulo
    Escribir "Segundos de actividad del terminal (uptime):"
    Leer segundosActividadTerminal
    
    Escribir "DNI de la persona:"
    Leer dniPersona
    Escribir "Nombre completo:"
    Leer nombrePersona
    Escribir "Perfil de acceso (E = Estudiante, D = Docente, V = Visita):"
    Leer perfilPersona

    esEntrada <- Verdadero
    
    Escribir "Introduce hora y minuto de entrada (por ejemplo, 8 15):"
    Leer horaEntrada
    Leer minutoEntrada
    Escribir "Introduce hora y minuto de salida (por ejemplo, 14 10):"
    Leer horaSalida
    Leer minutoSalida
        
    minutosTotalesEntrada <- (horaEntrada * MINUTOS_POR_HORA) + minutoEntrada
    minutosTotalesSalida <- (horaSalida * MINUTOS_POR_HORA) + minutoSalida
    minutosEstanciaTotal <- minutosTotalesSalida - minutosTotalesEntrada
    
    personasEnCentro <- personasEnCentro + 1
    aforoDisponible <- aforoDisponible - 1
    idUltimoFichaje <- idUltimoFichaje + 1

    // Generación dinámica en producción (vestíbulo y puerta norte)
    codigoTokenVestibulo <- Aleatorio(1000, 9999)
    codigoTokenPuertaNorte <- Aleatorio(1000, 9999)
    
    // [NUEVO DÍA 15] Simulación de calibración técnica con retorno decimal y lógico
    codigoTestCalibracion <- 5555 // En Java procederá del generador con semilla fija
    fluctuacionTermica <- 0.45    // En Java: generador.nextDouble()
    tempCalibrada <- tempVestibulo + fluctuacionTermica
    sensorNfcOperativo <- Verdadero // En Java: generador.nextBoolean()
      
    tokenResumenVestibulo <- PREFIJO_CENTRO + "-" + dniPersona + "-T" + ConvertirATexto(terminalId) + "-SEC#" + ConvertirATexto(codigoTokenVestibulo) + "-REG" + ConvertirATexto(idUltimoFichaje)
    tokenResumenPuertaNorte <- PREFIJO_CENTRO + "-" + dniPersona + "-T102-SEC#" + ConvertirATexto(codigoTokenPuertaNorte) + "-REG" + ConvertirATexto(idUltimoFichaje)
    
    horasUptime <- trunc(segundosActividadTerminal / SEGUNDOS_POR_HORA)
    minutosUptime <- trunc((segundosActividadTerminal MOD SEGUNDOS_POR_HORA) / SEGUNDOS_POR_MINUTO)
    segundosUptime <- segundosActividadTerminal MOD SEGUNDOS_POR_MINUTO
    
    aforoTotal <- personasEnCentro + aforoDisponible
    porcentajeOcupacionReal <- (personasEnCentro * FACTOR_PORCENTAJE) / aforoTotal
    porcentajeOcupacionEntero <- trunc(porcentajeOcupacionReal)
    
    Escribir ""
    Escribir "======================================================================"
    Escribir "             INFORME OFICIAL DE ACCESO EN VESTIBULO                   "
    Escribir "======================================================================"
    Escribir "CENTRO:             ", NOMBRE_CENTRO
    Escribir "REGISTRO N. :       #", idUltimoFichaje
    Escribir "TOKEN VESTIBULO:    ", tokenResumenVestibulo
    Escribir "TOKEN PUERTA NORTE: ", tokenResumenPuertaNorte
    Escribir "PERSONA:            ", nombrePersona, " | PERFIL: ", perfilPersona
    Escribir "IDENTIFICADOR:      ", dniPersona, " | SENTIDO: Entrada (", esEntrada, ")"
    Escribir "----------------------------------------------------------------------"
    Escribir "HORARIO:            Entrada ", horaEntrada, ":", minutoEntrada, " | Salida ", horaSalida, ":", minutoSalida
    Escribir "PERMANENCIA:        ", minutosEstanciaTotal, " minutos en el centro."
    Escribir "AFORO (", aforoTotal, "):       ", redon(porcentajeOcupacionReal * 100) / 100, " % (Panel: ", porcentajeOcupacionEntero, " %)"
    Escribir "ACTIVO:             ", horasUptime, "h ", minutosUptime, "m ", segundosUptime, "s"
    Escribir "----------------------------------------------------------------------"
    Escribir "DIAGNOSTICO MATUTINO DE SENSORES (CALIBRACION CON SEMILLA):"
    Escribir "Codigo patron test: #", codigoTestCalibracion
    Escribir "Temp. calibrada:    ", tempCalibrada, " C (Fluctuacion: +", fluctuacionTermica, " C)"
    Escribir "Sensor NFC soporte: Activo (", sensorNfcOperativo, ")"
    Escribir "REGISTRO LOG:       ", RUTA_LOGS
    Escribir "======================================================================"
FinAlgoritmo
```

##### Paso B. Implementación en Java (`pr/src/ControlAccesoQR.java` — v1.3)

```java
/**
 * SISTEMA DE CONTROL DE ASISTENCIA POR CÓDIGO QR
 * Cliente: IES El Caminàs (Castellón de la Plana)
 * Consultora: AzaharTech Software Consulting
 *
 * Versión 1.3:
 * Constructores con parámetros (semilla fija), tipos de retorno (double, boolean)
 * y calibración diagnóstica de sensores de hardware.
 *
 * @author Equipo AzaharTech (Alba Torres, Pau Ferrer)
 * @version 1.3 (Octubre 2026)
 * @since JDK 21 LTS
 * 
 */

import java.util.Scanner;
import java.util.Random;

public class ControlAccesoQR {
    public static void main(String[] args) {
        // ---------------------------------------------------------------------
        // 1. CONSTANTES INMUTABLES DEL SISTEMA (Configuración corporativa)
        // ---------------------------------------------------------------------
        final String NOMBRE_CENTRO = "IES El Caminàs (Castellón)";
        final String PREFIJO_CENTRO = "CAMINAS";
        final String RUTA_LOGS = ".\\caminas\\terminal\\logs";
        final int MINUTOS_POR_HORA = 60;
        final int SEGUNDOS_POR_HORA = 3600;
        final int SEGUNDOS_POR_MINUTO = 60;
        final double FACTOR_PORCENTAJE = 100.0;
        final int BASE_TOKEN_SEGURIDAD = 1000;
        final int RANGO_TOKEN_SEGURIDAD = 9000;


        // [NUEVO DÍA 15] Semilla fija para pruebas de calibración repetibles
        final long SEMILLA_CALIBRACION = 987654321L;
        final double MAX_VARIACION_TERMICA = 0.5;

        // ---------------------------------------------------------------------
        // 2. DECLARACIÓN DE VARIABLES Y RESERVA DE OBJETOS EN MEMORIA
        // ---------------------------------------------------------------------
        Scanner teclado = new Scanner(System.in);

        // Generadores independientes para los dos monitores (Semana 1 / Día 14)
        Random generadorVestibulo   = new Random();
        Random generadorPuertaNorte = new Random();

        // [NUEVO DÍA 15] Constructor con parámetro: generador determinista para diagnóstico
        Random generadorCalibracion = new Random(SEMILLA_CALIBRACION);

        // Variables de hardware y terminal
        int terminalId;
        double tempVestibulo;

        // Variables de identidad y perfil de la persona
        String nombrePersona;
        String dniPersona;
        char perfilPersona;
        boolean esEntrada;

        // Variables de cómputo horario y estancia
        int horaEntrada, minutoEntrada;
        int horaSalida, minutoSalida;
        int minutosTotalesEntrada;
        int minutosTotalesSalida;
        int minutosEstanciaTotal;

        // Dos tokens independientes generados para cada monitor
        String tokenResumenVestibulo;
        String tokenResumenPuertaNorte;

        // Variables de aforo del recinto y numeración de ticket
        int personasEnCentro = 1200;
        int aforoDisponible = 800;
        int idUltimoFichaje = 1042;

        // Variables de diagnóstico y tiempo de servicio (uptime)
        int segundosActividadTerminal;
        int horasUptime;
        int minutosUptime;
        int segundosUptime;

        // Variables de aforo consolidado y porcentajes
        int aforoTotal;
        double porcentajeOcupacionReal;
        int porcentajeOcupacionEntero;

        // Códigos pseudoaleatorios independientes
        int codigoTokenVestibulo;
        int codigoTokenPuertaNorte;

        // [NUEVO DÍA 15] Variables para almacenar distintos tipos de retorno de métodos
        int codigoTestCalibracion;
        double fluctuacionTermica;
        double tempCalibrada;
        boolean sensorNfcOperativo;

        // ---------------------------------------------------------------------
        // 3. CAPTURA INTERACTIVA DE DATOS
        // ---------------------------------------------------------------------
        System.out.println("=================================================");
        System.out.println("   AZAHARTECH - TERMINAL " + NOMBRE_CENTRO);
        System.out.println("   Versión 1.3 (diagnóstico y retornos de objetos) ");
        System.out.println("=================================================");
        System.out.print("ID del terminal principal: ");
        terminalId = teclado.nextInt();

        System.out.print("Temperatura del sensor (ºC): ");
        tempVestibulo = teclado.nextDouble();

        System.out.print("Segundos de actividad del terminal (uptime): ");
        segundosActividadTerminal = teclado.nextInt();

        teclado.nextLine(); // Limpieza obligatoria del buffer de teclado

        System.out.print("DNI de la persona: ");
        dniPersona = teclado.nextLine();

        System.out.print("Nombre completo: ");
        nombrePersona = teclado.nextLine();

        System.out.print("Perfil de acceso (E = Estudiante, D = Docente, V = Visita): ");
        perfilPersona = teclado.next().charAt(0);

        esEntrada = true;

        System.out.print("Hora de entrada (0-23): ");
        horaEntrada = teclado.nextInt();

        System.out.print("Minuto de entrada (0-59): ");
        minutoEntrada = teclado.nextInt();

        System.out.print("Hora de salida (0-23): ");
        horaSalida = teclado.nextInt();

        System.out.print("Minuto de salida (0-59): ");
        minutoSalida = teclado.nextInt();

        // ---------------------------------------------------------------------
        // 4. PROCESAMIENTO SECUENCIAL Y OPERACIONES MATEMÁTICAS
        // ---------------------------------------------------------------------
        // Conversión horaria a minutos y cálculo de permanencia neta
        minutosTotalesEntrada = (horaEntrada * MINUTOS_POR_HORA) + minutoEntrada;
        minutosTotalesSalida = (horaSalida * MINUTOS_POR_HORA) + minutoSalida;
        minutosEstanciaTotal = minutosTotalesSalida - minutosTotalesEntrada;

        // Operadores unarios de incremento y decremento de estado
        personasEnCentro++;
        aforoDisponible--;
        idUltimoFichaje++;

        // Generación dinámica de tokens para los dos monitores autónomos
        codigoTokenVestibulo   = BASE_TOKEN_SEGURIDAD + generadorVestibulo.nextInt(RANGO_TOKEN_SEGURIDAD);
        codigoTokenPuertaNorte = BASE_TOKEN_SEGURIDAD + generadorPuertaNorte.nextInt(RANGO_TOKEN_SEGURIDAD);

        // [NUEVO DÍA 15] Invocación de métodos con distintos tipos de retorno:
        // 1. Método con retorno entero (int): código patrón reproducible
        codigoTestCalibracion = BASE_TOKEN_SEGURIDAD + generadorCalibracion.nextInt(RANGO_TOKEN_SEGURIDAD);

        // 2. Método con retorno decimal (double): devuelve un valor entre [0.0 y 1.0)
        fluctuacionTermica = generadorCalibracion.nextDouble() * MAX_VARIACION_TERMICA; // Escala la variación a un máximo de 0.5 ºC
        tempCalibrada = tempVestibulo + fluctuacionTermica;

        // 3. Método con retorno booleano (boolean): simula el estado del sensor de respaldo
        sensorNfcOperativo = generadorCalibracion.nextBoolean();

        // Composición acumulativa de ambos tokens
        tokenResumenVestibulo = PREFIJO_CENTRO + "-" + dniPersona;
        tokenResumenVestibulo += "-T" + terminalId;
        tokenResumenVestibulo += "-SEC#" + codigoTokenVestibulo;
        tokenResumenVestibulo += "-ESTANCIA-" + minutosEstanciaTotal;
        tokenResumenVestibulo += "-REG" + idUltimoFichaje;

        tokenResumenPuertaNorte = PREFIJO_CENTRO + "-" + dniPersona;
        tokenResumenPuertaNorte += "-T102"; // ID fijo asignado al monitor norte
        tokenResumenPuertaNorte += "-SEC#" + codigoTokenPuertaNorte;
        tokenResumenPuertaNorte += "-ESTANCIA-" + minutosEstanciaTotal;
        tokenResumenPuertaNorte += "-REG" + idUltimoFichaje;

        // Descomposición temporal exacta mediante división entera y módulo (%)
        horasUptime = segundosActividadTerminal / SEGUNDOS_POR_HORA;
        minutosUptime = (segundosActividadTerminal % SEGUNDOS_POR_HORA) / SEGUNDOS_POR_MINUTO;
        segundosUptime = segundosActividadTerminal % SEGUNDOS_POR_MINUTO;

        // Cálculo de porcentaje con casting explícito a (double) para evitar división a 0
        aforoTotal = personasEnCentro + aforoDisponible;
        porcentajeOcupacionReal = ((double) personasEnCentro / aforoTotal) * FACTOR_PORCENTAJE;
        porcentajeOcupacionEntero = (int) porcentajeOcupacionReal;

        // ---------------------------------------------------------------------
        // 5. SALIDA FORMATEADA PROFESIONAL (System.out.printf)
        // ---------------------------------------------------------------------
        System.out.println("\n======================================================================");
        System.out.println("             INFORME OFICIAL DE ACCESO EN VESTÍBULO                   ");
        System.out.println("======================================================================");
        System.out.printf("CENTRO:               %-30s%n", NOMBRE_CENTRO);
        System.out.printf("REGISTRO N.º:         #%05d%n", idUltimoFichaje);
        System.out.printf("TOKEN QR VESTÍBULO:   %s%n", tokenResumenVestibulo);
        System.out.printf("TOKEN QR PUERTA NORTE:%s (Monitor autónomo)%n", tokenResumenPuertaNorte);
        System.out.printf("PERSONA:              %-30s | PERFIL: %c%n", nombrePersona, perfilPersona);
        System.out.printf("IDENTIFICADOR:        %-12s | SENTIDO: Entrada (%b)%n", dniPersona, esEntrada);
        System.out.println("----------------------------------------------------------------------");
        System.out.printf("HORARIO:              Entrada %02d:%02d | Salida %02d:%02d%n", horaEntrada, minutoEntrada, horaSalida, minutoSalida);
        System.out.printf("PERMANENCIA:          %03d minutos en el centro.%n", minutosEstanciaTotal);
        System.out.printf("AFORO (%04d):         %6.2f %% (Panel: %03d %%)%n", aforoTotal, porcentajeOcupacionReal, porcentajeOcupacionEntero);
        System.out.printf("ACTIVO:               %02dh %02dm %02ds%n", horasUptime, minutosUptime, segundosUptime);
        System.out.println("----------------------------------------------------------------------");
        System.out.println("DIAGNÓSTICO TÉCNICO DE SENSORES (CALIBRACIÓN CON SEMILLA):");
        System.out.printf("CÓDIGO PATRÓN TEST:   #%04d (Secuencia verificada)%n", codigoTestCalibracion);
        System.out.printf("SENSOR TÉRMICO CALIB: %.2f ºC (Fluctuación calculada: +%.2f ºC)%n", tempCalibrada, fluctuacionTermica);
        System.out.printf("SENSOR NFC RESPALDO:  Activo (%b)%n", sensorNfcOperativo);
        System.out.printf("REGISTRO LOG:         %s%n", RUTA_LOGS);
        System.out.println("======================================================================");

        teclado.close();
    }
}
```

---

#### 4. Segunda sesión: dojo de entrenamiento y katas de código

Pasamos al dojo de entrenamiento para programar, con tres **katas de código** centradas en constructores especializados, firmas de métodos y manipulación de diferentes tipos de retorno:

---

##### Kata 1 (Cinturón blanco / Nivel base). Métodos con retorno `double` y `boolean` en tu proyecto
* **Objetivo:** Invocar sobre un objeto predefinido al menos un método que devuelva un valor decimal (`nextDouble()`) y otro que devuelva un valor lógico (`nextBoolean()`) adaptados al dominio de tu proyecto propio.
* **Instrucciones:** Abre tu archivo `MiProyecto.java` (en versión v1.2) e incorpora las siguientes llamadas:
    * En **Aventura conversacional**: invocar `nextBoolean()` para determinar si un cofre encontrado contiene trampa y `nextDouble()` para calcular un factor multiplicador de impacto crítico entre `1.0` y `1.5`.
    * En **Motor de recomendación**: invocar `nextDouble()` para simular un factor de afinidad normalizado entre `0.0` y `1.0`, y `nextBoolean()` para indicar si el usuario ya ha consumido previamente el ítem sugerido.
    * En **Simulador de físicas 2D**: invocar `nextDouble()` para generar un coeficiente de elasticidad superficial aleatorio entre `0.0` y `1.0`, y `nextBoolean()` para establecer el sentido vectorial del viento (+ / -).
    * En **Bóveda de contraseñas**: invocar `nextBoolean()` para simular si la credencial requiere obligatoriamente doble factor de autenticación (2FA), y `nextDouble()` para calcular un índice ponderado de entropía de la clave.

---

##### Kata 2 (Cinturón marrón / Nivel avanzado). La fábrica de números reales escalados
* **Contexto técnico:** El método `random.nextDouble()` siempre devuelve un valor en el intervalo semiabierto $[0.0, 1.0)$.
* **Desafío de la kata:** Diseña una fórmula matemática secuencial en una sola línea que utilice `random.nextDouble()` para generar un número decimal pseudoaleatorio acotado exactamente en un rango real $[MIN, MAX)$ arbitrario (por ejemplo, temperaturas entre $18.5$ y $26.5$ ºC):
  $$\text{ResultadoDecimal} = MIN + (\text{random.nextDouble}() \times (MAX - MIN))$$
* **Evidencia de maestría:** Prueba tu fórmula en una clase de test con $MIN = 12.5$ y $MAX = 18.0$. Ejecútala 5 veces y formatea la salida mediante `System.out.printf("Valor generado: %.3f%n", resultadoDecimal)`. Comprueba que ningún valor queda por debajo de 12.5 ni alcanza los 18.0.

---

##### Kata 3 (Cinturón negro / «Hacker AzaharTech»). El determinismo de la semilla en auditoría de software
* **Contexto de ingeniería:** En auditorías de ciberseguridad y simulaciones científicas, los experimentos deben ser reproducibles al 100 %. Si dos peritos informáticos ejecutan el software, deben obtener exactamente los mismos resultados.
* **Misión de la kata:**
    1. Crea una clase de pruebas llamada `AuditoriaSemilla.java`.
    2. Instancia un objeto `Random` fijando una semilla concreta: `new Random(42L)`.
    3. Genera y muestra consecutivamente: un entero con `nextInt(100)`, un decimal con `nextDouble()` y un booleano con `nextBoolean()`.
    4. Ejecuta el programa una vez. Copia los tres resultados en tu libreta.
    5. Ejecuta el programa por segunda y tercera vez.
    6. **Pregunta técnica para tu cuaderno:** ¿Por qué los números son rigurosamente idénticos en cada ejecución si estamos usando una clase llamada «Random»? Escribe dos líneas de comentario explicando la diferencia entre **aleatoriedad física pura** y **generación pseudoaleatoria determinista**.

---

### Día 16 - 2 sesiones

---

#### 1. Caso guía en AzaharTech
Son las tres de la tarde del jueves 8 de octubre. Llegamos a la última sesión semanal de Programación (mañana viernes es festivo del 9 de octubre). En la sala técnica de **AzaharTech**, **Pau Ferrer** muestra una duda técnica a **Alba Torres** y a **Laia Claramunt**:

> *«El martes me explicasteis que al hacer `new Random()` dos veces se crean dos generadores independientes en el Heap, y que al hacer `g2 = g1` solo se copia la dirección de memoria creando un alias. Lo entendí en la teoría, pero ¿cómo puedo comprobarlo yo mismo dentro de mi código Java sin herramientas externas?»*.

**Alba Torres** abre la consola de IntelliJ:
> *«Java nos ofrece los mecanismos en el propio lenguaje. Si le pides a Java que imprima una variable de referencia por consola, el compilador imprime el nombre de la clase seguido de una arroba (`@`) y un código alfanumérico en hexadecimal: es la **huella de identidad de ese objeto en la memoria física**.*
>
> *Además, cuando comparamos dos objetos con el operador **`==`**, Java no compara lo que hay dentro del objeto; **compara si ambas variables apuntan a la misma dirección de memoria en el Heap**.*
>
> *Hoy cerraremos la primera semana consolidando la versión **v1.3** de vuestro proyecto propio y utilizaremos la propia consola para auditar identidades de memoria y referencias nulas»*.

---

#### 2. Fundamento teórico: identidad de objetos, operador `==` e impresión de referencias

```
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                        IDENTIDAD DE OBJETOS EN LA MEMORIA RAM                          │
├───────────────────────────────────┬────────────────────────────────────────────────────┤
│ 1. IMPRESIÓN POR DEFECTO          │ 2. COMPARACIÓN CON EL OPERADOR '=='                │
├───────────────────────────────────┼────────────────────────────────────────────────────┤
│ Random r = new Random();          │ Random a = new Random();                           │
│ System.out.println(r);            │ Random b = new Random();                           │
│                                   │ Random c = a;                                      │
│ Salida: java.util.Random@7ffe8a1  │ • (a == b) devuelve FALSE (dos objetos en Heap).   │
│         └──────────────┘ └──────┘ │ • (a == c) devuelve TRUE (mismo objeto en Heap).   │
│              Clase       Dirección│                                                    │
└───────────────────────────────────┴────────────────────────────────────────────────────┘
```

##### A. La huella de un objeto en memoria: `NombreClase@hash`
Cuando le pasas un objeto a `System.out.println()`, Java invoca internamente a su método de representación textual (`toString()`). Por defecto, para cualquier objeto estándar de la biblioteca que no redefina este texto, imprime:
$$\text{NombreDelPaquete.NombreClase} + \text{"@"} + \text{CódigoHexadecimalDeMemoria}$$
* Si dos variables imprimen códigos distintos (*ej. `@7ffe8a1` y `@1a2b3c`*), tenemos la certeza matemática de que residen en dos posiciones de memoria independientes del Heap.
* Si imprimen el mismo código, son dos punteros hacia la misma instancia física.

##### B. El operador `==` en tipos referencia
A diferencia de los tipos primitivos (donde `5 == 5` compara valores numéricos), en los objetos el operador **`==` compara direcciones de memoria**:
* Evalúa si ambas referencias del Stack apuntan al **mismo bloque del Heap**.
* No compara si los generadores están configurados igual o si tienen la misma semilla; solo compara si **son físicamente el mismo objeto**.

##### C. El literal `null` frente a una referencia activa
* Si imprimimos por consola una variable que vale `null`:
  ```java
  Random generadorVacio = null;
  System.out.println(generadorVacio); // Imprime el texto literal: "null"
  ```
La JVM no falla al imprimirlo; simplemente avisa de que el puntero está vacío.
* El fallo catastrófico ocurre al intentar acceder a través de él:
  ```java
  generadorVacio.nextInt(); // ¡CRASH!: NullPointerException en tiempo de ejecución
  ```

---

#### 3. Verificación de identidad en `ControlAccesoQR v1.3`

Vamos a ver cómo auditar las referencias del caso guía del IES El Caminàs utilizando únicamente instrucciones de consola:

```java
// Comprobación de identidad de los generadores de vestíbulo y puerta norte
System.out.println("\n----------------------------------------------------------------------");
System.out.println("AUDITORÍA DE IDENTIDAD EN MEMORIA RAM (HEAP):");
System.out.println("Huella monitor vestíbulo:    " + generadorVestibulo);
System.out.println("Huella monitor puerta norte: " + generadorPuertaNorte);

// Verificación con el operador ==
boolean sonElMismoObjeto = (generadorVestibulo == generadorPuertaNorte);
System.out.println("¿Apuntan al mismo espacio en memoria?: " + sonElMismoObjeto); // Devuelve false
```

* Puede comprobarse en la salida de consola que ambas huellas (`Random@...`) son diferentes y que la comparación devuelve `false`, garantizando que cada monitor tiene su propio ciclo de vida.

---

#### 4. Segunda sesión: dojo de entrenamiento y katas de código

Pasamos al dojo de entrenamiento para programar, con tres **katas de consolidación técnica** sobre identidad de objetos y tipos de retorno:

---

##### Kata 1 (Cinturón blanco / Nivel base). Consolidación v1.3 en el proyecto propio
* **Objetivo:** Asegurar que tu clase maestra `MiProyecto.java` incorpora todas las novedades de la primera semana del Sprint 2:
    1. Al menos dos instancias independientes creadas con `new Random()` para propósitos diferenciados.
    2. Una tercera instancia creada con constructor parametrizado (`new Random(semillaFija)`) para pruebas repetibles, almacenando la semilla en una constante `final long`.
    3. Invocación de métodos que retornen `int` (`nextInt`), `double` (`nextDouble`) y `boolean` (`nextBoolean`).
* **Aplicación según tu proyecto elegido:**
    * En **Aventura conversacional**: dos generadores independientes para las tiradas de héroe y enemigo, más un generador de calibración con semilla para reproducir siempre la misma mazmorra de pruebas.
    * En **Motor de recomendación**: dos generadores para variaciones de catálogo y afinidad, más un generador determinista para auditorías del algoritmo de sugerencias.
    * En **Simulador de físicas 2D**: dos generadores para velocidades y ángulos de partículas, más un generador con semilla para reproducir la misma trayectoria balística de test.
    * En **Bóveda de contraseñas**: dos generadores para PIN y salting dinámicos, más un generador con semilla fija para auditorías forenses repetibles.

---

##### Kata 2 (Cinturón marrón / Nivel avanzado). La prueba de la huella de memoria y el operador `==`
* **Contexto técnico:** Demostrar mediante código por consola la diferencia entre alias y objetos independientes.
* **Misión de la kata:**
    1. En una clase de prueba llamada `AuditoriaIdentidad.java`, escribe el siguiente experimento:
       ```java
       Random motorOriginal = new Random();
       Random motorAlias = motorOriginal;   // Copia de referencia (puntero)
       Random motorClon = new Random();     // Nuevo objeto con new
  
       System.out.println("Huella motorOriginal: " + motorOriginal);
       System.out.println("Huella motorAlias:    " + motorAlias);
       System.out.println("Huella motorClon:     " + motorClon);
  
       boolean pruebaAlias = (motorOriginal == motorAlias);
       boolean pruebaClon  = (motorOriginal == motorClon);
  
       System.out.println("¿Original y Alias son el mismo objeto?: " + pruebaAlias);
       System.out.println("¿Original y Clon son el mismo objeto?:  " + pruebaClon);
       ```
    2. Ejecuta el programa y analiza la salida.
    3. **Pregunta técnica para tu cuaderno:**  
       Explica en dos líneas por qué `pruebaAlias` da `true` y `pruebaClon` da `false`, haciendo referencia al Stack y al Heap.

---

##### Kata 3 (Cinturón negro / «Hacker AzaharTech»). Comportamiento de `null` en consola
* **Contexto de robustez:** Conocer cómo reacciona Java ante variables no inicializadas o desconectadas.
* **Misión de la kata:**
    1. En la misma clase de prueba, añade las siguientes líneas al final:
       ```java
       Random motorDesconectado = null;
  
       // 1. Impresión directa de la referencia nula:
       System.out.println("Estado de motorDesconectado: " + motorDesconectado);
  
       // 2. Operación de comparación segura:
       boolean estaInicializado = (motorDesconectado != null);
       System.out.println("¿El objeto está listo para usarse?: " + estaInicializado);
       ```
    2. Comprueba que el programa compila y ejecuta limpiamente imprimiendo `null` y `false`.
    3. Ahora escribe deliberadamente: `motorDesconectado.nextInt();` y observa el mensaje de error `NullPointerException` en la consola.
    4. Elimina la línea conflictiva para dejar el código limpio y libre de errores.

---

#### 5. Cierre formal en Git y sincronización en IntelliJ
Cada estudiante finaliza y entrega el avance de la primera semana del Sprint 2 desde la interfaz gráfica de IntelliJ IDEA:

1. Aplica el autoformateo oficial: `Ctrl + Alt + L`.
2. Optimiza las importaciones para limpiar dependencias no usadas: `Ctrl + Alt + O`.
3. Abre el panel lateral **Commit** (`Alt + 0` o `Ctrl + K`).
4. Selecciona tu archivo `pr/src/MiProyecto.java` (y su diseño en PSeInt si procede).
5. Escribe el mensaje convencional de cierre de semana:
   ```text
   feat(pr): consolidar version 1.3 con gestion de objetos, semillas y verificacion de memoria
   ```
6. Pulsa **Commit and Push...** y confirma el envío al servidor remoto de GitHub.
7. Comprueba en el navegador que tu repositorio muestra la versión v1.3 actualizada sin errores de compilación.

Tiene todo el sentido del mundo: en un centro educativo no solo entran estudiantes, sino también docentes y personal de administración y servicios (PAS), cuya jornada laboral de referencia es de **8 horas diarias (480 minutos)**.

A continuación tienes el **Día 17 completo actualizado**, sustituyendo los 360 minutos (6 h) por los **480 minutos (8 h)** tanto en la teoría como en PSeInt y Java:

---

## Semana 5. Métodos estáticos, clase Math y clases envoltorio (wrappers)
Tras asentar el funcionamiento de la memoria dinámica en el montículo (Heap) durante la primera semana, 
el equipo de desarrollo de AzaharTech aborda en esta segunda semana la frontera entre los métodos ligados 
a objetos vivos y los métodos estáticos (de clase), que ofrecen utilidades de cálculo directo sin necesidad 
de reservar memoria con el operador new.  

A lo largo de estas sesiones nos adentraremos en el paquete java.lang 
para exprimir la clase Math (acotando lecturas de sensores con Math.min y Math.max, calculando desviaciones 
horarias respecto a la jornada de 8 horas con Math.abs y aplicando potencias y redondeos matemáticos), al tiempo
que descubriremos el papel de las clases envoltorio (Wrappers) como Integer y Double para parsear texto a
magnitudes numéricas.  

A través de nuestro dojo de katas diarias y la evolución continua del código, dotaremos 
a nuestro software de precisión, elegancia y robustez matemática, aprendiendo a elegir la herramienta idónea 
para cada necesidad de ingeniería.
---

### Día 17 -  2 sesiones

---

#### 1. Caso guía en AzaharTech
Son las tres de la tarde del martes 13 de octubre (ayer lunes fue festivo nacional del 12 de octubre). En la sala técnica de **AzaharTech**, **Pau Ferrer** tiene abierta la consola de IntelliJ IDEA con cara de desconcierto.

Ha intentado calcular la diferencia absoluta de tiempo y acotar las lecturas del sensor de temperatura del **IES El Caminàs**, y ha escrito en el código:

```java
Math calculadora = new Math(); // Error de compilación inmediato
```

IntelliJ subraya la instrucción en rojo con un mensaje tajante: *«'Math()' has private access in 'java.lang.Math'»*.

Pau comenta al equipo:
> *«La semana pasada aprendimos que para usar una clase predefinida como `Random` o `Scanner` teníamos que usar `new` para crear el objeto en el Heap. He intentado hacer `new Math()` para calcular el valor absoluto y el máximo de aforo, y el compilador me dice que el constructor es privado y que no puedo instanciarla. ¿Por qué con `Random` sí y con `Math` no?»*.

**Alba Torres** y **Laia Claramunt** sonríen. Alba toma el teclado:
> *«Pau, acabas de descubrir la gran frontera de la POO: la diferencia entre **métodos de instancia** y **métodos estáticos (*de clase*)**.*
>
> *Un generador `Random` necesita almacenar un estado interno en el Heap (su semilla) que cambia con cada tirada; por eso requiere un objeto individual creado con `new`. Pero calcular un valor absoluto, un valor máximo o el redondeo de un número no requiere que la máquina recuerde nada de la operación anterior: es una función matemática pura.*
>
> *La biblioteca estándar de Java incluye la clase **`java.lang.Math`**, repleta de **métodos estáticos**: herramientas que se invocan directamente desde el nombre de la clase, sin instanciar objetos con `new`. Hoy aprenderemos a utilizarlos para acotar datos y calcular desviaciones de tiempo de forma puramente secuencial»*.

---

#### 2. Fundamento teórico: métodos de instancia frente a métodos estáticos y la clase `Math`

```
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                        MÉTODOS DE INSTANCIA VS. MÉTODOS ESTÁTICOS                      │
├───────────────────────────────────┬────────────────────────────────────────────────────┤
│ 1. MÉTODOS DE INSTANCIA           │ 2. MÉTODOS ESTÁTICOS (De clase)                    │
├───────────────────────────────────┼────────────────────────────────────────────────────┤
│ • Requieren crear un objeto con   │ • NO requieren instanciar objetos con 'new'.       │
│   el operador 'new' en el Heap.   │ • Pertenecen a la clase en sí, no a una instancia. │
│ • Operan sobre el estado interno  │ • Son funciones de cálculo puro y utilidades que   │
│   del objeto concreto.            │   reciben datos y devuelven un resultado.          │
│ • Invocación: objeto.metodo()     │ • Invocación: NombreClase.metodo()                 │
│   ej. generador.nextInt(9000);    │   ej. Math.max(a, b);                              │
└───────────────────────────────────┴────────────────────────────────────────────────────┘
```

##### A. El paquete universal `java.lang`
A diferencia de `Scanner` o `Random` (que exigen escribir `import java.util...` al principio del archivo), la clase `Math` reside en el paquete **`java.lang`**. Este paquete contiene las clases estructurales de Java (`System`, `String`, `Math`, `Integer`) y **la JVM lo importa de forma automática y transparente en todos los archivos**, por lo que no requiere ninguna sentencia `import`.

##### B. Métodos esenciales de `java.lang.Math`
Todos los métodos de la clase `Math` son estáticos, públicos y reciben argumentos numéricos:

```
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                        CATÁLOGO BÁSICO DE LA CLASE MATH                                │
├──────────────────────────┬───────────────────┬─────────────────────────────────────────┤
│ Invocación estática      │ Tipo de retorno   │ Efecto técnico                          │
├──────────────────────────┼───────────────────┼─────────────────────────────────────────┤
│ Math.abs(x)              │ Mismo tipo que x  │ Devuelve el valor absoluto (sin signo). │
├──────────────────────────┼───────────────────┼─────────────────────────────────────────┤
│ Math.max(a, b)           │ Mayor de los dos  │ Devuelve el valor más alto entre a y b. │
├──────────────────────────┼───────────────────┼─────────────────────────────────────────┤
│ Math.min(a, b)           │ Menor de los dos  │ Devuelve el valor más bajo entre a y b. │
├──────────────────────────┼───────────────────┼─────────────────────────────────────────┤
│ Constante Math.PI        │ double            │ Constante pública: 3.141592653589793    │
└──────────────────────────┴───────────────────┴─────────────────────────────────────────┘
```

##### C. La técnica del acotamiento en rango (*clamp*) sin usar condicionales
En sistemas embebidos y terminales físicos, los sensores pueden registrar picos de lectura erróneos debido a interferencias eléctricas (por ejemplo, que el sensor del vestíbulo marque $55.0$ ºC o $-10.0$ ºC).

Combinando **`Math.max()`** y **`Math.min()`**, podemos acotar cualquier lectura dentro de un rango de seguridad $[MIN, MAX]$ en una sola línea de código secuencial, **sin necesidad de recurrir a estructuras condicionales `if`**:

$$\text{ValorAcotado} = \text{Math.max}(MIN, \text{Math.min}(\text{lecturaBruta}, MAX))$$

* Si `lecturaBruta` supera el $MAX$, `Math.min` la recorta a $MAX$.
* Si `lecturaBruta` desciende por debajo de $MIN$, `Math.max` la eleva a $MIN$.

---

#### 3. Evolución del caso guía a `ControlAccesoQR v1.4`

Ampliamos el programa incorporando el uso de métodos estáticos de `Math`:
1. Definimos una **jornada laboral/centro de referencia de 480 minutos (8 horas)**, representativa del personal docente y PAS.
2. Calculamos con **`Math.abs()`** la desviación absoluta en minutos que la estancia de la persona representa respecto a esa jornada de 8 horas.
3. Acotamos la lectura térmica del sensor mediante **`Math.max()` y `Math.min()`** para que el valor visualizado en pantalla nunca baje de $15.0$ ºC ni supere los $35.0$ ºC.
4. Acotamos el aforo con **`Math.min()`** para garantizar que el panel nunca muestre más personas que la capacidad máxima física del centro (2000 plazas).

##### Paso A. Algoritmo en PSeInt (`pr/pseudocodigo/ControlAccesoQR.psc` — v1.4)

```psc
Algoritmo ControlAccesoQR
    // =========================================================================
    // SISTEMA DE CONTROL DE ASISTENCIA QR - IES EL CAMINAS (Castellon)
    // Version: 1.4 (Evolucion: metodos estaticos y funciones de acotacion)
    // =========================================================================

    Definir NOMBRE_CENTRO, PREFIJO_CENTRO, RUTA_LOGS Como Cadena
    Definir MINUTOS_POR_HORA, SEGUNDOS_POR_HORA, SEGUNDOS_POR_MINUTO Como Entero
    Definir FACTOR_PORCENTAJE Como Real
    Definir SEMILLA_CALIBRACION Como Entero
    
    // [NUEVO DÍA 17] Constantes para límites de seguridad y jornada de referencia de 8 horas
    Definir JORNADA_BASE_MINUTOS Como Entero
    Definir TEMP_MIN_SEGURA, TEMP_MAX_SEGURA Como Real
    
    NOMBRE_CENTRO <- "IES El Caminas (Castellon)"
    PREFIJO_CENTRO <- "CAMINAS"
    RUTA_LOGS <- ".\\caminas\\terminal\\logs"
    MINUTOS_POR_HORA <- 60
    SEGUNDOS_POR_HORA <- 3600
    SEGUNDOS_POR_MINUTO <- 60
    FACTOR_PORCENTAJE <- 100.0
    SEMILLA_CALIBRACION <- 987654321
    JORNADA_BASE_MINUTOS <- 480 // 8 horas de jornada laboral/centro de referencia
    TEMP_MIN_SEGURA <- 15.0
    TEMP_MAX_SEGURA <- 35.0

    Definir terminalId Como Entero
    Definir tempVestibulo, fluctuacionTermica, tempCalibrada, tempSeguraAcotada Como Real
    Definir nombrePersona, dniPersona Como Cadena
    Definir perfilPersona Como Caracter
    Definir esEntrada Como Logico
    
    Definir horaEntrada, minutoEntrada Como Entero
    Definir horaSalida, minutoSalida Como Entero
    Definir minutosTotalesEntrada, minutosTotalesSalida, minutosEstanciaTotal Como Entero
    Definir tokenResumenVestibulo, tokenResumenPuertaNorte Como Cadena
    
    Definir personasEnCentro, aforoDisponible, idUltimoFichaje Como Entero
    personasEnCentro <- 1200
    aforoDisponible <- 800
    idUltimoFichaje <- 1042
    
    Definir segundosActividadTerminal Como Entero
    Definir horasUptime, minutosUptime, segundosUptime Como Entero
    Definir aforoTotal Como Entero
    Definir porcentajeOcupacionReal Como Real
    Definir porcentajeOcupacionEntero Como Entero

    Definir codigoTokenVestibulo, codigoTokenPuertaNorte Como Entero
    Definir codigoTestCalibracion Como Entero
    Definir sensorNfcOperativo Como Logico

    // [NUEVO DÍA 17] Variables para cálculos matemáticos con funciones abs, max y min
    Definir desviacionJornadaMinutos Como Entero
    Definir aforoSeguroVisualizado Como Entero

    Escribir "AZAHARTECH: TERMINAL ", NOMBRE_CENTRO, " (v1.4 - Funciones Matematicas)"
    Escribir "ID del terminal principal:"
    Leer terminalId
    Escribir "Temperatura base del sensor (ºC):"
    Leer tempVestibulo
    Escribir "Segundos de actividad del terminal (uptime):"
    Leer segundosActividadTerminal
    
    Escribir "DNI de la persona:"
    Leer dniPersona
    Escribir "Nombre completo:"
    Leer nombrePersona
    Escribir "Perfil de acceso (E = Estudiante, D = Docente, V = Visita):"
    Leer perfilPersona

    esEntrada <- Verdadero
    
    Escribir "Introduce hora y minuto de entrada (por ejemplo, 8 15):"
    Leer horaEntrada
    Leer minutoEntrada
    Escribir "Introduce hora y minuto de salida (por ejemplo, 14 10):"
    Leer horaSalida
    Leer minutoSalida
        
    minutosTotalesEntrada <- (horaEntrada * MINUTOS_POR_HORA) + minutoEntrada
    minutosTotalesSalida <- (horaSalida * MINUTOS_POR_HORA) + minutoSalida
    minutosEstanciaTotal <- minutosTotalesSalida - minutosTotalesEntrada
    
    personasEnCentro <- personasEnCentro + 1
    aforoDisponible <- aforoDisponible - 1
    idUltimoFichaje <- idUltimoFichaje + 1

    codigoTokenVestibulo <- Aleatorio(1000, 9999)
    codigoTokenPuertaNorte <- Aleatorio(1000, 9999)
    
    codigoTestCalibracion <- 5555
    fluctuacionTermica <- 0.45
    tempCalibrada <- tempVestibulo + fluctuacionTermica
    sensorNfcOperativo <- Verdadero
    
    // [NUEVO DÍA 17] Cálculo de desviación absoluta respecto a 8 horas (480 min)
    desviacionJornadaMinutos <- abs(minutosEstanciaTotal - JORNADA_BASE_MINUTOS)
    
    // Simulación de acotación en PSeInt:
    Si tempCalibrada > TEMP_MAX_SEGURA Entonces
        tempSeguraAcotada <- TEMP_MAX_SEGURA
    SiNo
        Si tempCalibrada < TEMP_MIN_SEGURA Entonces
            tempSeguraAcotada <- TEMP_MIN_SEGURA
        SiNo
            tempSeguraAcotada <- tempCalibrada
        FinSi
    FinSi
      
    tokenResumenVestibulo <- PREFIJO_CENTRO + "-" + dniPersona + "-T" + ConvertirATexto(terminalId) + "-SEC#" + ConvertirATexto(codigoTokenVestibulo) + "-REG" + ConvertirATexto(idUltimoFichaje)
    tokenResumenPuertaNorte <- PREFIJO_CENTRO + "-" + dniPersona + "-T102-SEC#" + ConvertirATexto(codigoTokenPuertaNorte) + "-REG" + ConvertirATexto(idUltimoFichaje)
    
    horasUptime <- trunc(segundosActividadTerminal / SEGUNDOS_POR_HORA)
    minutosUptime <- trunc((segundosActividadTerminal MOD SEGUNDOS_POR_HORA) / SEGUNDOS_POR_MINUTO)
    segundosUptime <- segundosActividadTerminal MOD SEGUNDOS_POR_MINUTO
    
    aforoTotal <- personasEnCentro + aforoDisponible
    porcentajeOcupacionReal <- (personasEnCentro * FACTOR_PORCENTAJE) / aforoTotal
    porcentajeOcupacionEntero <- trunc(porcentajeOcupacionReal)
    
    Escribir ""
    Escribir "======================================================================"
    Escribir "             INFORME OFICIAL DE ACCESO EN VESTIBULO                   "
    Escribir "======================================================================"
    Escribir "CENTRO:             ", NOMBRE_CENTRO
    Escribir "REGISTRO N. :       #", idUltimoFichaje
    Escribir "TOKEN VESTIBULO:    ", tokenResumenVestibulo
    Escribir "TOKEN PUERTA NORTE: ", tokenResumenPuertaNorte
    Escribir "PERSONA:            ", nombrePersona, " | PERFIL: ", perfilPersona
    Escribir "IDENTIFICADOR:      ", dniPersona, " | SENTIDO: Entrada (", esEntrada, ")"
    Escribir "----------------------------------------------------------------------"
    Escribir "HORARIO:            Entrada ", horaEntrada, ":", minutoEntrada, " | Salida ", horaSalida, ":", minutoSalida
    Escribir "PERMANENCIA:        ", minutosEstanciaTotal, " minutos en el centro."
    Escribir "DESVIACION JORNADA: ", desviacionJornadaMinutos, " min respecto a jornada completa (480 min)."
    Escribir "AFORO (", aforoTotal, "):       ", redon(porcentajeOcupacionReal * 100) / 100, " % (Panel: ", porcentajeOcupacionEntero, " %)"
    Escribir "ACTIVO:             ", horasUptime, "h ", minutosUptime, "m ", segundosUptime, "s"
    Escribir "----------------------------------------------------------------------"
    Escribir "TELEMETRÍA DE SENSORES (MATH):"
    Escribir "Sensor térmico real:  ", tempCalibrada, " C"
    Escribir "Sensor acotado seguro:", tempSeguraAcotada, " C [Rango 15.0 - 35.0 C]"
    Escribir "Sensor NFC soporte:   Activo (", sensorNfcOperativo, ")"
    Escribir "REGISTRO LOG:         ", RUTA_LOGS
    Escribir "======================================================================"
FinAlgoritmo
```

##### Paso B. Implementación en Java (`pr/src/ControlAccesoQR.java` — v1.4)

```java
/**
 * SISTEMA DE CONTROL DE ASISTENCIA POR CÓDIGO QR
 * Cliente: IES El Caminàs (Castellón de la Plana)
 * Consultora: AzaharTech Software Consulting
 * 
 * Versión 1.4: 
 * Invocación de métodos estáticos sin instanciación (java.lang.Math),
 * cálculo de desviaciones absolutas con Math.abs() y acotación segura con Math.min()/max().
 * 
 * @author Equipo AzaharTech (Alba Torres, Pau Ferrer)
 * @version 1.4 (Octubre 2026)
 * @since JDK 21 LTS
 *
 */
import java.util.Scanner;
import java.util.Random;

public class ControlAccesoQR {
    public static void main(String[] args) {
        // ---------------------------------------------------------------------
        // 1. CONSTANTES INMUTABLES DEL SISTEMA
        // ---------------------------------------------------------------------
        final String NOMBRE_CENTRO = "IES El Caminàs (Castellón)";
        final String PREFIJO_CENTRO = "CAMINAS";
        final String RUTA_LOGS = ".\\caminas\\terminal\\logs";
        final int MINUTOS_POR_HORA = 60;
        final int SEGUNDOS_POR_HORA = 3600;
        final int SEGUNDOS_POR_MINUTO = 60;
        final double FACTOR_PORCENTAJE = 100.0;
        final int BASE_TOKEN_SEGURIDAD = 1000;
        final int RANGO_TOKEN_SEGURIDAD = 9000;
        final double MAX_VARIACION_TERMICA = 0.5; 
        final long SEMILLA_CALIBRACION = 987654321L;

        // [NUEVO DÍA 17] Constantes de hardware y jornada laboral/centro de 8 horas
        final int JORNADA_BASE_MINUTOS = 480; // 8 horas de permanencia de referencia (docentes/PAS)
        final double TEMP_MIN_SEGURA = 15.0;  // Límite inferior admisible
        final double TEMP_MAX_SEGURA = 35.0;  // Límite superior admisible

        // ---------------------------------------------------------------------
        // 2. DECLARACIÓN DE VARIABLES Y OBJETOS
        // ---------------------------------------------------------------------
        Scanner teclado = new Scanner(System.in);

        // Métodos de instancia: requieren crear objetos con 'new' en el Heap
        Random generadorVestibulo   = new Random();
        Random generadorPuertaNorte = new Random();
        Random generadorCalibracion = new Random(SEMILLA_CALIBRACION);

        int terminalId;
        double tempVestibulo;

        String nombrePersona, dniPersona;
        char perfilPersona;
        boolean esEntrada;

        int horaEntrada, minutoEntrada;
        int horaSalida, minutoSalida;
        int minutosTotalesEntrada, minutosTotalesSalida, minutosEstanciaTotal;
        String tokenResumenVestibulo;
        String tokenResumenPuertaNorte;

        int personasEnCentro = 1200;
        int aforoDisponible = 800;
        int idUltimoFichaje = 1042;

        int segundosActividadTerminal;
        int horasUptime, minutosUptime, segundosUptime;

        int aforoTotal;
        double porcentajeOcupacionReal;
        int porcentajeOcupacionEntero;

        int codigoTokenVestibulo;
        int codigoTokenPuertaNorte;

        int codigoTestCalibracion;
        double fluctuacionTermica;
        double tempCalibrada;
        boolean sensorNfcOperativo;

        // [NUEVO DÍA 17] Variables para resultados de métodos estáticos de Math
        int desviacionJornadaMinutos;
        double tempSeguraAcotada;
        int aforoSeguroVisualizado;

        // ---------------------------------------------------------------------
        // 3. CAPTURA DE DATOS
        // ---------------------------------------------------------------------
        System.out.println("=================================================");
        System.out.println("   AZAHARTECH - TERMINAL " + NOMBRE_CENTRO);
        System.out.println("   Versión 1.4 (métodos estáticos de la clase Math) ");
        System.out.println("=================================================");
        System.out.print("ID del terminal principal: ");
        terminalId = teclado.nextInt();

        System.out.print("Temperatura base del sensor (ºC): ");
        tempVestibulo = teclado.nextDouble();

        System.out.print("Segundos de actividad del terminal (uptime): ");
        segundosActividadTerminal = teclado.nextInt();

        teclado.nextLine(); // Limpieza de buffer

        System.out.print("DNI de la persona: ");
        dniPersona = teclado.nextLine();

        System.out.print("Nombre completo: ");
        nombrePersona = teclado.nextLine();

        System.out.print("Perfil de acceso (E = Estudiante, D = Docente, V = Visita): ");
        perfilPersona = teclado.next().charAt(0);

        esEntrada = true;

        System.out.print("Hora de entrada (0-23): ");
        horaEntrada = teclado.nextInt();

        System.out.print("Minuto de entrada (0-59): ");
        minutoEntrada = teclado.nextInt();

        System.out.print("Hora de salida (0-23): ");
        horaSalida = teclado.nextInt();

        System.out.print("Minuto de salida (0-59): ");
        minutoSalida = teclado.nextInt();

        // ---------------------------------------------------------------------
        // 4. PROCESAMIENTO SECUENCIAL
        // ---------------------------------------------------------------------
        minutosTotalesEntrada = (horaEntrada * MINUTOS_POR_HORA) + minutoEntrada;
        minutosTotalesSalida = (horaSalida * MINUTOS_POR_HORA) + minutoSalida;
        minutosEstanciaTotal = minutosTotalesSalida - minutosTotalesEntrada;

        personasEnCentro++;
        aforoDisponible--;
        idUltimoFichaje++;

        codigoTokenVestibulo   = BASE_TOKEN_SEGURIDAD + generadorVestibulo.nextInt(RANGO_TOKEN_SEGURIDAD);
        codigoTokenPuertaNorte = BASE_TOKEN_SEGURIDAD + generadorPuertaNorte.nextInt(RANGO_TOKEN_SEGURIDAD);

        codigoTestCalibracion = BASE_TOKEN_SEGURIDAD + generadorCalibracion.nextInt(RANGO_TOKEN_SEGURIDAD);
        fluctuacionTermica = generadorCalibracion.nextDouble() * MAX_VARIACION_TERMICA;
        tempCalibrada = tempVestibulo + fluctuacionTermica;
        sensorNfcOperativo = generadorCalibracion.nextBoolean();

        // [NUEVO DÍA 17] Invocación de métodos estáticos de Math (sin operador new):
        // 1. Math.abs(): Calcula la desviación absoluta respecto a la jornada de 8 horas (480 min)
        desviacionJornadaMinutos = Math.abs(minutosEstanciaTotal - JORNADA_BASE_MINUTOS);

        // 2. Acotación en rango [15.0 - 35.0] combinando Math.max() y Math.min() sin condicionales
        tempSeguraAcotada = Math.max(TEMP_MIN_SEGURA, Math.min(tempCalibrada, TEMP_MAX_SEGURA));

        // 3. Math.min(): Evita que el aforo reportado supere la capacidad física de 2000 plazas
        aforoTotal = personasEnCentro + aforoDisponible;
        aforoSeguroVisualizado = Math.min(personasEnCentro, aforoTotal);

        tokenResumenVestibulo = PREFIJO_CENTRO + "-" + dniPersona;
        tokenResumenVestibulo += "-T" + terminalId;
        tokenResumenVestibulo += "-SEC#" + codigoTokenVestibulo;
        tokenResumenVestibulo += "-ESTANCIA-" + minutosEstanciaTotal;
        tokenResumenVestibulo += "-REG" + idUltimoFichaje;

        tokenResumenPuertaNorte = PREFIJO_CENTRO + "-" + dniPersona;
        tokenResumenPuertaNorte += "-T102";
        tokenResumenPuertaNorte += "-SEC#" + codigoTokenPuertaNorte;
        tokenResumenPuertaNorte += "-ESTANCIA-" + minutosEstanciaTotal;
        tokenResumenPuertaNorte += "-REG" + idUltimoFichaje;

        horasUptime = segundosActividadTerminal / SEGUNDOS_POR_HORA;
        minutosUptime = (segundosActividadTerminal % SEGUNDOS_POR_HORA) / SEGUNDOS_POR_MINUTO;
        segundosUptime = segundosActividadTerminal % SEGUNDOS_POR_MINUTO;

        porcentajeOcupacionReal = ((double) aforoSeguroVisualizado / aforoTotal) * FACTOR_PORCENTAJE;
        porcentajeOcupacionEntero = (int) porcentajeOcupacionReal;

        // ---------------------------------------------------------------------
        // 5. SALIDA FORMATEADA PROFESIONAL (printf)
        // ---------------------------------------------------------------------
        System.out.println("\n======================================================================");
        System.out.println("             INFORME OFICIAL DE ACCESO EN VESTÍBULO                   ");
        System.out.println("======================================================================");
        System.out.printf("CENTRO:               %-30s%n", NOMBRE_CENTRO);
        System.out.printf("REGISTRO N.º:         #%05d%n", idUltimoFichaje);
        System.out.printf("TOKEN QR VESTÍBULO:   %s%n", tokenResumenVestibulo);
        System.out.printf("TOKEN QR PUERTA NORTE:%s%n", tokenResumenPuertaNorte);
        System.out.printf("PERSONA:              %-30s | PERFIL: %c%n", nombrePersona, perfilPersona);
        System.out.printf("IDENTIFICADOR:        %-12s | SENTIDO: Entrada (%b)%n", dniPersona, esEntrada);
        System.out.println("----------------------------------------------------------------------");
        System.out.printf("HORARIO:              Entrada %02d:%02d | Salida %02d:%02d%n", horaEntrada, minutoEntrada, horaSalida, minutoSalida);
        System.out.printf("PERMANENCIA:          %03d minutos en el centro.%n", minutosEstanciaTotal);
        System.out.printf("DESVIACIÓN JORNADA:   %03d min respecto a jornada completa (480 min).%n", desviacionJornadaMinutos);
        System.out.printf("AFORO (%04d):         %6.2f %% (Panel: %03d %%)%n", aforoTotal, porcentajeOcupacionReal, porcentajeOcupacionEntero);
        System.out.printf("ACTIVO:               %02dh %02dm %02ds%n", horasUptime, minutosUptime, segundosUptime);
        System.out.println("----------------------------------------------------------------------");
        System.out.println("TELEMETRÍA Y CONTROL DE SENSORES (MÉTODOS ESTÁTICOS MATH):");
        System.out.printf("SENSOR TÉRMICO REAL:  %.2f ºC (Calibrado con fluctuación)%n", tempCalibrada);
        System.out.printf("SENSOR TÉRMICO ACOT.: %.2f ºC [Rango seguro: %.1f - %.1f ºC]%n", tempSeguraAcotada, TEMP_MIN_SEGURA, TEMP_MAX_SEGURA);
        System.out.printf("AFORO COMPUTADO:      %d personas (Acotado con Math.min)%n", aforoSeguroVisualizado);
        System.out.printf("SENSOR NFC RESPALDO:  Activo (%b)%n", sensorNfcOperativo);
        System.out.printf("REGISTRO LOG:         %s%n", RUTA_LOGS);
        System.out.println("======================================================================");

        teclado.close();
    }
}
```

---

#### 4. Segunda sesión: dojo de entrenamiento y katas de código

Pasamos al dojo de entrenamiento para programar, con tres **katas de dificultad progresiva** sobre métodos estáticos, la clase `Math` y funciones de acotación matemática:

---

##### Kata 1 (Cinturón blanco / Nivel base). Métodos estáticos de `Math` en tu proyecto
* **Objetivo:** Incorporar a tu clase `MiProyecto.java` (versión v1.3) al menos una llamada a `Math.abs()` para calcular una diferencia absoluta y una llamada a `Math.min()` o `Math.max()` para seleccionar un valor extremo.
* **Aplicación según tu proyecto elegido:**
  * En **Aventura conversacional**: calcular con `Math.abs()` la distancia entre la posición del héroe y la posición del cofre, y asegurar con `Math.max()` que la energía recuperada no baje de cero.
  * En **Motor de recomendación**: calcular con `Math.abs()` la desviación absoluta entre la valoración del usuario y la media histórica del ítem, y usar `Math.min()` para seleccionar el límite de puntuación máxima admitida.
  * En **Simulador de físicas 2D**: calcular con `Math.abs()` la magnitud escalar de la velocidad de choque, y usar `Math.max()` para asegurar que la masa de una partícula nunca descienda de un valor base.
  * En **Bóveda de contraseñas**: calcular con `Math.abs()` la diferencia en días entre la fecha de último cambio de clave y la fecha recomendada, y usar `Math.min()` para acotar el número de intentos permitidos.

---

##### Kata 2 (Cinturón marrón / Nivel avanzado): La función de acotamiento en rango (*clamp*)
* **Contexto técnico:** En simuladores y sistemas de control, la función *clamp* confina un valor para que nunca descienda de un mínimo ni supere un máximo combinando dos llamadas anidadas.
* **Misión de la kata:**
  1. En una clase de prueba llamada `PruebaAcotamiento.java`, crea un cálculo secuencial que reciba un valor decimal y lo confine en el rango $[10.0, 50.0]$ anidando `Math.max` y `Math.min` en una sola expresión:
     ```java
     double valorOriginal = 72.8; // Prueba sucesivamente con 5.0, 25.0 y 72.8
     double valorAcotado = Math.max(10.0, Math.min(valorOriginal, 50.0));
     System.out.printf("Original: %.1f -> Acotado: %.1f%n", valorOriginal, valorAcotado);
     ```
  2. Comprueba el comportamiento con los tres casos posibles:
    * Si introduces `5.0` (por debajo del mínimo), la expresión devuelve `10.0`.
    * Si introduces `25.0` (dentro del rango), la expresión devuelve `25.0`.
    * Si introduces `72.8` (por encima del máximo), la expresión devuelve `50.0`.
  3. Redacta dos líneas de comentario en tu código explicando el orden de evaluación: **cómo la llamada interna a `Math.min()` fija el techo superior y cómo la llamada externa a `Math.max()` asegura el suelo inferior**.

---

##### Kata 3 (Cinturón negro / Kata «Hacker AzaharTech»): La caja de herramientas estática sin operador `new`
* **Contexto de ingeniería:** Comprender por qué las clases de utilidades puras no necesitan reservar memoria en el Heap ni crear objetos.
* **Misión de la kata:**
  1. Crea una clase de prueba temporal llamada `TestCajaHerramientas.java`.
  2. Escribe deliberadamente la línea:
     ```java
     Math m = new Math(); // Intento de instanciación
     ```
  3. Observa cómo el compilador de Java prohíbe la creación del objeto porque `Math` no es un molde para crear ejemplares, sino un catálogo de herramientas compartidas. Comenta esa línea para que el programa pueda compilar.
  4. Demuestra el uso directo de las herramientas de `Math` sin variables previas calculando el área de cobertura circular de una antena del vestíbulo ($A = \pi \times r^2$):
     ```java
     double radioMetros = 4.5;
     // Usamos la constante pública Math.PI directamente desde la clase
     double areaCobertura = Math.PI * (radioMetros * radioMetros);
     System.out.printf("Área de cobertura calculada: %.2f m²%n", areaCobertura);
     ```
  5. Anota en tu cuaderno técnico la conclusión: *«A diferencia de `Random`, donde cada generador necesita su propio objeto para guardar su semilla en el Heap, los métodos y constantes de `Math` pertenecen a la clase y están disponibles inmediatamente sin consumir memoria de instanciación»*.

---

### Día 18 - 2 sesiones

---

#### 1. Caso guía en AzaharTech
Son las tres de la tarde del miércoles 14 de octubre. En la sala técnica de **AzaharTech**, **Pau Ferrer** se enfrenta a dos nuevos retos de cálculo geométrico y visual para el **IES El Caminàs**:

1. **La distancia de la antena del vestíbulo:** Para ubicar con precisión el lector óptico en el techo, los técnicos de redes necesitan calcular la distancia diagonal directa (la hipotenusa) entre la antena del techo y el punto de paso de la persona en el suelo. Pau conoce la distancia en el eje horizontal ($x = 3.0\text{ m}$) y en el eje vertical ($y = 4.0\text{ m}$), y ha intentado calcular la hipotenusa escribiendo:
   ```java
   double hipotenusa = (x ^ 2) + (y ^ 2); // Error conceptual grave en Java
   ```
La consola imprime un número disparatado porque en Java el símbolo `^` **no calcula potencias**, sino que realiza una operación lógica a nivel de bits llamada *XOR*.
2. **El indicador del panel LED.** Cuando la ocupación del recinto es de $60.95\text{ }\%$, el panel luminoso del vestíbulo debe mostrar un número entero. Como en el Sprint 1 aplicamos un casting `(int) 60.95`, el sistema muestra `60 %`. Los conserjes se quejan porque un $60.95\text{ }\%$ está prácticamente en el $61\text{ }\%$, y el casting simple solo trunca decimales en lugar de aplicar las reglas matemáticas de redondeo.

**Alba Torres** y **Laia Claramunt** se acercan al puesto de Pau:
> *«Pau, para calcular potencias, raíces y redondeos oficiales no inventamos operadores ni usamos casting destructivo. Volvemos a recurrir a la caja de herramientas de **`java.lang.Math`**:*
>
> *1. Para potencias utilizamos el método estático **`Math.pow(base, exponente)`**.*  
> *2. Para raíces cuadradas invocamos **`Math.sqrt(valor)`**.*  
> *3. Y para redondear decimales según la regla matemática oficial ($\ge 0.5$ sube, $< 0.5$ baja), usamos **`Math.round(valor)`**.*
>
> *Hoy evolucionaremos `ControlAccesoQR` a la versión **v1.5**: resolveremos la distancia de cobertura de los sensores y emitiremos porcentajes redondeados con rigor profesional»*.

---

#### 2. Fundamento teórico: potencias, raíces y redondeo matemático

```
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                        FUNCIONES MATEMÁTICAS AVANZADAS EN MATH                         │
├──────────────────────────┬───────────────────┬─────────────────────────────────────────┤
│ Invocación estática      │ Tipo de retorno   │ Comportamiento técnico                  │
├──────────────────────────┼───────────────────┼─────────────────────────────────────────┤
│ Math.pow(base, exp)      │ double            │ Eleva la 'base' al 'exponente'.         │
│                          │                   │ Siempre devuelve double (ej. 2^3 = 8.0) │
├──────────────────────────┼───────────────────┼─────────────────────────────────────────┤
│ Math.sqrt(x)             │ double            │ Calcula la raíz cuadrada positiva de x. │
│                          │                   │ Si x < 0, devuelve NaN (Not a Number).  │
├──────────────────────────┼───────────────────┼─────────────────────────────────────────┤
│ Math.round(double x)     │ long              │ Redondea al entero más próximo:         │
│                          │                   │ • 60.49 -> 60                           │
│                          │                   │ • 60.50 -> 61                           │
└──────────────────────────┴───────────────────┴─────────────────────────────────────────┘
```

##### A. El cálculo de potencias con `Math.pow()` frente a la trampa del operador `^`
En muchos lenguajes o calculadoras, el acento circunflejo `^` representa una potencia. **En Java no.**
* El operador `^` es un operador binario de álgebra booleana a nivel de bits (*XOR*). Escribir `3 ^ 2` en Java no calcula $3^2 = 9$; compara los bits `0011` y `0010` resultando en el número `1`.
* Para calcular potencias en Java es obligatorio invocar al método estático `Math.pow(double base, double exponente)`:
  ```java
  double cuadrado = Math.pow(3.0, 2.0); // Devuelve 9.0
  ```

##### B. Raíces cuadradas con `Math.sqrt()` y el teorema de Pitágoras
El método `Math.sqrt()` (*Square Root*) permite calcular distancias euclídeas, radios de cobertura o módulos de vectores combinándolo con potencias en una única línea de código:

$$d = \sqrt{x^2 + y^2} \quad \Longrightarrow \quad \text{double d} = \text{Math.sqrt}(\text{Math.pow}(x, 2) + \text{Math.pow}(y, 2));$$

##### C. Truncamiento mediante *casting* frente a redondeo matemático con `Math.round()`
Es vital distinguir qué hace la CPU en cada caso:
1. **Truncamiento con casting `(int)`:** Descarta sin miramientos la parte decimal.
  * `(int) 60.10` da `60`
  * `(int) 60.99` da `60` *(¡pierde casi una unidad entera!)*
2. **Redondeo matemático con `Math.round()`:** Aplica la regla estándar del redondeo aritmético:
  * Suma $0.5$ al valor y trunca hacia abajo el resultado.
  * `Math.round(60.49)` devuelve `60L`.
  * `Math.round(60.50)` devuelve `61L`.

> ⚠️ **Atención al tipo de retorno de `Math.round()`:**  
> Cuando le pasamos un argumento de tipo `double`, `Math.round()` devuelve un entero largo de tipo **`long`** (64 bits). Si queremos almacenarlo en una variable `int`, debemos aplicar un casting explícito:  
> `int ocupacionRedondeada = (int) Math.round(porcentajeDecimal);`

---

#### 3. Evolución del caso guía a `ControlAccesoQR v1.5`

Ampliamos el programa incorporando:
1. Las coordenadas espaciales del sensor del vestíbulo ($x = 3.0\text{ m}$, $y = 4.0\text{ m}$) y el cálculo de la distancia diagonal de cobertura mediante `Math.pow()` y `Math.sqrt()`.
2. El redondeo matemático oficial del porcentaje de aforo con `Math.round()` para el panel visual del centro, comparándolo con el valor truncado.

##### Paso A. Algoritmo en PSeInt (`pr/pseudocodigo/ControlAccesoQR.psc` — v1.5)

```psc
Algoritmo ControlAccesoQR
    // =========================================================================
    // SISTEMA DE CONTROL DE ASISTENCIA QR - IES EL CAMINAS (Castellon)
    // Version: 1.5 (Evolucion: potencias, raices y redondeo con Math)
    // =========================================================================

    Definir NOMBRE_CENTRO, PREFIJO_CENTRO, RUTA_LOGS Como Cadena
    Definir MINUTOS_POR_HORA, SEGUNDOS_POR_HORA, SEGUNDOS_POR_MINUTO Como Entero
    Definir FACTOR_PORCENTAJE Como Real
    Definir SEMILLA_CALIBRACION Como Entero
    Definir JORNADA_BASE_MINUTOS Como Entero
    Definir TEMP_MIN_SEGURA, TEMP_MAX_SEGURA Como Real
    
    NOMBRE_CENTRO <- "IES El Caminas (Castellon)"
    PREFIJO_CENTRO <- "CAMINAS"
    RUTA_LOGS <- ".\caminas\terminal\logs"
    MINUTOS_POR_HORA <- 60
    SEGUNDOS_POR_HORA <- 3600
    SEGUNDOS_POR_MINUTO <- 60
    FACTOR_PORCENTAJE <- 100.0
    SEMILLA_CALIBRACION <- 987654321
    JORNADA_BASE_MINUTOS <- 480
    TEMP_MIN_SEGURA <- 15.0
    TEMP_MAX_SEGURA <- 35.0

    Definir terminalId Como Entero
    Definir tempVestibulo, fluctuacionTermica, tempCalibrada, tempSeguraAcotada Como Real
    Definir nombrePersona, dniPersona Como Cadena
    Definir perfilPersona Como Caracter
    Definir esEntrada Como Logico
    
    Definir horaEntrada, minutoEntrada Como Entero
    Definir horaSalida, minutoSalida Como Entero
    Definir minutosTotalesEntrada, minutosTotalesSalida, minutosEstanciaTotal Como Entero
    Definir tokenResumenVestibulo, tokenResumenPuertaNorte Como Cadena
    
    Definir personasEnCentro, aforoDisponible, idUltimoFichaje Como Entero
    personasEnCentro <- 1200
    aforoDisponible <- 800
    idUltimoFichaje <- 1042
    
    Definir segundosActividadTerminal Como Entero
    Definir horasUptime, minutosUptime, segundosUptime Como Entero
    Definir aforoTotal Como Entero
    Definir porcentajeOcupacionReal Como Real
    Definir porcentajeOcupacionTruncado Como Entero

    Definir codigoTokenVestibulo, codigoTokenPuertaNorte Como Entero
    Definir codigoTestCalibracion Como Entero
    Definir sensorNfcOperativo Como Logico
    Definir desviacionJornadaMinutos Como Entero
    Definir aforoSeguroVisualizado Como Entero

    // [NUEVO DÍA 18] Variables para telemetría espacial y redondeo matemático
    Definir coordenadaXMetros, coordenadaYMetros, distanciaDiagonalAntena Como Real
    Definir porcentajeOcupacionRedondeado Como Entero

    Escribir "AZAHARTECH: TERMINAL ", NOMBRE_CENTRO, " (v1.5 - geometria y redondeo)"
    Escribir "ID del terminal principal:"
    Leer terminalId
    Escribir "Temperatura base del sensor (ºC):"
    Leer tempVestibulo
    Escribir "Segundos de actividad del terminal (uptime):"
    Leer segundosActividadTerminal
    
    // [NUEVO DÍA 18] Coordenadas para cálculo de cobertura
    Escribir "Distancia horizontal al torno en metros (eje X):"
    Leer coordenadaXMetros
    Escribir "Altura del sensor en metros (eje Y):"
    Leer coordenadaYMetros
    
    Escribir "DNI de la persona:"
    Leer dniPersona
    Escribir "Nombre completo:"
    Leer nombrePersona
    Escribir "Perfil de acceso (E = Estudiante, D = Docente, V = Visita):"
    Leer perfilPersona

    esEntrada <- Verdadero
    
    Escribir "Introduce hora y minuto de entrada (por ejemplo, 8 15):"
    Leer horaEntrada
    Leer minutoEntrada
    Escribir "Introduce hora y minuto de salida (por ejemplo, 14 10):"
    Leer horaSalida
    Leer minutoSalida
        
    minutosTotalesEntrada <- (horaEntrada * MINUTOS_POR_HORA) + minutoEntrada
    minutosTotalesSalida <- (horaSalida * MINUTOS_POR_HORA) + minutoSalida
    minutosEstanciaTotal <- minutosTotalesSalida - minutosTotalesEntrada
    
    personasEnCentro <- personasEnCentro + 1
    aforoDisponible <- aforoDisponible - 1
    idUltimoFichaje <- idUltimoFichaje + 1

    codigoTokenVestibulo <- Aleatorio(1000, 9999)
    codigoTokenPuertaNorte <- Aleatorio(1000, 9999)
    codigoTestCalibracion <- 5555
    fluctuacionTermica <- 0.45
    tempCalibrada <- tempVestibulo + fluctuacionTermica
    sensorNfcOperativo <- Verdadero
    
    desviacionJornadaMinutos <- abs(minutosEstanciaTotal - JORNADA_BASE_MINUTOS)
    
    Si tempCalibrada > TEMP_MAX_SEGURA Entonces
        tempSeguraAcotada <- TEMP_MAX_SEGURA
    SiNo
        Si tempCalibrada < TEMP_MIN_SEGURA Entonces
            tempSeguraAcotada <- TEMP_MIN_SEGURA
        SiNo
            tempSeguraAcotada <- tempCalibrada
        FinSi
    FinSi
      
    tokenResumenVestibulo <- PREFIJO_CENTRO + "-" + dniPersona + "-T" + ConvertirATexto(terminalId) + "-SEC#" + ConvertirATexto(codigoTokenVestibulo) + "-REG" + ConvertirATexto(idUltimoFichaje)
    tokenResumenPuertaNorte <- PREFIJO_CENTRO + "-" + dniPersona + "-T102-SEC#" + ConvertirATexto(codigoTokenPuertaNorte) + "-REG" + ConvertirATexto(idUltimoFichaje)
    
    horasUptime <- trunc(segundosActividadTerminal / SEGUNDOS_POR_HORA)
    minutosUptime <- trunc((segundosActividadTerminal MOD SEGUNDOS_POR_HORA) / SEGUNDOS_POR_MINUTO)
    segundosUptime <- segundosActividadTerminal MOD SEGUNDOS_POR_MINUTO
    
    aforoTotal <- personasEnCentro + aforoDisponible
    aforoSeguroVisualizado <- personasEnCentro
    porcentajeOcupacionReal <- (aforoSeguroVisualizado * FACTOR_PORCENTAJE) / aforoTotal
    
    // [NUEVO DÍA 18] Comparación entre truncamiento y redondeo
    porcentajeOcupacionTruncado <- trunc(porcentajeOcupacionReal)
    porcentajeOcupacionRedondeado <- redon(porcentajeOcupacionReal)
    
    // [NUEVO DÍA 18] Teorema de Pitágoras con raíz y potencias
    distanciaDiagonalAntena <- rc((coordenadaXMetros ^ 2) + (coordenadaYMetros ^ 2))
    
    Escribir ""
    Escribir "======================================================================"
    Escribir "             INFORME OFICIAL DE ACCESO EN VESTIBULO                   "
    Escribir "======================================================================"
    Escribir "CENTRO:             ", NOMBRE_CENTRO
    Escribir "REGISTRO N. :       #", idUltimoFichaje
    Escribir "TOKEN VESTIBULO:    ", tokenResumenVestibulo
    Escribir "TOKEN PUERTA NORTE: ", tokenResumenPuertaNorte
    Escribir "PERSONA:            ", nombrePersona, " | PERFIL: ", perfilPersona
    Escribir "IDENTIFICADOR:      ", dniPersona, " | SENTIDO: Entrada (", esEntrada, ")"
    Escribir "----------------------------------------------------------------------"
    Escribir "HORARIO:            Entrada ", horaEntrada, ":", minutoEntrada, " | Salida ", horaSalida, ":", minutoSalida
    Escribir "PERMANENCIA:        ", minutosEstanciaTotal, " minutos en el centro."
    Escribir "DESVIACION JORNADA: ", desviacionJornadaMinutos, " min respecto a jornada completa (480 min)."
    Escribir "----------------------------------------------------------------------"
    Escribir "ESTADÍSTICAS Y REDONDEOS (MATH):"
    Escribir "Ocupación calculada:  ", porcentajeOcupacionReal, " %"
    Escribir "Ocupación truncada:   ", porcentajeOcupacionTruncado, " % (Casting simple)"
    Escribir "Ocupación redondeada: ", porcentajeOcupacionRedondeado, " % (Math.round oficial)"
    Escribir "Distancia antena QR:  ", distanciaDiagonalAntena, " metros (Pitágoras con sqrt/pow)"
    Escribir "ACTIVO:               ", horasUptime, "h ", minutosUptime, "m ", segundosUptime, "s"
    Escribir "REGISTRO LOG:         ", RUTA_LOGS
    Escribir "======================================================================"
FinAlgoritmo
```

##### Paso B. Implementación en Java (`pr/src/ControlAccesoQR.java` — v1.5)

```java
/**
 * SISTEMA DE CONTROL DE ASISTENCIA POR CÓDIGO QR
 * Cliente: IES El Caminàs (Castellón de la Plana)
 * Consultora: AzaharTech Software Consulting
 * 
 * Versión 1.5:
 * Métodos estáticos avanzados de Math: potencias (Math.pow),
 * raíces (Math.sqrt) y redondeo aritmético oficial (Math.round).
 * 
 * @author Equipo AzaharTech (Alba Torres, Pau Ferrer)
 * @version 1.5 (Octubre 2026)
 * @since JDK 21 LTS
 *
 */
import java.util.Scanner;
import java.util.Random;

public class ControlAccesoQR {
    public static void main(String[] args) {
        // Constantes inmutables
        final String NOMBRE_CENTRO = "IES El Caminàs (Castellón)";
        final String PREFIJO_CENTRO = "CAMINAS";
        final String RUTA_LOGS = ".\\caminas\\terminal\\logs";
        final int MINUTOS_POR_HORA = 60;
        final int SEGUNDOS_POR_HORA = 3600;
        final int SEGUNDOS_POR_MINUTO = 60;
        final double FACTOR_PORCENTAJE = 100.0;
        final int BASE_TOKEN_SEGURIDAD = 1000;
        final int RANGO_TOKEN_SEGURIDAD = 9000;
        final double MAX_VARIACION_TERMICA = 0.5;
        final long SEMILLA_CALIBRACION = 987654321L;
        final int JORNADA_BASE_MINUTOS = 480;
        final double TEMP_MIN_SEGURA = 15.0;
        final double TEMP_MAX_SEGURA = 35.0;
        
        Scanner teclado = new Scanner(System.in);

        Random generadorVestibulo   = new Random();
        Random generadorPuertaNorte = new Random();
        Random generadorCalibracion = new Random(SEMILLA_CALIBRACION);

        int terminalId;
        double tempVestibulo;

        String nombrePersona, dniPersona;
        char perfilPersona;
        boolean esEntrada;

        int horaEntrada, minutoEntrada;
        int horaSalida, minutoSalida;
        int minutosTotalesEntrada, minutosTotalesSalida, minutosEstanciaTotal;
        String tokenResumenVestibulo;
        String tokenResumenPuertaNorte;

        int personasEnCentro = 1200;
        int aforoDisponible = 800;
        int idUltimoFichaje = 1042;

        int segundosActividadTerminal;
        int horasUptime, minutosUptime, segundosUptime;

        int aforoTotal;
        double porcentajeOcupacionReal;
        int porcentajeOcupacionTruncado;

        // [NUEVO DÍA 18] Variables para redondeo y cálculo espacial con Math
        int porcentajeOcupacionRedondeado;
        double coordenadaXMetros;
        double coordenadaYMetros;
        double distanciaDiagonalAntena;

        int codigoTokenVestibulo;
        int codigoTokenPuertaNorte;
        int codigoTestCalibracion;
        double fluctuacionTermica;
        double tempCalibrada;
        boolean sensorNfcOperativo;
        int desviacionJornadaMinutos;
        double tempSeguraAcotada;
        int aforoSeguroVisualizado;

        System.out.println("=================================================");
        System.out.println("   AZAHARTECH - TERMINAL " + NOMBRE_CENTRO);
        System.out.println("   Versión 1.5 (geometría y redondeos de Math)    ");
        System.out.println("=================================================");
        System.out.print("ID del terminal principal: ");
        terminalId = teclado.nextInt();

        System.out.print("Temperatura base del sensor (ºC): ");
        tempVestibulo = teclado.nextDouble();

        System.out.print("Segundos de actividad del terminal (uptime): ");
        segundosActividadTerminal = teclado.nextInt();

        // [NUEVO DÍA 18] Entrada de coordenadas del sensor
        System.out.print("Distancia horizontal al torno en metros (eje X): ");
        coordenadaXMetros = teclado.nextDouble();

        System.out.print("Altura del sensor en metros (eje Y): ");
        coordenadaYMetros = teclado.nextDouble();

        teclado.nextLine(); // Limpieza de buffer

        System.out.print("DNI de la persona: ");
        dniPersona = teclado.nextLine();

        System.out.print("Nombre completo: ");
        nombrePersona = teclado.nextLine();

        System.out.print("Perfil de acceso (E = Estudiante, D = Docente, V = Visita): ");
        perfilPersona = teclado.next().charAt(0);

        esEntrada = true;

        System.out.print("Hora de entrada (0-23): ");
        horaEntrada = teclado.nextInt();

        System.out.print("Minuto de entrada (0-59): ");
        minutoEntrada = teclado.nextInt();

        System.out.print("Hora de salida (0-23): ");
        horaSalida = teclado.nextInt();

        System.out.print("Minuto de salida (0-59): ");
        minutoSalida = teclado.nextInt();

        // Procesamiento secuencial
        minutosTotalesEntrada = (horaEntrada * MINUTOS_POR_HORA) + minutoEntrada;
        minutosTotalesSalida = (horaSalida * MINUTOS_POR_HORA) + minutoSalida;
        minutosEstanciaTotal = minutosTotalesSalida - minutosTotalesEntrada;

        personasEnCentro++;
        aforoDisponible--;
        idUltimoFichaje++;

        codigoTokenVestibulo   = BASE_TOKEN_SEGURIDAD + generadorVestibulo.nextInt(RANGO_TOKEN_SEGURIDAD);
        codigoTokenPuertaNorte = BASE_TOKEN_SEGURIDAD + generadorPuertaNorte.nextInt(RANGO_TOKEN_SEGURIDAD);

        codigoTestCalibracion = BASE_TOKEN_SEGURIDAD + generadorCalibracion.nextInt(RANGO_TOKEN_SEGURIDAD);
        fluctuacionTermica = generadorCalibracion.nextDouble() * MAX_VARIACION_TERMICA;
        tempCalibrada = tempVestibulo + fluctuacionTermica;
        sensorNfcOperativo = generadorCalibracion.nextBoolean();

        desviacionJornadaMinutos = Math.abs(minutosEstanciaTotal - JORNADA_BASE_MINUTOS);
        tempSeguraAcotada = Math.max(TEMP_MIN_SEGURA, Math.min(tempCalibrada, TEMP_MAX_SEGURA));

        aforoTotal = personasEnCentro + aforoDisponible;
        aforoSeguroVisualizado = Math.min(personasEnCentro, aforoTotal);

        tokenResumenVestibulo = PREFIJO_CENTRO + "-" + dniPersona;
        tokenResumenVestibulo += "-T" + terminalId;
        tokenResumenVestibulo += "-SEC#" + codigoTokenVestibulo;
        tokenResumenVestibulo += "-ESTANCIA-" + minutosEstanciaTotal;
        tokenResumenVestibulo += "-REG" + idUltimoFichaje;

        tokenResumenPuertaNorte = PREFIJO_CENTRO + "-" + dniPersona;
        tokenResumenPuertaNorte += "-T102";
        tokenResumenPuertaNorte += "-SEC#" + codigoTokenPuertaNorte;
        tokenResumenPuertaNorte += "-ESTANCIA-" + minutosEstanciaTotal;
        tokenResumenPuertaNorte += "-REG" + idUltimoFichaje;

        horasUptime = segundosActividadTerminal / SEGUNDOS_POR_HORA;
        minutosUptime = (segundosActividadTerminal % SEGUNDOS_POR_HORA) / SEGUNDOS_POR_MINUTO;
        segundosUptime = segundosActividadTerminal % SEGUNDOS_POR_MINUTO;

        porcentajeOcupacionReal = ((double) aforoSeguroVisualizado / aforoTotal) * FACTOR_PORCENTAJE;
        
        // Truncamiento simple con casting a int (vía antigua)
        porcentajeOcupacionTruncado = (int) porcentajeOcupacionReal;

        // [NUEVO DÍA 18] Redondeo matemático oficial con Math.round()
        // Math.round(double) devuelve un long, por lo que aplicamos casting a (int)
        porcentajeOcupacionRedondeado = (int) Math.round(porcentajeOcupacionReal);

        // [NUEVO DÍA 18] Cálculo de hipotenusa/distancia euclídea: d = sqrt(x^2 + y^2)
        distanciaDiagonalAntena = Math.sqrt(Math.pow(coordenadaXMetros, 2.0) + Math.pow(coordenadaYMetros, 2.0));

        // Salida formateada
        System.out.println("\n======================================================================");
        System.out.println("             INFORME OFICIAL DE ACCESO EN VESTÍBULO                   ");
        System.out.println("======================================================================");
        System.out.printf("CENTRO:               %-30s%n", NOMBRE_CENTRO);
        System.out.printf("REGISTRO N.º:         #%05d%n", idUltimoFichaje);
        System.out.printf("TOKEN QR VESTÍBULO:   %s%n", tokenResumenVestibulo);
        System.out.printf("TOKEN QR PUERTA NORTE:%s%n", tokenResumenPuertaNorte);
        System.out.printf("PERSONA:              %-30s | PERFIL: %c%n", nombrePersona, perfilPersona);
        System.out.printf("IDENTIFICADOR:        %-12s | SENTIDO: Entrada (%b)%n", dniPersona, esEntrada);
        System.out.println("----------------------------------------------------------------------");
        System.out.printf("HORARIO:              Entrada %02d:%02d | Salida %02d:%02d%n", horaEntrada, minutoEntrada, horaSalida, minutoSalida);
        System.out.printf("PERMANENCIA:          %03d minutos en el centro.%n", minutosEstanciaTotal);
        System.out.printf("DESVIACIÓN JORNADA:   %03d min respecto a jornada completa (480 min).%n", desviacionJornadaMinutos);
        System.out.println("----------------------------------------------------------------------");
        System.out.println("CÁLCULOS MATEMÁTICOS AVANZADOS (POW, SQRT, ROUND):");
        System.out.printf("OCUPACIÓN EXACTA:     %6.2f %%%n", porcentajeOcupacionReal);
        System.out.printf("OCUPACIÓN TRUNCADA:   %d %% (Casting destructivo)%n", porcentajeOcupacionTruncado);
        System.out.printf("OCUPACIÓN REDONDEADA: %d %% (Math.round oficial)%n", porcentajeOcupacionRedondeado);
        System.out.printf("DISTANCIA A ANTENA:   %.2f metros (Pitágoras con pow y sqrt)%n", distanciaDiagonalAntena);
        System.out.printf("ACTIVO:               %02dh %02dm %02ds (Sensor: %.1f ºC)%n", horasUptime, minutosUptime, segundosUptime, tempSeguraAcotada);
        System.out.printf("REGISTRO LOG:         %s%n", RUTA_LOGS);
        System.out.println("======================================================================");

        teclado.close();
    }
}
```

---

#### 4. Segunda sesión: dojo de entrenamiento y katas de código

Pasamos al dojo de entrenamiento para programar, con tres **katas de dificultad progresiva** sobre potencias, raíces cuadradas y redondeo matemático:

---

##### Kata 1 (Cinturón blanco / Nivel base). `Math.pow`, `Math.sqrt` y `Math.round` en tu proyecto
* **Objetivo:** Incorporar a tu clase `MiProyecto.java` (versión v1.4) al menos una operación de potencia (`Math.pow`), una raíz cuadrada (`Math.sqrt`) o un redondeo oficial (`Math.round`) adaptado al dominio de tu proyecto propio.
* **Aplicación según tu proyecto elegido:**
  * En **Aventura conversacional**: calcular el radio de alcance de un hechizo expansivo mediante el teorema de Pitágoras ($r = \sqrt{x^2 + y^2}$) usando `Math.pow` y `Math.sqrt`, y redondear los puntos de experiencia finales con `Math.round()`.
  * En **Motor de recomendación**: calcular la distancia euclídea entre dos vectores de afinidad ($d = \sqrt{\Delta a^2 + \Delta b^2}$) con `Math.pow` y `Math.sqrt`, y redondear la puntuación final de afinidad a un entero de 0 a 100 con `Math.round()`.
  * En **Simulador de físicas 2D**: calcular la velocidad total resultante de una partícula ($v = \sqrt{v_x^2 + v_y^2}$) con `Math.pow` y `Math.sqrt`, y redondear la coordenada física de impacto al píxel entero más próximo con `Math.round()`.
  * En **Bóveda de contraseñas**: estimar el espacio combinatorio de fuerza bruta elevando el número de caracteres a la longitud de la clave mediante `Math.pow()`, y redondear el índice de entropía resultante con `Math.round()`.

---

##### Kata 2 (Cinturón marrón / Nivel avanzado). La trampa del tipo devuelto por `Math.round()`
* **Contexto técnico:** Entender las firmas y los tipos devueltos por la biblioteca de Java. El método `Math.round(double)` devuelve un entero largo de 64 bits (`long`), no un `int`.
* **Misión de la kata:**
  1. En una clase de prueba llamada `PruebaRedondeo.java`, intenta escribir la siguiente línea:
     ```java
     double nota = 8.75;
     int notaRedondeada = Math.round(nota); // Error de compilación inmediato
     ```
  2. Pasa el cursor por encima del error en IntelliJ y comprueba el mensaje: *«Incompatible types: found long, required int»*.
  3. Aplica el casting explícito adecuado para resolverlo: `(int) Math.round(nota)`.
  4. Imprime una tabla comparativa en la consola evaluando qué ocurre con el casting simple `(int)` frente a `Math.round()` para los siguientes cuatro números: `4.2`, `4.5`, `4.8` y `4.99`.
  5. Anota en tu cuaderno técnico por qué `(int) 4.8` da `4`, mientras que `Math.round(4.8)` da `5`.

---

##### Kata 3 (Cinturón negro / «Hacker AzaharTech»). El operador `^` frente a `Math.pow()`
* **Contexto de arquitectura:** Comprender qué hace realmente el procesador cuando usamos operadores binarios a nivel de bits.
* **Misión de la kata:**
  1. En una clase de prueba temporal llamada `PruebaXor.java`, ejecuta el siguiente código:
     ```java
     int calculoErroneo = 2 ^ 3;
     double calculoCorrecto = Math.pow(2.0, 3.0);

     System.out.println("Resultado de (2 ^ 3):      " + calculoErroneo);
     System.out.println("Resultado de Math.pow(2, 3): " + calculoCorrecto);
     ```
  2. Comprueba en la consola que `(2 ^ 3)` devuelve **`1`**, mientras que `Math.pow(2, 3)` devuelve **`8.0`**.
  3. Redacta dos líneas de comentario explicando qué ha ocurrido:
    * El número 2 en binario es `0010`.
    * El número 3 en binario es `0011`.
    * La operación XOR (`^`) compara bit a bit: devuelve 1 solo si los bits son distintos (`0010 ^ 0011 = 0001`, que equivale al número decimal 1).
    * Por tanto, para potencias matemáticas en Java es imprescindible invocar siempre al método estático `Math.pow()`.

---

### Día 19 - 2 sesiones

---

#### 1. Caso guía en AzaharTech
Son las tres de la tarde del jueves 15 de octubre. En la sala de integración de **AzaharTech**, **Pau Ferrer** muestra una incidencia con la que se ha topado al cargar los parámetros iniciales de la pantalla de TV del **IES El Caminàs**.

Los archivos de configuración del sistema y las peticiones de red que envían los móviles de las personas al cazar el QR transmiten toda la información en formato de texto plano (`String`). El archivo de configuración suministra parámetros en texto como `"101"` (para el identificador del monitor) o `"2000"` (para el aforo del centro).

Pau ha intentado convertir ese texto en un entero aplicando el casting que aprendimos en el Sprint 1:

```java
String textoTerminal = "101";
int terminalId = (int) textoTerminal; // Error de compilación inmediato
```

IntelliJ detiene la compilación en seco con el mensaje: *«Inconvertible types; cannot cast 'java.lang.String' to 'int'»*.
Pau comenta desconcertado:
> *«Pensaba que con el casting entre paréntesis podía convertir cualquier dato. ¿Por qué no puedo transformar un texto que contiene números en un entero?»*.

**Alba Torres** y **Laia Claramunt** se acercan al monitor:
> *«Pau, el casting con `(int)` o `(double)` solo funciona entre tipos numéricos primitivos porque la CPU simplemente reinterpreta los bits en la memoria Stack. Pero una cadena `String` es un objeto complejo en el Heap que contiene caracteres tipográficos.*
>
> *Para convertir texto en magnitudes numéricas reales necesitamos **parsear (*parsing*)**, es decir, analizar la cadena carácter a carácter y reconstruir su valor numérico en memoria.*
>
> *Para ello, Java proporciona las **clases envoltorio (*Wrappers*)**: `Integer`, `Double`, `Boolean` y `Character`. Hoy aprenderemos a utilizar sus métodos estáticos de parseo (`parseInt`, `parseDouble`), a consultar sus constantes de límite de memoria (`MAX_VALUE`) y evolucionaremos `ControlAccesoQR` a la versión **v1.6»***.

---

#### 2. Fundamento teórico: clases envoltorio (*wrappers*), parseo y utilidades de caracteres

```
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                        TIPOS PRIMITIVOS Y SUS CLASES ENVOLTORIO                        │
├─────────────────────┬───────────────────────┬──────────────────────────────────────────┤
│ Tipo Primitivo      │ Clase Envoltorio      │ Método estático de parseo (de String a X)│
│ (En memoria Stack)  │ (Objeto en java.lang) │                                          │
├─────────────────────┼───────────────────────┼──────────────────────────────────────────┤
│ int                 │ Integer               │ Integer.parseInt("101")    -> 101        │
├─────────────────────┼───────────────────────┼──────────────────────────────────────────┤
│ double              │ Double                │ Double.parseDouble("21.5") -> 21.5       │
├─────────────────────┼───────────────────────┼──────────────────────────────────────────┤
│ boolean             │ Boolean               │ Boolean.parseBoolean("true")-> true      │
├─────────────────────┼───────────────────────┼──────────────────────────────────────────┤
│ char                │ Character             │ Character.toUpperCase('a') -> 'A'        │
└─────────────────────┴───────────────────────┴──────────────────────────────────────────┘
```

##### A. ¿Qué es una clase envoltorio (*wrapper class*)?
Java mantiene una separación estricta entre tipos primitivos (que no son objetos y no tienen métodos) y el mundo de la POO. Para permitir que los primitivos puedan interactuar con utilidades de objetos, Java incluye en `java.lang` una **clase envoltorio para cada tipo primitivo**.

Pertenecen a `java.lang`, por lo que **se importan automáticamente** sin necesidad de `import`.

##### B. Métodos estáticos de parseo: `Integer.parseInt()` y `Double.parseDouble()`
El proceso de **parsear (*parse*)** consiste en tomar una cadena de texto formada por dígitos alfanuméricos y traducirla a su representación numérica binaria en la memoria:

```java
String textoAforo = "2000";
int aforoTotal = Integer.parseInt(textoAforo); // Devuelve el primitivo int 2000

String textoTemp = "21.75";
double temperatura = Double.parseDouble(textoTemp); // Devuelve el primitivo double 21.75
```

> **Aviso sobre excepciones** Si la cadena contiene caracteres no numéricos (*por ejemplo, `Integer.parseInt("101A")`*), Java lanzará un error en tiempo de ejecución llamado `NumberFormatException`. Durante este Sprint 2 trabajamos con cadenas con formato numérico correcto; la captura y control formal de estas excepciones se estudiará en el **Sprint 3 (RA3)** mediante bloques `try-catch`.

##### C. Constantes de límite de memoria: `MAX_VALUE` y `MIN_VALUE`
Las clases envoltorio exponen constantes públicas que nos permiten consultar los límites físicos absolutos que puede almacenar cada tipo sin desbordar la memoria:
* `Integer.MAX_VALUE`: $2.147.483.647$ (el número entero más alto representable en 32 bits).
* `Integer.MIN_VALUE`: $-2.147.483.648$.
* `Double.MAX_VALUE`: Aprox. $1.79 \times 10^{308}$.

##### D. Métodos de utilidad de la clase `Character`
La clase envoltorio `Character` ofrece métodos estáticos indispensables para manipular caracteres individuales sin necesidad de tablas ASCII manuales:
* `Character.toUpperCase(char c)`: Convierte un carácter a mayúscula (*ej. `'e'` $\rightarrow$ `'E'`*).
* `Character.toLowerCase(char c)`: Convierte un carácter a minúscula.
* `Character.isDigit(char c)`: Devuelve un `boolean` indicando si el carácter es un dígito numérico entre `'0'` y `'9'`.
* `Character.isLetter(char c)`: Devuelve un `boolean` indicando si el carácter es una letra alfabética.

---

#### 3. Evolución del caso guía a `ControlAccesoQR v1.6`

Ampliamos el programa incorporando el uso de clases envoltorio:
1. Simulamos que la configuración del terminal llega empaquetada como texto (`"101"` y `"2000"`) y la parseamos a variables enteras con **`Integer.parseInt()`**.
2. Normalizamos el perfil de acceso del usuario (`perfilPersona`) convirtiéndolo automáticamente a mayúsculas mediante **`Character.toUpperCase()`**.
3. Verificamos con **`Character.isLetter()`** si el perfil introducido es un carácter alfabético válido.
4. Consultamos **`Integer.MAX_VALUE`** en la telemetría del terminal para certificar el límite máximo teórico de registros antes de desbordar la memoria del contador.

##### Paso A. Algoritmo en PSeInt (`pr/pseudocodigo/ControlAccesoQR.psc` — v1.6)

```psc
Algoritmo ControlAccesoQR
    // =========================================================================
    // SISTEMA DE CONTROL DE ASISTENCIA QR - IES EL CAMINAS (Castellon)
    // Version: 1.6 (Evolucion: Parseo de Cadenas y Normalizacion de Caracteres)
    // =========================================================================

    Definir NOMBRE_CENTRO, PREFIJO_CENTRO, RUTA_LOGS Como Cadena
    Definir MINUTOS_POR_HORA, SEGUNDOS_POR_HORA, SEGUNDOS_POR_MINUTO Como Entero
    Definir FACTOR_PORCENTAJE Como Real
    Definir SEMILLA_CALIBRACION Como Entero
    Definir JORNADA_BASE_MINUTOS Como Entero
    Definir TEMP_MIN_SEGURA, TEMP_MAX_SEGURA Como Real
    
    NOMBRE_CENTRO <- "IES El Caminas (Castellon)"
    PREFIJO_CENTRO <- "CAMINAS"
    RUTA_LOGS <- ".\caminas\terminal\logs"
    MINUTOS_POR_HORA <- 60
    SEGUNDOS_POR_HORA <- 3600
    SEGUNDOS_POR_MINUTO <- 60
    FACTOR_PORCENTAJE <- 100.0
    SEMILLA_CALIBRACION <- 987654321
    JORNADA_BASE_MINUTOS <- 480
    TEMP_MIN_SEGURA <- 15.0
    TEMP_MAX_SEGURA <- 35.0

    // [NUEVO DÍA 19] Variables de configuración recibidas en texto para parsear
    Definir textoTerminalId, textoAforoConfig Como Cadena
    Definir terminalId Como Entero
    Definir tempVestibulo Como Real
    
    Definir nombrePersona, dniPersona Como Cadena
    Definir perfilPersona, perfilNormalizado Como Caracter
    Definir esEntrada, esPerfilValido Como Logico
    
    Definir horaEntrada, minutoEntrada, horaSalida, minutoSalida Como Entero
    Definir minutosTotalesEntrada, minutosTotalesSalida, minutosEstanciaTotal Como Entero
    Definir tokenResumenVestibulo, tokenResumenPuertaNorte Como Cadena
    
    Definir personasEnCentro, aforoDisponible, idUltimoFichaje Como Entero
    personasEnCentro <- 1200
    aforoDisponible <- 800
    idUltimoFichaje <- 1042
    
    Definir segundosActividadTerminal Como Entero
    Definir horasUptime, minutosUptime, segundosUptime Como Entero
    Definir aforoTotal Como Entero
    Definir porcentajeOcupacionReal Como Real
    Definir porcentajeOcupacionTruncado, porcentajeOcupacionRedondeado Como Entero

    Definir codigoTokenVestibulo, codigoTokenPuertaNorte Como Entero
    Definir codigoTestCalibracion Como Entero
    Definir sensorNfcOperativo Como Logico
    Definir desviacionJornadaMinutos Como Entero
    Definir aforoSeguroVisualizado Como Entero
    Definir coordenadaXMetros, coordenadaYMetros, distanciaDiagonalAntena Como Real

    Escribir "AZAHARTECH: TERMINAL ", NOMBRE_CENTRO, " (v1.6 - parseo y wrappers)"
    
    // [NUEVO DÍA 19] Entrada de configuración como texto plano simulado
    Escribir "ID del terminal:"
    Leer textoTerminalId
    // En PSeInt la función ConvertirANumero equivale a Integer.parseInt()
    terminalId <- ConvertirANumero(textoTerminalId)
    
    Escribir "Temperatura base del sensor (ºC):"
    Leer tempVestibulo
    Escribir "Segundos de actividad del terminal (uptime):"
    Leer segundosActividadTerminal
    
    Escribir "Distancia horizontal al torno en metros (eje X):"
    Leer coordenadaXMetros
    Escribir "Altura del sensor en metros (eje Y):"
    Leer coordenadaYMetros
    
    Escribir "DNI de la persona:"
    Leer dniPersona
    Escribir "Nombre completo:"
    Leer nombrePersona
    Escribir "Perfil de acceso (e = estudiante, d = docente, v = visita):"
    Leer perfilPersona

    // [NUEVO DÍA 19] Normalización a mayúsculas
    perfilNormalizado <- Mayusculas(perfilPersona)
    esPerfilValido <- Verdadero

    esEntrada <- Verdadero
    
    Escribir "Introduce hora y minuto de entrada (por ejemplo, 8 15):"
    Leer horaEntrada
    Leer minutoEntrada
    Escribir "Introduce hora y minuto de salida (por ejemplo, 14 10):"
    Leer horaSalida
    Leer minutoSalida
        
    minutosTotalesEntrada <- (horaEntrada * MINUTOS_POR_HORA) + minutoEntrada
    minutosTotalesSalida <- (horaSalida * MINUTOS_POR_HORA) + minutoSalida
    minutosEstanciaTotal <- minutosTotalesSalida - minutosTotalesEntrada
    
    personasEnCentro <- personasEnCentro + 1
    aforoDisponible <- aforoDisponible - 1
    idUltimoFichaje <- idUltimoFichaje + 1

    codigoTokenVestibulo <- Aleatorio(1000, 9999)
    codigoTokenPuertaNorte <- Aleatorio(1000, 9999)
    codigoTestCalibracion <- 5555
    fluctuacionTermica <- 0.45
    tempCalibrada <- tempVestibulo + fluctuacionTermica
    sensorNfcOperativo <- Verdadero
    
    desviacionJornadaMinutos <- abs(minutosEstanciaTotal - JORNADA_BASE_MINUTOS)
    
    Si tempCalibrada > TEMP_MAX_SEGURA Entonces
        tempSeguraAcotada <- TEMP_MAX_SEGURA
    SiNo
        Si tempCalibrada < TEMP_MIN_SEGURA Entonces
            tempSeguraAcotada <- TEMP_MIN_SEGURA
        SiNo
            tempSeguraAcotada <- tempCalibrada
        FinSi
    FinSi
      
    tokenResumenVestibulo <- PREFIJO_CENTRO + "-" + dniPersona + "-T" + ConvertirATexto(terminalId) + "-SEC#" + ConvertirATexto(codigoTokenVestibulo) + "-REG" + ConvertirATexto(idUltimoFichaje)
    tokenResumenPuertaNorte <- PREFIJO_CENTRO + "-" + dniPersona + "-T102-SEC#" + ConvertirATexto(codigoTokenPuertaNorte) + "-REG" + ConvertirATexto(idUltimoFichaje)
    
    horasUptime <- trunc(segundosActividadTerminal / SEGUNDOS_POR_HORA)
    minutosUptime <- trunc((segundosActividadTerminal MOD SEGUNDOS_POR_HORA) / SEGUNDOS_POR_MINUTO)
    segundosUptime <- segundosActividadTerminal MOD SEGUNDOS_POR_MINUTO
    
    aforoTotal <- personasEnCentro + aforoDisponible
    aforoSeguroVisualizado <- personasEnCentro
    porcentajeOcupacionReal <- (aforoSeguroVisualizado * FACTOR_PORCENTAJE) / aforoTotal
    porcentajeOcupacionTruncado <- trunc(porcentajeOcupacionReal)
    porcentajeOcupacionRedondeado <- redon(porcentajeOcupacionReal)
    
    distanciaDiagonalAntena <- rc((coordenadaXMetros ^ 2) + (coordenadaYMetros ^ 2))
    
    Escribir ""
    Escribir "======================================================================"
    Escribir "             INFORME OFICIAL DE ACCESO EN VESTIBULO                   "
    Escribir "======================================================================"
    Escribir "CENTRO:             ", NOMBRE_CENTRO
    Escribir "TERMINAL PARSEADO:  #", terminalId, " (Origen texto: '", textoTerminalId, "')"
    Escribir "REGISTRO N. :       #", idUltimoFichaje
    Escribir "TOKEN VESTIBULO:    ", tokenResumenVestibulo
    Escribir "TOKEN PUERTA NORTE: ", tokenResumenPuertaNorte
    Escribir "PERSONA:            ", nombrePersona
    Escribir "PERFIL PROCESADO:   ", perfilNormalizado, " [Original: ", perfilPersona, "]"
    Escribir "IDENTIFICADOR:      ", dniPersona, " | SENTIDO: Entrada (", esEntrada, ")"
    Escribir "----------------------------------------------------------------------"
    Escribir "HORARIO:            Entrada ", horaEntrada, ":", minutoEntrada, " | Salida ", horaSalida, ":", minutoSalida
    Escribir "PERMANENCIA:        ", minutosEstanciaTotal, " minutos en el centro."
    Escribir "DESVIACION JORNADA: ", desviacionJornadaMinutos, " min respecto a jornada completa (480 min)."
    Escribir "OCUPACION REDONDEAD:", porcentajeOcupacionRedondeado, " %"
    Escribir "DISTANCIA ANTENA:   ", distanciaDiagonalAntena, " metros"
    Escribir "ACTIVO:             ", horasUptime, "h ", minutosUptime, "m ", segundosUptime, "s"
    Escribir "REGISTRO LOG:       ", RUTA_LOGS
    Escribir "======================================================================"
FinAlgoritmo
```

##### Paso B. Implementación en Java (`pr/src/ControlAccesoQR.java` — v1.6)

```java
/**
 * SISTEMA DE CONTROL DE ASISTENCIA POR CÓDIGO QR
 * Cliente: IES El Caminàs (Castellón de la Plana)
 * Consultora: AzaharTech Software Consulting
 * 
 * Versión 1.6:
 * Clases envoltorio (wrappers), parseo de texto con Integer.parseInt(),
 * utilidades de Character y constantes de límite de memoria (Integer.MAX_VALUE).
 *
 * @author Equipo AzaharTech (Alba Torres, Pau Ferrer)
 * @version 1.6 (Octubre 2026)
 * @since JDK 21 LTS
 *
 */
import java.util.Scanner;
import java.util.Random;

public class ControlAccesoQR {
    public static void main(String[] args) {
        // Constantes inmutables
        final String NOMBRE_CENTRO = "IES El Caminàs (Castellón)";
        final String PREFIJO_CENTRO = "CAMINAS";
        final String RUTA_LOGS = ".\\caminas\\terminal\\logs";
        final int MINUTOS_POR_HORA = 60;
        final int SEGUNDOS_POR_HORA = 3600;
        final int SEGUNDOS_POR_MINUTO = 60;
        final double FACTOR_PORCENTAJE = 100.0;
        final int BASE_TOKEN_SEGURIDAD = 1000;
        final int RANGO_TOKEN_SEGURIDAD = 9000;
        final double MAX_VARIACION_TERMICA = 0.5;
        final long SEMILLA_CALIBRACION = 987654321L;
        final int JORNADA_BASE_MINUTOS = 480;
        final double TEMP_MIN_SEGURA = 15.0;
        final double TEMP_MAX_SEGURA = 35.0;
        
        
        Scanner teclado = new Scanner(System.in);

        Random generadorVestibulo   = new Random();
        Random generadorPuertaNorte = new Random();
        Random generadorCalibracion = new Random(SEMILLA_CALIBRACION);

        // [NUEVO DÍA 19] Variables de configuración recibidas en texto plano para parsear
        String textoTerminalId;
        int terminalId;
        double tempVestibulo;

        String nombrePersona, dniPersona;
        char perfilPersonaBruto;
        char perfilNormalizado; // Normalizado con Character.toUpperCase
        boolean esLetraValida;
        boolean esEntrada;

        int horaEntrada, minutoEntrada;
        int horaSalida, minutoSalida;
        int minutosTotalesEntrada, minutosTotalesSalida, minutosEstanciaTotal;
        String tokenResumenVestibulo;
        String tokenResumenPuertaNorte;

        int personasEnCentro = 1200;
        int aforoDisponible = 800;
        int idUltimoFichaje = 1042;

        int segundosActividadTerminal;
        int horasUptime, minutosUptime, segundosUptime;

        int aforoTotal;
        double porcentajeOcupacionReal;
        int porcentajeOcupacionTruncado;
        int porcentajeOcupacionRedondeado;

        double coordenadaXMetros;
        double coordenadaYMetros;
        double distanciaDiagonalAntena;

        int codigoTokenVestibulo;
        int codigoTokenPuertaNorte;
        int codigoTestCalibracion;
        double fluctuacionTermica;
        double tempCalibrada;
        boolean sensorNfcOperativo;
        int desviacionJornadaMinutos;
        double tempSeguraAcotada;
        int aforoSeguroVisualizado;

        System.out.println("=================================================");
        System.out.println("   AZAHARTECH - TERMINAL " + NOMBRE_CENTRO);
        System.out.println("   Versión 1.6 (wrappers y parseo de cadenas)    ");
        System.out.println("=================================================");
        
        // [NUEVO DÍA 19] Captura en texto y conversión mediante Integer.parseInt()
        System.out.print("ID del terminal: ");
        textoTerminalId = teclado.nextLine();
        terminalId = Integer.parseInt(textoTerminalId); // Parseo estático de String a int

        System.out.print("Temperatura base del sensor (ºC): ");
        tempVestibulo = teclado.nextDouble();

        System.out.print("Segundos de actividad del terminal (uptime): ");
        segundosActividadTerminal = teclado.nextInt();

        System.out.print("Distancia horizontal al torno en metros (eje X): ");
        coordenadaXMetros = teclado.nextDouble();

        System.out.print("Altura del sensor en metros (eje Y): ");
        coordenadaYMetros = teclado.nextDouble();

        teclado.nextLine(); // Limpieza obligatoria del buffer

        System.out.print("DNI de la persona: ");
        dniPersona = teclado.nextLine();

        System.out.print("Nombre completo: ");
        nombrePersona = teclado.nextLine();

        System.out.print("Perfil de acceso (e = estudiante, d = docente, v = visita): ");
        perfilPersonaBruto = teclado.next().charAt(0);

        // [NUEVO DÍA 19] Utilidades estáticas de la clase envoltorio Character
        perfilNormalizado = Character.toUpperCase(perfilPersonaBruto); // Pasa a mayúscula
        esLetraValida = Character.isLetter(perfilNormalizado);         // Verifica que es alfabético

        esEntrada = true;

        System.out.print("Hora de entrada (0-23): ");
        horaEntrada = teclado.nextInt();

        System.out.print("Minuto de entrada (0-59): ");
        minutoEntrada = teclado.nextInt();

        System.out.print("Hora de salida (0-23): ");
        horaSalida = teclado.nextInt();

        System.out.print("Minuto de salida (0-59): ");
        minutoSalida = teclado.nextInt();

        // Procesamiento secuencial
        minutosTotalesEntrada = (horaEntrada * MINUTOS_POR_HORA) + minutoEntrada;
        minutosTotalesSalida = (horaSalida * MINUTOS_POR_HORA) + minutoSalida;
        minutosEstanciaTotal = minutosTotalesSalida - minutosTotalesEntrada;

        personasEnCentro++;
        aforoDisponible--;
        idUltimoFichaje++;

        codigoTokenVestibulo   = BASE_TOKEN_SEGURIDAD + generadorVestibulo.nextInt(RANGO_TOKEN_SEGURIDAD);
        codigoTokenPuertaNorte = BASE_TOKEN_SEGURIDAD + generadorPuertaNorte.nextInt(RANGO_TOKEN_SEGURIDAD);

        codigoTestCalibracion = BASE_TOKEN_SEGURIDAD + generadorCalibracion.nextInt(RANGO_TOKEN_SEGURIDAD);
        fluctuacionTermica = generadorCalibracion.nextDouble() * MAX_VARIACION_TERMICA;
        tempCalibrada = tempVestibulo + fluctuacionTermica;
        sensorNfcOperativo = generadorCalibracion.nextBoolean();

        desviacionJornadaMinutos = Math.abs(minutosEstanciaTotal - JORNADA_BASE_MINUTOS);
        tempSeguraAcotada = Math.max(TEMP_MIN_SEGURA, Math.min(tempCalibrada, TEMP_MAX_SEGURA));

        aforoTotal = personasEnCentro + aforoDisponible;
        aforoSeguroVisualizado = Math.min(personasEnCentro, aforoTotal);

        tokenResumenVestibulo = PREFIJO_CENTRO + "-" + dniPersona;
        tokenResumenVestibulo += "-T" + terminalId;
        tokenResumenVestibulo += "-SEC#" + codigoTokenVestibulo;
        tokenResumenVestibulo += "-ESTANCIA-" + minutosEstanciaTotal;
        tokenResumenVestibulo += "-REG" + idUltimoFichaje;

        tokenResumenPuertaNorte = PREFIJO_CENTRO + "-" + dniPersona;
        tokenResumenPuertaNorte += "-T102";
        tokenResumenPuertaNorte += "-SEC#" + codigoTokenPuertaNorte;
        tokenResumenPuertaNorte += "-ESTANCIA-" + minutosEstanciaTotal;
        tokenResumenPuertaNorte += "-REG" + idUltimoFichaje;

        horasUptime = segundosActividadTerminal / SEGUNDOS_POR_HORA;
        minutosUptime = (segundosActividadTerminal % SEGUNDOS_POR_HORA) / SEGUNDOS_POR_MINUTO;
        segundosUptime = segundosActividadTerminal % SEGUNDOS_POR_MINUTO;

        porcentajeOcupacionReal = ((double) aforoSeguroVisualizado / aforoTotal) * FACTOR_PORCENTAJE;
        porcentajeOcupacionTruncado = (int) porcentajeOcupacionReal;
        porcentajeOcupacionRedondeado = (int) Math.round(porcentajeOcupacionReal);

        distanciaDiagonalAntena = Math.sqrt(Math.pow(coordenadaXMetros, 2.0) + Math.pow(coordenadaYMetros, 2.0));

        // Salida formateada con printf
        System.out.println("\n======================================================================");
        System.out.println("             INFORME OFICIAL DE ACCESO EN VESTÍBULO                   ");
        System.out.println("======================================================================");
        System.out.printf("CENTRO:               %-30s%n", NOMBRE_CENTRO);
        System.out.printf("TERMINAL PARSEADO:    #%04d (Dato origen texto: '%s')%n", terminalId, textoTerminalId);
        System.out.printf("REGISTRO N.º:         #%05d%n", idUltimoFichaje);
        System.out.printf("TOKEN QR VESTÍBULO:   %s%n", tokenResumenVestibulo);
        System.out.printf("TOKEN QR PUERTA NORTE:%s%n", tokenResumenPuertaNorte);
        System.out.printf("PERSONA:              %-30s | PERFIL NORMALIZADO: %c%n", nombrePersona, perfilNormalizado);
        System.out.printf("PERFIL VÁLIDO:        %b (isLetter verificado)%n", esLetraValida);
        System.out.printf("IDENTIFICADOR:        %-12s | SENTIDO: Entrada (%b)%n", dniPersona, esEntrada);
        System.out.println("----------------------------------------------------------------------");
        System.out.printf("HORARIO:              Entrada %02d:%02d | Salida %02d:%02d%n", horaEntrada, minutoEntrada, horaSalida, minutoSalida);
        System.out.printf("PERMANENCIA:          %03d minutos en el centro.%n", minutosEstanciaTotal);
        System.out.printf("DESVIACIÓN JORNADA:   %03d min respecto a jornada completa (480 min).%n", desviacionJornadaMinutos);
        System.out.println("----------------------------------------------------------------------");
        System.out.printf("OCUPACIÓN REDONDEADA: %d %% (Math.round)%n", porcentajeOcupacionRedondeado);
        System.out.printf("DISTANCIA A ANTENA:   %.2f metros%n", distanciaDiagonalAntena);
        System.out.printf("ACTIVO:               %02dh %02dm %02ds (Sensor: %.1f ºC)%n", horasUptime, minutosUptime, segundosUptime, tempSeguraAcotada);
        System.out.printf("LÍMITE BUFFER (WRAP): %d (Integer.MAX_VALUE)%n", Integer.MAX_VALUE);
        System.out.printf("REGISTRO LOG:         %s%n", RUTA_LOGS);
        System.out.println("======================================================================");

        teclado.close();
    }
}
```

---

#### 4. Segunda sesión: dojo de entrenamiento y katas de código

Pasamos al dojo de entrenamiento para programar, con tres **katas de dificultad progresiva** sobre clases envoltorio, métodos de parseo y constantes de rango:

---

##### Kata 1 (Cinturón blanco / Nivel base). Parseo de entradas con `Integer.parseInt()` y `Double.parseDouble()`
* **Objetivo:** Capturar al menos un dato numérico de tu proyecto propio en formato de texto plano (`String`) y transformarlo a primitivo mediante métodos de parseo de las clases *Wrapper*.
* **Aplicación según tu proyecto elegido:**
  * En **Aventura conversacional**: capturar el código de armadura como texto (`"75"`) y parsearlo a `int` con `Integer.parseInt()`, y capturar la bonificación de peso (`"1.25"`) y parsearla con `Double.parseDouble()`.
  * En **Motor de recomendación**: capturar el código de año de estreno en texto (`"2024"`) y transformarlo con `Integer.parseInt()`, y parsear la puntuación de crítica externa (`"8.75"`) con `Double.parseDouble()`.
  * En **Simulador de físicas 2D**: capturar el valor de fricción en texto (`"0.05"`) y parsearlo con `Double.parseDouble()`, y parsear el número de partículas del lote con `Integer.parseInt()`.
  * En **Bóveda de contraseñas**: capturar el tiempo de caducidad en texto (`"90"`) y transformarlo con `Integer.parseInt()`, y parsear el coeficiente de fortaleza con `Double.parseDouble()`.

---

##### Kata 2 (Cinturón marrón / Nivel avanzado). Inspección y normalización con la clase `Character`
* **Contexto técnico:** Demostrar el uso de utilidades estáticas de caracteres para limpiar entradas sin usar condicionales.
* **Misión de la kata:**
  1. En una clase de prueba llamada `PruebaCaracteres.java`, solicita al usuario un código de autorización compuesto por un único carácter (por ejemplo, una letra de zona o sector).
  2. Utiliza `Character.toUpperCase(c)` para normalizar el carácter a mayúscula automáticamente.
  3. Utiliza `Character.isLetter(c)` y `Character.isDigit(c)` asignando los resultados a dos variables booleanas independientes.
  4. Imprime por consola:
     ```java
     System.out.printf("Carácter procesado: '%c'%n", caracterNormalizado);
     System.out.printf("¿Es alfabético?: %b | ¿Es numérico?: %b%n", esAlfabetico, esNumerico);
     ```
  5. Prueba el programa introduciendo una minúscula (`'a'`), un número (`'7'`) y un símbolo especial (`'@'`), verificando cómo responden los métodos booleanos.

---

##### Kata 3 (Cinturón negro / «Hacker AzaharTech»). El desbordamiento de `Integer.MAX_VALUE`
* **Contexto de sistemas:** En ingeniería del software, ignorar los límites de las clases envoltorio provoca graves fallos de desbordamiento (*overflow*).
* **Misión de la kata:**
  1. En una clase de prueba temporal llamada `PruebaDesbordamiento.java`, escribe el siguiente experimento:
     ```java
     int topeMaximo = Integer.MAX_VALUE;
     System.out.println("Valor máximo de Integer:  " + topeMaximo);

     // Provocamos un desbordamiento sumando 1
     int desbordado = topeMaximo + 1;
     System.out.println("Valor tras sumar 1 (+1):  " + desbordado);
     ```
  2. Ejecuta el programa y comprueba que al sumar 1 al valor máximo positivo ($2.147.483.647$), la variable pasa automáticamente a valer el límite negativo más extremo ($-2.147.483.648$).
  3. Redacta dos líneas de comentario en tu código explicando por qué ocurre esto: **Java utiliza aritmética modular de complemento a dos con 32 bits, por lo que desbordar el bit de signo convierte el número positivo más alto en el número negativo más bajo**.

---

### Día 20 - 2 sesiones

---

#### 1. Caso guía en AzaharTech
Son las tres de la tarde del lunes 19 de octubre. Arranca la tercera y última semana del Sprint 2. En la sala de desarrollo de **AzaharTech**, **Laia Claramunt** y **Pau Ferrer** analizan las cadenas de texto que envían los teléfonos móviles de los estudiantes al cazar el código QR proyectado en las pantallas de TV del **IES El Caminàs**.

Pau muestra un problema recurrente detectado en las pruebas de campo:
> *«Muchos estudiantes introducen su DNI con errores tipográficos: algunos teclean 8 caracteres olvidando la letra final, otros añaden espacios accidentales al principio o al final, y en ocasiones el identificador no contiene el formato esperado.*
>
> *Hasta ahora hemos tratado las cadenas `String` como simples contenedores de texto que leíamos con `Scanner` y uníamos con el operador `+`. Pero necesitamos inspeccionar el texto por dentro: saber cuántos caracteres tiene exactamente, extraer la letra final de forma automática y localizar la posición de los guiones separadores»*.

**Alba Torres** interviene para enfocar la sesión:
> *«En Java, `String` no es un tipo primitivo; es una **clase de objetos predefinida** extraordinariamente potente que reside en el paquete `java.lang`. Cada texto que declaramos es un objeto en el Heap con un arsenal de métodos para inspeccionar su contenido.*
>
> *Hoy aprenderemos cómo gestiona Java las cadenas en la memoria, qué significa que un `String` sea **inmutable** y dominaremos los métodos de inspección esenciales: **`length()`**, **`charAt()`**, **`isEmpty()`**, **`contains()`** e **`indexOf()`**.*
>
> *Evolucionaremos `ControlAccesoQR` a la versión **v1.7**, auditando la longitud y los caracteres del DNI y del token de forma 100 % secuencial»*.

---

#### 2. Fundamento teórico: la clase `String`, inmutabilidad y métodos de inspección

```
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                        ANATOMÍA DE UNA CADENA EN MEMORIA (ÍNDICES)                     │
├────────────────────────────────────────────────────────────────────────────────────────┤
│ String dni = "45123789K";                                                              │
│                                                                                        │
│ Carácter:  [ '4' ][ '5' ][ '1' ][ '2' ][ '3' ][ '7' ][ '8' ][ '9' ][ 'K' ]             │
│ Índice:       0      1      2      3      4      5      6      7      8                │
│                                                                                        │
│ • Longitud total (length): 9 caracteres.                                               │
│ • Primer carácter:         dni.charAt(0)                -> '4'                         │
│ • Último carácter:         dni.charAt(dni.length() - 1) -> 'K'                         │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

##### A. Inmutabilidad de la clase `String`
En Java, los objetos de la clase `String` son **inmutables**: una vez que se crea una cadena en la memoria Heap, **su contenido jamás puede ser modificado**.
* Cuando realizamos una concatenación como `texto += " nuevo"`, Java no altera el objeto original: crea un **objeto completamente nuevo** en el Heap con el texto resultante y reasigna la variable de referencia para que apunte al nuevo bloque.
* Los métodos de la clase `String` nunca modifican la cadena original; realizan la operación y devuelven un nuevo resultado.

##### B. Métodos fundamentales de inspección y tamaño
Al pertenecer a `java.lang`, no requieren ninguna sentencia `import`:

```
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                        MÉTODOS DE INSPECCIÓN DE LA CLASE STRING                        │
├──────────────────────────┬───────────────────┬─────────────────────────────────────────┤
│ Firma del método         │ Tipo de retorno   │ Comportamiento técnico                  │
├──────────────────────────┼───────────────────┼─────────────────────────────────────────┤
│ cadena.length()          │ int               │ Devuelve el número total de caracteres. │
├──────────────────────────┼───────────────────┼─────────────────────────────────────────┤
│ cadena.charAt(int index) │ char              │ Devuelve el carácter en la posición     │
│                          │                   │ especificada (índice base 0).           │
├──────────────────────────┼───────────────────┼─────────────────────────────────────────┤
│ cadena.isEmpty()         │ boolean           │ Devuelve true si length() es igual a 0. │
├──────────────────────────┼───────────────────┼─────────────────────────────────────────┤
│ cadena.contains(texto)   │ boolean           │ Devuelve true si la cadena contiene la  │
│                          │                   │ subsecuencia de texto buscada.          │
├──────────────────────────┼───────────────────┼─────────────────────────────────────────┤
│ cadena.indexOf(texto)    │ int               │ Devuelve la posición de la primera      │
│                          │                   │ aparición. Si no existe, devuelve -1.   │
└──────────────────────────┴───────────────────┴─────────────────────────────────────────┘
```

##### C. La regla del índice base 0 y la extracción del último carácter
En Java, los índices de las cadenas comienzan siempre en **`0`**:
* El primer carácter reside en el índice `0`.
* El último carácter reside obligatoriamente en el índice **`cadena.length() - 1`**.
* **Peligro con los límites.** Intentar acceder a `cadena.charAt(cadena.length())` provoca una excepción crítica en tiempo de ejecución: **`StringIndexOutOfBoundsException`**, ya que ese índice queda fuera del rango asignado.

##### D. Evaluación con banderas booleanas
Podemos evaluar la validez estructural de una cadena asignando el resultado de las comparaciones directamente a variables `boolean`:

```java
// Comprobación de longitud exacta de 9 caracteres
boolean longitudDniCorrecta = (dniPersona.length() == 9);

// Comprobación de existencia del prefijo mediante indexOf
boolean contieneSeparador = (tokenResumen.indexOf("-") != -1);
```

---

#### 3. Evolución del caso guía a `ControlAccesoQR v1.7`

Ampliamos el programa guía incorporando métodos de inspección de cadenas:
1. Calculamos la longitud exacta del nombre y del DNI de la persona mediante **`length()`**.
2. Extraemos automáticamente la letra final del DNI mediante **`charAt(length() - 1)`**.
3. Verificamos mediante **`contains()`** que el token del vestíbulo contiene el prefijo oficial `"CAMINAS"`.
4. Localizamos mediante **`indexOf()`** la posición del primer guion separador del token.
5. Evaluamos con una expresión booleana si la longitud del DNI cumple la norma oficial de 9 caracteres.

##### Paso A. Algoritmo en PSeInt (`pr/pseudocodigo/ControlAccesoQR.psc` — v1.7)

```psc
Algoritmo ControlAccesoQR
    // =========================================================================
    // SISTEMA DE CONTROL DE ASISTENCIA QR - IES EL CAMINAS (Castellon)
    // Version: 1.7 (Evolucion: inspeccion de cadenas y extraccion de caracteres)
    // =========================================================================

    Definir NOMBRE_CENTRO, PREFIJO_CENTRO, RUTA_LOGS Como Cadena
    Definir MINUTOS_POR_HORA, SEGUNDOS_POR_HORA, SEGUNDOS_POR_MINUTO Como Entero
    Definir FACTOR_PORCENTAJE Como Real
    Definir SEMILLA_CALIBRACION Como Entero
    Definir JORNADA_BASE_MINUTOS Como Entero
    Definir TEMP_MIN_SEGURA, TEMP_MAX_SEGURA Como Real
    
    // [NUEVO DÍA 20] Constante para longitud oficial del DNI
    Definir LONGITUD_DNI_ESTANDAR Como Entero
    
    NOMBRE_CENTRO <- "IES El Caminas (Castellon)"
    PREFIJO_CENTRO <- "CAMINAS"
    RUTA_LOGS <- ".\caminas\terminal\logs"
    MINUTOS_POR_HORA <- 60
    SEGUNDOS_POR_HORA <- 3600
    SEGUNDOS_POR_MINUTO <- 60
    FACTOR_PORCENTAJE <- 100.0
    SEMILLA_CALIBRACION <- 987654321
    JORNADA_BASE_MINUTOS <- 480
    TEMP_MIN_SEGURA <- 15.0
    TEMP_MAX_SEGURA <- 35.0
    LONGITUD_DNI_ESTANDAR <- 9

    Definir textoTerminalId Como Cadena
    Definir terminalId Como Entero
    Definir tempVestibulo Como Real
    
    Definir nombrePersona, dniPersona Como Cadena
    Definir perfilPersona, perfilNormalizado Como Caracter
    Definir esEntrada Como Logico
    
    Definir horaEntrada, minutoEntrada, horaSalida, minutoSalida Como Entero
    Definir minutosTotalesEntrada, minutosTotalesSalida, minutosEstanciaTotal Como Entero
    Definir tokenResumenVestibulo, tokenResumenPuertaNorte Como Cadena
    
    Definir personasEnCentro, aforoDisponible, idUltimoFichaje Como Entero
    personasEnCentro <- 1200
    aforoDisponible <- 800
    idUltimoFichaje <- 1042
    
    Definir segundosActividadTerminal Como Entero
    Definir horasUptime, minutosUptime, segundosUptime Como Entero
    Definir aforoTotal Como Entero
    Definir porcentajeOcupacionReal Como Real
    Definir porcentajeOcupacionTruncado, porcentajeOcupacionRedondeado Como Entero

    Definir codigoTokenVestibulo, codigoTokenPuertaNorte Como Entero
    Definir codigoTestCalibracion Como Entero
    Definir sensorNfcOperativo Como Logico
    Definir desviacionJornadaMinutos Como Entero
    Definir aforoSeguroVisualizado Como Entero
    Definir coordenadaXMetros, coordenadaYMetros, distanciaDiagonalAntena Como Real

    // [NUEVO DÍA 20] Variables para inspección de cadenas
    Definir longitudDni, longitudNombre Como Entero
    Definir letraFinalDni Como Caracter
    Definir esLongitudDniCorrecta Como Logico
    Definir posicionPrimerGuion Como Entero

    Escribir "AZAHARTECH: TERMINAL ", NOMBRE_CENTRO, " (v1.7 - Inspeccion String)"
    Escribir "ID del terminal:"
    Leer textoTerminalId
    terminalId <- ConvertirANumero(textoTerminalId)
    
    Escribir "Temperatura base del sensor (ºC):"
    Leer tempVestibulo
    Escribir "Segundos de actividad del terminal (uptime):"
    Leer segundosActividadTerminal
    
    Escribir "Distancia horizontal al torno en metros (eje X):"
    Leer coordenadaXMetros
    Escribir "Altura de la pantalla en metros (eje Y):"
    Leer coordenadaYMetros
    
    Escribir "DNI de la persona:"
    Leer dniPersona
    Escribir "Nombre completo:"
    Leer nombrePersona
    Escribir "Perfil de acceso (e = estudiante, d = docente, v = visita):"
    Leer perfilPersona

    perfilNormalizado <- Mayusculas(perfilPersona)
    esEntrada <- Verdadero
    
    Escribir "Introduce hora y minuto de entrada (por ejemplo, 8 15):"
    Leer horaEntrada
    Leer minutoEntrada
    Escribir "Introduce hora y minuto de salida (por ejemplo, 14 10):"
    Leer horaSalida
    Leer minutoSalida
        
    minutosTotalesEntrada <- (horaEntrada * MINUTOS_POR_HORA) + minutoEntrada
    minutosTotalesSalida <- (horaSalida * MINUTOS_POR_HORA) + minutoSalida
    minutosEstanciaTotal <- minutosTotalesSalida - minutosTotalesEntrada
    
    personasEnCentro <- personasEnCentro + 1
    aforoDisponible <- aforoDisponible - 1
    idUltimoFichaje <- idUltimoFichaje + 1

    codigoTokenVestibulo <- Aleatorio(1000, 9999)
    codigoTokenPuertaNorte <- Aleatorio(1000, 9999)
    codigoTestCalibracion <- 5555
    fluctuacionTermica <- 0.45
    tempCalibrada <- tempVestibulo + fluctuacionTermica
    sensorNfcOperativo <- Verdadero
    
    desviacionJornadaMinutos <- abs(minutosEstanciaTotal - JORNADA_BASE_MINUTOS)
    
    Si tempCalibrada > TEMP_MAX_SEGURA Entonces
        tempSeguraAcotada <- TEMP_MAX_SEGURA
    SiNo
        Si tempCalibrada < TEMP_MIN_SEGURA Entonces
            tempSeguraAcotada <- TEMP_MIN_SEGURA
        SiNo
            tempSeguraAcotada <- tempCalibrada
        FinSi
    FinSi
      
    tokenResumenVestibulo <- PREFIJO_CENTRO + "-" + dniPersona + "-T" + ConvertirATexto(terminalId) + "-SEC#" + ConvertirATexto(codigoTokenVestibulo) + "-REG" + ConvertirATexto(idUltimoFichaje)
    tokenResumenPuertaNorte <- PREFIJO_CENTRO + "-" + dniPersona + "-T102-SEC#" + ConvertirATexto(codigoTokenPuertaNorte) + "-REG" + ConvertirATexto(idUltimoFichaje)
    
    horasUptime <- trunc(segundosActividadTerminal / SEGUNDOS_POR_HORA)
    minutosUptime <- trunc((segundosActividadTerminal MOD SEGUNDOS_POR_HORA) / SEGUNDOS_POR_MINUTO)
    segundosUptime <- segundosActividadTerminal MOD SEGUNDOS_POR_MINUTO
    
    aforoTotal <- personasEnCentro + aforoDisponible
    aforoSeguroVisualizado <- personasEnCentro
    porcentajeOcupacionReal <- (aforoSeguroVisualizado * FACTOR_PORCENTAJE) / aforoTotal
    porcentajeOcupacionTruncado <- trunc(porcentajeOcupacionReal)
    porcentajeOcupacionRedondeado <- redon(porcentajeOcupacionReal)
    
    distanciaDiagonalAntena <- rc((coordenadaXMetros ^ 2) + (coordenadaYMetros ^ 2))

    // [NUEVO DÍA 20] Inspección de longitud y extracción de caracteres
    longitudDni <- Longitud(dniPersona)
    longitudNombre <- Longitud(nombrePersona)
    letraFinalDni <- Subcadena(dniPersona, longitudDni, longitudDni) // En PSeInt índices base 1
    esLongitudDniCorrecta <- (longitudDni == LONGITUD_DNI_ESTANDAR)
    posicionPrimerGuion <- 7 // En Java calculada exactamente con indexOf("-")
    
    Escribir ""
    Escribir "======================================================================"
    Escribir "             INFORME OFICIAL DE ACCESO EN VESTIBULO                   "
    Escribir "======================================================================"
    Escribir "CENTRO:             ", NOMBRE_CENTRO
    Escribir "REGISTRO N. :       #", idUltimoFichaje
    Escribir "TOKEN VESTIBULO:    ", tokenResumenVestibulo
    Escribir "TOKEN PUERTA NORTE: ", tokenResumenPuertaNorte
    Escribir "PERSONA:            ", nombrePersona, " (Longitud: ", longitudNombre, " caracteres)"
    Escribir "DNI INSPECCIONADO:  ", dniPersona, " | Longitud: ", longitudDni, " | Letra final: ", letraFinalDni
    Escribir "DNI FORMATO 9 CAR.: ", esLongitudDniCorrecta
    Escribir "IDENTIFICADOR:      ", dniPersona, " | SENTIDO: Entrada (", esEntrada, ")"
    Escribir "----------------------------------------------------------------------"
    Escribir "HORARIO:            Entrada ", horaEntrada, ":", minutoEntrada, " | Salida ", horaSalida, ":", minutoSalida
    Escribir "PERMANENCIA:        ", minutosEstanciaTotal, " minutos en el centro."
    Escribir "DESVIACION JORNADA: ", desviacionJornadaMinutos, " min respecto a jornada completa (480 min)."
    Escribir "OCUPACION REDONDEAD:", porcentajeOcupacionRedondeado, " %"
    Escribir "DISTANCIA A TV:     ", distanciaDiagonalAntena, " metros"
    Escribir "ACTIVO:             ", horasUptime, "h ", minutosUptime, "m ", segundosUptime, "s"
    Escribir "REGISTRO LOG:       ", RUTA_LOGS
    Escribir "======================================================================"
FinAlgoritmo
```

##### Paso B. Implementación en Java (`pr/src/ControlAccesoQR.java` — v1.7)

```java
/**
 * SISTEMA DE CONTROL DE ASISTENCIA POR CÓDIGO QR
 * Cliente: IES El Caminàs (Castellón de la Plana)
 * Consultora: AzaharTech Software Consulting
 * 
 * Versión 1.7: 
 * Métodos de inspección de cadenas (length, charAt, contains, indexOf)
 * aplicados a la validación estructural del DNI y formato del token en memoria.
 * 
 * @author Equipo AzaharTech (Alba Torres, Pau Ferrer)
 * @version 1.7 (Octubre 2026)
 * @since JDK 21 LTS
 *
 */
import java.util.Scanner;
import java.util.Random;

public class ControlAccesoQR {
    public static void main(String[] args) {
        // Constantes inmutables
        final String NOMBRE_CENTRO = "IES El Caminàs (Castellón)";
        final String PREFIJO_CENTRO = "CAMINAS";
        final String RUTA_LOGS = ".\\caminas\\terminal\\logs";
        final int MINUTOS_POR_HORA = 60;
        final int SEGUNDOS_POR_HORA = 3600;
        final int SEGUNDOS_POR_MINUTO = 60;
        final double FACTOR_PORCENTAJE = 100.0;
        final int BASE_TOKEN_SEGURIDAD = 1000;
        final int RANGO_TOKEN_SEGURIDAD = 9000;
        final double MAX_VARIACION_TERMICA = 0.5;
        final long SEMILLA_CALIBRACION = 987654321L;
        final int JORNADA_BASE_MINUTOS = 480;
        final double TEMP_MIN_SEGURA = 15.0;
        final double TEMP_MAX_SEGURA = 35.0;
        
        // [NUEVO DÍA 20] Longitud normativa estándar de un DNI/NIE en España
        final int LONGITUD_DNI_ESTANDAR = 9;

        Scanner teclado = new Scanner(System.in);

        Random generadorVestibulo   = new Random();
        Random generadorPuertaNorte = new Random();
        Random generadorCalibracion = new Random(SEMILLA_CALIBRACION);

        String textoTerminalId;
        int terminalId;
        double tempVestibulo;

        String nombrePersona, dniPersona;
        char perfilPersonaBruto;
        char perfilNormalizado;
        boolean esEntrada;

        int horaEntrada, minutoEntrada;
        int horaSalida, minutoSalida;
        int minutosTotalesEntrada, minutosTotalesSalida, minutosEstanciaTotal;
        String tokenResumenVestibulo;
        String tokenResumenPuertaNorte;

        int personasEnCentro = 1200;
        int aforoDisponible = 800;
        int idUltimoFichaje = 1042;

        int segundosActividadTerminal;
        int horasUptime, minutosUptime, segundosUptime;

        int aforoTotal;
        double porcentajeOcupacionReal;
        int porcentajeOcupacionTruncado;
        int porcentajeOcupacionRedondeado;

        double coordenadaXMetros;
        double coordenadaYMetros;
        double distanciaDiagonalAntena;

        int codigoTokenVestibulo;
        int codigoTokenPuertaNorte;
        int codigoTestCalibracion;
        double fluctuacionTermica;
        double tempCalibrada;
        boolean sensorNfcOperativo;
        int desviacionJornadaMinutos;
        double tempSeguraAcotada;
        int aforoSeguroVisualizado;

        // [NUEVO DÍA 20] Variables para inspección de objetos String
        int longitudDni;
        int longitudNombre;
        char letraFinalDni;
        boolean esLongitudDniCorrecta;
        boolean tokenContienePrefijo;
        int posicionPrimerSeparador;

        System.out.println("=================================================");
        System.out.println("   AZAHARTECH - TERMINAL " + NOMBRE_CENTRO);
        System.out.println("   Versión 1.7 (inspección y métodos de String)  ");
        System.out.println("=================================================");
        System.out.print("ID del terminal: ");
        textoTerminalId = teclado.nextLine();
        terminalId = Integer.parseInt(textoTerminalId);

        System.out.print("Temperatura base del sensor (ºC): ");
        tempVestibulo = teclado.nextDouble();

        System.out.print("Segundos de actividad del terminal (uptime): ");
        segundosActividadTerminal = teclado.nextInt();

        System.out.print("Distancia horizontal a la pantalla en metros (eje X): ");
        coordenadaXMetros = teclado.nextDouble();

        System.out.print("Altura de la pantalla de TV en metros (eje Y): ");
        coordenadaYMetros = teclado.nextDouble();

        teclado.nextLine(); // Limpieza obligatoria del buffer

        System.out.print("DNI de la persona: ");
        dniPersona = teclado.nextLine();

        System.out.print("Nombre completo: ");
        nombrePersona = teclado.nextLine();

        System.out.print("Perfil de acceso (e = estudiante, d = docente, v = visita): ");
        perfilPersonaBruto = teclado.next().charAt(0);
        perfilNormalizado = Character.toUpperCase(perfilPersonaBruto);

        esEntrada = true;

        System.out.print("Hora de entrada (0-23): ");
        horaEntrada = teclado.nextInt();

        System.out.print("Minuto de entrada (0-59): ");
        minutoEntrada = teclado.nextInt();

        System.out.print("Hora de salida (0-23): ");
        horaSalida = teclado.nextInt();

        System.out.print("Minuto de salida (0-59): ");
        minutoSalida = teclado.nextInt();

        // Procesamiento secuencial
        minutosTotalesEntrada = (horaEntrada * MINUTOS_POR_HORA) + minutoEntrada;
        minutosTotalesSalida = (horaSalida * MINUTOS_POR_HORA) + minutoSalida;
        minutosEstanciaTotal = minutosTotalesSalida - minutosTotalesEntrada;

        personasEnCentro++;
        aforoDisponible--;
        idUltimoFichaje++;

        codigoTokenVestibulo   = BASE_TOKEN_SEGURIDAD + generadorVestibulo.nextInt(RANGO_TOKEN_SEGURIDAD);
        codigoTokenPuertaNorte = BASE_TOKEN_SEGURIDAD + generadorPuertaNorte.nextInt(RANGO_TOKEN_SEGURIDAD);

        codigoTestCalibracion = BASE_TOKEN_SEGURIDAD + generadorCalibracion.nextInt(RANGO_TOKEN_SEGURIDAD);
        fluctuacionTermica = generadorCalibracion.nextDouble() * MAX_VARIACION_TERMICA;
        tempCalibrada = tempVestibulo + fluctuacionTermica;
        sensorNfcOperativo = generadorCalibracion.nextBoolean();

        desviacionJornadaMinutos = Math.abs(minutosEstanciaTotal - JORNADA_BASE_MINUTOS);
        tempSeguraAcotada = Math.max(TEMP_MIN_SEGURA, Math.min(tempCalibrada, TEMP_MAX_SEGURA));

        aforoTotal = personasEnCentro + aforoDisponible;
        aforoSeguroVisualizado = Math.min(personasEnCentro, aforoTotal);

        tokenResumenVestibulo = PREFIJO_CENTRO + "-" + dniPersona;
        tokenResumenVestibulo += "-T" + terminalId;
        tokenResumenVestibulo += "-SEC#" + codigoTokenVestibulo;
        tokenResumenVestibulo += "-ESTANCIA-" + minutosEstanciaTotal;
        tokenResumenVestibulo += "-REG" + idUltimoFichaje;

        tokenResumenPuertaNorte = PREFIJO_CENTRO + "-" + dniPersona;
        tokenResumenPuertaNorte += "-T102";
        tokenResumenPuertaNorte += "-SEC#" + codigoTokenPuertaNorte;
        tokenResumenPuertaNorte += "-ESTANCIA-" + minutosEstanciaTotal;
        tokenResumenPuertaNorte += "-REG" + idUltimoFichaje;

        horasUptime = segundosActividadTerminal / SEGUNDOS_POR_HORA;
        minutosUptime = (segundosActividadTerminal % SEGUNDOS_POR_HORA) / SEGUNDOS_POR_MINUTO;
        segundosUptime = segundosActividadTerminal % SEGUNDOS_POR_MINUTO;

        porcentajeOcupacionReal = ((double) aforoSeguroVisualizado / aforoTotal) * FACTOR_PORCENTAJE;
        porcentajeOcupacionTruncado = (int) porcentajeOcupacionReal;
        porcentajeOcupacionRedondeado = (int) Math.round(porcentajeOcupacionReal);

        // Distancia directa de enfoque visual hacia la pantalla de TV
        distanciaDiagonalAntena = Math.sqrt(Math.pow(coordenadaXMetros, 2.0) + Math.pow(coordenadaYMetros, 2.0));

        // [NUEVO DÍA 20] Invocación de métodos de inspección sobre objetos String
        longitudDni = dniPersona.length();
        longitudNombre = nombrePersona.length();
        
        // Extracción segura del último carácter usando length() - 1
        letraFinalDni = dniPersona.charAt(dniPersona.length() - 1);

        // Evaluación booleana sin condicionales: comprobamos la longitud oficial
        esLongitudDniCorrecta = (longitudDni == LONGITUD_DNI_ESTANDAR);

        // Búsqueda de contenido y posición del primer delimitador '-'
        tokenContienePrefijo = tokenResumenVestibulo.contains(PREFIJO_CENTRO);
        posicionPrimerSeparador = tokenResumenVestibulo.indexOf("-");

        // Salida formateada con printf
        System.out.println("\n======================================================================");
        System.out.println("             INFORME OFICIAL DE ACCESO EN VESTÍBULO                   ");
        System.out.println("======================================================================");
        System.out.printf("CENTRO:               %-30s%n", NOMBRE_CENTRO);
        System.out.printf("REGISTRO N.º:         #%05d%n", idUltimoFichaje);
        System.out.printf("TOKEN QR VESTÍBULO:   %s%n", tokenResumenVestibulo);
        System.out.printf("TOKEN QR PUERTA NORTE:%s%n", tokenResumenPuertaNorte);
        System.out.printf("PERSONA:              %-30s | LONGITUD: %d caracteres%n", nombrePersona, longitudNombre);
        System.out.printf("DNI INSPECCIONADO:    %-12s | LETRA FINAL: '%c'%n", dniPersona, letraFinalDni);
        System.out.printf("DNI NORMATIVO (9 CAR):%b (Comprobado con length)%n", esLongitudDniCorrecta);
        System.out.printf("TOKEN VÁLIDO:         %b (Contiene '%s' en pos. %d)%n", tokenContienePrefijo, PREFIJO_CENTRO, posicionPrimerSeparador);
        System.out.printf("PERFIL PROCESADO:     %c | SENTIDO: Entrada (%b)%n", perfilNormalizado, esEntrada);
        System.out.println("----------------------------------------------------------------------");
        System.out.printf("HORARIO:              Entrada %02d:%02d | Salida %02d:%02d%n", horaEntrada, minutoEntrada, horaSalida, minutoSalida);
        System.out.printf("PERMANENCIA:          %03d minutos en el centro.%n", minutosEstanciaTotal);
        System.out.printf("DESVIACIÓN JORNADA:   %03d min respecto a jornada completa (480 min).%n", desviacionJornadaMinutos);
        System.out.println("----------------------------------------------------------------------");
        System.out.printf("OCUPACIÓN REDONDEADA: %d %% (Math.round)%n", porcentajeOcupacionRedondeado);
        System.out.printf("DISTANCIA ENFOQUE TV: %.2f metros (Línea de visión a pantalla)%n", distanciaDiagonalAntena);
        System.out.printf("ACTIVO:               %02dh %02dm %02ds (Sensor: %.1f ºC)%n", horasUptime, minutosUptime, segundosUptime, tempSeguraAcotada);
        System.out.printf("REGISTRO LOG:         %s%n", RUTA_LOGS);
        System.out.println("======================================================================");

        teclado.close();
    }
}
```

---

#### 4. Segunda sesión: dojo de entrenamiento y katas de código

Pasamos al dojo de entrenamiento para programar, con tres **katas de dificultad progresiva** sobre inspección de cadenas, cálculo de posiciones relativas y el modelo de memoria del *String pool*:

---

##### Kata 1 (Cinturón blanco / Nivel base). Métodos de inspección en tu proyecto propio
* **Objetivo.** Incorporar a tu clase `MiProyecto.java` (versión v1.6) llamadas a `length()`, `charAt()`, `contains()` o `indexOf()` para auditar un texto identificativo de tu sistema.
* **Aplicación según tu proyecto elegido:**
  * En **Aventura conversacional**. Medir la longitud del nombre del héroe con `length()`, extraer la letra inicial con `charAt(0)` y verificar mediante `contains()` si el nombre incluye algún título nobiliario (*por ejemplo, `"Lord"` o `"Sir"`*).
  * En **Motor de recomendación**. Comprobar la longitud del título del ítem con `length()`, verificar con `contains()` si el identificador incluye el prefijo oficial de categoría (*por ejemplo, `"MOV-"`*) y localizar con `indexOf("-")` la posición del separador.
  * En **Simulador de físicas 2D** Inspeccionar la cadena de material de la partícula con `length()`, extraer el primer carácter con `charAt(0)` y comprobar con `contains("elastico")` si el nombre define un comportamiento especial.
  * En **Bóveda de contraseñas**. Medir la longitud exacta de la contraseña almacenada mediante `length()`, extraer el primer y el último carácter con `charAt(0)` y `charAt(length() - 1)`, y comprobar con `indexOf("#")` la presencia del delimitador de seguridad.

---

##### Kata 2 (Cinturón marrón / Nivel avanzado). Extracción segura de extremos e inspección de índices
* **Contexto técnico:** Evitar la temida excepción `StringIndexOutOfBoundsException` dominando la relación entre longitud y el índice base 0.
* **Misión de la kata:**
  1. En una clase de prueba llamada `PruebaInspeccionString.java`, declara una cadena con este código:
     ```java
     String codigoSerie = "AZ-PROYECTO-2026-X";
     ```
  2. Escribe instrucciones para calcular e imprimir por consola:
    * La longitud total del código.
    * El primer carácter del código.
    * El último carácter.
    * La posición donde aparece el primer guion.
    * Un booleano que compruebe si el código contiene el año actual.
  3. Ejecuta el código cambiando el contenido de `codigoSerie` por una palabra más corta (*por ejemplo, `"OK-1"`*) y verifica que el programa sigue extrayendo todo sin romperse.

---

##### Kata 3 (Cinturón negro / «Hacker AzaharTech»). El misterio del *String pool* en la memoria Heap
* **Contexto de arquitectura.** Para ahorrar memoria RAM, la Máquina Virtual de Java gestiona una zona especial dentro del Heap llamada **String pool** (piscina de cadenas literales).
* **Misión de la kata:**
  1. En una clase de prueba temporal llamada `PruebaStringPool.java`, escribe el siguiente experimento:
     ```java
     String s1 = "DAM";
     String s2 = "DAM";               // Mismo literal en código fuente
     String s3 = new String("DAM");   // Forzamos un objeto nuevo con new

     // Comparación de punteros en memoria con ==
     boolean pruebaPool   = (s1 == s2);
     boolean pruebaNew    = (s1 == s3);
     boolean pruebaEquals = s1.equals(s3);

     System.out.println("¿s1 y s2 apuntan a la misma dirección?: " + pruebaPool);
     System.out.println("¿s1 y s3 apuntan a la misma dirección?: " + pruebaNew);
     System.out.println("¿s1 y s3 tienen el mismo contenido?:    " + pruebaEquals);
     ```
  2. Ejecuta el programa y comprueba que `s1 == s2` devuelve **`true`**, pero `s1 == s3` devuelve **`false`**.
  3. Anota en tu cuaderno técnico la conclusión de ingeniería:  
     *«Cuando declaramos cadenas mediante literales directos, Java reutiliza la misma instancia del String pool para ahorrar memoria. Pero cuando usamos el operador `new`, obligamos a la JVM a crear un objeto independiente en el Heap fuera del pool. Por eso en Java **jamás se comparan textos con `==`** y es obligatorio usar siempre el método `.equals()` para comparar su contenido»*.

---

### Día 21 - 2 sesiones

---

#### 1. Caso guía en AzaharTech
Son las tres de la tarde del martes 20 de octubre. En la sala técnica de **AzaharTech**, **Pau Ferrer** muestra a **Alba Torres** y a **Laia Claramunt** una nueva dificultad detectada en las cadenas de texto que envían los móviles de los estudiantes al registrarse frente a las pantallas del **IES El Caminàs**.

Pau proyecta la consola:
> *«Ayer aprendimos a medir la longitud del DNI y a buscar caracteres con `indexOf()`. Pero los datos reales de los usuarios llegan muy sucios: algunos alumnos introducen espacios accidentales al principio o al final de su nombre o DNI (por ejemplo, `"  45123789K  "`), otros escriben su nombre todo en minúsculas y otros en mayúsculas.*
>
> *Además, jefatura de estudios nos pide dos transformaciones:*  
> *1. Descomponer el DNI para separar los 8 dígitos numéricos de la letra final de control.*    
> *2. Generar un formato alternativo de registro donde los guiones del token QR se sustituyan por barras inclinadas (`/`) para exportar a sistemas externos.*  
>
> *He intentado hacer `dniPersona.toUpperCase();`, pero al imprimirlo sigue saliendo en minúsculas. ¿Por qué Java no cambia la variable?»*.

Alba Torres toma el teclado y le recuerda la gran propiedad de las cadenas:
> *«Pau, recuerda la regla de oro: **los objetos `String` son inmutables**. Ningún método de `String` modifica jamás la cadena original en el Heap. Lo que hacen es **crear y devolver un nuevo objeto con el texto transformado**.*
>
> *Si no guardas el resultado en una variable, ese nuevo texto se pierde en el Heap y se convierte en basura. Hoy aprenderemos a limpiar espacios con **`trim()`**, a normalizar textos con **`toUpperCase()`** y **`toLowerCase()`**, a sustituir caracteres con **`replace()`** y a extraer fragmentos con **`substring()`**»*.

---

#### 2. Fundamento teórico: métodos de transformación y extracción en la clase `String`

```
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                        TRANSFORMACIONES DE CADENAS EN MEMORIA                          │
├───────────────────────────────┬────────────────────────────────────────────────────────┤
│ 1. ERROR CLÁSICO (Sin efecto) │ 2. USO CORRECTO (Reasignación de referencia)           │
├───────────────────────────────┼────────────────────────────────────────────────────────┤
│ String dni = "  45123789k  "; │ String dni = "  45123789k  ";                          │
│ dni.trim();                   │ dni = dni.trim().toUpperCase();                        │
│ dni.toUpperCase();            │                                                        │
│                               │ 'dni' apunta ahora a la NUEVA cadena limpia y en       │
│ 'dni' sigue valiendo lo mismo │ mayúsculas: "45123789K". El texto sucio original queda │
│ porque los String no mutan.   │ huérfano para el Garbage Collector.                    │
└───────────────────────────────┴────────────────────────────────────────────────────────┘
```

##### A. Métodos de limpieza y normalización
1. **`cadena.trim()`.** Devuelve una nueva cadena eliminando todos los espacios en blanco iniciales y finales (*leading and trailing spaces*). Los espacios intermedios entre palabras no se modifican.
2. **`cadena.toUpperCase()` y `cadena.toLowerCase()`.** Devuelven una nueva cadena transformando todos los caracteres alfabéticos a mayúsculas o minúsculas respectivamente, respetando números y símbolos de puntuación.
3. **`cadena.replace(objetivo, reemplazo)`.** Devuelve una nueva cadena sustituyendo todas las apariciones de un carácter o texto por otro:
   ```java
   String token = "CAMINAS-101-SEC#8492";
   String tokenBarras = token.replace("-", "/"); // Devuelve "CAMINAS/101/SEC#8492"
   ```

##### B. Extracción de fragmentos de texto: el método `substring()`
El método `substring()` permite recortar y extraer porciones de una cadena indicando sus posiciones de índice:

```
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                        FUNCIONAMIENTO DEL MÉTODO SUBSTRING                             │
├────────────────────────────────────────────────────────────────────────────────────────┤
│ String dni = "45123789K";                                                              │
│ Índices:       0 1 2 3 4 5 6 7 8                                                       │
│                                                                                        │
│ 1. substring(inicio): Desde 'inicio' hasta el final de la cadena                       │
│    dni.substring(8)      -> Devuelve "K"                                               │
│                                                                                        │
│ 2. substring(inicio, fin): Desde 'inicio' hasta 'fin - 1' (el final queda excluido)    │
│    dni.substring(0, 8)   -> Devuelve "45123789" (extrae los índices 0 al 7)            │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

* **La regla del extremo excluido.** En `cadena.substring(inicio, fin)`, el carácter en la posición `fin` **nunca se incluye**. La longitud del texto extraído es exactamente igual a la resta `fin - inicio` (*por ejemplo, en `(0, 8)`, la longitud es $8 - 0 = 8$ caracteres*).
* **Encadenamiento de métodos (*Method Chaining*):**  
  Como cada método devuelve un nuevo objeto `String`, podemos invocar varios métodos en una sola instrucción secuencial:
  ```java
  String nombreLimpio = teclado.nextLine().trim().toUpperCase();
  ```

---

#### 3. Evolución del caso guía a `ControlAccesoQR v1.8`

Ampliamos el programa incorporando:
1. Limpieza de espacios accidentales del DNI y del nombre con **`trim()`**.
2. Normalización del DNI a mayúsculas con **`toUpperCase()`**.
3. Descomposición del DNI en dos subcadenas mediante **`substring()`**: los 8 dígitos numéricos por un lado y la letra de control por otro.
4. Generación de un formato de log alternativo sustituyendo los guiones del token por barras inclinadas con **`replace()`**.
5. Extracción del prefijo oficial del centro recortando desde el inicio hasta el primer guion con **`substring(0, indexOf("-"))`**.

##### Paso A. Algoritmo en PSeInt (`pr/pseudocodigo/ControlAccesoQR.psc` — v1.8)

```psc
Algoritmo ControlAccesoQR
    // =========================================================================
    // SISTEMA DE CONTROL DE ASISTENCIA QR - IES EL CAMINAS (Castellon)
    // Version: 1.8 (Evolucion: limpieza, transformacion y subcadenas)
    // =========================================================================

    Definir NOMBRE_CENTRO, PREFIJO_CENTRO, RUTA_LOGS Como Cadena
    Definir MINUTOS_POR_HORA, SEGUNDOS_POR_HORA, SEGUNDOS_POR_MINUTO Como Entero
    Definir FACTOR_PORCENTAJE Como Real
    Definir SEMILLA_CALIBRACION Como Entero
    Definir JORNADA_BASE_MINUTOS Como Entero
    Definir TEMP_MIN_SEGURA, TEMP_MAX_SEGURA Como Real
    Definir LONGITUD_DNI_ESTANDAR Como Entero
    
    NOMBRE_CENTRO <- "IES El Caminas (Castellon)"
    PREFIJO_CENTRO <- "CAMINAS"
    RUTA_LOGS <- ".\caminas\terminal\logs"
    MINUTOS_POR_HORA <- 60
    SEGUNDOS_POR_HORA <- 3600
    SEGUNDOS_POR_MINUTO <- 60
    FACTOR_PORCENTAJE <- 100.0
    SEMILLA_CALIBRACION <- 987654321
    JORNADA_BASE_MINUTOS <- 480
    TEMP_MIN_SEGURA <- 15.0
    TEMP_MAX_SEGURA <- 35.0
    LONGITUD_DNI_ESTANDAR <- 9

    Definir textoTerminalId Como Cadena
    Definir terminalId Como Entero
    Definir tempVestibulo Como Real
    
    Definir nombrePersona, dniPersona Como Cadena
    Definir perfilPersona, perfilNormalizado Como Caracter
    Definir esEntrada Como Logico
    
    Definir horaEntrada, minutoEntrada, horaSalida, minutoSalida Como Entero
    Definir minutosTotalesEntrada, minutosTotalesSalida, minutosEstanciaTotal Como Entero
    Definir tokenResumenVestibulo, tokenResumenPuertaNorte Como Cadena
    
    Definir personasEnCentro, aforoDisponible, idUltimoFichaje Como Entero
    personasEnCentro <- 1200
    aforoDisponible <- 800
    idUltimoFichaje <- 1042
    
    Definir segundosActividadTerminal Como Entero
    Definir horasUptime, minutosUptime, segundosUptime Como Entero
    Definir aforoTotal Como Entero
    Definir porcentajeOcupacionReal Como Real
    Definir porcentajeOcupacionTruncado, porcentajeOcupacionRedondeado Como Entero

    Definir codigoTokenVestibulo, codigoTokenPuertaNorte Como Entero
    Definir codigoTestCalibracion Como Entero
    Definir sensorNfcOperativo Como Logico
    Definir desviacionJornadaMinutos Como Entero
    Definir aforoSeguroVisualizado Como Entero
    Definir coordenadaXMetros, coordenadaYMetros, distanciaDiagonalAntena Como Real

    Definir longitudDni, longitudNombre Como Entero
    Definir letraFinalDni Como Caracter
    Definir esLongitudDniCorrecta Como Logico
    Definir posicionPrimerGuion Como Entero

    // [NUEVO DÍA 21] Variables para datos limpios, subcadenas y reemplazos
    Definir dniNumeros Como Cadena
    Definir tokenFormatoBarras Como Cadena
    Definir prefijoExtraido Como Cadena

    Escribir "AZAHARTECH: TERMINAL ", NOMBRE_CENTRO, " (v1.8 - Transformacion String)"
    Escribir "ID del terminal:"
    Leer textoTerminalId
    terminalId <- ConvertirANumero(textoTerminalId)
    
    Escribir "Temperatura base del sensor (ºC):"
    Leer tempVestibulo
    Escribir "Segundos de actividad del terminal (uptime):"
    Leer segundosActividadTerminal
    
    Escribir "Distancia horizontal a pantalla en metros (eje X):"
    Leer coordenadaXMetros
    Escribir "Altura de la pantalla en metros (eje Y):"
    Leer coordenadaYMetros
    
    Escribir "DNI de la persona:"
    Leer dniPersona
    Escribir "Nombre completo:"
    Leer nombrePersona
    Escribir "Perfil de acceso (e = estudiante, d = docente, v = visita):"
    Leer perfilPersona

    perfilNormalizado <- Mayusculas(perfilPersona)
    esEntrada <- Verdadero
    
    Escribir "Introduce hora y minuto de entrada (por ejemplo, 8 15):"
    Leer horaEntrada
    Leer minutoEntrada
    Escribir "Introduce hora y minuto de salida (por ejemplo, 14 10):"
    Leer horaSalida
    Leer minutoSalida
        
    minutosTotalesEntrada <- (horaEntrada * MINUTOS_POR_HORA) + minutoEntrada
    minutosTotalesSalida <- (horaSalida * MINUTOS_POR_HORA) + minutoSalida
    minutosEstanciaTotal <- minutosTotalesSalida - minutosTotalesEntrada
    
    personasEnCentro <- personasEnCentro + 1
    aforoDisponible <- aforoDisponible - 1
    idUltimoFichaje <- idUltimoFichaje + 1

    codigoTokenVestibulo <- Aleatorio(1000, 9999)
    codigoTokenPuertaNorte <- Aleatorio(1000, 9999)
    codigoTestCalibracion <- 5555
    fluctuacionTermica <- 0.45
    tempCalibrada <- tempVestibulo + fluctuacionTermica
    sensorNfcOperativo <- Verdadero
    
    desviacionJornadaMinutos <- abs(minutosEstanciaTotal - JORNADA_BASE_MINUTOS)
    
    Si tempCalibrada > TEMP_MAX_SEGURA Entonces
        tempSeguraAcotada <- TEMP_MAX_SEGURA
    SiNo
        Si tempCalibrada < TEMP_MIN_SEGURA Entonces
            tempSeguraAcotada <- TEMP_MIN_SEGURA
        SiNo
            tempSeguraAcotada <- tempCalibrada
        FinSi
    FinSi
      
    tokenResumenVestibulo <- PREFIJO_CENTRO + "-" + dniPersona + "-T" + ConvertirATexto(terminalId) + "-SEC#" + ConvertirATexto(codigoTokenVestibulo) + "-REG" + ConvertirATexto(idUltimoFichaje)
    tokenResumenPuertaNorte <- PREFIJO_CENTRO + "-" + dniPersona + "-T102-SEC#" + ConvertirATexto(codigoTokenPuertaNorte) + "-REG" + ConvertirATexto(idUltimoFichaje)
    
    horasUptime <- trunc(segundosActividadTerminal / SEGUNDOS_POR_HORA)
    minutosUptime <- trunc((segundosActividadTerminal MOD SEGUNDOS_POR_HORA) / SEGUNDOS_POR_MINUTO)
    segundosUptime <- segundosActividadTerminal MOD SEGUNDOS_POR_MINUTO
    
    aforoTotal <- personasEnCentro + aforoDisponible
    aforoSeguroVisualizado <- personasEnCentro
    porcentajeOcupacionReal <- (aforoSeguroVisualizado * FACTOR_PORCENTAJE) / aforoTotal
    porcentajeOcupacionTruncado <- trunc(porcentajeOcupacionReal)
    porcentajeOcupacionRedondeado <- redon(porcentajeOcupacionReal)
    
    distanciaDiagonalAntena <- rc((coordenadaXMetros ^ 2) + (coordenadaYMetros ^ 2))

    longitudDni <- Longitud(dniPersona)
    longitudNombre <- Longitud(nombrePersona)
    letraFinalDni <- Subcadena(dniPersona, longitudDni, longitudDni)
    esLongitudDniCorrecta <- (longitudDni == LONGITUD_DNI_ESTANDAR)
    posicionPrimerGuion <- 7

    // [NUEVO DÍA 21] Extracción de los dígitos del DNI (en PSeInt índices base 1: del 1 al 8)
    dniNumeros <- Subcadena(dniPersona, 1, longitudDni - 1)
    
    // Simulación de reemplazo de delimitadores en PSeInt
    tokenFormatoBarras <- "CAMINAS/DNI/T101/SEC#OK" // En Java implementado con replace("-", "/")
    prefijoExtraido <- Subcadena(tokenResumenVestibulo, 1, 7) // En Java: substring(0, indexOf("-"))
    
    Escribir ""
    Escribir "======================================================================"
    Escribir "             INFORME OFICIAL DE ACCESO EN VESTIBULO                   "
    Escribir "======================================================================"
    Escribir "CENTRO:             ", NOMBRE_CENTRO
    Escribir "REGISTRO N. :       #", idUltimoFichaje
    Escribir "TOKEN VESTIBULO:    ", tokenResumenVestibulo
    Escribir "TOKEN LOG BARRAS:   ", tokenFormatoBarras
    Escribir "PREFIJO EXTRAIDO:   ", prefijoExtraido
    Escribir "PERSONA NORMALIZADA:", Mayusculas(nombrePersona)
    Escribir "DNI DESGLOSADO:     Numero: ", dniNumeros, " | Control: '", letraFinalDni, "'"
    Escribir "----------------------------------------------------------------------"
    Escribir "HORARIO:            Entrada ", horaEntrada, ":", minutoEntrada, " | Salida ", horaSalida, ":", minutoSalida
    Escribir "PERMANENCIA:        ", minutosEstanciaTotal, " minutos en el centro."
    Escribir "DESVIACION JORNADA: ", desviacionJornadaMinutos, " min respecto a jornada completa (480 min)."
    Escribir "OCUPACION REDONDEAD:", porcentajeOcupacionRedondeado, " %"
    Escribir "DISTANCIA A TV:     ", distanciaDiagonalAntena, " metros"
    Escribir "ACTIVO:             ", horasUptime, "h ", minutosUptime, "m ", segundosUptime, "s"
    Escribir "REGISTRO LOG:       ", RUTA_LOGS
    Escribir "======================================================================"
FinAlgoritmo
```

##### Paso B. Implementación en Java (`pr/src/ControlAccesoQR.java` — v1.8)

```java
/**
 * SISTEMA DE CONTROL DE ASISTENCIA POR CÓDIGO QR
 * Cliente: IES El Caminàs (Castellón de la Plana)
 * Consultora: AzaharTech Software Consulting
 * 
 * Versión 1.8:
 * Transformación y extracción de cadenas con String:
 * trim(), toUpperCase(), toLowerCase(), replace() y substring().
 * 
 * @author Equipo AzaharTech (Alba Torres, Pau Ferrer)
 * @version 1.8 (Octubre 2026)
 * @since JDK 21 LTS
 * 
 */
import java.util.Scanner;
import java.util.Random;

public class ControlAccesoQR {
    public static void main(String[] args) {
        // Constantes inmutables
        final String NOMBRE_CENTRO = "IES El Caminàs (Castellón)";
        final String PREFIJO_CENTRO = "CAMINAS";
        final String RUTA_LOGS = ".\\caminas\\terminal\\logs";
        final int MINUTOS_POR_HORA = 60;
        final int SEGUNDOS_POR_HORA = 3600;
        final int SEGUNDOS_POR_MINUTO = 60;
        final double FACTOR_PORCENTAJE = 100.0;
        final long SEMILLA_CALIBRACION = 987654321L;
        final int BASE_TOKEN_SEGURIDAD = 1000;
        final int RANGO_TOKEN_SEGURIDAD = 9000;
        final double MAX_VARIACION_TERMICA = 0.5;
        final long SEMILLA_CALIBRACION = 987654321L;
        final int JORNADA_BASE_MINUTOS = 480;
        final double TEMP_MIN_SEGURA = 15.0;
        final double TEMP_MAX_SEGURA = 35.0;
        final int LONGITUD_DNI_ESTANDAR = 9;
        
        Scanner teclado = new Scanner(System.in);

        Random generadorVestibulo   = new Random();
        Random generadorPuertaNorte = new Random();
        Random generadorCalibracion = new Random(SEMILLA_CALIBRACION);

        String textoTerminalId;
        int terminalId;
        double tempVestibulo;

        String nombrePersonaBruto, nombrePersonaLimpio;
        String dniPersonaBruto, dniPersonaLimpio;
        char perfilPersonaBruto;
        char perfilNormalizado;
        boolean esEntrada;

        int horaEntrada, minutoEntrada;
        int horaSalida, minutoSalida;
        int minutosTotalesEntrada, minutosTotalesSalida, minutosEstanciaTotal;
        String tokenResumenVestibulo;
        String tokenResumenPuertaNorte;

        int personasEnCentro = 1200;
        int aforoDisponible = 800;
        int idUltimoFichaje = 1042;

        int segundosActividadTerminal;
        int horasUptime, minutosUptime, segundosUptime;

        int aforoTotal;
        double porcentajeOcupacionReal;
        int porcentajeOcupacionTruncado;
        int porcentajeOcupacionRedondeado;

        double coordenadaXMetros;
        double coordenadaYMetros;
        double distanciaDiagonalAntena;

        int codigoTokenVestibulo;
        int codigoTokenPuertaNorte;
        int codigoTestCalibracion;
        double fluctuacionTermica;
        double tempCalibrada;
        boolean sensorNfcOperativo;
        int desviacionJornadaMinutos;
        double tempSeguraAcotada;
        int aforoSeguroVisualizado;

        int longitudDni;
        int longitudNombre;
        char letraFinalDni;
        boolean esLongitudDniCorrecta;
        boolean tokenContienePrefijo;
        int posicionPrimerSeparador;

        // [NUEVO DÍA 21] Variables para cadenas transformadas y extraídas
        String numeroDniSolo;
        String letraDniTexto;
        String tokenFormatoBarras;
        String prefijoExtraido;

        System.out.println("=================================================");
        System.out.println("   AZAHARTECH - TERMINAL " + NOMBRE_CENTRO);
        System.out.println("   Versión 1.8 (transformación de texto y substrings)");
        System.out.println("=================================================");
        System.out.print("ID del terminal: ");
        textoTerminalId = teclado.nextLine();
        terminalId = Integer.parseInt(textoTerminalId);

        System.out.print("Temperatura base del sensor (ºC): ");
        tempVestibulo = teclado.nextDouble();

        System.out.print("Segundos de actividad del terminal (uptime): ");
        segundosActividadTerminal = teclado.nextInt();

        System.out.print("Distancia horizontal a la pantalla en metros (eje X): ");
        coordenadaXMetros = teclado.nextDouble();

        System.out.print("Altura de la pantalla de TV en metros (eje Y): ");
        coordenadaYMetros = teclado.nextDouble();

        teclado.nextLine(); // Limpieza obligatoria del buffer

        System.out.print("DNI de la persona (puede incluir espacios): ");
        dniPersonaBruto = teclado.nextLine();

        System.out.print("Nombre completo (puede incluir espacios): ");
        nombrePersonaBruto = teclado.nextLine();

        // [NUEVO DÍA 21] Limpieza de espacios accidentales con trim() y normalización
        dniPersonaLimpio = dniPersonaBruto.trim().toUpperCase();
        nombrePersonaLimpio = nombrePersonaBruto.trim();

        System.out.print("Perfil de acceso (e = estudiante, d = docente, v = visita): ");
        perfilPersonaBruto = teclado.next().charAt(0);
        perfilNormalizado = Character.toUpperCase(perfilPersonaBruto);

        esEntrada = true;

        System.out.print("Hora de entrada (0-23): ");
        horaEntrada = teclado.nextInt();

        System.out.print("Minuto de entrada (0-59): ");
        minutoEntrada = teclado.nextInt();

        System.out.print("Hora de salida (0-23): ");
        horaSalida = teclado.nextInt();

        System.out.print("Minuto de salida (0-59): ");
        minutoSalida = teclado.nextInt();

        // Procesamiento secuencial
        minutosTotalesEntrada = (horaEntrada * MINUTOS_POR_HORA) + minutoEntrada;
        minutosTotalesSalida = (horaSalida * MINUTOS_POR_HORA) + minutoSalida;
        minutosEstanciaTotal = minutosTotalesSalida - minutosTotalesEntrada;

        personasEnCentro++;
        aforoDisponible--;
        idUltimoFichaje++;

        codigoTokenVestibulo   = BASE_TOKEN_SEGURIDAD + generadorVestibulo.nextInt(RANGO_TOKEN_SEGURIDAD);
        codigoTokenPuertaNorte = BASE_TOKEN_SEGURIDAD + generadorPuertaNorte.nextInt(RANGO_TOKEN_SEGURIDAD);

        codigoTestCalibracion = BASE_TOKEN_SEGURIDAD + generadorCalibracion.nextInt(RANGO_TOKEN_SEGURIDAD);
        fluctuacionTermica = generadorCalibracion.nextDouble() * MAX_VARIACION_TERMICA;
        tempCalibrada = tempVestibulo + fluctuacionTermica;
        sensorNfcOperativo = generadorCalibracion.nextBoolean();

        desviacionJornadaMinutos = Math.abs(minutosEstanciaTotal - JORNADA_BASE_MINUTOS);
        tempSeguraAcotada = Math.max(TEMP_MIN_SEGURA, Math.min(tempCalibrada, TEMP_MAX_SEGURA));

        aforoTotal = personasEnCentro + aforoDisponible;
        aforoSeguroVisualizado = Math.min(personasEnCentro, aforoTotal);

        tokenResumenVestibulo = PREFIJO_CENTRO + "-" + dniPersonaLimpio;
        tokenResumenVestibulo += "-T" + terminalId;
        tokenResumenVestibulo += "-SEC#" + codigoTokenVestibulo;
        tokenResumenVestibulo += "-ESTANCIA-" + minutosEstanciaTotal;
        tokenResumenVestibulo += "-REG" + idUltimoFichaje;

        tokenResumenPuertaNorte = PREFIJO_CENTRO + "-" + dniPersonaLimpio;
        tokenResumenPuertaNorte += "-T102";
        tokenResumenPuertaNorte += "-SEC#" + codigoTokenPuertaNorte;
        tokenResumenPuertaNorte += "-ESTANCIA-" + minutosEstanciaTotal;
        tokenResumenPuertaNorte += "-REG" + idUltimoFichaje;

        horasUptime = segundosActividadTerminal / SEGUNDOS_POR_HORA;
        minutosUptime = (segundosActividadTerminal % SEGUNDOS_POR_HORA) / SEGUNDOS_POR_MINUTO;
        segundosUptime = segundosActividadTerminal % SEGUNDOS_POR_MINUTO;

        porcentajeOcupacionReal = ((double) aforoSeguroVisualizado / aforoTotal) * FACTOR_PORCENTAJE;
        porcentajeOcupacionTruncado = (int) porcentajeOcupacionReal;
        porcentajeOcupacionRedondeado = (int) Math.round(porcentajeOcupacionReal);

        distanciaDiagonalAntena = Math.sqrt(Math.pow(coordenadaXMetros, 2.0) + Math.pow(coordenadaYMetros, 2.0));

        longitudDni = dniPersonaLimpio.length();
        longitudNombre = nombrePersonaLimpio.length();
        letraFinalDni = dniPersonaLimpio.charAt(dniPersonaLimpio.length() - 1);
        esLongitudDniCorrecta = (longitudDni == LONGITUD_DNI_ESTANDAR);
        tokenContienePrefijo = tokenResumenVestibulo.contains(PREFIJO_CENTRO);
        posicionPrimerSeparador = tokenResumenVestibulo.indexOf("-");

        // [NUEVO DÍA 21] 1. Extracción de los dígitos del DNI (desde 0 hasta el penúltimo carácter)
        numeroDniSolo = dniPersonaLimpio.substring(0, dniPersonaLimpio.length() - 1);

        // [NUEVO DÍA 21] 2. Extracción de la letra en formato String
        letraDniTexto = dniPersonaLimpio.substring(dniPersonaLimpio.length() - 1);

        // [NUEVO DÍA 21] 3. Sustitución de delimitadores con replace()
        tokenFormatoBarras = tokenResumenVestibulo.replace("-", "/");

        // [NUEVO DÍA 21] 4. Extracción dinámica del prefijo recortando hasta el primer guion
        prefijoExtraido = tokenResumenVestibulo.substring(0, posicionPrimerSeparador);

        // Salida formateada con printf
        System.out.println("\n======================================================================");
        System.out.println("             INFORME OFICIAL DE ACCESO EN VESTÍBULO                   ");
        System.out.println("======================================================================");
        System.out.printf("CENTRO:               %-30s%n", NOMBRE_CENTRO);
        System.out.printf("PREFIJO EXTRAÍDO:     %-15s (substring dinámico)%n", prefijoExtraido);
        System.out.printf("REGISTRO N.º:         #%05d%n", idUltimoFichaje);
        System.out.printf("TOKEN QR VESTÍBULO:   %s%n", tokenResumenVestibulo);
        System.out.printf("TOKEN FORMATO BARRAS: %s (replace)%n", tokenFormatoBarras);
        System.out.printf("PERSONA LIMPIA:       %-30s | LONGITUD: %d%n", nombrePersonaLimpio.toUpperCase(), longitudNombre);
        System.out.printf("DNI DESGLOSADO:       Número: %s | Letra: '%s'%n", numeroDniSolo, letraDniTexto);
        System.out.printf("DNI NORMATIVO (9 CAR):%b%n", esLongitudDniCorrecta);
        System.out.printf("PERFIL PROCESADO:     %c | SENTIDO: Entrada (%b)%n", perfilNormalizado, esEntrada);
        System.out.println("----------------------------------------------------------------------");
        System.out.printf("HORARIO:              Entrada %02d:%02d | Salida %02d:%02d%n", horaEntrada, minutoEntrada, horaSalida, minutoSalida);
        System.out.printf("PERMANENCIA:          %03d minutos en el centro.%n", minutosEstanciaTotal);
        System.out.printf("DESVIACIÓN JORNADA:   %03d min respecto a jornada completa (480 min).%n", desviacionJornadaMinutos);
        System.out.println("----------------------------------------------------------------------");
        System.out.printf("OCUPACIÓN REDONDEADA: %d %% (Math.round)%n", porcentajeOcupacionRedondeado);
        System.out.printf("DISTANCIA ENFOQUE TV: %.2f metros (Línea de visión a pantalla)%n", distanciaDiagonalAntena);
        System.out.printf("ACTIVO:               %02dh %02dm %02ds (Sensor: %.1f ºC)%n", horasUptime, minutosUptime, segundosUptime, tempSeguraAcotada);
        System.out.printf("REGISTRO LOG:         %s%n", RUTA_LOGS);
        System.out.println("======================================================================");

        teclado.close();
    }
}
```

---

#### 4. Segunda sesión: dojo de entrenamiento y katas de código

Pasamos al dojo de entrenamiento para programar, con tres **katas de dificultad progresiva** sobre transformación, recorte de subcadenas y reemplazo de texto:

---

##### Kata 1 (Cinturón blanco / Nivel base). Transformación y extracción en tu proyecto propio
* **Objetivo.** Incorporar a tu clase `MiProyecto.java` (versión v1.7) al menos una limpieza con `trim()`, una normalización con `toUpperCase()` o `toLowerCase()`, un recorte con `substring()` y una sustitución con `replace()`.
* **Aplicación según tu proyecto elegido:**
  * En **Aventura conversacional**. Limpiar el nombre del jugador con `trim()`, pasar el nombre a mayúsculas con `toUpperCase()`, extraer el prefijo de la clase del héroe con `substring(0, 3)` y sustituir guiones de la ficha de personaje por espacios con `replace()`.
  * En **Motor de recomendación**. Limpiar los espacios de la etiqueta de búsqueda con `trim()`, normalizar el género a minúsculas con `toLowerCase()`, extraer el año de estreno desde el identificador con `substring()` y cambiar barras por guiones con `replace()`.
  * En **Simulador de físicas 2D**. Normalizar la clave de la partícula a mayúsculas con `toUpperCase()`, recortar el código de material con `substring()` y sustituir comas decimales por puntos con `replace()`.
  * En **Bóveda de contraseñas**. Eliminar espacios accidentales en la entrada de la clave con `trim()`, extraer los primeros cuatro caracteres del identificador como prefijo seguro con `substring(0, 4)` y generar una versión ofuscada reemplazando caracteres sensibles con `replace()`.

---

##### Kata 2 (Cinturón marrón / Nivel avanzado). La regla del `endIndex` y el troceado dinámico
* **Contexto técnico.** En `cadena.substring(inicio, fin)`, el índice `fin` queda excluido. Además, los índices no deben codificarse con números fijos mágicos, sino calcularse dinámicamente con `indexOf()`.
* **Misión de la kata:**
  1. En una clase de prueba llamada `PruebaSubstrings.java`, declara la siguiente cadena estructurada:
     ```java
     String registroCompleto = "REF-2026:PRODUCTO_X#FINAL";
     ```
  2. Utiliza `indexOf()` y `substring()` para extraer dinámicamente en cuatro variables independientes:
    * El prefijo: desde el inicio hasta el primer guion (`"REF"`).
    * El año: entre el guion y los dos puntos (`"2026"`).
    * El nombre: entre los dos puntos y la almohadilla (`"PRODUCTO_X"`).
    * El sufijo: desde la almohadilla hasta el final (`"FINAL"`).
  3. Imprime cada fragmento en una línea separada.
  4. Modifica la variable inicial por `"ID-99:SENSOR_B#OFF"` y comprueba que tu algoritmo sigue extrayendo las cuatro piezas con exactitud sin tocar los índices.

---

##### Kata 3 (Cinturón negro / «Hacker AzaharTech»). La trampa de la inmutabilidad y el encadenamiento
* **Contexto de memoria.** Demostrar empíricamente qué ocurre cuando se transforman cadenas sin capturar la nueva referencia devuelta.
* **Misión de la kata:**
  1. En una clase de prueba llamada `PruebaInmutabilidad.java`, escribe:
     ```java
     String correo = "  usuario@iescaminas.es  ";

     // Intento erróneo de modificación in situ:
     correo.trim();
     correo.toUpperCase();

     System.out.println("Intento 1 (sin reasignar): [" + correo + "]");

     // Uso correcto con encadenamiento de métodos (method chaining):
     correo = correo.trim().toUpperCase().replace(".ES", ".COM");

     System.out.println("Intento 2 (reasignando):   [" + correo + "]");
     ```
  2. Comprueba que el Intento 1 sigue imprimiendo los espacios y las minúsculas intactas, mientras que el Intento 2 produce el texto transformado.
  3. Redacta dos líneas de comentario en tu código explicando por qué el Intento 1 generó dos objetos huérfanos en el Heap que fueron recogidos por el Garbage Collector sin llegar a usarse.

---

### Día 22 - 2 sesiones

---

#### 1. Caso guía en AzaharTech
Son las tres de la tarde del miércoles 21 de octubre. En la sala técnica de **AzaharTech**, **Pau Ferrer** ejecuta la versión v1.8 de `ControlAccesoQR.java`. 

El programa solicita por consola:
```text
Introduce hora de entrada (0-23): 8
Introduce minuto de entrada (0-59): 15
Introduce hora de salida (0-23): 14
Introduce minuto de salida (0-59): 10
```

Y a continuación ejecuta las fórmulas manuales que diseñamos en el Sprint 1:
```java
minutosTotalesEntrada = (horaEntrada * MINUTOS_POR_HORA) + minutoEntrada;
minutosTotalesSalida = (horaSalida * MINUTOS_POR_HORA) + minutoSalida;
minutosEstanciaTotal = minutosTotalesSalida - minutosTotalesEntrada;
```

**Laia Claramunt** se detiene junto al puesto de Pau y plantea la reflexión de fondo:
> *«Pau, en el Sprint 1 estas fórmulas manuales con `MINUTOS_POR_HORA` eran necesarias porque estábamos aprendiendo a operar con variables primitivas y constantes. Pero en la realidad del IES El Caminàs, **ningún estudiante teclea a qué hora entra ni a qué hora sale**.*
>
> *Cuando el alumno caza el código QR con su móvil frente a la pantalla de TV del vestíbulo, el servidor captura de forma automática la **marca temporal real del reloj del sistema operativo**.*
>
> *Además, calcular diferencias de tiempo convirtiendo a mano con constantes como `MINUTOS_POR_HORA` sigue siendo un enfoque artesanal: no contempla cambios de fecha, zonas horarias ni estándares internacionales. Java cuenta con una biblioteca moderna de fechas y horas: el paquete **`java.time`**.*
>
> *Hoy eliminaremos esas conversiones manuales y evolucionaremos `ControlAccesoQR` a la versión **v1.9**: utilizaremos **`LocalTime`** para capturar la hora del reloj, **`LocalDate`** para la fecha del día y la clase **`Duration`** para calcular la permanencia exacta entre dos marcas de tiempo de forma automática»*.

---

#### 2. Fundamento teórico: la API moderna de fechas y horas (`java.time`)

```
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                        ARQUITECTURA DE LA API JAVA.TIME                                │
├──────────────────────────┬───────────────────┬─────────────────────────────────────────┤
│ Clase                    │ Qué representa    │ Ejemplo de contenido                    │
├──────────────────────────┼───────────────────┼─────────────────────────────────────────┤
│ LocalTime                │ Hora sin fecha    │ 08:15:30.450 (Horas, minutos, segundos) │
├──────────────────────────┼───────────────────┼─────────────────────────────────────────┤
│ LocalDate                │ Fecha sin hora    │ 2026-10-21 (Año, mes, día estándar ISO) │
├──────────────────────────┼───────────────────┼─────────────────────────────────────────┤
│ Duration                 │ Cantidad de tiempo│ PT5H55M (Intervalo temporal medido en   │
│                          │ entre dos horas   │ segundos, minutos u horas)              │
└──────────────────────────┴───────────────────┴─────────────────────────────────────────┘
```

##### A. ¿Por qué `java.time` y no las antiguas clases `Date` o `Calendar`?
En versiones antiguas de Java (anteriores a Java 8), la gestión temporal se realizaba con `java.util.Date` y `java.util.Calendar`. Eran clases defectuosas: eran mutables en memoria (cualquiera podía alterar una fecha ya registrada por error), los meses empezaban en `0` (enero era el mes 0) y provocaban graves errores de sincronización.

La API moderna **`java.time`** (estándar ISO-8601):
* Es **completamente inmutable**: ningún método altera el objeto; siempre devuelven una nueva instancia en el Heap.
* Es segura, precisa hasta el nivel de nanosegundos y muy fácil de leer.

##### B. Creación de horas y fechas mediante métodos estáticos (sin operador `new`)

Al igual que aprendimos la semana pasada con la clase `Math` o con `Integer.parseInt()`, en `LocalTime` y `LocalDate` no utilizamos el operador `new`.

En su lugar, invocamos **métodos estáticos directamente desde el nombre de la clase** para obtener el objeto ya preparado:

1. **Captura del reloj del sistema en tiempo real (`now()`).**  
   Invocamos al método estático `now()` para que el sistema operativo lea el reloj interno de la máquina:
   ```java
   LocalTime horaActual = LocalTime.now(); // Lee el reloj en este instante (ej. 15:04:12)
   LocalDate fechaHoy = LocalDate.now();   // Lee la fecha actual del sistema (ej. 2026-10-21)
   ```

2. **Fijar una hora o fecha concreta (`of()`).**  
   Invocamos al método estático `of(...)` pasándole los valores numéricos entre paréntesis como argumentos:
   ```java
   LocalTime horaApertura = LocalTime.of(8, 0);       // 08:00 h
   LocalTime horaAcceso   = LocalTime.of(8, 15);      // 08:15 h
   LocalDate inicioCurso  = LocalDate.of(2026, 9, 14); // 14 de septiembre de 2026
   ```

##### C. Métodos de inspección y desplazamiento
* **Extracción de componentes:**
  * `hora.getHour()`. Devuelve la hora en formato 0–23 (`int`).
  * `hora.getMinute()`. Devuelve el minuto 0–59 (`int`).
  * `hora.getSecond()`. Devuelve el segundo 0–59 (`int`).
* **Desplazamiento inmutable:**
  * `hora.plusMinutes(45)`. Devuelve un nuevo `LocalTime` con 45 minutos sumados.
  * `hora.minusHours(2)`. Devuelve un nuevo `LocalTime` restando 2 horas.

##### D. Cálculo automático de duraciones con `Duration.between()`
Para calcular el tiempo transcurrido entre dos marcas horarias sin realizar operaciones matemáticas manuales:

```java
LocalTime entrada = LocalTime.of(8, 15);
LocalTime salida  = LocalTime.of(14, 10);

// Duration calcula el intervalo exacto entre ambas marcas
Duration estancia = Duration.between(entrada, salida);

long minutosTotales = estancia.toMinutes(); // Devuelve 355 minutos directamente
```

---

#### 3. Evolución del caso guía a `ControlAccesoQR v1.9`

Ampliamos el programa guía incorporando:
1. Las directivas de importación de la API moderna: `import java.time.LocalTime;`, `import java.time.LocalDate;` y `import java.time.Duration;`.
2. Captura automática de la fecha actual del centro con **`LocalDate.now()`**.
3. Captura del instante exacto de emisión del QR en la pantalla de TV con **`LocalTime.now()`**.
4. Sustitución de los cálculos manuales `(hora * MINUTOS_POR_HORA) + minuto` por objetos **`LocalTime.of()`** y cálculo automático de la estancia con **`Duration.between().toMinutes()`**.
5. Emisión de la fecha oficial y marcas horarias mediante formateo `printf`.

##### Paso A. Algoritmo en PSeInt (`pr/pseudocodigo/ControlAccesoQR.psc` — v1.9)

```psc
Algoritmo ControlAccesoQR
    // =========================================================================
    // SISTEMA DE CONTROL DE ASISTENCIA QR - IES EL CAMINAS (Castellon)
    // Version: 1.9 (Evolucion: marcas horarias temporales y calculo de duracion)
    // =========================================================================

    Definir NOMBRE_CENTRO, PREFIJO_CENTRO, RUTA_LOGS Como Cadena
    Definir MINUTOS_POR_HORA, SEGUNDOS_POR_HORA, SEGUNDOS_POR_MINUTO Como Entero
    Definir FACTOR_PORCENTAJE Como Real
    Definir SEMILLA_CALIBRACION Como Entero
    Definir JORNADA_BASE_MINUTOS Como Entero
    Definir TEMP_MIN_SEGURA, TEMP_MAX_SEGURA Como Real
    Definir LONGITUD_DNI_ESTANDAR Como Entero
    
    NOMBRE_CENTRO <- "IES El Caminas (Castellon)"
    PREFIJO_CENTRO <- "CAMINAS"
    RUTA_LOGS <- ".\caminas\terminal\logs"
    MINUTOS_POR_HORA <- 60
    SEGUNDOS_POR_HORA <- 3600
    SEGUNDOS_POR_MINUTO <- 60
    FACTOR_PORCENTAJE <- 100.0
    SEMILLA_CALIBRACION <- 987654321
    JORNADA_BASE_MINUTOS <- 480
    TEMP_MIN_SEGURA <- 15.0
    TEMP_MAX_SEGURA <- 35.0
    LONGITUD_DNI_ESTANDAR <- 9

    Definir textoTerminalId Como Cadena
    Definir terminalId Como Entero
    Definir tempVestibulo Como Real
    
    Definir nombrePersona, dniPersona Como Cadena
    Definir perfilPersona, perfilNormalizado Como Caracter
    Definir esEntrada Como Logico
    
    // [NUEVO DÍA 22] Variables para marcas horarias (en Java representadas por LocalTime/LocalDate)
    Definir fechaSesion Como Cadena
    Definir horaEntrada, minutoEntrada, horaSalida, minutoSalida Como Entero
    Definir minutosEstanciaTotal Como Entero
    Definir tokenResumenVestibulo, tokenResumenPuertaNorte Como Cadena
    
    Definir personasEnCentro, aforoDisponible, idUltimoFichaje Como Entero
    personasEnCentro <- 1200
    aforoDisponible <- 800
    idUltimoFichaje <- 1042
    
    Definir segundosActividadTerminal Como Entero
    Definir horasUptime, minutosUptime, segundosUptime Como Entero
    Definir aforoTotal Como Entero
    Definir porcentajeOcupacionReal Como Real
    Definir porcentajeOcupacionTruncado, porcentajeOcupacionRedondeado Como Entero

    Definir codigoTokenVestibulo, codigoTokenPuertaNorte Como Entero
    Definir codigoTestCalibracion Como Entero
    Definir sensorNfcOperativo Como Logico
    Definir desviacionJornadaMinutos Como Entero
    Definir aforoSeguroVisualizado Como Entero
    Definir coordenadaXMetros, coordenadaYMetros, distanciaDiagonalAntena Como Real

    Definir longitudDni, longitudNombre Como Entero
    Definir letraFinalDni Como Caracter
    Definir esLongitudDniCorrecta Como Logico
    Definir posicionPrimerGuion Como Entero
    Definir numeroDniSolo Como Cadena
    Definir tokenFormatoBarras Como Cadena
    Definir prefijoExtraido Como Cadena

    fechaSesion <- "2026-10-21" // Simulación de LocalDate.now()

    Escribir "AZAHARTECH: TERMINAL ", NOMBRE_CENTRO, " (v1.9 - API tiempo)"
    Escribir "Introduce identificador del terminal en texto (ej. 101):"
    Leer textoTerminalId
    terminalId <- ConvertirANumero(textoTerminalId)
    
    Escribir "Temperatura base del sensor (ºC):"
    Leer tempVestibulo
    Escribir "Segundos de actividad del terminal (uptime):"
    Leer segundosActividadTerminal
    
    Escribir "Distancia horizontal a pantalla en metros (eje X):"
    Leer coordenadaXMetros
    Escribir "Altura de la pantalla en metros (eje Y):"
    Leer coordenadaYMetros
    
    Escribir "DNI de la persona:"
    Leer dniPersona
    Escribir "Nombre completo:"
    Leer nombrePersona
    Escribir "Perfil de acceso (e = estudiante, d = docente, v = visita):"
    Leer perfilPersona

    perfilNormalizado <- Mayusculas(perfilPersona)
    esEntrada <- Verdadero
    
    Escribir "Introduce hora y minuto de entrada (por ejemplo, 8 15):"
    Leer horaEntrada
    Leer minutoEntrada
    Escribir "Introduce hora y minuto de salida (por ejemplo, 14 10):"
    Leer horaSalida
    Leer minutoSalida
        
    // [NUEVO DÍA 22] El cálculo de estancia en Java se delega en Duration.between()
    minutosTotalesEntrada <- (horaEntrada * MINUTOS_POR_HORA) + minutoEntrada
    minutosTotalesSalida <- (horaSalida * MINUTOS_POR_HORA) + minutoSalida
    minutosEstanciaTotal <- minutosTotalesSalida - minutosTotalesEntrada
    
    personasEnCentro <- personasEnCentro + 1
    aforoDisponible <- aforoDisponible - 1
    idUltimoFichaje <- idUltimoFichaje + 1

    codigoTokenVestibulo <- Aleatorio(1000, 9999)
    codigoTokenPuertaNorte <- Aleatorio(1000, 9999)
    codigoTestCalibracion <- 5555
    fluctuacionTermica <- 0.45
    tempCalibrada <- tempVestibulo + fluctuacionTermica
    sensorNfcOperativo <- Verdadero
    
    desviacionJornadaMinutos <- abs(minutosEstanciaTotal - JORNADA_BASE_MINUTOS)
    
    Si tempCalibrada > TEMP_MAX_SEGURA Entonces
        tempSeguraAcotada <- TEMP_MAX_SEGURA
    SiNo
        Si tempCalibrada < TEMP_MIN_SEGURA Entonces
            tempSeguraAcotada <- TEMP_MIN_SEGURA
        SiNo
            tempSeguraAcotada <- tempCalibrada
        FinSi
    FinSi
      
    tokenResumenVestibulo <- PREFIJO_CENTRO + "-" + dniPersona + "-T" + ConvertirATexto(terminalId) + "-SEC#" + ConvertirATexto(codigoTokenVestibulo) + "-REG" + ConvertirATexto(idUltimoFichaje)
    tokenResumenPuertaNorte <- PREFIJO_CENTRO + "-" + dniPersona + "-T102-SEC#" + ConvertirATexto(codigoTokenPuertaNorte) + "-REG" + ConvertirATexto(idUltimoFichaje)
    
    horasUptime <- trunc(segundosActividadTerminal / SEGUNDOS_POR_HORA)
    minutosUptime <- trunc((segundosActividadTerminal MOD SEGUNDOS_POR_HORA) / SEGUNDOS_POR_MINUTO)
    segundosUptime <- segundosActividadTerminal MOD SEGUNDOS_POR_MINUTO
    
    aforoTotal <- personasEnCentro + aforoDisponible
    aforoSeguroVisualizado <- personasEnCentro
    porcentajeOcupacionReal <- (aforoSeguroVisualizado * FACTOR_PORCENTAJE) / aforoTotal
    porcentajeOcupacionTruncado <- trunc(porcentajeOcupacionReal)
    porcentajeOcupacionRedondeado <- redon(porcentajeOcupacionReal)
    
    distanciaDiagonalAntena <- rc((coordenadaXMetros ^ 2) + (coordenadaYMetros ^ 2))

    longitudDni <- Longitud(dniPersona)
    longitudNombre <- Longitud(nombrePersona)
    letraFinalDni <- Subcadena(dniPersona, longitudDni, longitudDni)
    esLongitudDniCorrecta <- (longitudDni == LONGITUD_DNI_ESTANDAR)
    posicionPrimerGuion <- 7

    dniNumeros <- Subcadena(dniPersona, 1, longitudDni - 1)
    tokenFormatoBarras <- "CAMINAS/DNI/T101/SEC#OK"
    prefijoExtraido <- Subcadena(tokenResumenVestibulo, 1, 7)
    
    Escribir ""
    Escribir "======================================================================"
    Escribir "             INFORME OFICIAL DE ACCESO EN VESTIBULO                   "
    Escribir "======================================================================"
    Escribir "CENTRO:             ", NOMBRE_CENTRO, " | FECHA: ", fechaSesion
    Escribir "REGISTRO N. :       #", idUltimoFichaje
    Escribir "TOKEN VESTIBULO:    ", tokenResumenVestibulo
    Escribir "TOKEN PUERTA NORTE: ", tokenResumenPuertaNorte
    Escribir "PERSONA:            ", Mayusculas(nombrePersona)
    Escribir "DNI DESGLOSADO:     Numero: ", dniNumeros, " | Control: '", letraFinalDni, "'"
    Escribir "IDENTIFICADOR:      ", dniPersona, " | SENTIDO: Entrada (", esEntrada, ")"
    Escribir "----------------------------------------------------------------------"
    Escribir "HORARIO REGISTRADO: Entrada ", horaEntrada, ":", minutoEntrada, " | Salida ", horaSalida, ":", minutoSalida
    Escribir "PERMANENCIA TOTAL:  ", minutosEstanciaTotal, " minutos (calculo automatico Duration)."
    Escribir "DESVIACION JORNADA: ", desviacionJornadaMinutos, " min respecto a jornada completa (480 min)."
    Escribir "OCUPACION REDONDEAD:", porcentajeOcupacionRedondeado, " %"
    Escribir "DISTANCIA A TV:     ", distanciaDiagonalAntena, " metros"
    Escribir "ACTIVO:             ", horasUptime, "h ", minutosUptime, "m ", segundosUptime, "s"
    Escribir "REGISTRO LOG:       ", RUTA_LOGS
    Escribir "======================================================================"
FinAlgoritmo
```

##### Paso B. Implementación en Java (`pr/src/ControlAccesoQR.java` — v1.9)

```java
/**
 * SISTEMA DE CONTROL DE ASISTENCIA POR CÓDIGO QR
 * Cliente: IES El Caminàs (Castellón de la Plana)
 * Consultora: AzaharTech Software Consulting
 * 
 * Versión 1.9:
 * Incorporación de la API moderna java.time (LocalTime, LocalDate y Duration).
 * Se eliminan las conversiones matemáticas manuales y se capturan marcas de tiempo reales.
 * 
 * @author Equipo AzaharTech (Alba Torres, Pau Ferrer)
 * @version 1.9 (Octubre 2026)
 * @since JDK 21 LTS
 * 
 */
import java.util.Scanner;
import java.util.Random;

// [NUEVO DÍA 22] Importación de la API moderna de fechas y tiempos de Java
import java.time.LocalTime;
import java.time.LocalDate;
import java.time.Duration;

public class ControlAccesoQR {
    public static void main(String[] args) {
        // Constantes inmutables
        final String NOMBRE_CENTRO = "IES El Caminàs (Castellón)";
        final String PREFIJO_CENTRO = "CAMINAS";
        final String RUTA_LOGS = ".\\caminas\\terminal\\logs";
        final int MINUTOS_POR_HORA = 60;
        final int SEGUNDOS_POR_HORA = 3600;
        final int SEGUNDOS_POR_MINUTO = 60;
        final double FACTOR_PORCENTAJE = 100.0;
        final long SEMILLA_CALIBRACION = 987654321L;
        final int BASE_TOKEN_SEGURIDAD = 1000;
        final int RANGO_TOKEN_SEGURIDAD = 9000;
        final double MAX_VARIACION_TERMICA = 0.5;
        final long SEMILLA_CALIBRACION = 987654321L;
        final int JORNADA_BASE_MINUTOS = 480;
        final double TEMP_MIN_SEGURA = 15.0;
        final double TEMP_MAX_SEGURA = 35.0;
        final int LONGITUD_DNI_ESTANDAR = 9;

        Scanner teclado = new Scanner(System.in);

        Random generadorVestibulo   = new Random();
        Random generadorPuertaNorte = new Random();
        Random generadorCalibracion = new Random(SEMILLA_CALIBRACION);

        // [NUEVO DÍA 22] Captura automática de fecha y hora del sistema mediante métodos estáticos
        LocalDate fechaActual = LocalDate.now();      // Fecha actual del sistema operativo
        LocalTime horaEmisionQr = LocalTime.now();    // Instante exacto de generación del QR

        String textoTerminalId;
        int terminalId;
        double tempVestibulo;

        String nombrePersonaBruto, nombrePersonaLimpio;
        String dniPersonaBruto, dniPersonaLimpio;
        char perfilPersonaBruto;
        char perfilNormalizado;
        boolean esEntrada;

        int horaEntrada, minutoEntrada;
        int horaSalida, minutoSalida;

        // [NUEVO DÍA 22] Variables de tiempo y duración con java.time
        LocalTime tiempoEntrada;
        LocalTime tiempoSalida;
        Duration duracionEstancia;
        long minutosEstanciaTotal; // toMinutes() devuelve un long

        String tokenResumenVestibulo;
        String tokenResumenPuertaNorte;

        int personasEnCentro = 1200;
        int aforoDisponible = 800;
        int idUltimoFichaje = 1042;

        int segundosActividadTerminal;
        int horasUptime, minutosUptime, segundosUptime;

        int aforoTotal;
        double porcentajeOcupacionReal;
        int porcentajeOcupacionTruncado;
        int porcentajeOcupacionRedondeado;

        double coordenadaXMetros;
        double coordenadaYMetros;
        double distanciaDiagonalAntena;

        int codigoTokenVestibulo;
        int codigoTokenPuertaNorte;
        int codigoTestCalibracion;
        double fluctuacionTermica;
        double tempCalibrada;
        boolean sensorNfcOperativo;
        long desviacionJornadaMinutos;
        int aforoSeguroVisualizado;

        int longitudDni;
        int longitudNombre;
        char letraFinalDni;
        boolean esLongitudDniCorrecta;
        boolean tokenContienePrefijo;
        int posicionPrimerSeparador;

        String numeroDniSolo;
        String letraDniTexto;
        String tokenFormatoBarras;
        String prefijoExtraido;

        System.out.println("=================================================");
        System.out.println("   AZAHARTECH - TERMINAL " + NOMBRE_CENTRO);
        System.out.println("   Versión 1.9 (API moderna de fechas java.time) ");
        System.out.println("=================================================");
        System.out.print("Introduce identificador del terminal en texto (ej. 101): ");
        textoTerminalId = teclado.nextLine();
        terminalId = Integer.parseInt(textoTerminalId);

        System.out.print("Temperatura base del sensor (ºC): ");
        tempVestibulo = teclado.nextDouble();

        System.out.print("Segundos de actividad del terminal (uptime): ");
        segundosActividadTerminal = teclado.nextInt();

        System.out.print("Distancia horizontal a la pantalla en metros (eje X): ");
        coordenadaXMetros = teclado.nextDouble();

        System.out.print("Altura de la pantalla de TV en metros (eje Y): ");
        coordenadaYMetros = teclado.nextDouble();

        teclado.nextLine(); // Limpieza obligatoria del buffer

        System.out.print("DNI de la persona: ");
        dniPersonaBruto = teclado.nextLine();

        System.out.print("Nombre completo: ");
        nombrePersonaBruto = teclado.nextLine();

        dniPersonaLimpio = dniPersonaBruto.trim().toUpperCase();
        nombrePersonaLimpio = nombrePersonaBruto.trim();

        System.out.print("Perfil de acceso (e = estudiante, d = docente, v = visita): ");
        perfilPersonaBruto = teclado.next().charAt(0);
        perfilNormalizado = Character.toUpperCase(perfilPersonaBruto);

        esEntrada = true;

        System.out.print("Hora de entrada (0-23): ");
        horaEntrada = teclado.nextInt();

        System.out.print("Minuto de entrada (0-59): ");
        minutoEntrada = teclado.nextInt();

        System.out.print("Hora de salida (0-23): ");
        horaSalida = teclado.nextInt();

        System.out.print("Minuto de salida (0-59): ");
        minutoSalida = teclado.nextInt();

        // ---------------------------------------------------------------------
        // [NUEVO DÍA 22] PROCESAMIENTO CON JAVA.TIME (SIN CÁLCULOS MANUALES)
        // ---------------------------------------------------------------------
        // 1. Construcción de objetos LocalTime con el método estático of()
        tiempoEntrada = LocalTime.of(horaEntrada, minutoEntrada);
        tiempoSalida  = LocalTime.of(horaSalida, minutoSalida);

        // 2. Cálculo automático del intervalo de estancia mediante Duration
        duracionEstancia = Duration.between(tiempoEntrada, tiempoSalida);
        minutosEstanciaTotal = duracionEstancia.toMinutes(); // Conversión limpia y directa

        personasEnCentro++;
        aforoDisponible--;
        idUltimoFichaje++;

        codigoTokenVestibulo   = BASE_TOKEN_SEGURIDAD + generadorVestibulo.nextInt(RANGO_TOKEN_SEGURIDAD);
        codigoTokenPuertaNorte = BASE_TOKEN_SEGURIDAD + generadorPuertaNorte.nextInt(RANGO_TOKEN_SEGURIDAD);

        codigoTestCalibracion = BASE_TOKEN_SEGURIDAD + generadorCalibracion.nextInt(RANGO_TOKEN_SEGURIDAD);
        fluctuacionTermica = generadorCalibracion.nextDouble() * MAX_VARIACION_TERMICA;
        tempCalibrada = tempVestibulo + fluctuacionTermica;
        sensorNfcOperativo = generadorCalibracion.nextBoolean();

        // Cálculo de desviación usando Math.abs sobre el resultado de Duration
        desviacionJornadaMinutos = Math.abs(minutosEstanciaTotal - JORNADA_BASE_MINUTOS);
        tempSeguraAcotada = Math.max(TEMP_MIN_SEGURA, Math.min(tempCalibrada, TEMP_MAX_SEGURA));

        aforoTotal = personasEnCentro + aforoDisponible;
        aforoSeguroVisualizado = Math.min(personasEnCentro, aforoTotal);

        tokenResumenVestibulo = PREFIJO_CENTRO + "-" + dniPersonaLimpio;
        tokenResumenVestibulo += "-T" + terminalId;
        tokenResumenVestibulo += "-SEC#" + codigoTokenVestibulo;
        tokenResumenVestibulo += "-ESTANCIA-" + minutosEstanciaTotal;
        tokenResumenVestibulo += "-REG" + idUltimoFichaje;

        tokenResumenPuertaNorte = PREFIJO_CENTRO + "-" + dniPersonaLimpio;
        tokenResumenPuertaNorte += "-T102";
        tokenResumenPuertaNorte += "-SEC#" + codigoTokenPuertaNorte;
        tokenResumenPuertaNorte += "-ESTANCIA-" + minutosEstanciaTotal;
        tokenResumenPuertaNorte += "-REG" + idUltimoFichaje;

        horasUptime = segundosActividadTerminal / SEGUNDOS_POR_HORA;
        minutosUptime = (segundosActividadTerminal % SEGUNDOS_POR_HORA) / SEGUNDOS_POR_MINUTO;
        segundosUptime = segundosActividadTerminal % SEGUNDOS_POR_MINUTO;

        porcentajeOcupacionReal = ((double) aforoSeguroVisualizado / aforoTotal) * FACTOR_PORCENTAJE;
        porcentajeOcupacionTruncado = (int) porcentajeOcupacionReal;
        porcentajeOcupacionRedondeado = (int) Math.round(porcentajeOcupacionReal);

        distanciaDiagonalAntena = Math.sqrt(Math.pow(coordenadaXMetros, 2.0) + Math.pow(coordenadaYMetros, 2.0));

        longitudDni = dniPersonaLimpio.length();
        longitudNombre = nombrePersonaLimpio.length();
        letraFinalDni = dniPersonaLimpio.charAt(dniPersonaLimpio.length() - 1);
        esLongitudDniCorrecta = (longitudDni == LONGITUD_DNI_ESTANDAR);
        tokenContienePrefijo = tokenResumenVestibulo.contains(PREFIJO_CENTRO);
        posicionPrimerSeparador = tokenResumenVestibulo.indexOf("-");

        numeroDniSolo = dniPersonaLimpio.substring(0, dniPersonaLimpio.length() - 1);
        letraDniTexto = dniPersonaLimpio.substring(dniPersonaLimpio.length() - 1);
        tokenFormatoBarras = tokenResumenVestibulo.replace("-", "/");
        prefijoExtraido = tokenResumenVestibulo.substring(0, posicionPrimerSeparador);

        // Salida formateada con printf integrando marcas de tiempo de java.time
        System.out.println("\n======================================================================");
        System.out.println("             INFORME OFICIAL DE ACCESO EN VESTÍBULO                   ");
        System.out.println("======================================================================");
        System.out.printf("CENTRO:               %-30s | FECHA: %s%n", NOMBRE_CENTRO, fechaActual);
        System.out.printf("REGISTRO N.º:         #%05d | HORA EMISIÓN QR: %s%n", idUltimoFichaje, horaEmisionQr);
        System.out.printf("TOKEN QR VESTÍBULO:   %s%n", tokenResumenVestibulo);
        System.out.printf("TOKEN QR PUERTA NORTE:%s%n", tokenResumenPuertaNorte);
        System.out.printf("PERSONA:              %-30s | LONGITUD: %d%n", nombrePersonaLimpio.toUpperCase(), longitudNombre);
        System.out.printf("DNI DESGLOSADO:       Número: %s | Letra: '%s'%n", numeroDniSolo, letraDniTexto);
        System.out.printf("PERFIL PROCESADO:     %c | SENTIDO: Entrada (%b)%n", perfilNormalizado, esEntrada);
        System.out.println("----------------------------------------------------------------------");
        System.out.printf("HORARIO FORMAL:       Entrada %s | Salida %s (LocalTime)%n", tiempoEntrada, tiempoSalida);
        System.out.printf("PERMANENCIA EXACTA:   %03d minutos en el centro (Duration.between)%n", minutosEstanciaTotal);
        System.out.printf("DESVIACIÓN JORNADA:   %03d min respecto a jornada completa (480 min).%n", desviacionJornadaMinutos);
        System.out.println("----------------------------------------------------------------------");
        System.out.printf("OCUPACIÓN REDONDEADA: %d %% (Math.round)%n", porcentajeOcupacionRedondeado);
        System.out.printf("DISTANCIA ENFOQUE TV: %.2f metros (Línea de visión a pantalla)%n", distanciaDiagonalAntena);
        System.out.printf("ACTIVO:               %02dh %02dm %02ds (Sensor: %.1f ºC)%n", horasUptime, minutosUptime, segundosUptime, tempSeguraAcotada);
        System.out.printf("REGISTRO LOG:         %s%n", RUTA_LOGS);
        System.out.println("======================================================================");

        teclado.close();
    }
}
```

---

#### 4. Segunda sesión: dojo de entrenamiento y katas de código

Pasamos al dojo de entrenamiento para programar, con tres **katas de dificultad progresiva** sobre la API `java.time`, marcas temporales reales y cálculos de duración:

---

##### Kata 1 (Cinturón blanco / Nivel base). Integración de `java.time` en tu proyecto propio
* **Objetivo.** Incorporar a tu clase `MiProyecto.java` (versión v1.8) la captura de la fecha actual con `LocalDate.now()`, la hora actual con `LocalTime.now()` y el cálculo de una duración temporal con `Duration.between()` sin multiplicaciones manuales.
* **Aplicación según tu proyecto elegido:**
  * En **Aventura conversacional**. Registrar la fecha y hora oficial de inicio de la partida con `LocalDate.now()` y `LocalTime.now()`, y calcular los minutos reales de sesión transcurridos con `Duration.between(horaInicio, horaActual)`.
  * En **Motor de recomendación**. Registrar el instante exacto en que se emite la recomendación con `LocalTime.now()`, y proyectar a qué hora terminará la película sumando su duración mediante `horaActual.plusMinutes(duracionMinutos)`.
  * En **Simulador de físicas 2D**. Registrar la marca temporal exacta de inicio y fin del cálculo cinemático con `LocalTime`, y calcular los segundos transcurridos utilizando `Duration.between().toSeconds()`.
  * En **Bóveda de contraseñas**. Capturar la fecha actual con `LocalDate.now()`, calcular la fecha exacta de expiración sumando días con `fechaActual.plusDays(diasVigencia)`, y registrar la hora del último cambio de clave con `LocalTime.now()`.

---

##### Kata 2 (Cinturón marrón / Nivel avanzado). Proyecciones horarias con `plusMinutes()` y `minusHours()`
* **Contexto técnico.** Demostrar que los métodos de desplazamiento temporal de `LocalTime` son inmutables y devuelven un nuevo objeto, gestionando el cambio de día de forma automática sin condicionales.
* **Misión de la kata:**
  1. En una clase de prueba llamada `PruebaHorarios.java`, declara una hora base:
     ```java
     LocalTime inicioTurno = LocalTime.of(22, 45); // 22:45 h
     ```
  2. Calcula y muestra por consola qué hora será tras:
    * Una primera ronda de 50 minutos: `inicioTurno.plusMinutes(50)`.
    * Un descanso de 20 minutos adicionales: `inicioTurno.plusMinutes(70)`.
    * El momento en que debió prepararse el equipo hace 3 horas: `inicioTurno.minusHours(3)`.
  3. Comprueba cómo al sumar 50 minutos a las `22:45`, Java pasa automáticamente a las `23:35`, y al sumar 90 minutos pasa a las `00:15` del día siguiente sin necesidad de programar ningún algoritmo de control.

---

##### Kata 3 (Cinturón negro / «Hacker AzaharTech»). La trampa del cruce de medianoche en `Duration`
* **Contexto de sistemas.** En sistemas de control de turnos que operan de noche, ¿qué ocurre si la entrada se produce a las `23:00` y la salida a las `02:00`?
* **Misión de la kata:**
  1. En una clase de prueba temporal llamada `PruebaMedianoche.java`, ejecuta:
     ```java
     LocalTime entradaNoche = LocalTime.of(23, 0);
     LocalTime salidaMadrugada = LocalTime.of(2, 0);

     Duration intervalo = Duration.between(entradaNoche, salidaMadrugada);
     System.out.println("Minutos calculados: " + intervalo.toMinutes());
     ```
  2. Comprueba con asombro que la consola imprime un número negativo: **`-1260 minutos`** ($-21$ horas).
  3. Redacta dos líneas de comentario en tu código explicando por qué ocurre esto:  
     *«`LocalTime` solo almacena horas del reloj sin fecha asociada. Al calcular la diferencia entre las 23:00 y las 02:00 del mismo día, Java calcula que las 02:00 ocurrieron 21 horas antes. Para turnos que cruzan la medianoche en sistemas profesionales es necesario combinar fecha y hora mediante la clase `LocalDateTime`»*.

---

### Día 23 - 2 sesiones

---

#### 1. Caso guía en AzaharTech
Son las tres de la tarde del jueves 22 de octubre. Concluimos las 24 horas lectivas del **Sprint 2 de Programación**. 

En la sala de reuniones de **AzaharTech**, **Pau Ferrer** proyecta el archivo `ControlAccesoQR.java`. El programa compila sin errores, genera los tokens con `Random`, calcula distancias con `Math`, parsea datos con `Integer` y gestiona las horas reales con `LocalTime` y `Duration`.

Sin embargo, **Alba Torres** señala el monitor con preocupación:
> *«Pau, el software funciona, pero hemos creado un 'método Dios' (*God Method*): tenemos más de ciento veinte líneas de código acumuladas dentro del `main`. La captura por teclado, las conversiones de hora, la descomposición de segundos, las fórmulas matemáticas y la composición de los tokens están todas mezcladas en un único bloque monolítico.*
>
> *Si mañana el IES El Caminàs nos pide reutilizar la fórmula del token QR para una aplicación web o para la puerta norte, tendríamos que copiar y pegar código duplicado.*
>
> *En AzaharTech el código debe ser modular y mantenible. Hasta ahora hemos aprendido a invocar métodos estáticos ajenos de la biblioteca de Java como `Math.max()` o `Integer.parseInt()`. Hoy daremos el gran salto de ingeniería: **aprenderemos a codificar nuestros propios métodos estáticos auxiliares** con sus parámetros y valores de retorno, transformando el `main` en un orquestador limpio y elegante»*.

---

#### 2. Fundamento teórico: codificación de métodos estáticos propios, parámetros y retorno

```
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                        ANATOMÍA DE UN MÉTODO ESTÁTICO EN JAVA                          │
├────────────────────────────────────────────────────────────────────────────────────────┤
│ public static tipoRetorno nombreMetodo( TipoParam1 param1, TipoParam2 param2... ) {    │
│     // 1. Ámbito local: variables que solo existen dentro de este método               │
│     // 2. Procesamiento o cálculo                                                      │
│     return valorCalculado; // Devuelve el dato a quien lo invocó                       │
│ }                                                                                      │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

##### A. ¿Por qué creamos métodos estáticos dentro de la clase?
Java permite modularizar cualquier programa estructurándolo en **métodos estáticos auxiliares (*funciones de clase*)** dentro del mismo archivo por los siguientes motivos:
1. **Legibilidad y auto-documentación.** El código se lee como un texto estructurado donde el nombre del método explica *qué* hace, ocultando los detalles de *cómo* lo hace.
2. **Reutilización (*DRY - Don't Repeat Yourself*).** Si necesitamos calcular la distancia o construir un token en dos sitios diferentes, invocamos al método sin duplicar fórmulas.
3. **Mantenibilidad y aislamiento.** Si cambia la regla de cálculo de la estancia, solo modificamos el método responsable, sin tocar el resto de la aplicación.

##### B. Parámetros formales y el paso de parámetros por valor
* **Parámetros formales.** Son las variables declaradas entre los paréntesis de la cabecera del método que reciben los datos necesarios para operar (*por ejemplo, `LocalTime entrada, LocalTime salida`*).
* **Argumentos reales.** Son los valores o variables concretas que se le pasan al método en el momento de la llamada desde el `main` (*por ejemplo, `tiempoEntrada, tiempoSalida`*).
* **Paso por valor estricto en Java.** Java **siempre pasa los argumentos por valor (hace una copia)**:
  * Si pasas un tipo primitivo (`int`, `double`), el método recibe una copia del número. Modificar el parámetro dentro del método jamás altera la variable original del `main`.
  * Si pasas un objeto, se copia el puntero de referencia en el Stack, permitiendo operar sobre la instancia compartida en el Heap.

##### C. La sentencia `return` y el ámbito local (*Scope*)
* **Tipo de retorno.** Indica qué tipo de dato devuelven las instrucciones del método (`int`, `double`, `String`, etc.). Si el método no devuelve ningún dato y solo ejecuta acciones de salida, se define como **`void`**.
* **La instrucción `return`.** Detiene de forma inmediata la ejecución del método y transfiere el resultado al punto exacto del `main` donde se produjo la llamada.
* **Ámbito local (*Scope*).** Cualquier variable declarada dentro de un método auxiliar nace y muere dentro de sus llaves `{}`. No consume memoria en el Stack cuando el método termina.

---

#### 3. El código guia final para el Sprint 2: `ControlAccesoQR v2.0` (modularizado)

Refactorizamos la clase guia `ControlAccesoQR.java` descomponiendo los bloques de cálculo en cuatro métodos estáticos auxiliares limpios:

```java
/**
 * SISTEMA DE CONTROL DE ASISTENCIA POR CÓDIGO QR
 * Cliente: IES El Caminàs (Castellón de la Plana)
 * Consultora: AzaharTech Software Consulting
 * 
 * Versión 2.0:
 * Sistema integral de acceso orientado a objetos basado en clases predefinidas.
 * Incorpora la codificación de métodos estáticos propios con parámetros y retorno,
 * desacoplando la lógica de cálculo, descomposición temporal y construcción de tokens.
 * 
 * @author Equipo AzaharTech (Alba Torres, Pau Ferrer)
 * @version 2.0 (Octubre 2026)
 * @since JDK 21 LTS
 * 
 */

import java.util.Scanner;
import java.util.Random;
import java.time.LocalTime;
import java.time.LocalDate;
import java.time.Duration;

public class ControlAccesoQR {

    // -------------------------------------------------------------------------
    // 1. CONSTANTES INMUTABLES DEL SISTEMA (configuración corporativa)
    // -------------------------------------------------------------------------
    final static String NOMBRE_CENTRO = "IES El Caminàs (Castellón)";
    final static String PREFIJO_CENTRO = "CAMINAS";
    final static String RUTA_LOGS = ".\\caminas\\terminal\\logs";
    
    final static int MINUTOS_POR_HORA = 60;
    final static int SEGUNDOS_POR_HORA = 3600;
    final static int SEGUNDOS_POR_MINUTO = 60;
    final static int JORNADA_BASE_MINUTOS = 480; // 8 horas
    final static double FACTOR_PORCENTAJE = 100.0;

    final static int ID_TERMINAL_PUERTA_NORTE = 102;
    final static int BASE_TOKEN_SEGURIDAD = 1000;
    final static int RANGO_TOKEN_SEGURIDAD = 9000;
    final static long SEMILLA_CALIBRACION = 987654321L;

    final static double TEMP_MIN_SEGURA = 15.0;
    final static double TEMP_MAX_SEGURA = 35.0;
    final static double MAX_VARIACION_TERMICA = 0.5;
    final static int LONGITUD_DNI_ESTANDAR = 9;

    // -------------------------------------------------------------------------
    // 2. MÉTODOS ESTÁTICOS PROPIOS (modularización de la lógica del sistema)
    // -------------------------------------------------------------------------

    /**
     * Calcula la estancia neta en minutos entre dos marcas temporales usando Duration.
     */
    public static int calcularEstanciaMinutos(LocalTime entrada, LocalTime salida) {
        return (int) Duration.between(entrada, salida).toMinutes();
    }

    /**
     * Calcula el porcentaje de ocupación con precisión decimal aplicando casting explícito.
     */
    public static double calcularPorcentajeOcupacion(int personas, int aforoTotal) {
        return ((double) personas / aforoTotal) * FACTOR_PORCENTAJE;
    }

    /**
     * Construye y concatena el token QR oficial parametrizado para cualquier monitor.
     */
    public static String construirTokenQR(String prefijo, String dni, int terminalId, int codigoSec, int idFichaje) {
        String token = prefijo + "-" + dni;
        token += "-T" + terminalId;
        token += "-SEC#" + codigoSec;
        token += "-REG" + idFichaje;
        return token;
    }

    /**
     * Calcula la distancia euclídea (hipotenusa) a la pantalla mediante el teorema de Pitágoras.
     */
    public static double calcularDistanciaDiagonal(double x, double y) {
        return Math.sqrt(Math.pow(x, 2.0) + Math.pow(y, 2.0));
    }

    // -------------------------------------------------------------------------
    // 3. MÉTODO PRINCIPAL: ORQUESTADOR SECUENCIAL DEL FLUJO
    // -------------------------------------------------------------------------
    public static void main(String[] args) {
        Scanner teclado = new Scanner(System.in);

        // Generadores independientes en el Heap
        Random generadorVestibulo   = new Random();
        Random generadorPuertaNorte = new Random();
        Random generadorCalibracion = new Random(SEMILLA_CALIBRACION);

        LocalDate fechaActual = LocalDate.now();
        LocalTime horaEmisionQr = LocalTime.now();

        int terminalId;
        double tempVestibulo;

        String nombrePersonaBruto, nombrePersonaLimpio;
        String dniPersonaBruto, dniPersonaLimpio;
        char perfilPersonaBruto;
        char perfilNormalizado;
        boolean esLetraValida;
        boolean esEntrada = true;

        int horaEntrada, minutoEntrada;
        int horaSalida, minutoSalida;

        int personasEnCentro = 1200;
        int aforoDisponible = 800;
        int idUltimoFichaje = 1042;

        int segundosActividadTerminal;
        double coordenadaXMetros, coordenadaYMetros;

        // Captura de datos interactiva
        System.out.println("=================================================");
        System.out.println("   AZAHARTECH - TERMINAL " + NOMBRE_CENTRO);
        System.out.println("   Versión 2.0 Oficial (Arquitectura Modular)       ");
        System.out.println("=================================================");
        System.out.print("Introduce identificador del terminal en texto (ej. 101): ");
        terminalId = Integer.parseInt(teclado.nextLine());

        System.out.print("Temperatura base del sensor (ºC): ");
        tempVestibulo = teclado.nextDouble();

        System.out.print("Segundos de actividad del terminal (uptime): ");
        segundosActividadTerminal = teclado.nextInt();

        System.out.print("Distancia horizontal a pantalla en metros (eje X): ");
        coordenadaXMetros = teclado.nextDouble();

        System.out.print("Altura de la pantalla de TV en metros (eje Y): ");
        coordenadaYMetros = teclado.nextDouble();

        teclado.nextLine(); // Limpieza del buffer de entrada

        System.out.print("DNI de la persona: ");
        dniPersonaBruto = teclado.nextLine();

        System.out.print("Nombre completo: ");
        nombrePersonaBruto = teclado.nextLine();

        // Normalización y limpieza con métodos de String y Character
        dniPersonaLimpio = dniPersonaBruto.trim().toUpperCase();
        nombrePersonaLimpio = nombrePersonaBruto.trim().toUpperCase();

        System.out.print("Perfil de acceso (e = estudiante, d = docente, v = visita): ");
        perfilPersonaBruto = teclado.next().charAt(0);
        perfilNormalizado = Character.toUpperCase(perfilPersonaBruto);
        esLetraValida = Character.isLetter(perfilNormalizado);

        System.out.print("Hora de entrada (0-23): ");
        horaEntrada = teclado.nextInt();
        System.out.print("Minuto de entrada (0-59): ");
        minutoEntrada = teclado.nextInt();

        System.out.print("Hora de salida (0-23): ");
        horaSalida = teclado.nextInt();
        System.out.print("Minuto de salida (0-59): ");
        minutoSalida = teclado.nextInt();

        // ---------------------------------------------------------------------
        // INVOCACIÓN DE NUESTROS MÉTODOS ESTÁTICOS PROPIOS
        // ---------------------------------------------------------------------
        LocalTime tiempoEntrada = LocalTime.of(horaEntrada, minutoEntrada);
        LocalTime tiempoSalida  = LocalTime.of(horaSalida, minutoSalida);

        // 1. Llamada a método propio de cómputo de tiempo
        int minutosEstanciaTotal = calcularEstanciaMinutos(tiempoEntrada, tiempoSalida);

        personasEnCentro++;
        aforoDisponible--;
        idUltimoFichaje++;

        // Generación de códigos numéricos
        int codigoTokenVestibulo   = BASE_TOKEN_SEGURIDAD + generadorVestibulo.nextInt(RANGO_TOKEN_SEGURIDAD);
        int codigoTokenPuertaNorte = BASE_TOKEN_SEGURIDAD + generadorPuertaNorte.nextInt(RANGO_TOKEN_SEGURIDAD);
        int codigoTestCalibracion  = BASE_TOKEN_SEGURIDAD + generadorCalibracion.nextInt(RANGO_TOKEN_SEGURIDAD);

        double tempCalibrada = tempVestibulo + (generadorCalibracion.nextDouble() * MAX_VARIACION_TERMICA);
        boolean sensorNfcOperativo = generadorCalibracion.nextBoolean();

        int desviacionJornadaMinutos = Math.abs(minutosEstanciaTotal - JORNADA_BASE_MINUTOS);
        double tempSeguraAcotada = Math.max(TEMP_MIN_SEGURA, Math.min(tempCalibrada, TEMP_MAX_SEGURA));

        int aforoTotal = personasEnCentro + aforoDisponible;
        int aforoSeguroVisualizado = Math.min(personasEnCentro, aforoTotal);

        // 2. Llamadas a método propio para construir los tokens de forma limpia y reutilizable
        String tokenResumenVestibulo   = construirTokenQR(PREFIJO_CENTRO, dniPersonaLimpio, terminalId, codigoTokenVestibulo, idUltimoFichaje);
        String tokenResumenPuertaNorte = construirTokenQR(PREFIJO_CENTRO, dniPersonaLimpio, ID_TERMINAL_PUERTA_NORTE, codigoTokenPuertaNorte, idUltimoFichaje);

        // 3. Llamada a método propio para el cálculo porcentual con casting
        double porcentajeOcupacionReal = calcularPorcentajeOcupacion(aforoSeguroVisualizado, aforoTotal);
        int porcentajeOcupacionRedondeado = (int) Math.round(porcentajeOcupacionReal);

        // 4. Llamada a método propio para la distancia geométrica
        double distanciaDiagonalAntena = calcularDistanciaDiagonal(coordenadaXMetros, coordenadaYMetros);

        // Descomposición temporal de servicio del terminal
        int horasUptime   = segundosActividadTerminal / SEGUNDOS_POR_HORA;
        int minutosUptime = (segundosActividadTerminal % SEGUNDOS_POR_HORA) / SEGUNDOS_POR_MINUTO;
        int segundosUptime = segundosActividadTerminal % SEGUNDOS_POR_MINUTO;

        // Métodos de String para desglose de DNI
        String numeroDniSolo = dniPersonaLimpio.substring(0, dniPersonaLimpio.length() - 1);
        char letraFinalDni   = dniPersonaLimpio.charAt(dniPersonaLimpio.length() - 1);
        boolean esLongitudDniCorrecta = (dniPersonaLimpio.length() == LONGITUD_DNI_ESTANDAR);
        String tokenFormatoBarras = tokenResumenVestibulo.replace("-", "/");

        // ---------------------------------------------------------------------
        // SALIDA FORMATEADA PROFESIONAL (System.out.printf)
        // ---------------------------------------------------------------------
        System.out.println("\n======================================================================");
        System.out.println("             INFORME OFICIAL DE ACCESO EN VESTÍBULO                   ");
        System.out.println("======================================================================");
        System.out.printf("CENTRO:               %-30s | FECHA: %s%n", NOMBRE_CENTRO, fechaActual);
        System.out.printf("REGISTRO N.º:         #%05d | HORA EMISIÓN QR: %s%n", idUltimoFichaje, horaEmisionQr);
        System.out.printf("TOKEN QR VESTÍBULO:   %s%n", tokenResumenVestibulo);
        System.out.printf("TOKEN FORMATO BARRAS: %s (replace)%n", tokenFormatoBarras);
        System.out.printf("TOKEN QR PUERTA NORTE:%s%n", tokenResumenPuertaNorte);
        System.out.printf("PERSONA:              %-30s | LONGITUD: %d%n", nombrePersonaLimpio, nombrePersonaLimpio.length());
        System.out.printf("DNI DESGLOSADO:       Número: %s | Letra: '%c'%n", numeroDniSolo, letraFinalDni);
        System.out.printf("PERFIL PROCESADO:     %c (Válido: %b) | SENTIDO: Entrada (%b)%n", perfilNormalizado, esLetraValida, esEntrada);
        System.out.println("----------------------------------------------------------------------");
        System.out.printf("HORARIO FORMAL:       Entrada %s | Salida %s (LocalTime)%n", tiempoEntrada, tiempoSalida);
        System.out.printf("PERMANENCIA EXACTA:   %03d minutos (Método modular calcularEstancia)%n", minutosEstanciaTotal);
        System.out.printf("DESVIACIÓN JORNADA:   %03d min respecto a jornada completa (480 min).%n", desviacionJornadaMinutos);
        System.out.println("----------------------------------------------------------------------");
        System.out.printf("OCUPACIÓN REDONDEADA: %d %% (Math.round) | Exacta: %6.2f %%%n", porcentajeOcupacionRedondeado, porcentajeOcupacionReal);
        System.out.printf("DISTANCIA ENFOQUE TV: %.2f metros (Método modular calcularDistancia)%n", distanciaDiagonalAntena);
        System.out.printf("ACTIVO:               %02dh %02dm %02ds (Sensor acotado: %.1f ºC)%n", horasUptime, minutosUptime, segundosUptime, tempSeguraAcotada);
        System.out.printf("DIAGNÓSTICO TEST:     Patrón #%04d | Sensor NFC: Activo (%b)%n", codigoTestCalibracion, sensorNfcOperativo);
        System.out.printf("REGISTRO LOG:         %s%n", RUTA_LOGS);
        System.out.println("======================================================================");

        teclado.close();
    }
}
```

---

#### 4. Segunda sesión: dojo de entrenamiento y katas de código

Pasamos al dojo de entrenamiento para programar, con tres **katas de programación modular** para entrenar la codificación de métodos estáticos, la gestión de parámetros y el ámbito de memoria:

---

##### Kata 1 (Cinturón blanco / Nivel base). Extracción de métodos estáticos en tu proyecto propio
* **Objetivo.** Refactorizar tu clase `MiProyecto.java` (versión v1.9) extrayendo al menos **dos métodos estáticos auxiliares propios** (`public static`) con parámetros de entrada y sentencia `return`, eliminando la acumulación de cálculos dentro del `main`.
* **Aplicación según tu proyecto elegido:**
  * En **Aventura conversacional**. Codificar un método estático `public static int calcularDanoTotal(int fuerza, int factorArma)` y un método `public static String formatearEstadoHeroe(String nombre, int vida, int oro)`.
  * En **Motor de recomendación**. Codificar un método estático `public static double calcularPuntuacionAfinidad(double base, double pesoTag)` y un método `public static String generarCodigoItem(String categoria, int anio, int id)`.
  * En **Simulador de físicas 2D**. Codificar un método estático `public static double calcularModuloVelocidad(double vx, double vy)` y un método `public static double proyectarPosicionFinal(double vInicial, double aceleracion, double tiempo)`.
  * En **Bóveda de contraseñas**. Codificar un método estático `public static int calcularDiasRestantes(int diasTotales, int diasConsumidos)` y un método `public static String formatearCredencialSegura(String servicio, String usuario, int pin)`.

---

##### Kata 2 (Cinturón marrón / Nivel avanzado). El aislamiento del ámbito local (*scope*) y paso por valor
* **Contexto técnico.** Demostrar empíricamente que modificar un parámetro primitivo dentro de un método estático no tiene ningún efecto sobre la variable original del `main`.
* **Misión de la kata:**
  1. En una clase de prueba llamada `PruebaPasoPorValor.java`, escribe el siguiente método estático:
     ```java
     public static void intentarModificar(int numero) {
         numero = numero + 100; // Modificamos el parámetro local
         System.out.println("Dentro del método: numero = " + numero);
     }
     ```
  2. En el `main`, declara `int valor = 50;`, invoca `intentarModificar(valor);` e imprime después el contenido de `valor`.
  3. Comprueba en la consola que fuera del método la variable sigue valiendo `50`.
  4. Redacta dos líneas de comentario en tu código explicando por qué ocurre esto: **Java pasa los argumentos primitivos por copia de valor; el método opera sobre una celda del Stack aislada que se destruye al llegar a su llave de cierre**.

---

##### Kata 3 (Cinturón negro / «Hacker AzaharTech»). Composición y encadenamiento de métodos estáticos
* **Contexto de arquitectura.** Demostrar cómo el valor devuelto por un método estático puede actuar como argumento de entrada de otro sin necesidad de crear variables intermedias en el `main`.
* **Misión de la kata:**
  1. En tu proyecto propio o en una clase de prueba, codifica dos métodos estáticos complementarios: uno que calcule un valor numérico (`calcularSubtotal`) y otro que lo reciba para aplicar un factor (`aplicarImpuesto`).
  2. En el `main`, realiza la invocación encadenada en una sola línea:
     ```java
     double totalFinal = aplicarImpuesto(calcularSubtotal(unidades, precioBase));
     ```
  3. Anota en tu cuaderno técnico cómo la JVM apila y desapila las llamadas en el *Call Stack* de dentro hacia afuera.

---

#### 5. Cierre formal en Git y sincronización visual en IntelliJ
Cada estudiante finaliza el Sprint 2 registrando su código modular en GitHub desde la interfaz gráfica de IntelliJ IDEA:

1. Aplica el autoformateo oficial: `Ctrl + Alt + L`.
2. Optimiza las importaciones para limpiar dependencias no usadas: `Ctrl + Alt + O`.
3. Abre el panel lateral **Commit** (`Alt + 0` o `Ctrl + K`).
4. Selecciona tu archivo `pr/src/MiProyecto.java`.
5. Escribe el mensaje convencional de entrega del hito:
   ```text
   refactor(pr): modularizar aplicacion v2.0 con metodos estaticos propios y cierre de sprint 2
   ```
6. Pulsa **Commit and Push...** y confirma el envío al servidor remoto de GitHub.
7. Mañana viernes, durante la sesión de Entornos de Desarrollo, registrarás todo el repositorio bajo la etiqueta oficial de release **`v0.2.0-sprint2`**.