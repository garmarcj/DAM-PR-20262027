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

Ampliamos el código maestro sustituyendo el código fijo por la generación de un token pseudoaleatorio de seguridad con la clase predefinida `Random`.

##### Paso A. Algoritmo en PSeInt (`pr/pseudocodigo/ControlAccesoQR.psc` — v1.1)

```psc
Algoritmo ControlAccesoQR
    // =========================================================================
    // SISTEMA DE CONTROL DE ASISTENCIA QR - IES EL CAMINAS (Castellon)
    // Version: 1.1 (Evolucion: Generacion Aleatoria de Seguridad)
    // =========================================================================

    Definir NOMBRE_CENTRO, PREFIJO_CENTRO, RUTA_LOGS Como Cadena
    Definir MINUTOS_POR_HORA, SEGUNDOS_POR_HORA, SEGUNDOS_POR_MINUTO Como Entero
    Definir FACTOR_PORCENTAJE Como Real
    
    NOMBRE_CENTRO <- "IES El Caminas (Castellon)"
    PREFIJO_CENTRO <- "CAMINAS"
    RUTA_LOGS <- ".\\caminas\\terminal\\logs"
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
 * Versión 1.1 (Sprint 2 - Día 13):
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
        final String RUTA_LOGS = ".\\caminas\\terminal\\logs";
        final int MINUTOS_POR_HORA = 60;
        final int SEGUNDOS_POR_HORA = 3600;
        final int SEGUNDOS_POR_MINUTO = 60;
        final double FACTOR_PORCENTAJE = 100.0;

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
        codigoSeguridadAleatorio = 1000 + generadorSeguridad.nextInt(9000);

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
* **Instrucciones:** Abre tu archivo maestro `MiProyecto.java` (en versión v1.0) e incorpora un generador aleatorio:
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

## Semana 4. Fundamentos de POO, instanciación y gestión de memoria (Stack vs. Heap)

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

Ampliamos el programa maestro modelando la infraestructura de los dos accesos del centro educativo: instanciamos **dos generadores independientes con `new`** para garantizar la autonomía de emisión entre el monitor del vestíbulo y el monitor de la Puerta Norte.

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
    RUTA_LOGS <- ".\\caminas\\terminal\\logs"
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
 * Versión 1.2 (Sprint 2 - Día 14):
 * Múltiples instancias en el Heap con 'new' frente a copias de punteros en el Stack.
 * Soporte para dos monitores de acceso independientes (Vestíbulo principal y Puerta Norte).
 *
 * @author Equipo AzaharTech (Alba Torres, Pau Ferrer)
 * @version 1.2 (Octubre 2026)
 * @since JDK 21 LTS
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
        codigoTokenVestibulo   = 1000 + generadorVestibulo.nextInt(9000);   // Token vestíbulo [1000 - 9999]
        codigoTokenPuertaNorte = 1000 + generadorPuertaNorte.nextInt(9000); // Token puerta norte [1000 - 9999]

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
 * Versión 1.3 (Sprint 2 - Día 15):
 * Constructores con parámetros (semilla fija), tipos de retorno (double, boolean)
 * y calibración diagnóstica de sensores de hardware.
 *
 * @author Equipo AzaharTech (Alba Torres, Pau Ferrer)
 * @version 1.3 (Octubre 2026)
 * @since JDK 21 LTS
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

        // [NUEVO DÍA 15] Semilla fija para pruebas de calibración repetibles
        final long SEMILLA_CALIBRACION = 987654321L;

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
        System.out.println("   Versión 1.3 (Diagnóstico y Retornos de Objetos) ");
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
        codigoTokenVestibulo   = 1000 + generadorVestibulo.nextInt(9000);
        codigoTokenPuertaNorte = 1000 + generadorPuertaNorte.nextInt(9000);

        // [NUEVO DÍA 15] Invocación de métodos con distintos tipos de retorno:
        // 1. Método con retorno entero (int): código patrón reproducible
        codigoTestCalibracion = 1000 + generadorCalibracion.nextInt(9000);

        // 2. Método con retorno decimal (double): devuelve un valor entre [0.0 y 1.0)
        fluctuacionTermica = generadorCalibracion.nextDouble() * 0.5; // Escala la variación a un máximo de 0.5 ºC
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
* **Instrucciones:** Abre tu archivo maestro `MiProyecto.java` (en versión v1.2) e incorpora las siguientes llamadas:
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

#### 5. Cierre formal en Git y sincronización visual en IntelliJ
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

