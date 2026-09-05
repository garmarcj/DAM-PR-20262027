# MÓDULO PROFESIONAL: PROGRAMACIÓN (PR)

## SPRINT 1. Algorítmica y fundamentos de programación (3 semanas | 24 horas)

---

# SEMANA 1: EL NACIMIENTO DE LA APLICACIÓN — ESTRUCTURA, VARIABLES Y TIPOS DE DATOS (8 HORAS)
### Hilo conductor metodológico: «Código Incremental Vivo»
A lo largo de este sprint no crearemos ejercicios sueltos. Desarrollaremos y evolucionaremos **un único archivo de pseudocódigo (`ControlAccesoQR.psc`) y una única clase Java (`ControlAccesoQR.java`)** para el caso guía del **IES El Caminàs**, mientras cada estudiante hace evolucionar de forma paralela el archivo único de **su proyecto elegido de la bolsa de proyectos**.

---

## DÍA 1 (Lunes, 14 de septiembre de 2026 — 2 horas lectivas)
### Versión de la aplicación: `ControlAccesoQR v0.1` (Esqueleto y Parámetros Numéricos)
* **Objetivo técnico:** Crear el archivo fuente maestro, comprender la estructura de una clase Java y registrar los primeros datos numéricos del terminal (`int` y `double`).
* **Criterios de Evaluación vinculados:** RA1.a, RA1.b, RA1.c, RA1.d, RA1.e.

---

#### 1. Caso práctico narrativo en AzaharTech
Son las once de la mañana en la sede de **AzaharTech** en Castellón de la Plana. **Laia Claramunt** conecta su portátil al proyector principal. En pantalla aparece el entorno de desarrollo IntelliJ IDEA completamente vacío:

> *«Equipo, comenzamos el desarrollo del sistema de acceso para el **IES El Caminàs**. No vamos a hacer pruebas desechables en archivos temporales; desde hoy abrimos el archivo oficial de la aplicación: **`ControlAccesoQR`**.*
>
> *Una aplicación profesional crece capa a capa. El equipo directivo del instituto necesita que el terminal empiece registrando los parámetros físicos del vestíbulo: el número del aula donde se ubica la pantalla y la temperatura ambiente del sensor térmico.*
>
> *Hoy crearemos el esqueleto del programa, aprenderemos qué ocurre en la memoria RAM al reservar variables numéricas (`int` y `double`) y capturaremos los primeros datos desde el teclado»*.

---

#### 2. Fundamento teórico: Estructura del programa y variables numéricas en RAM

```
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                          EL MODELO DE MEMORIA DEL TERMINAL                             │
├───────────────────┬───────────────┬──────────────────────┬─────────────────────────────┤
│ Variable          │ Tipo en Java  │ Espacio en RAM       │ Valor almacenado            │
├───────────────────┼───────────────┼──────────────────────┼─────────────────────────────┤
│ terminalId        │ int           │ 32 bits (4 bytes)    │ 101 (Número entero)         │
│ tempVestibulo     │ double        │ 64 bits (8 bytes)    │ 21.5 (Coma flotante)        │
└───────────────────┴───────────────┴──────────────────────┴─────────────────────────────┘
```

1. **La clase como contenedor maestro:** En Java, todo código pertenece a una clase (`public class ControlAccesoQR`). El archivo en disco debe llamarse exactamente igual: `ControlAccesoQR.java`.
2. **El método de entrada (`main`):** La JVM busca la instrucción `public static void main(String[] args)` para iniciar la ejecución secuencial de arriba hacia abajo.
3. **Variables numéricas primitivas:**
    * **`int`:** Almacena números enteros sin decimales (de $-2.147$ a $+2.147$ millones).
    * **`double`:** Almacena números reales con precisión decimal de 64 bits.
4. **Captura con `Scanner`:** Creamos un canal de lectura interactivo (`Scanner teclado = new Scanner(System.in);`) que captura enteros con `nextInt()` y decimales con `nextDouble()`.

---

#### 3. Andamiaje didáctico: Código maestro `ControlAccesoQR v0.1`

##### Paso A. Algoritmo base en PSeInt (`pr/pseudocodigo/ControlAccesoQR.psc` — v0.1)
```psc
Algoritmo ControlAccesoQR
    // =========================================================================
    // SISTEMA DE CONTROL DE ASISTENCIA QR - IES EL CAMINAS (Castellon)
    // Version: 0.1 (Esqueleto y Parametros Fisicos)
    // =========================================================================
    
    // 1. Declaracion de variables en memoria
    Definir terminalId Como Entero
    Definir tempVestibulo Como Real
    
    // 2. Captura de parametros iniciales
    Escribir "================================================="
    Escribir "   AZAHARTECH - TERMINAL DE ACCESO VESTIBULO     "
    Escribir "   Cliente: IES El Caminas (Curso 2026/2027)     "
    Escribir "================================================="
    Escribir "Introduce el numero identificador del terminal (ID):"
    Leer terminalId
    Escribir "Introduce la temperatura del sensor del vestibulo (C):"
    Leer tempVestibulo
    
    // 3. Confirmacion de parametros en pantalla
    Escribir "-------------------------------------------------"
    Escribir "Terminal configurado: #", terminalId
    Escribir "Lectura termica:      ", tempVestibulo, " C"
    Escribir "Estado del hardware:  EN ESPERA"
FinAlgoritmo
```

##### Paso B. Traducción a Java en IntelliJ (`pr/src/ControlAccesoQR.java` — v0.1)
```java
/**
 * SISTEMA DE CONTROL DE ASISTENCIA POR CÓDIGO QR
 * Cliente: IES El Caminàs (Castellón de la Plana)
 * Consultora: AzaharTech Software Consulting
 * 
 * Versión 0.1: Esqueleto de ejecución y parámetros numéricos del terminal.
 * Módulo: Programación (PR) - Sprint 1 (RA1)
 */
import java.util.Scanner;

public class ControlAccesoQR {
    public static void main(String[] args) {
        // Inicialización del lector de consola
        Scanner teclado = new Scanner(System.in);
        
        // 1. Declaración de variables numéricas primitivas en memoria
        int terminalId;
        double tempVestibulo;
        
        // 2. Entrada de datos interactiva
        System.out.println("=================================================");
        System.out.println("   AZAHARTECH - TERMINAL DE ACCESO VESTÍBULO     ");
        System.out.println("   Cliente: IES El Caminàs (Curso 2026/2027)     ");
        System.out.println("=================================================");
        System.out.print("Introduce el número identificador del terminal (ID): ");
        terminalId = teclado.nextInt();
        
        System.out.print("Introduce la temperatura del sensor del vestíbulo (ºC): ");
        tempVestibulo = teclado.nextDouble();
        
        // 3. Salida de confirmación del terminal
        System.out.println("-------------------------------------------------");
        System.out.println("Terminal configurado: #" + terminalId);
        System.out.println("Lectura térmica:      " + tempVestibulo + " ºC");
        System.out.println("Estado del hardware:  EN ESPERA");
        
        teclado.close();
    }
}
```

---

#### 4. Trabajo del estudiante: Nacimiento de su proyecto propio
Cada estudiante crea en su carpeta `pr/pseudocodigo/` el archivo maestro de su proyecto propio (ej. `SistemaGestion.psc`) y en `pr/src/` la clase `SistemaGestion.java` v0.1:
* Define e inicializa dos variables numéricas (`int` y `double`) que representen datos físicos o de arranque de su negocio (ej. código de sucursal y aforo; número de almacén y tarifa base).
* Compila y verifica la ejecución en la consola de IntelliJ.

---
---

## DÍA 2 (Martes, 15 de septiembre de 2026 — 2 horas lectivas)
### Versión de la aplicación: `ControlAccesoQR v0.2` (Identidad y Estados Lógicos)
* **Objetivo técnico:** Modificar y expandir el archivo maestro de ayer incorporando tipos de texto (`String`), caracteres individuales (`char`), estados booleanos (`boolean`) y resolviendo el salto de buffer de `Scanner`.
* **Criterios de Evaluación vinculados:** RA1.a, RA1.d, RA1.e.

---

### 1. Caso práctico narrativo en AzaharTech
Es martes por la mañana. **Pau Ferrer** ejecuta `ControlAccesoQR` v0.1 y muestra la pantalla:
> *«El terminal ya arranca y guarda el número de terminal `101` y la temperatura `21.5`. Pero cuando un estudiante acerca el móvil a la pantalla del vestíbulo, el sistema no sabe a quién pertenece ese escaneo»*.

**Alba Torres** toma el teclado y abre el archivo de ayer:
> *«No vamos a crear un programa nuevo. Vamos a evolucionar `ControlAccesoQR.java`. Añadiremos los campos para el nombre del estudiante, su DNI, la letra de su grupo y una bandera lógica que indique si su matrícula está activa.*
>
> *Pero atención a la trampa de Java: al leer números antes que textos, el buffer del teclado guarda un salto de línea invisible (`\n`) que debemos limpiar para que el programa no se salte la lectura del nombre»*.

---

### 2. Fundamento teórico: Caracteres, Cadenas y el Buffer del Scanner

```
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                        EL PROBLEMA DEL BUFFER RESIDUAL EN SCANNER                      │
├────────────────────────────────────────────────────────────────────────────────────────┤
│ Usuario teclea: [ 1 ][ 0 ][ 1 ][ \n (Enter) ]                                          │
│ nextInt() lee:  [ 1 ][ 0 ][ 1 ]           ──► Deja el [ \n ] flotando en el buffer.    │
│ nextLine() lee: [ \n ]                    ──► Cree que el usuario pulsó Enter vacío.   │
│                                                                                        │
│ SOLUCIÓN:       teclado.nextLine();       ──► Limpia el buffer antes del texto real.   │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

1. **`char` frente a `String`:**
    * `char letraGrupo = 'B';`: Un único carácter delimitado obligatoriamente por comillas simples (`' '`).
    * `String nombre = "Juan Pérez";`: Objeto que gestiona texto de cualquier longitud entre comillas dobles (`" "`).
2. **`boolean` (Estado del sistema):** Almacena `true` o `false`. Ideal para variables de control como `matriculaActiva`.
3. **Limpieza del buffer:** Siempre que se invoque `nextInt()` o `nextDouble()` y la siguiente instrucción sea `nextLine()`, se debe intercalar una llamada a `teclado.nextLine();` para vaciar el salto de línea residual.

---

### 3. Andamiaje didáctico: Evolución a `ControlAccesoQR v0.2`

Observa cómo el código de ayer se amplía añadiendo los bloques de identidad sin eliminar lo anterior.

##### Paso A. Expansión en PSeInt (`pr/pseudocodigo/ControlAccesoQR.psc` — v0.2)
```psc
Algoritmo ControlAccesoQR
    // =========================================================================
    // SISTEMA DE CONTROL DE ASISTENCIA QR - IES EL CAMINAS (Castellon)
    // Version: 0.2 (Evolucion: Incorporacion de Identidad del Alumnado)
    // =========================================================================
    
    // 1. Declaracion de variables del terminal (Día 1)
    Definir terminalId Como Entero
    Definir tempVestibulo Como Real
    
    // [NUEVO DÍA 2] Declaracion de variables de identidad del usuario
    Definir nombreEstudiante Como Cadena
    Definir dniEstudiante Como Cadena
    Definir letraGrupo Como Caracter
    Definir matriculaActiva Como Logico
    
    // 2. Captura de parametros del terminal (Día 1)
    Escribir "=== AZAHARTECH: TERMINAL DE ACCESO VESTIBULO ==="
    Escribir "Introduce el numero identificador del terminal (ID):"
    Leer terminalId
    Escribir "Introduce la temperatura del sensor (C):"
    Leer tempVestibulo
    
    // [NUEVO DÍA 2] Captura de datos del alumno que escanea
    Escribir "-------------------------------------------------"
    Escribir "ESCANEANDO IDENTIFICADOR DE ESTUDIANTE..."
    Escribir "Introduce el DNI del alumno:"
    Leer dniEstudiante
    Escribir "Introduce el nombre completo del alumno:"
    Leer nombreEstudiante
    Escribir "Introduce la letra de su grupo (A, B o C):"
    Leer letraGrupo
    
    matriculaActiva <- Verdadero // Estado por defecto
    
    // 3. Salida de datos consolidada
    Escribir "================================================="
    Escribir "REGISTRO DE FICHAJE EMITIDO:"
    Escribir "Terminal:   #", terminalId, " (Sensor: ", tempVestibulo, " C)"
    Escribir "Estudiante: ", nombreEstudiante, " (DNI: ", dniEstudiante, ")"
    Escribir "Grupo:      ", letraGrupo, " DAM"
    Escribir "Matricula:  Activa (", matriculaActiva, ")"
    Escribir "================================================="
FinAlgoritmo
```

##### Paso B. Expansión en Java en IntelliJ (`pr/src/ControlAccesoQR.java` — v0.2)
Abrimos el archivo `ControlAccesoQR.java` en IntelliJ y lo modificamos directamente:

```java
/**
 * SISTEMA DE CONTROL DE ASISTENCIA POR CÓDIGO QR
 * Cliente: IES El Caminàs (Castellón de la Plana)
 * Consultora: AzaharTech Software Consulting
 * 
 * Versión 0.2: Incorporación de identidad del alumnado y tipos alfanuméricos.
 * Módulo: Programación (PR) - Sprint 1 (RA1)
 */
import java.util.Scanner;

public class ControlAccesoQR {
    public static void main(String[] args) {
        Scanner teclado = new Scanner(System.in);
        
        // 1. Variables de hardware y terminal (Día 1)
        int terminalId;
        double tempVestibulo;
        
        // [NUEVO DÍA 2] Variables de identidad y estado lógico
        String dniEstudiante;
        String nombreEstudiante;
        char letraGrupo;
        boolean matriculaActiva;
        
        // 2. Captura de parámetros del terminal (Día 1)
        System.out.println("=================================================");
        System.out.println("   AZAHARTECH - TERMINAL DE ACCESO VESTÍBULO     ");
        System.out.println("   Cliente: IES El Caminàs (Curso 2026/2027)     ");
        System.out.println("=================================================");
        System.out.print("Introduce el número identificador del terminal (ID): ");
        terminalId = teclado.nextInt();
        
        System.out.print("Introduce la temperatura del sensor (ºC): ");
        tempVestibulo = teclado.nextDouble();
        
        // [LIMPIEZA DE BUFFER OBLIGATORIA]
        teclado.nextLine(); // Consume el salto de línea residual antes de leer texto
        
        // [NUEVO DÍA 2] Captura de datos del alumno
        System.out.println("-------------------------------------------------");
        System.out.println("REGISTRO DE ACCESO EN CURSO...");
        System.out.print("Introduce el DNI del alumno: ");
        dniEstudiante = teclado.nextLine();
        
        System.out.print("Introduce el nombre completo del alumno: ");
        nombreEstudiante = teclado.nextLine(); // Permite capturar nombres y apellidos con espacios
        
        System.out.print("Introduce la letra de su grupo (A, B o C): ");
        letraGrupo = teclado.next().charAt(0); // Extrae el primer carácter introducido
        
        matriculaActiva = true; // Asignación de literal booleano
        
        // 3. Salida de datos consolidada
        System.out.println("=================================================");
        System.out.println("REGISTRO DE FICHAJE EMITIDO:");
        System.out.println("Terminal:   #" + terminalId + " (Sensor: " + tempVestibulo + " ºC)");
        System.out.println("Estudiante: " + nombreEstudiante + " (DNI: " + dniEstudiante + ")");
        System.out.println("Grupo:      " + letraGrupo + " DAM");
        System.out.println("Matrícula:  Activa (" + matriculaActiva + ")");
        System.out.println("=================================================");
        
        teclado.close();
    }
}
```

---

#### 4. Trabajo del estudiante: Evolución de su proyecto propio
Cada estudiante abre su archivo `SistemaGestion.psc` y `SistemaGestion.java`:
* Añade los campos alfanuméricos de su proyecto (`String` para descripciones o clientes, `char` para códigos de zona o categoría, `boolean` para disponibilidad o estado activo).
* Aplica la limpieza del buffer con `teclado.nextLine()`.
* Ejecuta pruebas verificando que se pueden introducir nombres con espacios sin saltos inesperados.

---
---

## DÍA 3 (Miércoles, 16 de septiembre de 2026 — 2 horas lectivas)
### Versión de la aplicación: `ControlAccesoQR v0.3` (Cálculos Aritméticos y Concatenación)
* **Objetivo técnico:** Incorporar a la aplicación viva el cálculo de tiempos lectivos, sumas aritméticas y la construcción del mensaje de confirmación mediante concatenación protegida.
* **Criterios de Evaluación vinculados:** RA1.a, RA1.d, RA1.e, RA1.g.

---

### 1. Caso práctico narrativo en AzaharTech
Es miércoles por la mañana. **Laia Claramunt** revisa la versión v0.2:
> *«El sistema ya sabe qué terminal lee y qué alumno pasa. Ahora el IES El Caminàs nos pide procesar el tiempo lectivo: cada acceso matutino suma una sesión base de 50 minutos lectivos. Si el alumno entra también por la tarde a un taller voluntario de refuerzo, debemos sumar ambos accesos y calcular cuántos minutos lectivos totales acumula en el centro hoy.*
>
> *Hoy aprenderemos a operar matemáticamente sobre las variables de nuestro programa y a componer un mensaje unificado donde convivan números y texto sin que el operador `+` distorsione los cálculos»*.

---

### 2. Fundamento teórico: Aritmética y Concatenación Segura

1. **La sobrecarga del operador `+`:**
    * Si ambos operandos son numéricos: realiza una **suma matemática** (`50 + 50 = 100`).
    * Si al menos un operando es una cadena de texto: realiza una **concatenación** (unión de textos).
2. **Evaluación de izquierda a derecha:**
   ```java
   System.out.println("Minutos: " + 50 + 50);   // Imprime "Minutos: 5050" (¡ERROR!)
   System.out.println("Minutos: " + (50 + 50)); // Imprime "Minutos: 100"  (CORRECTO)
   ```
3. **Multiplicación y prioridad:** La multiplicación (`*`) se evalúa antes que la suma (`+`), a menos que usemos paréntesis `()`.

---

### 3. Andamiaje didáctico: Evolución a `ControlAccesoQR v0.3`

Ampliamos el código de la versión v0.2 añadiendo el bloque de cálculo de sesiones lectivas y la composición del mensaje de confirmación.

##### Paso A. Expansión en PSeInt (`pr/pseudocodigo/ControlAccesoQR.psc` — v0.3)
```psc
Algoritmo ControlAccesoQR
    // =========================================================================
    // SISTEMA DE CONTROL DE ASISTENCIA QR - IES EL CAMINAS (Castellon)
    // Version: 0.3 (Evolucion: Calculo de Tiempos y Concatenacion)
    // =========================================================================
    
    // 1. Variables previas (Días 1 y 2)
    Definir terminalId Como Entero
    Definir tempVestibulo Como Real
    Definir nombreEstudiante, dniEstudiante Como Cadena
    Definir letraGrupo Como Caracter
    Definir matriculaActiva Como Logico
    
    // [NUEVO DÍA 3] Variables de computo de sesiones lectivas
    Definir sesionesManana, sesionesTarde Como Entero
    Definir totalSesiones Como Entero
    Definir minutosPorSesion Como Entero
    Definir minutosTotalesLectivos Como Entero
    Definir tokenResumen Como Cadena
    
    // 2. Captura de datos previos
    Escribir "=== AZAHARTECH: TERMINAL DE ACCESO VESTIBULO ==="
    Escribir "Introduce el ID del terminal y temperatura:"
    Leer terminalId
    Leer tempVestibulo
    
    Escribir "Introduce DNI, nombre y grupo:"
    Leer dniEstudiante
    Leer nombreEstudiante
    Leer letraGrupo
    matriculaActiva <- Verdadero
    
    // [NUEVO DÍA 3] Captura de sesiones lectivas
    Escribir "-------------------------------------------------"
    Escribir "COMPUTO DE HORAS LECTIVAS:"
    Escribir "Introduce sesiones programadas manana:"
    Leer sesionesManana
    Escribir "Introduce sesiones programadas tarde:"
    Leer sesionesTarde
    
    minutosPorSesion <- 50 // Cada clase dura 50 minutos
    
    // [NUEVO DÍA 3] Procesamiento aritmético secuencial
    totalSesiones <- sesionesManana + sesionesTarde
    minutosTotalesLectivos <- totalSesiones * minutosPorSesion
    
    // Composicion de la cadena de confirmacion
    tokenResumen <- dniEstudiante + "-SESIONES-" + ConvertirATexto(totalSesiones)
    
    // 3. Salida de datos unificada
    Escribir "================================================="
    Escribir "RESUMEN DE ASISTENCIA DIARIA:"
    Escribir "Alumno:     " + nombreEstudiante + " (" + letraGrupo + " DAM)"
    Escribir "Token:      " + tokenResumen
    Escribir "Sesiones:   " + ConvertirATexto(totalSesiones) + " clases (" + ConvertirATexto(sesionesManana) + "M + " + ConvertirATexto(sesionesTarde) + "T)"
    Escribir "Permanencia computada: " + ConvertirATexto(minutosTotalesLectivos) + " minutos lectivos."
    Escribir "================================================="
FinAlgoritmo
```

##### Paso B. Expansión en Java en IntelliJ (`pr/src/ControlAccesoQR.java` — v0.3)
Abrimos nuestro archivo `ControlAccesoQR.java` y lo evolucionamos:

```java
/**
 * SISTEMA DE CONTROL DE ASISTENCIA POR CÓDIGO QR
 * Cliente: IES El Caminàs (Castellón de la Plana)
 * Consultora: AzaharTech Software Consulting
 * 
 * Versión 0.3: Cálculo aritmético de tiempos de permanencia y concatenación.
 * Módulo: Programación (PR) - Sprint 1 (RA1)
 */
import java.util.Scanner;

public class ControlAccesoQR {
    public static void main(String[] args) {
        Scanner teclado = new Scanner(System.in);
        
        // 1. Variables de hardware y terminal (Día 1)
        int terminalId;
        double tempVestibulo;
        
        // Variables de identidad (Día 2)
        String dniEstudiante;
        String nombreEstudiante;
        char letraGrupo;
        boolean matriculaActiva;
        
        // [NUEVO DÍA 3] Variables de cómputo horario y concatenación
        int sesionesManana;
        int sesionesTarde;
        int totalSesiones;
        int minutosPorSesion = 50; // Cada periodo lectivo son 50 minutos
        int minutosTotalesLectivos;
        String tokenResumen;
        
        // 2. Captura de datos
        System.out.println("=================================================");
        System.out.println("   AZAHARTECH - TERMINAL DE ACCESO VESTÍBULO     ");
        System.out.println("   Cliente: IES El Caminàs (Curso 2026/2027)     ");
        System.out.println("=================================================");
        System.out.print("ID Terminal: ");
        terminalId = teclado.nextInt();
        
        System.out.print("Temperatura sensor (ºC): ");
        tempVestibulo = teclado.nextDouble();
        
        teclado.nextLine(); // Limpieza de buffer
        
        System.out.print("DNI Estudiante: ");
        dniEstudiante = teclado.nextLine();
        
        System.out.print("Nombre completo: ");
        nombreEstudiante = teclado.nextLine();
        
        System.out.print("Grupo (letra): ");
        letraGrupo = teclado.next().charAt(0);
        
        matriculaActiva = true;
        
        // [NUEVO DÍA 3] Entrada de sesiones lectivas
        System.out.println("-------------------------------------------------");
        System.out.print("Sesiones programadas turno mañana: ");
        sesionesManana = teclado.nextInt();
        
        System.out.print("Sesiones programadas turno tarde: ");
        sesionesTarde = teclado.nextInt();
        
        // [NUEVO DÍA 3] Procesamiento aritmético secuencial
        totalSesiones = sesionesManana + sesionesTarde;
        minutosTotalesLectivos = totalSesiones * minutosPorSesion;
        
        // Composición de cadena concatenada
        tokenResumen = dniEstudiante + "-SESIONES-" + totalSesiones;
        
        // 3. Salida de datos unificada
        System.out.println("=================================================");
        System.out.println("RESUMEN DE ASISTENCIA DIARIA:");
        System.out.println("Alumno:     " + nombreEstudiante + " (" + letraGrupo + " DAM)");
        System.out.println("Token:      " + tokenResumen);
        // Uso de paréntesis protectores para garantizar suma aritmética antes de concatenar:
        System.out.println("Sesiones:   " + (sesionesManana + sesionesTarde) + " clases (" + sesionesManana + "M + " + sesionesTarde + "T)");
        System.out.println("Permanencia computada: " + minutosTotalesLectivos + " minutos lectivos.");
        System.out.println("=================================================");
        
        teclado.close();
    }
}
```

---

#### 4. Trabajo del estudiante: Evolución de su proyecto propio
El estudiante abre su archivo único `SistemaGestion.java`:
* Incorpora el cálculo aritmético secuencial adaptado a su contexto (suma de dos cantidades numéricas, multiplicación por una tarifa base).
* Utiliza paréntesis `()` para proteger la operación dentro de los mensajes de salida.
* Compila y verifica que la concatenación no genera errores numéricos.

---
---

## DÍA 4 (Jueves, 17 de septiembre de 2026 — 2 horas lectivas)
### Versión de la aplicación: Consolidación de la v0.3 y Laboratorio de Integración del Proyecto Propio
* **Objetivo técnico:** Revisar la arquitectura acumulativa, aplicar estándares de estilo y publicar la versión v0.3 del proyecto propio en GitHub.
* **Criterios de Evaluación vinculados:** RA1.a, RA1.b, RA1.c, RA1.d, RA1.e, RA1.i.

---

### 1. Caso práctico narrativo en AzaharTech
Es jueves por la mañana. Concluyen las primeras 8 horas de Programación. **Laia Claramunt** revisa la versión v0.3 en el repositorio:
> *«Fijaos en lo que habéis logrado en solo cuatro días: no tenemos diez ejercicios sueltos en la papelera; tenemos **una aplicación viva que ya sabe capturar datos del hardware, identificar al usuario y calcular tiempos lectivos**.*
>
> *Hoy cada uno de vosotros va a dedicar estas dos horas a auditar, limpiar y documentar la versión v0.3 de su **proyecto propio de la bolsa de proyectos**. Aplicaremos la indentación oficial, cerraremos recursos y realizaremos el commit formal en GitHub»*.

---

### 2. Estándares de calidad de código en AzaharTech
1. **Autoformateo en IntelliJ:** Todo código debe pasar por el formateador automático del IDE pulsando **`Ctrl + Alt + L`** (en Windows/Linux) o **`Cmd + Option + L`** (en macOS). La indentación debe ser homogénea de 4 espacios.
2. **Nombres descriptivos:** Las variables deben reflejar su propósito (`minutosTotalesLectivos`, no `mtl`).
3. **Cierre de recursos:** Toda clase que utilice `Scanner` debe cerrarlo explícitamente con `.close()` al final del `main`.

---

### 3. Taller guiado: La versión v0.3 del Proyecto Propio del Estudiante

Cada estudiante verifica que su archivo único `pr/src/MiProyecto.java` cumple con el nivel de consolidación alcanzado en el caso guía:

```java
/**
 * PROYECTO: [Nombre de tu Proyecto Elegido de la Bolsa de Proyectos]
 * Consultora: AzaharTech Software Consulting
 * 
 * Versión 0.3: Módulo acumulativo de captura de datos, tipado y cálculo aritmético.
 * Módulo: Programación (PR) - Sprint 1 (RA1)
 * 
 * @author [Tus Apellidos, Tu Nombre]
 * @version 0.3 (Cierre Semana 1 - Septiembre 2026)
 */
import java.util.Scanner;

public class MiProyecto {
    public static void main(String[] args) {
        Scanner teclado = new Scanner(System.in);
        
        // 1. Variables numéricas de infraestructura (Día 1)
        int idSucursal;
        double balanceInicial;
        
        // 2. Variables alfanuméricas de cliente y estado (Día 2)
        String nombreCliente;
        String identificadorFiscal;
        char categoriaCliente;
        boolean cuentaVerificada = true;
        
        // 3. Variables de cálculo aritmético (Día 3)
        int operacionesManana;
        int operacionesTarde;
        int totalOperaciones;
        double tarifaPorOperacion = 12.50;
        double volumenTotalCalculado;
        
        // Captura de datos interactiva
        System.out.println("=================================================");
        System.out.println("   SISTEMA DE GESTIÓN OPERATIVA - AZAHARTECH     ");
        System.out.println("=================================================");
        System.out.print("ID Sucursal / Almacén: ");
        idSucursal = teclado.nextInt();
        
        System.out.print("Balance base (€): ");
        balanceInicial = teclado.nextDouble();
        
        teclado.nextLine(); // Limpieza obligatoria del buffer
        
        System.out.print("Identificador fiscal (DNI/CIF): ");
        identificadorFiscal = teclado.nextLine();
        
        System.out.print("Nombre completo del cliente: ");
        nombreCliente = teclado.nextLine();
        
        System.out.print("Categoría (A, B o C): ");
        categoriaCliente = teclado.next().charAt(0);
        
        System.out.print("Operaciones registradas mañana: ");
        operacionesManana = teclado.nextInt();
        
        System.out.print("Operaciones registradas tarde: ");
        operacionesTarde = teclado.nextInt();
        
        // Procesamiento aritmético secuencial acumulativo
        totalOperaciones = operacionesManana + operacionesTarde;
        volumenTotalCalculado = totalOperaciones * tarifaPorOperacion;
        
        // Salida estructurada de la versión v0.3
        System.out.println("-------------------------------------------------");
        System.out.println("RESUMEN DE OPERATIVA REGISTRADA:");
        System.out.println("Cliente:     " + nombreCliente + " (" + identificadorFiscal + ")");
        System.out.println("Categoría:   " + categoriaCliente + " | Estado activo: " + cuentaVerificada);
        System.out.println("Operaciones: " + (operacionesManana + operacionesTarde) + " totales");
        System.out.println("Volumen:     " + volumenTotalCalculado + " € computados.");
        System.out.println("=================================================");
        
        teclado.close();
    }
}
```

---

### 4. Cierre formal en Git y sincronización con GitHub

El estudiante confirma los avances de la primera semana en su repositorio:

```bash
git add pr/
git commit -m "feat(pr): consolidar aplicacion v0.3 con captura completa de datos y calculos base"
git push
```

---

### Balance pedagógico de la Semana 1 (Enfoque Código Vivo)
1. **Un solo código fuente en constante evolución:** El estudiante ha comprobado cómo `ControlAccesoQR.java` ha crecido desde un esqueleto de 15 líneas hasta un programa estructurado de 50 líneas sin desechar nada de lo aprendido.
2. **Cero saturación:** Solo se han abordado datos numéricos, alfanuméricos, booleanos, operadores elementales y concatenación, **100 % fieles al RA1**, sin condicionales ni bucles.
3. **El estudiante tiene su propio software en marcha:** Su archivo `MiProyecto.java` v0.3 está publicado en GitHub, listo para recibir en la **Semana 2** las operaciones de descomposición con módulo (`%`) y las conversiones de tipo (*casting*).

# MÓDULO PROFESIONAL: PROGRAMACIÓN (PR)

## SPRINT 1. Algorítmica y fundamentos de programación (3 semanas | 24 horas)

---

# SEMANA 2: EL MOTOR MATEMÁTICO — OPERADORES, EXPRESIONES Y CONVERSIONES DE TIPO (8 HORAS)
### Hilo conductor metodológico: «Código Incremental Vivo»
Continuamos trabajando sobre el **mismo archivo maestro** que dejamos al final de la Semana 1. Tomamos la versión `ControlAccesoQR v0.3` y la evolucionamos progresivamente hasta la versión `v0.6` integrando operadores compuestos, descomposición con módulo (`%`) y conversiones de tipo (*casting*), mientras cada estudiante replica esta misma evolución en la clase única de **su proyecto elegido de la bolsa de proyectos**.

---

## DÍA 5 (Lunes, 21 de septiembre de 2026 — 2 horas lectivas)
### Versión de la aplicación: `ControlAccesoQR v0.4` (Asignación Compuesta e Incrementos en Memoria)
* **Objetivo técnico:** Refactorizar el código maestro de la Semana 1 aplicando operadores de asignación compuesta (`+=`, `-=`, `*=`) y operadores unarios de incremento (`++`) y decremento (`--`) sobre los contadores y acumuladores de la aplicación.
* **Criterios de Evaluación vinculados:** RA1.a, RA1.d, RA1.e, RA1.g.

---

### 1. Caso práctico narrativo en AzaharTech
Es lunes por la mañana. En la sala técnica de **AzaharTech**, **Pau Ferrer** abre el archivo `ControlAccesoQR.java` tal y como quedó el jueves anterior (versión v0.3). Quiere añadir un contador para saber cuántos fichajes procesa el terminal a lo largo de la mañana y ha escrito:

```java
totalFichajesTerminal = totalFichajesTerminal + 1;
minutosTotalesLectivos = minutosTotalesLectivos + minutosExtra;
```

**Alba Torres** se acerca a su monitor, señala la pantalla y le explica:
> *«Pau, en un código profesional no duplicamos el nombre de la variable a ambos lados del signo igual. Para acumular valores o contar eventos utilizamos **operadores de asignación compuesta (`+=`)** y el **operador de incremento (`++`)**.*
>
> *Hacen que el código sea más compacto, reducen el riesgo de erratas al escribir nombres largos y permiten al compilador generar un código intermedio más optimizado.*
>
> *Hoy abriremos nuestro archivo `ControlAccesoQR` y lo refactorizaremos a la versión **v0.4**: sustituiremos las asignaciones largas por operadores compuestos y aprenderemos a distinguir el pre-incremento del post-incremento para evitar efectos secundarios en la memoria»*.

---

### 2. Fundamento teórico: Asignación compuesta, incremento y mutabilidad de memoria

```
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                        OPERADORES DE ASIGNACIÓN COMPUESTA EN JAVA                      │
├─────────────────────┬───────────────────────────┬──────────────────────────────────────┤
│ Expresión compacta  │ Expresión equivalente     │ Acción sobre la celda de memoria RAM │
├─────────────────────┼───────────────────────────┼──────────────────────────────────────┤
│ total += valor;     │ total = total + valor;    │ Suma 'valor' al dato actual de total │
├─────────────────────┼───────────────────────────┼──────────────────────────────────────┤
│ tiempo -= pausa;    │ tiempo = tiempo - pausa;  │ Resta 'pausa' al dato actual         │
├─────────────────────┼───────────────────────────┼──────────────────────────────────────┤
│ aforo *= factor;    │ aforo = aforo * factor;   │ Multiplica el contenido por 'factor' │
├─────────────────────┼───────────────────────────┼──────────────────────────────────────┤
│ cupo /= divisor;    │ cupo = cupo / divisor;    │ Divide el contenido entre 'divisor'  │
└─────────────────────┴───────────────────────────┴──────────────────────────────────────┘
```

#### A. Operadores unarios de incremento (`++`) y decremento (`--`)
Aumentan o disminuyen el valor de una variable entera exactamente en una unidad:
* **Post-incremento (`variable++`):** El valor actual de la variable se utiliza en la expresión donde se encuentra y, **justo después de ser leído**, la variable se incrementa en 1 en la memoria RAM.
* **Pre-incremento (`++variable`):** La variable se incrementa en 1 en la memoria RAM **antes** de que su valor sea leído o utilizado en la expresión circundante.

```java
// Ejemplo de análisis de memoria en AzaharTech:
int accesos = 10;
System.out.println(accesos++); // Imprime 10 en consola. En memoria RAM pasa a valer 11.
System.out.println(accesos);   // Imprime 11.
System.out.println(++accesos); // En memoria RAM sube a 12 y luego imprime 12.
```

---

### 3. Andamiaje didáctico: Refactorización a `ControlAccesoQR v0.4`

Tomamos el archivo único de la Semana 1 y lo refactorizamos incorporando el contador global del terminal y asignaciones compuestas.

##### Paso A. Actualización en PSeInt (`pr/pseudocodigo/ControlAccesoQR.psc` — v0.4)
```psc
Algoritmo ControlAccesoQR
    // =========================================================================
    // SISTEMA DE CONTROL DE ASISTENCIA QR - IES EL CAMINAS (Castellon)
    // Version: 0.4 (Evolucion: Asignacion Compuesta y Contadores de Sesion)
    // =========================================================================
    
    // Variables de hardware y terminal
    Definir terminalId Como Entero
    Definir tempVestibulo Como Real
    
    // Variables de identidad del estudiante
    Definir nombreEstudiante, dniEstudiante Como Cadena
    Definir letraGrupo Como Caracter
    Definir matriculaActiva Como Logico
    
    // Variables de sesiones lectivas
    Definir sesionesManana, sesionesTarde, totalSesiones Como Entero
    Definir minutosPorSesion, minutosTotalesLectivos Como Entero
    Definir tokenResumen Como Cadena
    
    // [NUEVO DÍA 5] Contadores y acumuladores globales del terminal
    Definir contadorFichajesTerminal Como Entero
    Definir minutosExtraGuardia Como Entero
    
    contadorFichajesTerminal <- 0
    
    // Entrada de datos del terminal y alumno
    Escribir "=== AZAHARTECH: TERMINAL IES EL CAMINAS (v0.4) ==="
    Escribir "Introduce ID del terminal y temperatura:"
    Leer terminalId
    Leer tempVestibulo
    
    Escribir "Introduce DNI, nombre y grupo:"
    Leer dniEstudiante
    Leer nombreEstudiante
    Leer letraGrupo
    matriculaActiva <- Verdadero
    
    Escribir "Introduce sesiones programadas manana y tarde:"
    Leer sesionesManana
    Leer sesionesTarde
    
    Escribir "Introduce minutos adicionales de guardia/tutoría:"
    Leer minutosExtraGuardia
    
    minutosPorSesion <- 50
    
    // [REFACTORIZACIÓN DÍA 5] Procesamiento aritmético secuencial acumulativo
    // Simulamos el incremento del contador de fichajes del terminal
    contadorFichajesTerminal <- contadorFichajesTerminal + 1
    
    totalSesiones <- sesionesManana + sesionesTarde
    minutosTotalesLectivos <- totalSesiones * minutosPorSesion
    
    // Acumulación compuesta de minutos extraordinarios
    minutosTotalesLectivos <- minutosTotalesLectivos + minutosExtraGuardia
    
    tokenResumen <- dniEstudiante + "-T" + ConvertirATexto(terminalId) + "-F" + ConvertirATexto(contadorFichajesTerminal)
    
    // Salida de datos actualizada
    Escribir "================================================="
    Escribir "FICHAJE CONFIRMADO (Registro #" + ConvertirATexto(contadorFichajesTerminal) + "):"
    Escribir "Alumno:      " + nombreEstudiante + " (" + letraGrupo + " DAM)"
    Escribir "Token QR:    " + tokenResumen
    Escribir "Total clases:" + ConvertirATexto(totalSesiones) + " sesiones."
    Escribir "Permanencia acumulada: " + ConvertirATexto(minutosTotalesLectivos) + " minutos."
    Escribir "================================================="
FinAlgoritmo
```

##### Paso B. Refactorización en Java en IntelliJ (`pr/src/ControlAccesoQR.java` — v0.4)
Abrimos nuestro archivo maestro `ControlAccesoQR.java` y aplicamos directamente la refactorización:

```java
/**
 * SISTEMA DE CONTROL DE ASISTENCIA POR CÓDIGO QR
 * Cliente: IES El Caminàs (Castellón de la Plana)
 * Consultora: AzaharTech Software Consulting
 * 
 * Versión 0.4: Operadores de asignación compuesta e incrementos unarios.
 * Módulo: Programación (PR) - Sprint 1 (RA1)
 */
import java.util.Scanner;

public class ControlAccesoQR {
    public static void main(String[] args) {
        Scanner teclado = new Scanner(System.in);
        
        // 1. Variables de hardware y terminal
        int terminalId;
        double tempVestibulo;
        
        // Variables de identidad
        String dniEstudiante;
        String nombreEstudiante;
        char letraGrupo;
        boolean matriculaActiva;
        
        // Variables de cómputo de sesiones
        int sesionesManana;
        int sesionesTarde;
        int totalSesiones;
        int minutosPorSesion = 50;
        int minutosTotalesLectivos;
        String tokenResumen;
        
        // [NUEVO DÍA 5] Contadores globales y acumuladores de tiempo
        int contadorFichajesTerminal = 0; // Inicialización en memoria
        int minutosExtraGuardia;
        
        // 2. Captura de datos
        System.out.println("=================================================");
        System.out.println("   AZAHARTECH - TERMINAL IES EL CAMINÀS (v0.4)   ");
        System.out.println("=================================================");
        System.out.print("ID Terminal: ");
        terminalId = teclado.nextInt();
        
        System.out.print("Temperatura sensor (ºC): ");
        tempVestibulo = teclado.nextDouble();
        
        teclado.nextLine(); // Limpieza obligatoria del buffer
        
        System.out.print("DNI Estudiante: ");
        dniEstudiante = teclado.nextLine();
        
        System.out.print("Nombre completo: ");
        nombreEstudiante = teclado.nextLine();
        
        System.out.print("Grupo (letra): ");
        letraGrupo = teclado.next().charAt(0);
        
        matriculaActiva = true;
        
        System.out.print("Sesiones programadas turno mañana: ");
        sesionesManana = teclado.nextInt();
        
        System.out.print("Sesiones programadas turno tarde: ");
        sesionesTarde = teclado.nextInt();
        
        System.out.print("Minutos adicionales de guardia/tutoría: ");
        minutosExtraGuardia = teclado.nextInt();
        
        // [REFACTORIZACIÓN DÍA 5] Uso de operador de incremento unario
        contadorFichajesTerminal++; // Registra el paso del alumno por el torno
        
        // Cálculo base
        totalSesiones = sesionesManana + sesionesTarde;
        minutosTotalesLectivos = totalSesiones * minutosPorSesion;
        
        // [REFACTORIZACIÓN DÍA 5] Uso de asignación compuesta (+=)
        minutosTotalesLectivos += minutosExtraGuardia; // Acumula minutos sin repetir variable
        
        // Composición del token identificador
        tokenResumen = dniEstudiante + "-T" + terminalId + "-F" + contadorFichajesTerminal;
        
        // 3. Salida de datos consolidada
        System.out.println("=================================================");
        System.out.println("FICHAJE CONFIRMADO (Registro #" + contadorFichajesTerminal + "):");
        System.out.println("Alumno:      " + nombreEstudiante + " (" + letraGrupo + " DAM)");
        System.out.println("Token QR:    " + tokenResumen);
        System.out.println("Total clases:" + totalSesiones + " sesiones.");
        System.out.println("Permanencia acumulada: " + minutosTotalesLectivos + " minutos.");
        System.out.println("=================================================");
        
        teclado.close();
    }
}
```

---

#### 4. Trabajo del estudiante: Refactorización de su proyecto propio
El estudiante abre su archivo maestro `MiProyecto.java` (que dejó en v0.3 el jueves anterior):
* Declara un contador de operaciones o accesos (`contadorOperaciones`) y lo incrementa con `++`.
* Sustituye las sumas de acumulación por operadores compuestos `+=` o `-=`.
* Ejecuta el código en IntelliJ comprobando que los acumuladores actualizan la memoria RAM correctamente.

---
---

## DÍA 6 (Martes, 22 de septiembre de 2026 — 2 horas lectivas)
### Versión de la aplicación: `ControlAccesoQR v0.5` (Descomposición Temporal con División y Módulo `%`)
* **Objetivo técnico:** Incorporar al programa maestro la división entera y el operador módulo (`%`) para descomponer los segundos brutos de funcionamiento en horas, minutos y segundos exactos sin usar condicionales.
* **Criterios de Evaluación vinculados:** RA1.a, RA1.d, RA1.e, RA1.g.

---

### 1. Caso práctico narrativo en AzaharTech
Es martes por la mañana. En la pantalla de control, **Pau Ferrer** muestra que el reloj interno del vestíbulo del IES El Caminàs devuelve el tiempo de actividad del terminal en **segundos brutos acumulados**: `7540` segundos.

Pau comenta:
> *«En la pantalla no podemos mostrarle al conserje que el terminal lleva '7540 segundos encendido'. El cliente necesita ver cuántas horas completas, minutos sobrantes y segundos exactos representa ese número.*
>
> *He intentado dividir `7540 / 60`, pero me da 125 minutos y no sé cómo extraer las horas y los segundos sin escribir un algoritmo larguísimo con restas»*.

**Alba Torres** toma el mando en la pizarra:
> *«No necesitas restas ni condicionales. La combinación de la **división entera (`/`)** y el **operador módulo o resto (`%`)** resuelve este problema en tres líneas de código puramente secuenciales.*
>
> *Hoy abriremos `ControlAccesoQR.java` y lo evolucionaremos a la versión **v0.5**: convertiremos nuestro programa en un motor capaz de descomponer cualquier magnitud temporal de forma exacta»*.

---

### 2. Fundamento teórico: La división entera frente al operador residuo (`%`)

```
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                        DESCOMPOSICIÓN MATEMÁTICA DE MAGNITUDES                         │
├────────────────────────────────────────────────────────────────────────────────────────┤
│ Total: 7540 segundos                                                                   │
│ 1. Horas completas:        7540 / 3600 = 2 horas (División entera descarta decimales)  │
│ 2. Segundos restantes:     7540 % 3600 = 340 segundos sobrantes que no llegan a 1 hora │
│ 3. Minutos de ese resto:   340 / 60    = 5 minutos                                     │
│ 4. Segundos finales:       7540 % 60   = 40 segundos restantes                         │
│                                                                                        │
│ RESULTADO EXACTO:          2 horas, 5 minutos y 40 segundos.                           │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

1. **La división entre enteros en Java (`int / int`):**
    * Realiza un truncamiento automático hacia cero, descartando cualquier parte fraccionaria.
    * `7540 / 3600` da exactamente **`2`**.
2. **El operador módulo (`%`):**
    * Devuelve el residuo que no ha podido ser absorbido por la división entera.
    * `7540 % 60` da exactamente **`40`** (porque $60 \times 125 = 7500$; sobran $40$).
3. **Poder algorítmico secuencial:** Permite desglosar monedas, tiempos, paquetes o ciclos de turnos sin requerir ninguna estructura condicional `if`.

---

### 3. Andamiaje didáctico: Evolución a `ControlAccesoQR v0.5`

Ampliamos el archivo maestro `ControlAccesoQR` incorporando la lectura de segundos de actividad del terminal y su descomposición horaria.

##### Paso A. Expansión en PSeInt (`pr/pseudocodigo/ControlAccesoQR.psc` — v0.5)
```psc
Algoritmo ControlAccesoQR
    // =========================================================================
    // SISTEMA DE CONTROL DE ASISTENCIA QR - IES EL CAMINAS (Castellon)
    // Version: 0.5 (Evolucion: Descomposicion Temporal con Division y Modulo)
    // =========================================================================
    
    // Variables previas
    Definir terminalId, contadorFichajesTerminal Como Entero
    Definir tempVestibulo Como Real
    Definir nombreEstudiante, dniEstudiante, tokenResumen Como Cadena
    Definir letraGrupo Como Caracter
    Definir matriculaActiva Como Logico
    Definir sesionesManana, sesionesTarde, totalSesiones, minutosTotalesLectivos Como Entero
    Definir minutosPorSesion, minutosExtraGuardia Como Entero
    
    // [NUEVO DÍA 6] Variables para descomposición horaria exacta
    Definir segundosActividadTerminal Como Entero
    Definir horasUptime, minutosUptime, segundosUptime Como Entero
    
    contadorFichajesTerminal <- 0
    minutosPorSesion <- 50
    
    // Captura de datos
    Escribir "=== AZAHARTECH: TERMINAL IES EL CAMINAS (v0.5) ==="
    Escribir "ID Terminal y temperatura:"
    Leer terminalId
    Leer tempVestibulo
    
    // [NUEVO DÍA 6] Entrada de segundos de funcionamiento del terminal
    Escribir "Introduce segundos de actividad del terminal (uptime):"
    Leer segundosActividadTerminal
    
    Escribir "Introduce DNI, nombre y grupo:"
    Leer dniEstudiante
    Leer nombreEstudiante
    Leer letraGrupo
    matriculaActiva <- Verdadero
    
    Escribir "Introduce sesiones de manana, tarde y guardia:"
    Leer sesionesManana
    Leer sesionesTarde
    Leer minutosExtraGuardia
    
    // Procesamiento
    contadorFichajesTerminal <- contadorFichajesTerminal + 1
    totalSesiones <- sesionesManana + sesionesTarde
    minutosTotalesLectivos <- (totalSesiones * minutosPorSesion) + minutosExtraGuardia
    
    // [NUEVO DÍA 6] Descomposición secuencial con división y módulo (%)
    horasUptime <- trunc(segundosActividadTerminal / 3600)
    minutosUptime <- trunc((segundosActividadTerminal MOD 3600) / 60)
    segundosUptime <- segundosActividadTerminal MOD 60
    
    tokenResumen <- dniEstudiante + "-T" + ConvertirATexto(terminalId) + "-F" + ConvertirATexto(contadorFichajesTerminal)
    
    // Salida consolidada
    Escribir "================================================="
    Escribir "FICHAJE REGISTRADO #" + ConvertirATexto(contadorFichajesTerminal)
    Escribir "Estudiante: ", nombreEstudiante, " | Grupo: ", letraGrupo, " DAM"
    Escribir "Token QR:   ", tokenResumen
    Escribir "Tiempo lectivo computado: ", minutosTotalesLectivos, " min."
    Escribir "-------------------------------------------------"
    Escribir "DIAGNOSTICO DEL TERMINAL (UPTIME):"
    Escribir horasUptime, " horas, ", minutosUptime, " minutos y ", segundosUptime, " segundos en servicio."
    Escribir "================================================="
FinAlgoritmo
```

##### Paso B. Expansión en Java en IntelliJ (`pr/src/ControlAccesoQR.java` — v0.5)
Abrimos nuestro archivo `ControlAccesoQR.java` y lo evolucionamos a v0.5:

```java
/**
 * SISTEMA DE CONTROL DE ASISTENCIA POR CÓDIGO QR
 * Cliente: IES El Caminàs (Castellón de la Plana)
 * Consultora: AzaharTech Software Consulting
 * 
 * Versión 0.5: Descomposición de magnitudes con división entera y operador módulo (%).
 * Módulo: Programación (PR) - Sprint 1 (RA1)
 */
import java.util.Scanner;

public class ControlAccesoQR {
    public static void main(String[] args) {
        Scanner teclado = new Scanner(System.in);
        
        // Variables de terminal
        int terminalId;
        double tempVestibulo;
        int contadorFichajesTerminal = 0;
        
        // [NUEVO DÍA 6] Variables de descomposición temporal
        int segundosActividadTerminal;
        int horasUptime;
        int minutosUptime;
        int segundosUptime;
        
        // Variables de identidad
        String dniEstudiante;
        String nombreEstudiante;
        char letraGrupo;
        boolean matriculaActiva = true;
        
        // Variables de cómputo de sesiones
        int sesionesManana;
        int sesionesTarde;
        int totalSesiones;
        int minutosPorSesion = 50;
        int minutosExtraGuardia;
        int minutosTotalesLectivos;
        String tokenResumen;
        
        // Captura de datos
        System.out.println("=================================================");
        System.out.println("   AZAHARTECH - TERMINAL IES EL CAMINÀS (v0.5)   ");
        System.out.println("=================================================");
        System.out.print("ID Terminal: ");
        terminalId = teclado.nextInt();
        
        System.out.print("Temperatura sensor (ºC): ");
        tempVestibulo = teclado.nextDouble();
        
        // [NUEVO DÍA 6] Entrada de segundos de actividad
        System.out.print("Segundos acumulados de actividad del terminal: ");
        segundosActividadTerminal = teclado.nextInt();
        
        teclado.nextLine(); // Limpieza de buffer
        
        System.out.print("DNI Estudiante: ");
        dniEstudiante = teclado.nextLine();
        
        System.out.print("Nombre completo: ");
        nombreEstudiante = teclado.nextLine();
        
        System.out.print("Grupo (letra): ");
        letraGrupo = teclado.next().charAt(0);
        
        System.out.print("Sesiones turno mañana: ");
        sesionesManana = teclado.nextInt();
        
        System.out.print("Sesiones turno tarde: ");
        sesionesTarde = teclado.nextInt();
        
        System.out.print("Minutos de guardia/tutoría: ");
        minutosExtraGuardia = teclado.nextInt();
        
        // Procesamiento
        contadorFichajesTerminal++;
        totalSesiones = sesionesManana + sesionesTarde;
        minutosTotalesLectivos = (totalSesiones * minutosPorSesion) + minutosExtraGuardia;
        
        // [NUEVO DÍA 6] Descomposición secuencial con división y módulo (%)
        horasUptime = segundosActividadTerminal / 3600;              // División entera
        minutosUptime = (segundosActividadTerminal % 3600) / 60;      // Resto de horas dividido entre 60
        segundosUptime = segundosActividadTerminal % 60;             // Resto final de segundos
        
        tokenResumen = dniEstudiante + "-T" + terminalId + "-F" + contadorFichajesTerminal;
        
        // Salida de datos consolidada
        System.out.println("=================================================");
        System.out.println("FICHAJE REGISTRADO #" + contadorFichajesTerminal);
        System.out.println("Estudiante: " + nombreEstudiante + " | Grupo: " + letraGrupo + " DAM");
        System.out.println("Token QR:   " + tokenResumen);
        System.out.println("Tiempo lectivo computado: " + minutosTotalesLectivos + " min.");
        System.out.println("-------------------------------------------------");
        System.out.println("DIAGNÓSTICO DEL TERMINAL (UPTIME):");
        System.out.println(horasUptime + " horas, " + minutosUptime + " minutos y " + segundosUptime + " segundos en servicio.");
        System.out.println("=================================================");
        
        teclado.close();
    }
}
```

---

#### 4. Trabajo del estudiante: Evolución de su proyecto propio
El estudiante abre su archivo maestro `MiProyecto.java` (que estaba en v0.4):
* Identifica una magnitud de su sistema que requiera ser descompuesta (ej. céntimos totales a euros y céntimos restantes; unidades de stock a cajas estándar y unidades sueltas; minutos de servicio a días y horas).
* Aplica la división entera `/` y el operador módulo `%`.
* Muestra el desglose por consola y comprueba con la calculadora que la reconstrucción matemática es perfecta.

---
---

## DÍA 7 (Miércoles, 23 de septiembre de 2026 — 2 horas lectivas)
### Versión de la aplicación: `ControlAccesoQR v0.6` (Jerarquía de Operadores y Casting Explícito)
* **Objetivo técnico:** Evitar la pérdida involuntaria de decimales en el cálculo de ratios y porcentajes mediante conversiones explícitas de tipo (*casting* `(double)`) y proteger la evaluación de expresiones con paréntesis `()`.
* **Criterios de Evaluación vinculados:** RA1.a, RA1.d, RA1.g, RA1.h.

---

### 1. Caso práctico narrativo en AzaharTech
Es miércoles por la mañana. En el laboratorio de pruebas, **Pau Ferrer** muestra la nueva funcionalidad que ha intentado añadir a `ControlAccesoQR`: calcular el porcentaje de alumnos que han entrado a primera hora respecto al aforo total del vestíbulo.

Ha introducido:
* Alumnos que han entrado: `36`
* Capacidad de aforo: `40`

Y ha escrito en el código:
```java
double porcentajeAforo = (alumnosEntrados / aforoMaximo) * 100;
```

Al ejecutarlo, la consola imprime:
```text
Porcentaje de ocupación: 0.0 %
```

Pau se lleva las manos a la cabeza:
> *«¡Pero si han entrado 36 de 40! ¡Eso es un 90 % exacto! ¿Por qué Java dice cero coma cero?»*.

**Alba Torres** y **Laia Claramunt** se acercan a la pantalla. Laia explica:
> *«Has caído en la trampa del tipado estático. `alumnosEntrados` y `aforoMaximo` son dos variables `int`. Java evalúa los paréntesis de izquierda a derecha: divide `36 / 40`, y como ambos son enteros, trunca los decimales y da `0`. Después multiplica `0 * 100`, que da `0`. Y solo al final, al guardarlo en la variable `double`, lo convierte en `0.0`.*
>
> *Para solucionar esto debemos aplicar un **casting explícito `(double)`**, forzando a la CPU a trabajar en coma flotante desde el primer paso de la división.*
>
> *Hoy evolucionaremos `ControlAccesoQR` a la versión **v0.6**: aprenderemos a blindar la precedencia matemática con paréntesis y dominaremos las conversiones implícitas y explícitas»*.

---

### 2. Fundamento teórico: Precedencia de operadores y *Casting* en Java

```
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                        TABLA DE PRECEDENCIA DE EVALUACIÓN EN JAVA                      │
├──────────────┬───────────────────────────────────────────┬─────────────────────────────┤
│ Prioridad    │ Operadores                                │ Dirección de evaluación     │
├──────────────┼───────────────────────────────────────────┼─────────────────────────────┤
│ 1. Paréntesis│ ( )                                       │ De dentro hacia afuera      │
├──────────────┼───────────────────────────────────────────┼─────────────────────────────┤
│ 2. Unarios   │ ++ , -- , + , - , (tipo) [Casting]        │ De derecha a izquierda      │
├──────────────┼───────────────────────────────────────────┼─────────────────────────────┤
│ 3. Aritmética│ * , / , %                                 │ De izquierda a derecha      │
├──────────────┼───────────────────────────────────────────┼─────────────────────────────┤
│ 4. Aritmética│ + , -                                     │ De izquierda a derecha      │
├──────────────┼───────────────────────────────────────────┼─────────────────────────────┤
│ 5. Asignación│ = , += , -= , *= , /= , %=                │ De derecha a izquierda      │
└──────────────┴───────────────────────────────────────────┴─────────────────────────────┘
```

#### A. Las conversiones de tipo en la memoria RAM
1. **Conversión Implícita (Ensanchamiento / *Widening*):**
    * Ocurre de forma automática y segura cuando un tipo de menor tamaño se almacena en uno mayor (`int` $\rightarrow$ `double`). No hay riesgo de pérdida de información.
2. **Conversión Explícita (Estrechamiento / *Narrowing / Casting*):**
    * Es obligatoria cuando el programador fuerza la conversión de un tipo de mayor precisión en uno menor (`double` $\rightarrow$ `int`).
    * **Se produce truncamiento:** Los decimales se eliminan por completo (no se redondean).
    * Se antepone el tipo destino entre paréntesis:
      ```java
      double porcentaje = 92.85;
      int porcentajeEntero = (int) porcentaje; // Almacena 92 (pierde .85)
      ```

#### B. La solución técnica mediante Casting en divisiones
Para calcular el porcentaje sin perder decimales en la división entera, forzamos que al menos una variable sea tratada como decimal:
```java
double porcentajeAforo = ((double) alumnosEntrados / aforoMaximo) * 100.0;
```
Al aplicar `(double)` sobre `alumnosEntrados`, la división pasa a ser de tipo `double / int`, lo que promociona toda la operación a coma flotante y devuelve el **`90.0 %`** exacto.

---

### 3. Andamiaje didáctico: Evolución a `ControlAccesoQR v0.6`

Ampliamos el archivo maestro incorporando el aforo máximo, el cálculo de porcentaje exacto con casting y el truncamiento a valor entero.

##### Paso A. Expansión en PSeInt (`pr/pseudocodigo/ControlAccesoQR.psc` — v0.6)
```psc
Algoritmo ControlAccesoQR
    // =========================================================================
    // SISTEMA DE CONTROL DE ASISTENCIA QR - IES EL CAMINAS (Castellon)
    // Version: 0.6 (Evolucion: Casting de Tipos y Calculo de Porcentajes)
    // =========================================================================
    
    // Variables previas
    Definir terminalId, contadorFichajesTerminal Como Entero
    Definir tempVestibulo Como Real
    Definir nombreEstudiante, dniEstudiante, tokenResumen Como Cadena
    Definir letraGrupo Como Caracter
    Definir matriculaActiva Como Logico
    Definir sesionesManana, sesionesTarde, totalSesiones, minutosTotalesLectivos Como Entero
    Definir minutosPorSesion, minutosExtraGuardia Como Entero
    Definir segundosActividadTerminal, horasUptime, minutosUptime, segundosUptime Como Entero
    
    // [NUEVO DÍA 7] Variables de aforo y cálculo porcentual
    Definir aforoMaximoVestibulo Como Entero
    Definir porcentajeOcupacionReal Como Real
    Definir porcentajeOcupacionTruncado Como Entero
    
    contadorFichajesTerminal <- 0
    minutosPorSesion <- 50
    
    // Captura de datos
    Escribir "=== AZAHARTECH: TERMINAL IES EL CAMINAS (v0.6) ==="
    Escribir "ID Terminal, temperatura y segundos de actividad:"
    Leer terminalId
    Leer tempVestibulo
    Leer segundosActividadTerminal
    
    // [NUEVO DÍA 7] Entrada de aforo máximo
    Escribir "Introduce el aforo maximo permitido del vestibulo:"
    Leer aforoMaximoVestibulo
    
    Escribir "Introduce DNI, nombre y grupo:"
    Leer dniEstudiante
    Leer nombreEstudiante
    Leer letraGrupo
    matriculaActiva <- Verdadero
    
    Escribir "Introduce sesiones de manana, tarde y guardia:"
    Leer sesionesManana
    Leer sesionesTarde
    Leer minutosExtraGuardia
    
    // Procesamiento
    contadorFichajesTerminal <- contadorFichajesTerminal + 1
    totalSesiones <- sesionesManana + sesionesTarde
    minutosTotalesLectivos <- (totalSesiones * minutosPorSesion) + minutosExtraGuardia
    
    horasUptime <- trunc(segundosActividadTerminal / 3600)
    minutosUptime <- trunc((segundosActividadTerminal MOD 3600) / 60)
    segundosUptime <- segundosActividadTerminal MOD 60
    
    // [NUEVO DÍA 7] Cálculo de porcentaje forzando cálculo real
    porcentajeOcupacionReal <- (contadorFichajesTerminal * 100.0) / aforoMaximoVestibulo
    porcentajeOcupacionTruncado <- trunc(porcentajeOcupacionReal)
    
    tokenResumen <- dniEstudiante + "-T" + ConvertirATexto(terminalId) + "-F" + ConvertirATexto(contadorFichajesTerminal)
    
    // Salida consolidada
    Escribir "================================================="
    Escribir "FICHAJE REGISTRADO #" + ConvertirATexto(contadorFichajesTerminal)
    Escribir "Estudiante: ", nombreEstudiante, " | Grupo: ", letraGrupo, " DAM"
    Escribir "Token QR:   ", tokenResumen
    Escribir "Tiempo lectivo computado: ", minutosTotalesLectivos, " min."
    Escribir "-------------------------------------------------"
    Escribir "MONITORIZACION DE AFORO Y HARDWARE:"
    Escribir "Ocupacion exacta:  ", porcentajeOcupacionReal, " %"
    Escribir "Ocupacion en panel:", porcentajeOcupacionTruncado, " %"
    Escribir "Tiempo en servicio:", horasUptime, "h ", minutosUptime, "m ", segundosUptime, "s"
    Escribir "================================================="
FinAlgoritmo
```

##### Paso B. Expansión en Java en IntelliJ (`pr/src/ControlAccesoQR.java` — v0.6)
Abrimos nuestro archivo maestro `ControlAccesoQR.java` y lo evolucionamos a v0.6:

```java
/**
 * SISTEMA DE CONTROL DE ASISTENCIA POR CÓDIGO QR
 * Cliente: IES El Caminàs (Castellón de la Plana)
 * Consultora: AzaharTech Software Consulting
 * 
 * Versión 0.6: Jerarquía de operadores, casting explícito y porcentajes.
 * Módulo: Programación (PR) - Sprint 1 (RA1)
 */
import java.util.Scanner;

public class ControlAccesoQR {
    public static void main(String[] args) {
        Scanner teclado = new Scanner(System.in);
        
        // Variables de terminal
        int terminalId;
        double tempVestibulo;
        int contadorFichajesTerminal = 0;
        int segundosActividadTerminal;
        int horasUptime;
        int minutosUptime;
        int segundosUptime;
        
        // [NUEVO DÍA 7] Variables de aforo y cálculo de porcentaje
        int aforoMaximoVestibulo;
        double porcentajeOcupacionReal;
        int porcentajeOcupacionTruncado;
        
        // Variables de identidad
        String dniEstudiante;
        String nombreEstudiante;
        char letraGrupo;
        boolean matriculaActiva = true;
        
        // Variables de cómputo de sesiones
        int sesionesManana;
        int sesionesTarde;
        int totalSesiones;
        int minutosPorSesion = 50;
        int minutosExtraGuardia;
        int minutosTotalesLectivos;
        String tokenResumen;
        
        // Captura de datos
        System.out.println("=================================================");
        System.out.println("   AZAHARTECH - TERMINAL IES EL CAMINÀS (v0.6)   ");
        System.out.println("=================================================");
        System.out.print("ID Terminal: ");
        terminalId = teclado.nextInt();
        
        System.out.print("Temperatura sensor (ºC): ");
        tempVestibulo = teclado.nextDouble();
        
        System.out.print("Segundos acumulados de actividad: ");
        segundosActividadTerminal = teclado.nextInt();
        
        // [NUEVO DÍA 7] Entrada de aforo
        System.out.print("Aforo máximo permitido en vestíbulo: ");
        aforoMaximoVestibulo = teclado.nextInt();
        
        teclado.nextLine(); // Limpieza de buffer
        
        System.out.print("DNI Estudiante: ");
        dniEstudiante = teclado.nextLine();
        
        System.out.print("Nombre completo: ");
        nombreEstudiante = teclado.nextLine();
        
        System.out.print("Grupo (letra): ");
        letraGrupo = teclado.next().charAt(0);
        
        System.out.print("Sesiones turno mañana: ");
        sesionesManana = teclado.nextInt();
        
        System.out.print("Sesiones turno tarde: ");
        sesionesTarde = teclado.nextInt();
        
        System.out.print("Minutos de guardia/tutoría: ");
        minutosExtraGuardia = teclado.nextInt();
        
        // Procesamiento
        contadorFichajesTerminal++;
        totalSesiones = sesionesManana + sesionesTarde;
        minutosTotalesLectivos = (totalSesiones * minutosPorSesion) + minutosExtraGuardia;
        
        horasUptime = segundosActividadTerminal / 3600;
        minutosUptime = (segundosActividadTerminal % 3600) / 60;
        segundosUptime = segundosActividadTerminal % 60;
        
        // [NUEVO DÍA 7] Casting explícito a (double) para evitar división entera truncada a 0
        porcentajeOcupacionReal = ((double) contadorFichajesTerminal / aforoMaximoVestibulo) * 100.0;
        
        // Casting explícito a (int) para obtener la parte entera deliberadamente
        porcentajeOcupacionTruncado = (int) porcentajeOcupacionReal;
        
        tokenResumen = dniEstudiante + "-T" + terminalId + "-F" + contadorFichajesTerminal;
        
        // Salida consolidada
        System.out.println("=================================================");
        System.out.println("FICHAJE REGISTRADO #" + contadorFichajesTerminal);
        System.out.println("Estudiante: " + nombreEstudiante + " | Grupo: " + letraGrupo + " DAM");
        System.out.println("Token QR:   " + tokenResumen);
        System.out.println("Tiempo lectivo computado: " + minutosTotalesLectivos + " min.");
        System.out.println("-------------------------------------------------");
        System.out.println("MONITORIZACIÓN DE AFORO Y HARDWARE:");
        System.out.println("Ocupación exacta:   " + porcentajeOcupacionReal + " %");
        System.out.println("Ocupación en panel: " + porcentajeOcupacionTruncado + " %");
        System.out.println("Tiempo en servicio: " + horasUptime + "h " + minutosUptime + "m " + segundosUptime + "s");
        System.out.println("=================================================");
        
        teclado.close();
    }
}
```

---

#### 4. Trabajo del estudiante: Evolución de su proyecto propio
El estudiante abre su archivo `MiProyecto.java` (en versión v0.5):
* Incorpora el cálculo de un ratio o porcentaje que divida dos variables enteras de su negocio.
* Aplica `(double)` sobre el dividendo para obtener el resultado decimal exacto.
* Aplica `(int)` para almacenar en una variable secundaria el valor truncado.
* Ejecuta pruebas verificando que no se produce el error de división entera a cero.

---
---

## DÍA 8 (Jueves, 24 de septiembre de 2026 — 2 horas lectivas)
### Versión de la aplicación: Consolidación de la v0.6 y Laboratorio de Integración de Cálculos
* **Objetivo técnico:** Auditar y verificar que el motor matemático del proyecto propio (operadores compuestos, módulo `%` y *casting*) compila limpiamente y realizar la entrega de mitad de sprint en GitHub.
* **Criterios de Evaluación vinculados:** RA1.a, RA1.b, RA1.c, RA1.d, RA1.e, RA1.g, RA1.h, RA1.i.

---

### 1. Caso práctico narrativo en AzaharTech
Es jueves al mediodía. Concluyen las primeras dos semanas del sprint. **Laia Claramunt** revisa en la pantalla de la sala el avance global:
> *«Hoy cerramos formalmente la Semana 2. En la Semana 1 creamos el esqueleto y la identidad de nuestro software. Durante estos últimos cuatro días habéis convertido el programa en un auténtico motor matemático: domináis los operadores compuestos (`+=`), la descomposición con módulo (`%`) y las conversiones de tipo con casting.*
>
> *Hoy dedicaremos estas dos horas a auditar y afianzar la versión **v0.6 de vuestro proyecto propio de la bolsa de proyectos**. No debe quedar ni un solo cálculo ambiguo ni una división entera accidental.*
>
> *Al sonar el timbre, la versión v0.6 de vuestra aplicación debe estar confirmada y subida a GitHub»*.

---

### 2. Checklist técnico de calidad del código para la versión v0.6
Antes de realizar el commit, cada estudiante debe verificar los siguientes 5 puntos en IntelliJ:
1. **Ausencia absoluta de condicionales o bucles:** El código se ejecuta de forma estrictamente secuencial de principio a fin (**100 % fiel a RA1**).
2. **Casting explícito justificado:** Al menos una división entre enteros cuenta con `(double)` para preservar la precisión decimal.
3. **Uso del operador módulo (`%`):** Al menos una magnitud se descompone o calcula mediante el residuo de una división.
4. **Protección con paréntesis `()`:** Las fórmulas complejas están agrupadas con paréntesis para garantizar la precedencia matemática sin ambigüedades.
5. **Indentación automática:** Se ha pulsado `Ctrl + Alt + L` para ordenar el código según el estándar de estilo oficial.

---

### 3. Taller guiado: La versión v0.6 del Proyecto Propio del Estudiante

Cada estudiante comprueba que su clase única `pr/src/MiProyecto.java` ha evolucionado de forma acumulativa e incremental:

```java
/**
 * PROYECTO: [Nombre de tu Proyecto Elegido de la Bolsa de Proyectos]
 * Consultora: AzaharTech Software Consulting
 * 
 * Versión 0.6: Motor secuencial de cálculo avanzado, descomposición con módulo
 * y conversiones de tipo explícitas (casting).
 * Módulo: Programación (PR) - Sprint 1 (RA1)
 * 
 * @author [Tus Apellidos, Tu Nombre]
 * @version 0.6 (Cierre Semana 2 - Septiembre 2026)
 */
import java.util.Scanner;

public class MiProyecto {
    public static void main(String[] args) {
        Scanner teclado = new Scanner(System.in);
        
        // 1. Variables de infraestructura (Día 1)
        int idSucursal;
        double balanceInicial;
        int contadorOperaciones = 0; // Día 5: Contador
        
        // [NUEVO DÍA 6] Variables de descomposición con módulo (%)
        int unidadesTotalesProcesadas;
        int lotesCompletos;
        int unidadesSobrantes;
        int capacidadPorLote = 24; // Empaquetado estándar
        
        // 2. Variables de cliente y estado (Día 2)
        String nombreCliente;
        String identificadorFiscal;
        char categoriaCliente;
        boolean cuentaVerificada = true;
        
        // 3. Variables de cálculo y casting (Días 3 y 7)
        int operacionesManana;
        int operacionesTarde;
        int totalOperaciones;
        double tarifaPorOperacion = 12.50;
        double volumenTotalCalculado;
        double ratioAprovechamientoReal;
        int ratioAprovechamientoEntero;
        
        // Captura de datos interactiva
        System.out.println("=================================================");
        System.out.println("   SISTEMA DE GESTIÓN OPERATIVA - AZAHARTECH     ");
        System.out.println("=================================================");
        System.out.print("ID Sucursal / Almacén: ");
        idSucursal = teclado.nextInt();
        
        System.out.print("Balance base (€): ");
        balanceInicial = teclado.nextDouble();
        
        System.out.print("Volumen bruto de unidades a empaquetar: ");
        unidadesTotalesProcesadas = teclado.nextInt();
        
        teclado.nextLine(); // Limpieza obligatoria de buffer
        
        System.out.print("Identificador fiscal (DNI/CIF): ");
        identificadorFiscal = teclado.nextLine();
        
        System.out.print("Nombre completo del cliente: ");
        nombreCliente = teclado.nextLine();
        
        System.out.print("Categoría (A, B o C): ");
        categoriaCliente = teclado.next().charAt(0);
        
        System.out.print("Operaciones turno mañana: ");
        operacionesManana = teclado.nextInt();
        
        System.out.print("Operaciones turno tarde: ");
        operacionesTarde = teclado.nextInt();
        
        // [DÍA 5] Uso de incremento unario y asignación compuesta
        contadorOperaciones++;
        totalOperaciones = operacionesManana + operacionesTarde;
        volumenTotalCalculado = totalOperaciones * tarifaPorOperacion;
        
        // [DÍA 6] Descomposición secuencial con división entera y módulo (%)
        lotesCompletos = unidadesTotalesProcesadas / capacidadPorLote;
        unidadesSobrantes = unidadesTotalesProcesadas % capacidadPorLote;
        
        // [DÍA 7] Casting explícito a (double) para cálculo de ratio porcentual exacto
        ratioAprovechamientoReal = (((double)(unidadesTotalesProcesadas - unidadesSobrantes)) / unidadesTotalesProcesadas) * 100.0;
        ratioAprovechamientoEntero = (int) ratioAprovechamientoReal; // Casting a entero
        
        // Salida estructurada de la versión v0.6
        System.out.println("-------------------------------------------------");
        System.out.println("REGISTRO DE OPERACIÓN #" + contadorOperaciones);
        System.out.println("Cliente:      " + nombreCliente + " (" + identificadorFiscal + ")");
        System.out.println("Categoría:    " + categoriaCliente + " | Estado: " + cuentaVerificada);
        System.out.println("Volumen op.:  " + volumenTotalCalculado + " € computados.");
        System.out.println("-------------------------------------------------");
        System.out.println("LOGÍSTICA Y EMPAQUETADO (MÓDULO %):");
        System.out.println("Lotes de " + capacidadPorLote + " uds: " + lotesCompletos + " completos.");
        System.out.println("Sobrantes:       " + unidadesSobrantes + " unidades sueltas.");
        System.out.println("Rendimiento:     " + ratioAprovechamientoReal + " % (Entero: " + ratioAprovechamientoEntero + " %)");
        System.out.println("=================================================");
        
        teclado.close();
    }
}
```

---

### 4. Cierre formal en Git y sincronización con GitHub

El estudiante actualiza su repositorio con los avances de la segunda semana:

```bash
git add pr/
git commit -m "feat(pr): evolucionar aplicacion propia a v0.6 con motor matematico, modulo y casting"
git push
```

---

### Balance pedagógico de la Semana 2 (Enfoque Código Vivo)
1. **Crecimiento orgánico del software:** La clase `ControlAccesoQR.java` ha evolucionado de la v0.3 a la v0.6 sin desechar una sola línea de la semana anterior.
2. **Dominio matemático sin condicionales:** El alumno ha resuelto descomposiciones de tiempo y cálculos de aforo usando exclusivamente división entera, módulo (`%`) y casting `(double)`.
3. **El proyecto propio sigue el mismo ritmo:** El estudiante dispone de la versión v0.6 de `MiProyecto.java` en GitHub, lista para recibir en la **Semana 3** las constantes inmutables `final`, secuencias de escape y el formateo profesional con `printf` para alcanzar la versión final `v1.0`.

# MÓDULO PROFESIONAL: PROGRAMACIÓN (PR)

## SPRINT 1. Algorítmica y fundamentos de programación (3 semanas | 24 horas)

---

# SEMANA 3: LA ARQUITECTURA DEL CÓDIGO — CONSTANTES, ESCAPES, PRINTF Y CONSOLIDACIÓN v1.0 (8 HORAS)
### Hilo conductor metodológico: «Código Incremental Vivo»
Llegamos a la semana final del Sprint 1. Partiendo de la versión `ControlAccesoQR v0.6` (que ya cuenta con captura completa, descomposición con módulo y casting), evolucionamos nuestro archivo maestro eliminando números mágicos (`v0.7`), maquetando con secuencias de escape (`v0.8`), formateando con `printf` (`v0.9`) y sellando la versión definitiva `v1.0` secuencial con comentarios formales, mientras el estudiante concluye paralelamente su archivo único `MiProyecto.java`.

---

## DÍA 9 (Lunes, 28 de septiembre de 2026 — 2 horas lectivas)
### Versión de la aplicación: `ControlAccesoQR v0.7` (Inmutabilidad con `final` y Extracción de Números Mágicos)
* **Objetivo técnico:** Refactorizar el código maestro extrayendo todos los literales fijos a constantes inmutables declaradas con la palabra clave `final` bajo la convención `UPPER_SNAKE_CASE`.
* **Criterios de Evaluación vinculados:** RA1.a, RA1.d, RA1.e, RA1.f.

---

### 1. Caso práctico narrativo en AzaharTech
Es lunes 28 de septiembre por la mañana. En la sala técnica de **AzaharTech**, **Alba Torres** proyecta en el monitor principal la clase `ControlAccesoQR.java` tal y como quedó el jueves anterior (versión v0.6). Con el cursor, resalta varios números y textos dispersos por las líneas de cálculo:

```java
int minutosTotalesLectivos = (totalSesiones * 50) + minutosExtraGuardia;
horasUptime = segundosActividadTerminal / 3600;
minutosUptime = (segundosActividadTerminal % 3600) / 60;
porcentajeOcupacionReal = ((double) contadorFichajesTerminal / aforoMaximoVestibulo) * 100.0;
```

Alba se dirige a **Pau Ferrer** y al estudiante:
> *«Fijaos en esos números: `50`, `3600`, `60`, `100.0`. En la ingeniería de software profesional los llamamos **números mágicos (*magic numbers*)**: valores fijos incrustados directamente en mitad de las operaciones matemáticas.*
>
> *¿Qué ocurre si la dirección del IES El Caminàs decide cambiar la duración de las sesiones lectivas a 55 minutos el curso que viene? Tendríamos que buscar ese `50` en mitad de las fórmulas, arriesgándonos a cambiar un dato por error.*
>
> *Hoy abriremos nuestro archivo `ControlAccesoQR` y lo refactorizaremos a la versión **v0.7**: extraeremos todos los valores fijos y los convertiremos en **constantes inmutables protegidas por el compilador con la palabra clave `final`** al inicio del método»*.

---

### 2. Fundamento teórico: Constantes inmutables y literales tipados

```
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                        ELIMINACIÓN DE NÚMEROS MÁGICOS EN MEMORIA                       │
├──────────────────────────┬───────────────────────────┬─────────────────────────────────┤
│ Código Frágil (Hardcoded)│ Código Profesional (final)│ Ventaja de Ingeniería           │
├──────────────────────────┼───────────────────────────┼─────────────────────────────────┤
│ total * 50;              │ total * MINUTOS_SESION;   │ Si cambia el valor, solo se     │
│ segundos / 3600;         │ segundos / SEGUNDOS_HORA; │ modifica en una única línea de  │
│ total * 100.0;           │ total * FACTOR_PORCENTAJE;│ configuración al inicio.        │
└──────────────────────────┴───────────────────────────┴─────────────────────────────────┘
```

1. **La palabra reservada `final`:** Le indica al compilador que la posición de memoria es de solo lectura. Cualquier intento de reasignar su valor provocará un error de compilación.
2. **Convención `UPPER_SNAKE_CASE`:** Todas las letras en mayúsculas separadas por guiones bajos (`_`). Permite que cualquier miembro del equipo identifique al instante que se trata de un valor inmutable.
3. **Literales numéricos tipados:** El sufijo `L` para enteros largos (`long`) y `F` para decimales simples (`float`).

---

### 3. Andamiaje didáctico: Refactorización a `ControlAccesoQR v0.7`

Abrimos el archivo maestro y extraemos todos los números fijos a la cabecera de la clase.

##### Paso A. Actualización en PSeInt (`pr/pseudocodigo/ControlAccesoQR.psc` — v0.7)
```psc
Algoritmo ControlAccesoQR
    // =========================================================================
    // SISTEMA DE CONTROL DE ASISTENCIA QR - IES EL CAMINAS (Castellon)
    // Version: 0.7 (Evolucion: Extraccion de Constantes Inmutables)
    // =========================================================================
    
    // [NUEVO DÍA 9] Declaracion centralizada de constantes de configuracion
    Definir NOMBRE_INSTITUTO, PREFIJO_CENTRO Como Cadena
    Definir MINUTOS_POR_SESION, SEGUNDOS_POR_HORA, SEGUNDOS_POR_MINUTO Como Entero
    Definir FACTOR_PORCENTAJE Como Real
    
    NOMBRE_INSTITUTO <- "IES El Caminas (Castellon)"
    PREFIJO_CENTRO <- "CAMINAS"
    MINUTOS_POR_SESION <- 50
    SEGUNDOS_POR_HORA <- 3600
    SEGUNDOS_POR_MINUTO <- 60
    FACTOR_PORCENTAJE <- 100.0
    
    // Variables de memoria
    Definir terminalId, contadorFichajesTerminal, aforoMaximoVestibulo Como Entero
    Definir tempVestibulo, porcentajeOcupacionReal Como Real
    Definir porcentajeOcupacionTruncado Como Entero
    Definir nombreEstudiante, dniEstudiante, tokenResumen Como Cadena
    Definir letraGrupo Como Caracter
    Definir matriculaActiva Como Logico
    Definir sesionesManana, sesionesTarde, totalSesiones, minutosTotalesLectivos Como Entero
    Definir minutosExtraGuardia, segundosActividadTerminal Como Entero
    Definir horasUptime, minutosUptime, segundosUptime Como Entero
    
    contadorFichajesTerminal <- 0
    
    // Captura de datos
    Escribir "=== AZAHARTECH: TERMINAL " + NOMBRE_INSTITUTO + " (v0.7) ==="
    Escribir "ID Terminal, temperatura y segundos de actividad:"
    Leer terminalId
    Leer tempVestibulo
    Leer segundosActividadTerminal
    
    Escribir "Aforo maximo del vestibulo:"
    Leer aforoMaximoVestibulo
    
    Escribir "DNI, nombre y grupo del alumno:"
    Leer dniEstudiante
    Leer nombreEstudiante
    Leer letraGrupo
    matriculaActiva <- Verdadero
    
    Escribir "Sesiones de manana, tarde y minutos de guardia:"
    Leer sesionesManana
    Leer sesionesTarde
    Leer minutosExtraGuardia
    
    // Procesamiento usando exclusivamente constantes
    contadorFichajesTerminal <- contadorFichajesTerminal + 1
    totalSesiones <- sesionesManana + sesionesTarde
    minutosTotalesLectivos <- (totalSesiones * MINUTOS_POR_SESION) + minutosExtraGuardia
    
    horasUptime <- trunc(segundosActividadTerminal / SEGUNDOS_POR_HORA)
    minutosUptime <- trunc((segundosActividadTerminal MOD SEGUNDOS_POR_HORA) / SEGUNDOS_POR_MINUTO)
    segundosUptime <- segundosActividadTerminal MOD SEGUNDOS_POR_MINUTO
    
    porcentajeOcupacionReal <- (contadorFichajesTerminal * FACTOR_PORCENTAJE) / aforoMaximoVestibulo
    porcentajeOcupacionTruncado <- trunc(porcentajeOcupacionReal)
    
    tokenResumen <- PREFIJO_CENTRO + "-" + dniEstudiante + "-T" + ConvertirATexto(terminalId)
    
    // Salida consolidada
    Escribir "================================================="
    Escribir "CENTRO:     ", NOMBRE_INSTITUTO
    Escribir "FICHAJE:    #", contadorFichajesTerminal, " | TOKEN: ", tokenResumen
    Escribir "ESTUDIANTE: ", nombreEstudiante, " (", letraGrupo, " DAM)"
    Escribir "PERMANENCIA:", minutosTotalesLectivos, " min lectivos."
    Escribir "AFORO:      ", porcentajeOcupacionReal, " % (Panel: ", porcentajeOcupacionTruncado, " %)"
    Escribir "UPTIME:     ", horasUptime, "h ", minutosUptime, "m ", segundosUptime, "s"
    Escribir "================================================="
FinAlgoritmo
```

##### Paso B. Refactorización en Java en IntelliJ (`pr/src/ControlAccesoQR.java` — v0.7)
```java
/**
 * SISTEMA DE CONTROL DE ASISTENCIA POR CÓDIGO QR
 * Cliente: IES El Caminàs (Castellón de la Plana)
 * Consultora: AzaharTech Software Consulting
 * 
 * Versión 0.7: Parametrización con constantes inmutables ('final') y eliminación de números mágicos.
 * Módulo: Programación (PR) - Sprint 1 (RA1)
 */
import java.util.Scanner;

public class ControlAccesoQR {
    public static void main(String[] args) {
        // -------------------------------------------------------------
        // 1. CONSTANTES INMUTABLES DEL SISTEMA (UPPER_SNAKE_CASE)
        // -------------------------------------------------------------
        final String NOMBRE_INSTITUTO = "IES El Caminàs (Castellón)";
        final String PREFIJO_CENTRO = "CAMINAS";
        final int MINUTOS_POR_SESION = 50;
        final int SEGUNDOS_POR_HORA = 3600;
        final int SEGUNDOS_POR_MINUTO = 60;
        final double FACTOR_PORCENTAJE = 100.0;
        
        // -------------------------------------------------------------
        // 2. VARIABLES DE MEMORIA
        // -------------------------------------------------------------
        Scanner teclado = new Scanner(System.in);
        
        int terminalId;
        double tempVestibulo;
        int contadorFichajesTerminal = 0;
        int segundosActividadTerminal;
        int aforoMaximoVestibulo;
        
        String dniEstudiante;
        String nombreEstudiante;
        char letraGrupo;
        boolean matriculaActiva = true;
        
        int sesionesManana;
        int sesionesTarde;
        int totalSesiones;
        int minutosExtraGuardia;
        int minutosTotalesLectivos;
        
        int horasUptime;
        int minutosUptime;
        int segundosUptime;
        double porcentajeOcupacionReal;
        int porcentajeOcupacionTruncado;
        String tokenResumen;
        
        // -------------------------------------------------------------
        // 3. ENTRADA DE DATOS
        // -------------------------------------------------------------
        System.out.println("=================================================");
        System.out.println("   AZAHARTECH - TERMINAL " + NOMBRE_INSTITUTO);
        System.out.println("   Versión 0.7 (Parámetros Inmutables Activos)   ");
        System.out.println("=================================================");
        System.out.print("ID Terminal: ");
        terminalId = teclado.nextInt();
        
        System.out.print("Temperatura sensor (ºC): ");
        tempVestibulo = teclado.nextDouble();
        
        System.out.print("Segundos acumulados de actividad: ");
        segundosActividadTerminal = teclado.nextInt();
        
        System.out.print("Aforo máximo permitido en vestíbulo: ");
        aforoMaximoVestibulo = teclado.nextInt();
        
        teclado.nextLine(); // Limpieza obligatoria del buffer
        
        System.out.print("DNI Estudiante: ");
        dniEstudiante = teclado.nextLine();
        
        System.out.print("Nombre completo: ");
        nombreEstudiante = teclado.nextLine();
        
        System.out.print("Grupo (letra): ");
        letraGrupo = teclado.next().charAt(0);
        
        System.out.print("Sesiones turno mañana: ");
        sesionesManana = teclado.nextInt();
        
        System.out.print("Sesiones turno tarde: ");
        sesionesTarde = teclado.nextInt();
        
        System.out.print("Minutos de guardia/tutoría: ");
        minutosExtraGuardia = teclado.nextInt();
        
        // -------------------------------------------------------------
        // 4. PROCESAMIENTO SECUENCIAL USANDO CONSTANTES
        // -------------------------------------------------------------
        contadorFichajesTerminal++;
        totalSesiones = sesionesManana + sesionesTarde;
        minutosTotalesLectivos = (totalSesiones * MINUTOS_POR_SESION) + minutosExtraGuardia;
        
        // Descomposición horaria con constantes inmutables
        horasUptime = segundosActividadTerminal / SEGUNDOS_POR_HORA;
        minutosUptime = (segundosActividadTerminal % SEGUNDOS_POR_HORA) / SEGUNDOS_POR_MINUTO;
        segundosUptime = segundosActividadTerminal % SEGUNDOS_POR_MINUTO;
        
        // Cálculo de porcentaje con casting y factor porcentual constante
        porcentajeOcupacionReal = ((double) contadorFichajesTerminal / aforoMaximoVestibulo) * FACTOR_PORCENTAJE;
        porcentajeOcupacionTruncado = (int) porcentajeOcupacionReal;
        
        tokenResumen = PREFIJO_CENTRO + "-" + dniEstudiante + "-T" + terminalId;
        
        // -------------------------------------------------------------
        // 5. SALIDA CONSOLIDADA
        // -------------------------------------------------------------
        System.out.println("=================================================");
        System.out.println("CENTRO:     " + NOMBRE_INSTITUTO);
        System.out.println("FICHAJE:    #" + contadorFichajesTerminal + " | TOKEN: " + tokenResumen);
        System.out.println("ESTUDIANTE: " + nombreEstudiante + " (" + letraGrupo + " DAM)");
        System.out.println("PERMANENCIA:" + minutosTotalesLectivos + " min lectivos.");
        System.out.println("AFORO:      " + porcentajeOcupacionReal + " % (Panel: " + porcentajeOcupacionTruncado + " %)");
        System.out.println("UPTIME:     " + horasUptime + "h " + minutosUptime + "m " + segundosUptime + "s");
        System.out.println("=================================================");
        
        teclado.close();
    }
}
```

---

#### 4. Trabajo del estudiante: Refactorización de su proyecto propio
El estudiante abre su archivo maestro `MiProyecto.java` (versión v0.6):
* Extrae todos los números y cadenas fijas a constantes `final` al inicio del método `main`.
* Sustituye en todas las operaciones del programa los literales sueltos por los identificadores de constantes en mayúsculas (`UPPER_SNAKE_CASE`).
* Compila y comprueba que el programa se ejecuta de forma idéntica, pero con una mantenibilidad infinitamente superior.

---
---

## DÍA 10 (Martes, 29 de septiembre de 2026 — 2 horas lectivas)
### Versión de la aplicación: `ControlAccesoQR v0.8` (Secuencias de Escape y Maquetación de Consola)
* **Objetivo técnico:** Maquetar la salida del terminal utilizando secuencias de escape universales (`\n`, `\t`, `\"`, `\\`), alineando etiquetas informativas y controlando saltos de línea sin concatenaciones superfluas.
* **Criterios de Evaluación vinculados:** RA1.a, RA1.d, RA1.f.

---

### 1. Caso práctico narrativo en AzaharTech
Es martes por la mañana. **Pau Ferrer** muestra la consola de IntelliJ con la versión v0.7 en ejecución:
> *«El cálculo con constantes funciona de maravilla, pero la salida sigue teniendo un aspecto descuidado: para separar bloques tengo que poner `System.out.println("");` repetidas veces y las palabras 'TOKEN', 'CENTRO' y 'PERMANENCIA' no están alineadas porque cada palabra tiene una longitud distinta»*.

**Laia Claramunt** y **Alba Torres** le indican la barra invertida (**`\`**):
> *«Hoy evolucionaremos `ControlAccesoQR.java` a la versión **v0.8**. Utilizaremos **secuencias de escape**: introduciremos tabuladores horizontales (`\t`) para crear columnas alineadas, saltos de línea (`\n`) para separar bloques en una sola instrucción y escaparemos comillas dobles (`\"`) para citar el protocolo oficial del centro educativo»*.

---

### 2. Fundamento teórico: Secuencias de escape en cadenas Java

```
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                        SECUENCIAS DE ESCAPE EN CADENAS DE TEXTO                        │
├──────────────┬────────────────────────┬────────────────────────────────────────────────┤
│ Secuencia    │ Nombre técnico         │ Efecto en la consola de salida                 │
├──────────────┼────────────────────────┼────────────────────────────────────────────────┤
│ \n           │ Salto de línea         │ Pasa a la siguiente línea inmediatamente.      │
├──────────────┼────────────────────────┼────────────────────────────────────────────────┤
│ \t           │ Tabulación horizontal  │ Avanza hasta la siguiente parada de columna.   │
├──────────────┼────────────────────────┼────────────────────────────────────────────────┤
│ \"           │ Comilla doble          │ Permite imprimir comillas dentro de un String. │
├──────────────┼────────────────────────┼────────────────────────────────────────────────┤
│ \\           │ Barra invertida        │ Imprime el carácter literal de barra \         │
└──────────────┴────────────────────────┴────────────────────────────────────────────────┘
```

---

### 3. Andamiaje didáctico: Evolución a `ControlAccesoQR v0.8`

Modificamos el bloque de salida del archivo maestro `ControlAccesoQR` incorporando las secuencias de escape.

##### Paso A. Expansión en PSeInt (`pr/pseudocodigo/ControlAccesoQR.psc` — v0.8)
```psc
Algoritmo ControlAccesoQR
    // =========================================================================
    // SISTEMA DE CONTROL DE ASISTENCIA QR - IES EL CAMINAS (Castellon)
    // Version: 0.8 (Evolucion: Secuencias de Escape y Formato de Consola)
    // =========================================================================
    
    // Constantes
    Definir NOMBRE_INSTITUTO, PREFIJO_CENTRO Como Cadena
    Definir MINUTOS_POR_SESION, SEGUNDOS_POR_HORA, SEGUNDOS_POR_MINUTO Como Entero
    Definir FACTOR_PORCENTAJE Como Real
    
    NOMBRE_INSTITUTO <- "IES El Caminas (Castellon)"
    PREFIJO_CENTRO <- "CAMINAS"
    MINUTOS_POR_SESION <- 50
    SEGUNDOS_POR_HORA <- 3600
    SEGUNDOS_POR_MINUTO <- 60
    FACTOR_PORCENTAJE <- 100.0
    
    // Variables
    Definir terminalId, contadorFichajesTerminal, aforoMaximoVestibulo Como Entero
    Definir tempVestibulo, porcentajeOcupacionReal Como Real
    Definir porcentajeOcupacionTruncado Como Entero
    Definir nombreEstudiante, dniEstudiante, tokenResumen Como Cadena
    Definir letraGrupo Como Caracter
    Definir matriculaActiva Como Logico
    Definir sesionesManana, sesionesTarde, totalSesiones, minutosTotalesLectivos Como Entero
    Definir minutosExtraGuardia, segundosActividadTerminal Como Entero
    Definir horasUptime, minutosUptime, segundosUptime Como Entero
    
    contadorFichajesTerminal <- 0
    
    // Entrada de datos
    Escribir "=== AZAHARTECH: TERMINAL IES EL CAMINAS (v0.8) ==="
    Escribir "ID Terminal, temperatura y segundos de actividad:"
    Leer terminalId
    Leer tempVestibulo
    Leer segundosActividadTerminal
    Escribir "Aforo maximo del vestibulo:"
    Leer aforoMaximoVestibulo
    
    Escribir "DNI, nombre y grupo del alumno:"
    Leer dniEstudiante
    Leer nombreEstudiante
    Leer letraGrupo
    matriculaActiva <- Verdadero
    
    Escribir "Sesiones de manana, tarde y minutos de guardia:"
    Leer sesionesManana
    Leer sesionesTarde
    Leer minutosExtraGuardia
    
    // Cálculos
    contadorFichajesTerminal <- contadorFichajesTerminal + 1
    totalSesiones <- sesionesManana + sesionesTarde
    minutosTotalesLectivos <- (totalSesiones * MINUTOS_POR_SESION) + minutosExtraGuardia
    
    horasUptime <- trunc(segundosActividadTerminal / SEGUNDOS_POR_HORA)
    minutosUptime <- trunc((segundosActividadTerminal MOD SEGUNDOS_POR_HORA) / SEGUNDOS_POR_MINUTO)
    segundosUptime <- segundosActividadTerminal MOD SEGUNDOS_POR_MINUTO
    
    porcentajeOcupacionReal <- (contadorFichajesTerminal * FACTOR_PORCENTAJE) / aforoMaximoVestibulo
    porcentajeOcupacionTruncado <- trunc(porcentajeOcupacionReal)
    
    tokenResumen <- PREFIJO_CENTRO + "-" + dniEstudiante + "-T" + ConvertirATexto(terminalId)
    
    // [NUEVO DÍA 10] Salida estructurada con tabuladores y saltos limpios
    Escribir ""
    Escribir "=========================================================="
    Escribir "ENTIDAD:\t", NOMBRE_INSTITUTO
    Escribir "SISTEMA:\t\"Control de Acceso Dinamico QR\""
    Escribir "UBICACION:\tVestibulo Principal \\ Edificio A"
    Escribir "=========================================================="
    Escribir "FICHAJE N.:\t#", contadorFichajesTerminal, "\t\tESTADO:\tVALIDO"
    Escribir "ESTUDIANTE:\t", nombreEstudiante
    Escribir "IDENTIFICADOR:\t", dniEstudiante, "\tGRUPO:\t", letraGrupo, " DAM"
    Escribir "TOKEN QR:\t", tokenResumen
    Escribir "----------------------------------------------------------"
    Escribir "PERMANENCIA:\t", minutosTotalesLectivos, " minutos lectivos."
    Escribir "OCUPACION:\t", porcentajeOcupacionReal, " % (Panel: ", porcentajeOcupacionTruncado, " %)"
    Escribir "SERVICIO:\t", horasUptime, "h ", minutosUptime, "m ", segundosUptime, "s"
    Escribir "=========================================================="
FinAlgoritmo
```

##### Paso B. Expansión en Java en IntelliJ (`pr/src/ControlAccesoQR.java` — v0.8)
Actualizamos directamente el bloque de salida de nuestro archivo maestro `ControlAccesoQR.java`:

```java
/**
 * SISTEMA DE CONTROL DE ASISTENCIA POR CÓDIGO QR
 * Cliente: IES El Caminàs (Castellón de la Plana)
 * Consultora: AzaharTech Software Consulting
 * 
 * Versión 0.8: Maquetación visual mediante secuencias de escape (\n, \t, \", \\).
 * Módulo: Programación (PR) - Sprint 1 (RA1)
 */
import java.util.Scanner;

public class ControlAccesoQR {
    public static void main(String[] args) {
        // Constantes inmutables
        final String NOMBRE_INSTITUTO = "IES El Caminàs (Castellón)";
        final String PREFIJO_CENTRO = "CAMINAS";
        final String RUTA_LOGS = "C:\\caminas\\terminal\\logs";
        final int MINUTOS_POR_SESION = 50;
        final int SEGUNDOS_POR_HORA = 3600;
        final int SEGUNDOS_POR_MINUTO = 60;
        final double FACTOR_PORCENTAJE = 100.0;
        
        Scanner teclado = new Scanner(System.in);
        
        int terminalId;
        double tempVestibulo;
        int contadorFichajesTerminal = 0;
        int segundosActividadTerminal;
        int aforoMaximoVestibulo;
        
        String dniEstudiante;
        String nombreEstudiante;
        char letraGrupo;
        boolean matriculaActiva = true;
        
        int sesionesManana;
        int sesionesTarde;
        int totalSesiones;
        int minutosExtraGuardia;
        int minutosTotalesLectivos;
        
        int horasUptime;
        int minutosUptime;
        int segundosUptime;
        double porcentajeOcupacionReal;
        int porcentajeOcupacionTruncado;
        String tokenResumen;
        
        // Entrada de datos
        System.out.println("=================================================");
        System.out.println("   AZAHARTECH - TERMINAL " + NOMBRE_INSTITUTO);
        System.out.println("=================================================");
        System.out.print("ID Terminal: ");
        terminalId = teclado.nextInt();
        
        System.out.print("Temperatura sensor (ºC): ");
        tempVestibulo = teclado.nextDouble();
        
        System.out.print("Segundos acumulados de actividad: ");
        segundosActividadTerminal = teclado.nextInt();
        
        System.out.print("Aforo máximo permitido en vestíbulo: ");
        aforoMaximoVestibulo = teclado.nextInt();
        
        teclado.nextLine(); // Limpieza de buffer
        
        System.out.print("DNI Estudiante: ");
        dniEstudiante = teclado.nextLine();
        
        System.out.print("Nombre completo: ");
        nombreEstudiante = teclado.nextLine();
        
        System.out.print("Grupo (letra): ");
        letraGrupo = teclado.next().charAt(0);
        
        System.out.print("Sesiones turno mañana: ");
        sesionesManana = teclado.nextInt();
        
        System.out.print("Sesiones turno tarde: ");
        sesionesTarde = teclado.nextInt();
        
        System.out.print("Minutos de guardia/tutoría: ");
        minutosExtraGuardia = teclado.nextInt();
        
        // Procesamiento
        contadorFichajesTerminal++;
        totalSesiones = sesionesManana + sesionesTarde;
        minutosTotalesLectivos = (totalSesiones * MINUTOS_POR_SESION) + minutosExtraGuardia;
        
        horasUptime = segundosActividadTerminal / SEGUNDOS_POR_HORA;
        minutosUptime = (segundosActividadTerminal % SEGUNDOS_POR_HORA) / SEGUNDOS_POR_MINUTO;
        segundosUptime = segundosActividadTerminal % SEGUNDOS_POR_MINUTO;
        
        porcentajeOcupacionReal = ((double) contadorFichajesTerminal / aforoMaximoVestibulo) * FACTOR_PORCENTAJE;
        porcentajeOcupacionTruncado = (int) porcentajeOcupacionReal;
        
        tokenResumen = PREFIJO_CENTRO + "-" + dniEstudiante + "-T" + terminalId;
        
        // [NUEVO DÍA 10] Salida estructurada con secuencias de escape
        System.out.println("\n==========================================================");
        System.out.println("ENTIDAD:\t" + NOMBRE_INSTITUTO);
        System.out.println("SISTEMA:\t\"Control de Acceso Dinámico QR\"");
        System.out.println("UBICACIÓN:\tVestíbulo Principal \\ Edificio A");
        System.out.println("==========================================================");
        System.out.println("FICHAJE N.º:\t#" + contadorFichajesTerminal + "\t\tESTADO:\tVALIDADO");
        System.out.println("ESTUDIANTE:\t" + nombreEstudiante);
        System.out.println("IDENTIFICADOR:\t" + dniEstudiante + "\tGRUPO:\t" + letraGrupo + " DAM");
        System.out.println("TOKEN QR:\t" + tokenResumen);
        System.out.println("----------------------------------------------------------");
        System.out.println("PERMANENCIA:\t" + minutosTotalesLectivos + " minutos lectivos.");
        System.out.println("OCUPACIÓN:\t" + porcentajeOcupacionReal + " % (Panel: " + porcentajeOcupacionTruncado + " %)");
        System.out.println("SERVICIO:\t" + horasUptime + "h " + minutosUptime + "m " + segundosUptime + "s");
        System.out.println("REGISTRO LOG:\t" + RUTA_LOGS);
        System.out.println("=========================================================");
        
        teclado.close();
    }
}
```

---

#### 4. Trabajo del estudiante: Evolución de su proyecto propio
El estudiante abre su archivo maestro `MiProyecto.java` (en v0.7):
* Sustituye las concatenaciones desalineadas por tabuladores `\t` para alinear las etiquetas de los datos.
* Añade comillas escapadas `\"` para citar el nombre comercial de su cliente y barras `\\` para rutas de auditoría.
* Comprueba que la salida luce limpia y profesional.

---
---

## DÍA 11 (Miércoles, 30 de septiembre de 2026 — 2 horas lectivas)
### Versión de la aplicación: `ControlAccesoQR v0.9` (Salida Formateada Profesional con `printf`)
* **Objetivo técnico:** Sustituir las concatenaciones de salida por plantillas de formato profesionales mediante `System.out.printf()`, controlando el ancho de columnas, la alineación y acotando los decimales con precisión matemática.
* **Criterios de Evaluación vinculados:** RA1.a, RA1.b, RA1.e.

---

### 1. Caso práctico narrativo en AzaharTech
Es miércoles por la mañana. **Laia Claramunt** revisa la versión v0.8:
> *«La tabulación con `\t` es un avance, pero tiene un punto débil: si el nombre de un alumno es muy largo (como 'Constantinopla'), el tabulador salta a la siguiente parada y rompe la columna. Además, los decimales del porcentaje siguen mostrando hasta quince dígitos.*
>
> *Hoy alcanzaremos la versión **v0.9**: eliminaremos los `println` del ticket y utilizaremos **`System.out.printf()`**. Definiremos anchos de campo fijos, alinearemos textos a la izquierda y números a la derecha, y redondearemos los decimales automáticamente a exactamente dos posiciones»*.

---

### 2. Fundamento teórico: Especificadores de formato en `System.out.printf()`

```
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                        PLANTILLA DE FORMATO PROFESIONAL PRINTF                         │
├───────────────────┬──────────────────────────────────┬─────────────────────────────────┤
│ Patrón            │ Función técnica                  │ Ejemplo                         │
├───────────────────┼──────────────────────────────────┼─────────────────────────────────┤
│ %-25s             │ Texto alineado a la IZQUIERDA en │ %-25s -> "Juan Pérez          " │
│                   │ un campo de 25 caracteres.       │                                 │
├───────────────────┼──────────────────────────────────┼─────────────────────────────────┤
│ %04d              │ Entero rellenado con ceros a la  │ %04d  -> "0007"                 │
│                   │ izquierda hasta 4 dígitos.       │                                 │
├───────────────────┼──────────────────────────────────┼─────────────────────────────────┤
│ %6.2f             │ Decimal con 2 decimales fijos en │ %6.2f -> " 98.75"               │
│                   │ un ancho total de 6 caracteres.  │                                 │
├───────────────────┼──────────────────────────────────┼─────────────────────────────────┤
│ %n                │ Salto de línea independiente     │ %n                              │
└───────────────────┴──────────────────────────────────┴─────────────────────────────────┘
```

---

### 3. Andamiaje didáctico: Evolución a `ControlAccesoQR v0.9`

Actualizamos el bloque final de impresión de `ControlAccesoQR` sustituyendo los textos concatenados por una plantilla con `printf`.

##### Paso A. Expansión en PSeInt (`pr/pseudocodigo/ControlAccesoQR.psc` — v0.9)
```psc
Algoritmo ControlAccesoQR
    // =========================================================================
    // SISTEMA DE CONTROL DE ASISTENCIA QR - IES EL CAMINAS (Castellon)
    // Version: 0.9 (Evolucion: Formato Tabular de Precision)
    // =========================================================================
    
    // Constantes
    Definir NOMBRE_INSTITUTO, PREFIJO_CENTRO Como Cadena
    Definir MINUTOS_POR_SESION, SEGUNDOS_POR_HORA, SEGUNDOS_POR_MINUTO Como Entero
    Definir FACTOR_PORCENTAJE Como Real
    
    NOMBRE_INSTITUTO <- "IES El Caminas (Castellon)"
    PREFIJO_CENTRO <- "CAMINAS"
    MINUTOS_POR_SESION <- 50
    SEGUNDOS_POR_HORA <- 3600
    SEGUNDOS_POR_MINUTO <- 60
    FACTOR_PORCENTAJE <- 100.0
    
    // Variables
    Definir terminalId, contadorFichajesTerminal, aforoMaximoVestibulo Como Entero
    Definir tempVestibulo, porcentajeOcupacionReal Como Real
    Definir porcentajeOcupacionTruncado Como Entero
    Definir nombreEstudiante, dniEstudiante, tokenResumen Como Cadena
    Definir letraGrupo Como Caracter
    Definir matriculaActiva Como Logico
    Definir sesionesManana, sesionesTarde, totalSesiones, minutosTotalesLectivos Como Entero
    Definir minutosExtraGuardia, segundosActividadTerminal Como Entero
    Definir horasUptime, minutosUptime, segundosUptime Como Entero
    
    contadorFichajesTerminal <- 0
    
    // Entrada de datos
    Escribir "=== AZAHARTECH: TERMINAL IES EL CAMINAS (v0.9) ==="
    Escribir "ID Terminal, temperatura y segundos de actividad:"
    Leer terminalId
    Leer tempVestibulo
    Leer segundosActividadTerminal
    Escribir "Aforo maximo del vestibulo:"
    Leer aforoMaximoVestibulo
    
    Escribir "DNI, nombre y grupo del alumno:"
    Leer dniEstudiante
    Leer nombreEstudiante
    Leer letraGrupo
    matriculaActiva <- Verdadero
    
    Escribir "Sesiones de manana, tarde y minutos de guardia:"
    Leer sesionesManana
    Leer sesionesTarde
    Leer minutosExtraGuardia
    
    // Cálculos
    contadorFichajesTerminal <- contadorFichajesTerminal + 1
    totalSesiones <- sesionesManana + sesionesTarde
    minutosTotalesLectivos <- (totalSesiones * MINUTOS_POR_SESION) + minutosExtraGuardia
    
    horasUptime <- trunc(segundosActividadTerminal / SEGUNDOS_POR_HORA)
    minutosUptime <- trunc((segundosActividadTerminal MOD SEGUNDOS_POR_HORA) / SEGUNDOS_POR_MINUTO)
    segundosUptime <- segundosActividadTerminal MOD SEGUNDOS_POR_MINUTO
    
    porcentajeOcupacionReal <- (contadorFichajesTerminal * FACTOR_PORCENTAJE) / aforoMaximoVestibulo
    porcentajeOcupacionTruncado <- trunc(porcentajeOcupacionReal)
    
    tokenResumen <- PREFIJO_CENTRO + "-" + dniEstudiante + "-T" + ConvertirATexto(terminalId)
    
    // Salida simulando redondeo a 2 decimales
    Escribir ""
    Escribir "======================================================================"
    Escribir "             INFORME OFICIAL DE ACCESO EN VESTIBULO                   "
    Escribir "======================================================================"
    Escribir "REGISTRO:    #", contadorFichajesTerminal, " | TOKEN: ", tokenResumen
    Escribir "ESTUDIANTE:  ", nombreEstudiante, " | GRUPO: ", letraGrupo, " DAM"
    Escribir "DNI:         ", dniEstudiante
    Escribir "PERMANENCIA: ", minutosTotalesLectivos, " min lectivos."
    Escribir "OCUPACION:   ", redon(porcentajeOcupacionReal * 100) / 100, " %"
    Escribir "UPTIME:      ", horasUptime, "h ", minutosUptime, "m ", segundosUptime, "s"
    Escribir "======================================================================"
FinAlgoritmo
```

##### Paso B. Expansión en Java en IntelliJ (`pr/src/ControlAccesoQR.java` — v0.9)
```java
/**
 * SISTEMA DE CONTROL DE ASISTENCIA POR CÓDIGO QR
 * Cliente: IES El Caminàs (Castellón de la Plana)
 * Consultora: AzaharTech Software Consulting
 * 
 * Versión 0.9: Salida formateada profesional con System.out.printf().
 * Módulo: Programación (PR) - Sprint 1 (RA1)
 */
import java.util.Scanner;

public class ControlAccesoQR {
    public static void main(String[] args) {
        // Constantes inmutables
        final String NOMBRE_INSTITUTO = "IES El Caminàs (Castellón)";
        final String PREFIJO_CENTRO = "CAMINAS";
        final int MINUTOS_POR_SESION = 50;
        final int SEGUNDOS_POR_HORA = 3600;
        final int SEGUNDOS_POR_MINUTO = 60;
        final double FACTOR_PORCENTAJE = 100.0;
        
        Scanner teclado = new Scanner(System.in);
        
        int terminalId;
        double tempVestibulo;
        int contadorFichajesTerminal = 0;
        int segundosActividadTerminal;
        int aforoMaximoVestibulo;
        
        String dniEstudiante;
        String nombreEstudiante;
        char letraGrupo;
        boolean matriculaActiva = true;
        
        int sesionesManana;
        int sesionesTarde;
        int totalSesiones;
        int minutosExtraGuardia;
        int minutosTotalesLectivos;
        
        int horasUptime;
        int minutosUptime;
        int segundosUptime;
        double porcentajeOcupacionReal;
        int porcentajeOcupacionTruncado;
        String tokenResumen;
        
        // Entrada de datos
        System.out.println("=================================================");
        System.out.println("   AZAHARTECH - TERMINAL " + NOMBRE_INSTITUTO);
        System.out.println("=================================================");
        System.out.print("ID Terminal: ");
        terminalId = teclado.nextInt();
        
        System.out.print("Temperatura sensor (ºC): ");
        tempVestibulo = teclado.nextDouble();
        
        System.out.print("Segundos acumulados de actividad: ");
        segundosActividadTerminal = teclado.nextInt();
        
        System.out.print("Aforo máximo permitido en vestíbulo: ");
        aforoMaximoVestibulo = teclado.nextInt();
        
        teclado.nextLine(); // Limpieza obligatoria del buffer
        
        System.out.print("DNI Estudiante: ");
        dniEstudiante = teclado.nextLine();
        
        System.out.print("Nombre completo: ");
        nombreEstudiante = teclado.nextLine();
        
        System.out.print("Grupo (letra): ");
        letraGrupo = teclado.next().charAt(0);
        
        System.out.print("Sesiones turno mañana: ");
        sesionesManana = teclado.nextInt();
        
        System.out.print("Sesiones turno tarde: ");
        sesionesTarde = teclado.nextInt();
        
        System.out.print("Minutos de guardia/tutoría: ");
        minutosExtraGuardia = teclado.nextInt();
        
        // Procesamiento
        contadorFichajesTerminal++;
        totalSesiones = sesionesManana + sesionesTarde;
        minutosTotalesLectivos = (totalSesiones * MINUTOS_POR_SESION) + minutosExtraGuardia;
        
        horasUptime = segundosActividadTerminal / SEGUNDOS_POR_HORA;
        minutosUptime = (segundosActividadTerminal % SEGUNDOS_POR_HORA) / SEGUNDOS_POR_MINUTO;
        segundosUptime = segundosActividadTerminal % SEGUNDOS_POR_MINUTO;
        
        porcentajeOcupacionReal = ((double) contadorFichajesTerminal / aforoMaximoVestibulo) * FACTOR_PORCENTAJE;
        porcentajeOcupacionTruncado = (int) porcentajeOcupacionReal;
        
        tokenResumen = PREFIJO_CENTRO + "-" + dniEstudiante + "-T" + terminalId;
        
        // [NUEVO DÍA 11] Salida formateada profesional con printf
        System.out.println("\n======================================================================");
        System.out.println("             INFORME OFICIAL DE ACCESO EN VESTÍBULO                   ");
        System.out.println("======================================================================");
        System.out.printf("CENTRO:          %-30s%n", NOMBRE_INSTITUTO);
        System.out.printf("REGISTRO N.º:    #%05d | TOKEN: %s%n", contadorFichajesTerminal, tokenResumen);
        System.out.printf("ESTUDIANTE:      %-30s | GRUPO: %c DAM%n", nombreEstudiante, letraGrupo);
        System.out.printf("IDENTIFICADOR:   %-12s | MATRÍCULA ACTIVA: %b%n", dniEstudiante, matriculaActiva);
        System.out.println("----------------------------------------------------------------------");
        System.out.printf("PERMANENCIA:     %03d minutos lectivos (%d sesiones)%n", minutosTotalesLectivos, totalSesiones);
        System.out.printf("AFORO OCUPADO:   %6.2f %% (Indicador panel: %03d %%)%n", porcentajeOcupacionReal, porcentajeOcupacionTruncado);
        System.out.printf("SERVICIO ACTIVO: %02dh %02dm %02ds (Sensor: %.1f ºC)%n", horasUptime, minutosUptime, segundosUptime, tempVestibulo);
        System.out.println("======================================================================");
        
        teclado.close();
    }
}
```

---

#### 4. Trabajo del estudiante: Evolución de su proyecto propio
El estudiante abre su archivo `MiProyecto.java` (versión v0.8):
* Sustituye todas las instrucciones de salida por llamadas a `System.out.printf()`.
* Aplica especificadores con ancho fijo (`%-25s`), enteros con ceros a la izquierda (`%04d`) y redondeo a dos decimales (`%.2f`).
* Ejecuta pruebas con distintos nombres para certificar que las columnas permanecen perfectamente alineadas.

---
---

## DÍA 12 (Jueves, 1 de octubre de 2026 — 2 horas lectivas)
### Versión de la aplicación: `ControlAccesoQR v1.0` (Documentación Javadoc y Cierre del Programa Secuencial)
* **Objetivo técnico:** Incorporar comentarios formales de cabecera y línea, aplicar estándares de autoformateo de código en IntelliJ y sellar la versión definitiva v1.0 secuencial en GitHub.
* **Criterios de Evaluación vinculados:** RA1.a, RA1.b, RA1.c, RA1.d, RA1.e, RA1.f, RA1.g, RA1.h, RA1.i.

---

### 1. Caso práctico narrativo en AzaharTech
Es jueves 1 de octubre. Mañana viernes concluye formalmente el **Sprint 1**. En la sala de juntas de **AzaharTech**, **Laia Claramunt** convoca a toda la célula de desarrollo frente al proyector:

> *«Equipo, contemplad lo que hemos construido en doce días de trabajo: no tenemos doce programas huérfanos. Tenemos **un único software vivo, robusto y profesional que ha crecido día a día**.*
>
> *Hoy alcanzamos la versión **v1.0**: añadiremos la cabecera formal de documentación Javadoc, limpiaremos cualquier advertencia del compilador y dejaremos sellado el código del Reto 1 de vuestro proyecto propio en GitHub para la evaluación de mañana»*.

---

### 2. Fundamento teórico: Documentación técnica y autoformateo

1. **Comentarios Javadoc (`/** ... */`):** Permiten a las herramientas de ingeniería extraer manuales técnicos automáticos en formato HTML:
    * `@author`: Desarrollador o célula de trabajo responsable.
    * `@version`: Número de versión semántica del software.
2. **Autoformateo en IntelliJ (`Ctrl + Alt + L`):** Reorganiza el código para que respete las directrices internacionales de indentación (4 espacios) y separación de operadores.

---

### 3. El Código Maestro Definitivo del Sprint 1: `ControlAccesoQR v1.0`

Presentamos la versión íntegra y definitiva que el docente modela como cierre del caso guía:

```java
/**
 * SISTEMA DE CONTROL DE ASISTENCIA POR CÓDIGO QR
 * Cliente: IES El Caminàs (Castellón de la Plana)
 * Consultora: AzaharTech Software Consulting
 * 
 * Versión 1.0 (Definitiva Sprint 1):
 * Programa secuencial integral que captura parámetros de terminal y usuario,
 * realiza descomposiciones temporales y cálculos porcentuales de aforo sin pérdida
 * de precisión decimal, y emite un informe oficial formateado mediante printf.
 * 
 * @author Equipo AzaharTech (Alba Torres, Pau Ferrer)
 * @version 1.0 (Octubre 2026)
 * @since JDK 21 LTS
 */
import java.util.Scanner;

public class ControlAccesoQR {
    public static void main(String[] args) {
        // ---------------------------------------------------------------------
        // 1. CONSTANTES INMUTABLES DEL SISTEMA (Configuración corporativa)
        // ---------------------------------------------------------------------
        final String NOMBRE_INSTITUTO = "IES El Caminàs (Castellón)";
        final String PREFIJO_CENTRO = "CAMINAS";
        final int MINUTOS_POR_SESION = 50;
        final int SEGUNDOS_POR_HORA = 3600;
        final int SEGUNDOS_POR_MINUTO = 60;
        final double FACTOR_PORCENTAJE = 100.0;
        
        // ---------------------------------------------------------------------
        // 2. DECLARACIÓN DE VARIABLES DE MEMORIA
        // ---------------------------------------------------------------------
        Scanner teclado = new Scanner(System.in);
        
        // Variables de hardware y terminal
        int terminalId;
        double tempVestibulo;
        int contadorFichajesTerminal = 0; // Contador acumulativo
        int segundosActividadTerminal;
        int aforoMaximoVestibulo;
        
        // Variables de identidad y estado del estudiante
        String dniEstudiante;
        String nombreEstudiante;
        char letraGrupo;
        boolean matriculaActiva = true;
        
        // Variables de cómputo de sesiones
        int sesionesManana;
        int sesionesTarde;
        int totalSesiones;
        int minutosExtraGuardia;
        int minutosTotalesLectivos;
        
        // Variables de magnitudes descompuestas y ratios
        int horasUptime;
        int minutosUptime;
        int segundosUptime;
        double porcentajeOcupacionReal;
        int porcentajeOcupacionTruncado;
        String tokenResumen;
        
        // ---------------------------------------------------------------------
        // 3. CAPTURA INTERACTIVA DE DATOS
        // ---------------------------------------------------------------------
        System.out.println("=================================================");
        System.out.println("   AZAHARTECH - TERMINAL " + NOMBRE_INSTITUTO);
        System.out.println("   Versión 1.0 Oficial (Programa Secuencial Base)");
        System.out.println("=================================================");
        System.out.print("ID Terminal: ");
        terminalId = teclado.nextInt();
        
        System.out.print("Temperatura sensor (ºC): ");
        tempVestibulo = teclado.nextDouble();
        
        System.out.print("Segundos acumulados de actividad: ");
        segundosActividadTerminal = teclado.nextInt();
        
        System.out.print("Aforo máximo permitido en vestíbulo: ");
        aforoMaximoVestibulo = teclado.nextInt();
        
        teclado.nextLine(); // Limpieza obligatoria del buffer de teclado
        
        System.out.print("DNI Estudiante: ");
        dniEstudiante = teclado.nextLine();
        
        System.out.print("Nombre completo: ");
        nombreEstudiante = teclado.nextLine();
        
        System.out.print("Grupo (letra): ");
        letraGrupo = teclado.next().charAt(0);
        
        System.out.print("Sesiones turno mañana: ");
        sesionesManana = teclado.nextInt();
        
        System.out.print("Sesiones turno tarde: ");
        sesionesTarde = teclado.nextInt();
        
        System.out.print("Minutos de guardia/tutoría: ");
        minutosExtraGuardia = teclado.nextInt();
        
        // ---------------------------------------------------------------------
        // 4. PROCESAMIENTO SECUENCIAL Y OPERACIONES MATEMÁTICAS
        // ---------------------------------------------------------------------
        // Incremento unario del contador de accesos
        contadorFichajesTerminal++;
        
        // Suma y cálculo de permanencia lectiva
        totalSesiones = sesionesManana + sesionesTarde;
        minutosTotalesLectivos = (totalSesiones * MINUTOS_POR_SESION) + minutosExtraGuardia;
        
        // Descomposición temporal exacta mediante división entera y módulo (%)
        horasUptime = segundosActividadTerminal / SEGUNDOS_POR_HORA;
        minutosUptime = (segundosActividadTerminal % SEGUNDOS_POR_HORA) / SEGUNDOS_POR_MINUTO;
        segundosUptime = segundosActividadTerminal % SEGUNDOS_POR_MINUTO;
        
        // Casting explícito a (double) para evitar la división entera a cero
        porcentajeOcupacionReal = ((double) contadorFichajesTerminal / aforoMaximoVestibulo) * FACTOR_PORCENTAJE;
        porcentajeOcupacionTruncado = (int) porcentajeOcupacionReal; // Casting a entero
        
        // Construcción del token identificador QR
        tokenResumen = PREFIJO_CENTRO + "-" + dniEstudiante + "-T" + terminalId;
        
        // ---------------------------------------------------------------------
        // 5. SALIDA FORMATEADA PROFESIONAL (System.out.printf)
        // ---------------------------------------------------------------------
        System.out.println("\n======================================================================");
        System.out.println("             INFORME OFICIAL DE ACCESO EN VESTÍBULO                   ");
        System.out.println("======================================================================");
        System.out.printf("CENTRO:          %-30s%n", NOMBRE_INSTITUTO);
        System.out.printf("REGISTRO N.º:    #%05d | TOKEN: %s%n", contadorFichajesTerminal, tokenResumen);
        System.out.printf("ESTUDIANTE:      %-30s | GRUPO: %c DAM%n", nombreEstudiante, letraGrupo);
        System.out.printf("IDENTIFICADOR:   %-12s | MATRÍCULA ACTIVA: %b%n", dniEstudiante, matriculaActiva);
        System.out.println("----------------------------------------------------------------------");
        System.out.printf("PERMANENCIA:     %03d minutos lectivos (%d sesiones)%n", minutosTotalesLectivos, totalSesiones);
        System.out.printf("AFORO OCUPADO:   %6.2f %% (Indicador panel: %03d %%)%n", porcentajeOcupacionReal, porcentajeOcupacionTruncado);
        System.out.printf("SERVICIO ACTIVO: %02dh %02dm %02ds (Sensor: %.1f ºC)%n", horasUptime, minutosUptime, segundosUptime, tempVestibulo);
        System.out.println("======================================================================");
        
        // Cierre preventivo del recurso de entrada
        teclado.close();
    }
}
```

---

#### 4. Trabajo del estudiante: La versión v1.0 de su proyecto propio
Cada estudiante finaliza su archivo `pr/src/MiProyecto.java` (o `Reto1Completo.java`):
1. Incorpora la cabecera Javadoc con sus datos.
2. Aplica el formateador de código en IntelliJ (`Ctrl + Alt + L`).
3. Comprueba que el programa compila limpiamente y que la salida por consola con `printf` es una tabla perfectamente alineada.
4. Realiza el commit final del módulo de Programación para el Sprint 1:
   ```bash
   git add pr/
   git commit -m "feat(pr): consolidar version 1.0 del programa secuencial completo para sprint 1"
   git push
   ```

---

### Balance final de la Semana 3 y cierre del Sprint 1 (PR - 24 horas)
* **Metodología de Código Vivo cumplida:** Durante las tres semanas hemos visto crecer **una única aplicación** desde su estructura vacía hasta un software secuencial completo de 120 líneas de código, limpio, formateado y libre de errores.
* **100 % ceñido a RA1:** Se han dominado todos los Criterios de Evaluación oficiales (**CE 1.a al CE 1.i**) sin anticipar condicionales ni bucles.
* **Entrega lista:** El estudiante tiene su proyecto propio en GitHub en versión `v1.0`, listo para ser sellado mañana viernes bajo la etiqueta **`v0.1.0-sprint1`**.