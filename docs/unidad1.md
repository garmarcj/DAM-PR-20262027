
---

# SPRINT 1. ALGORÍTMICA Y FUNDAMENTOS DE PROGRAMACIÓN (24 HORAS)

---

## SEMANA 1: FUNDAMENTOS, ESTRUCTURA DEL PROGRAMA Y VARIABLES (8 HORAS)

---

### DÍA 1 (Lunes - 2 horas): Pensamiento computacional, estructura básica de Java y variables numéricas

#### 1. Caso práctico narrativo
Es lunes a primera hora en **AzaharTech**. **Laia Claramunt** reúne al equipo (Alba Torres, Pau Ferrer y el nuevo desarrollador junior). Sobre la mesa hay un plano del vestíbulo del **IES El Caminàs**:
> *«Antes de programar ventanas o lectores de códigos, debemos construir el motor de procesamiento. Un ordenador no es inteligente: solo ejecuta instrucciones secuenciales a una velocidad descomunal. Para nuestro cliente, el primer paso es registrar datos numéricos básicos del acceso (el número de aula y la hora de entrada). Hoy aprenderemos a definir variables numéricas en memoria y a crear nuestro primer programa en Java»*.

#### 2. Fundamento teórico
* **Algoritmo:** Conjunto ordenado y finito de pasos para resolver un problema: Entrada de datos $\rightarrow$ Procesamiento $\rightarrow$ Salida de resultados.
* **Estructura de un programa Java:**
    * Todo código reside dentro de una clase (`public class NombreClase`).
    * El punto de entrada universal es el método principal: `public static void main(String[] args)`.
* **Variables numéricas primitivas:** Espacios en memoria RAM con nombre y tipo:
    * `int`: Números enteros de 32 bits (desde $-2.147.483.648$ hasta $+2.147.483.647$).
    * `double`: Números reales con decimales de doble precisión (64 bits, coma flotante).
* **Entrada y salida básica:** Clase `Scanner` para leer del teclado y `System.out.println()` para mostrar mensajes.

#### 3. Andamiaje didáctico (PSeInt $\rightarrow$ Java)
* **PSeInt (`pr/pseudocodigo/dia1_variables_numericas.psc`):**
```psc
Algoritmo Dia1VariablesNumericas
    // 1. Declaracion de variables
    Definir aula Como Entero
    Definir temperaturaVestibulo Como Real
    
    // 2. Entrada de datos
    Escribir "=== AZAHARTECH: TERMINAL IES EL CAMINAS ==="
    Escribir "Introduce el numero de aula asignada:"
    Leer aula
    Escribir "Introduce la temperatura actual del vestibulo (grados):"
    Leer temperaturaVestibulo
    
    // 3. Salida de datos
    Escribir "Aula configurada: ", aula
    Escribir "Sensor termico: ", temperaturaVestibulo, " C"
FinAlgoritmo
```

* **Traducción inmediata a Java en IntelliJ (`pr/src/Dia1VariablesNumericas.java`):**
```java
import java.util.Scanner;

public class Dia1VariablesNumericas {
    public static void main(String[] args) {
        // Objeto Scanner para capturar datos desde el teclado
        Scanner teclado = new Scanner(System.in);
        
        // Declaración de variables numéricas
        int aula;
        double temperaturaVestibulo;
        
        // Entrada de datos
        System.out.println("=== AZAHARTECH: TERMINAL IES EL CAMINAS ===");
        System.out.print("Introduce el numero de aula asignada: ");
        aula = teclado.nextInt();
        
        System.out.print("Introduce la temperatura actual del vestibulo (grados): ");
        temperaturaVestibulo = teclado.nextDouble();
        
        // Salida de información por consola
        System.out.println("Aula configurada: " + aula);
        System.out.println("Sensor termico: " + temperaturaVestibulo + " C");
        
        teclado.close();
    }
}
```

#### 4. Actividad del día para el proyecto propio
Cada estudiante identifica **dos datos numéricos** (uno entero y uno decimal) indispensables para su proyecto de la bolsa de proyectos (ej. aforo máximo y precio por hora, capacidad de almacén y peso de un producto), diseñando el algoritmo en PSeInt y codificándolo en Java.

---

### DÍA 2 (Martes - 2 horas): Tipos de datos alfanuméricos (`char`, `String`) y booleanos (`boolean`)

#### 1. Caso práctico narrativo
**Pau Ferrer** muestra un avance a Laia: el terminal ya guarda números, pero no puede identificar qué alumno está cruzando la puerta. Laia explica:
> *«Los números por sí solos no explican la realidad. Necesitamos registrar texto para el nombre del alumno, caracteres únicos para la letra del DNI o el grupo, y valores lógicos que nos indiquen si el alumno tiene la matrícula activa o suspendida. Hoy ampliaremos los tipos de datos de memoria»*.

#### 2. Fundamento teórico
* **Carácter individual (`char`):** Representa un único símbolo tipográfico, delimitado siempre por comillas simples (`' '`). Ocupa 16 bits y se basa en la tabla Unicode (`'A'`, `'8'`, `'@'`).
* **Cadena de texto (`String`):** No es un tipo primitivo estricto, sino una clase que permite almacenar secuencias de texto de longitud arbitraria, delimitadas por comillas dobles (`" "`).
* **Tipo lógico (`boolean`):** Solo puede almacenar dos estados posibles: `true` (verdadero) o `false` (falso). Ocupa conceptualmente 1 bit.

#### 3. Andamiaje didáctico (PSeInt $\rightarrow$ Java)
* **PSeInt (`pr/pseudocodigo/dia2_alfanumericos.psc`):**
```psc
Algoritmo Dia2Alfanumericos
    Definir nombreEstudiante Como Cadena
    Definir letraGrupo Como Caracter
    Definir matriculaActiva Como Logico
    
    Escribir "Introduce el nombre del estudiante:"
    Leer nombreEstudiante
    Escribir "Introduce la letra del grupo (A, B, C):"
    Leer letraGrupo
    
    // Asignamos directamente un valor booleano
    matriculaActiva <- Verdadero
    
    Escribir "--- FICHA DE ACCESO ---"
    Escribir "Estudiante: ", nombreEstudiante
    Escribir "Grupo: ", letraGrupo
    Escribir "Estado de matricula activo: ", matriculaActiva
FinAlgoritmo
```

* **Traducción inmediata a Java en IntelliJ (`pr/src/Dia2Alfanumericos.java`):**
```java
import java.util.Scanner;

public class Dia2Alfanumericos {
    public static void main(String[] args) {
        Scanner teclado = new Scanner(System.in);
        
        System.out.print("Introduce el nombre del estudiante: ");
        String nombreEstudiante = teclado.nextLine();
        
        System.out.print("Introduce la letra del grupo (A, B, C): ");
        char letraGrupo = teclado.next().charAt(0); // Captura el primer carácter
        
        boolean matriculaActiva = true; // Variable booleana
        
        System.out.println("--- FICHA DE ACCESO ---");
        System.out.println("Estudiante: " + nombreEstudiante);
        System.out.println("Grupo: " + letraGrupo);
        System.out.println("Estado de matricula activo: " + matriculaActiva);
        
        teclado.close();
    }
}
```

#### 4. Actividad del día para el proyecto propio
El estudiante incorpora a su proyecto propio variables de tipo `String` (nombre, identificador o descripción), `char` (código de categoría o zona) y `boolean` (estado operativo o disponibilidad), creando el código en PSeInt y Java.

---

### DÍA 3 (Miércoles - 2 horas): Operadores aritméticos elementales y concatenación de cadenas

#### 1. Caso práctico narrativo
**Alba Torres** está revisando los datos que imprime el terminal en consola y detecta un error de presentación: los mensajes salen entrecortados y algunos números se pegan al texto. Laia explica:
> *«En Java, el símbolo más (`+`) tiene doble función. Si suma dos números, realiza una operación matemática; si uno de los elementos es una cadena de texto, actúa como **operador de concatenación**, uniendo los textos. Debemos aprender a estructurar mensajes informativos fluidos y controlar cómo se evalúan las sumas»*.

#### 2. Fundamento teórico
* **Operadores aritméticos binarios:** Suma (`+`), Resta (`-`), Multiplicación (`*`).
* **Sobrecarga del operador `+`:**
    * `5 + 3` resulta `8` (aritmética).
    * `"Total: " + 5 + 3` resulta `"Total: 53"` (concatenación de izquierda a derecha).
    * `"Total: " + (5 + 3)` resulta `"Total: 8"` (los paréntesis fuerzan la suma antes de unir).

#### 3. Andamiaje didáctico (PSeInt $\rightarrow$ Java)
* **PSeInt (`pr/pseudocodigo/dia3_concatenacion.psc`):**
```psc
Algoritmo Dia3Concatenacion
    Definir dni Como Cadena
    Definir accesosManana, accesosTarde, totalAccesos Como Entero
    
    Escribir "Introduce el DNI del usuario:"
    Leer dni
    Escribir "Introduce los accesos registrados por la manana:"
    Leer accesosManana
    Escribir "Introduce los accesos registrados por la tarde:"
    Leer accesosTarde
    
    totalAccesos <- accesosManana + accesosTarde
    
    // Concatenacion con textos
    Escribir "El usuario con DNI " + dni + " acumula un total de " + ConvertirATexto(totalAccesos) + " accesos hoy."
FinAlgoritmo
```

* **Traducción inmediata a Java en IntelliJ (`pr/src/Dia3Concatenacion.java`):**
```java
import java.util.Scanner;

public class Dia3Concatenacion {
    public static void main(String[] args) {
        Scanner teclado = new Scanner(System.in);
        
        System.out.print("Introduce el DNI del usuario: ");
        String dni = teclado.nextLine();
        
        System.out.print("Introduce los accesos por la manana: ");
        int accesosManana = teclado.nextInt();
        
        System.out.print("Introduce los accesos por la tarde: ");
        int accesosTarde = teclado.nextInt();
        
        // Suma matemática
        int totalAccesos = accesosManana + accesosTarde;
        
        // Concatenación de texto y variables
        System.out.println("El usuario con DNI " + dni + " acumula un total de " + totalAccesos + " accesos hoy.");
        
        teclado.close();
    }
}
```

#### 4. Actividad del día para el proyecto propio
El estudiante diseña un módulo de composición de mensajes en su proyecto que capture dos magnitudes numéricas, realice una suma o resta matemática y emita una frase descriptiva completa uniendo texto y resultados.

---

### DÍA 4 (Jueves - 2 horas): Laboratorio de aplicación al proyecto propio (Paso 1)

#### 1. Caso práctico narrativo
Es jueves y Laia convoca a toda la célula de desarrollo:
> *«Hemos cerrado los tres primeros conceptos fundamentales: variables primitivas, tipos alfanuméricos y operadores de concatenación. Hoy dedicaremos las dos horas completas a estructurar el primer archivo oficial de vuestro proyecto propio de la bolsa de proyectos en el repositorio»*.

#### 2. Trabajo práctico guiado en el aula
1. Abrir IntelliJ IDEA en el espacio corporativo: `azahartech/nombreEquipo/nombreEstudiante/pr/`.
2. Crear el archivo de diseño en `pr/pseudocodigo/reto1_variables.psc`.
3. Crear la clase Java ejecutable en `pr/src/Reto1Variables.java`.
4. El programa debe:
    * Declarar al menos 4 variables de tipos distintos (`int`, `double`, `String`, `boolean`).
    * Capturar los datos mediante `Scanner`.
    * Realizar una operación matemática básica.
    * Mostrar un resumen de bienvenida por consola con concatenación limpia.
5. Ejecutar y verificar en la consola de IntelliJ.
6. Realizar commit y push en GitHub:
   ```bash
   git add pr/
   git commit -m "feat(pr): implementar captura de variables y presentacion de datos"
   git push
   ```

---

## SEMANA 2: OPERADORES, EXPRESIONES Y CONVERSIONES DE TIPO (8 HORAS)

---

### DÍA 5 (Lunes - 2 horas): Asignación compuesta y operadores de incremento y decremento

#### 1. Caso práctico narrativo
**Pau Ferrer** está escribiendo una rutina para contar los alumnos que pasan por el torniquete. Ha escrito: `contadorAlumnos = contadorAlumnos + 1;` y `tiempoTotal = tiempoTotal + tiempoNuevo;`. Alba Torres le enseña un atajo profesional:
> *«En programación profesional utilizamos **operadores de asignación compuesta y de incremento**. Hacen que el código sea más compacto, legible y eficiente a nivel de compilador. Hoy aprenderemos a utilizarlos y a evitar los efectos secundarios del pre y post-incremento»*.

#### 2. Fundamento teórico
* **Operadores de asignación compuesta:** Combinan una operación aritmética con la asignación del resultado a la misma variable:
    * `x += 5;` equivale a `x = x + 5;`
    * `x -= 2;` equivale a `x = x - 2;`
    * `x *= 3;` equivale a `x = x * 3;`
* **Operadores unarios de incremento (`++`) y decremento (`--`):**
    * **Post-incremento (`x++`):** Se usa el valor actual de `x` en la expresión y **luego** se incrementa en 1.
    * **Pre-incremento (`++x`):** Primero se incrementa `x` en 1 y **luego** se usa el nuevo valor.

#### 3. Andamiaje didáctico (PSeInt $\rightarrow$ Java)
* **PSeInt (`pr/pseudocodigo/dia5_incrementos.psc`):**
```psc
Algoritmo Dia5Incrementos
    Definir contadorAccesos Como Entero
    Definir minutosAcumulados Como Entero
    
    contadorAccesos <- 0
    minutosAcumulados <- 0
    
    // Simulamos la llegada de un alumno
    contadorAccesos <- contadorAccesos + 1
    minutosAcumulados <- minutosAcumulados + 15
    
    // Simulamos la llegada de otro alumno
    contadorAccesos <- contadorAccesos + 1
    minutosAcumulados <- minutosAcumulados + 10
    
    Escribir "Total de alumnos contados: ", contadorAccesos
    Escribir "Minutos totales acumulados: ", minutosAcumulados
FinAlgoritmo
```

* **Traducción inmediata a Java en IntelliJ (`pr/src/Dia5Incrementos.java`):**
```java
public class Dia5Incrementos {
    public static void main(String[] args) {
        int contadorAccesos = 0;
        int minutosAcumulados = 0;
        
        // Uso de operadores de incremento y asignación compuesta
        contadorAccesos++;         // Equivale a contadorAccesos = contadorAccesos + 1
        minutosAcumulados += 15;   // Equivale a minutosAcumulados = minutosAcumulados + 15
        
        contadorAccesos++;
        minutosAcumulados += 10;
        
        System.out.println("Total de alumnos contados: " + contadorAccesos);
        System.out.println("Minutos totales acumulados: " + minutosAcumulados);
        
        // Demostración de pre vs post incremento
        int a = 5;
        int b = a++; // b recibe 5, luego 'a' pasa a valer 6
        System.out.println("Post-incremento -> b: " + b + ", a: " + a);
        
        int x = 5;
        int y = ++x; // 'x' pasa a valer 6, y 'y' recibe 6
        System.out.println("Pre-incremento  -> y: " + y + ", x: " + x);
    }
}
```

#### 4. Actividad del día para el proyecto propio
El estudiante realiza una prueba de escritorio en papel trazando los valores de 3 variables de su proyecto que utilizan operadores `+=`, `-=` y `++`, programándolo después en Java para validar que el resultado coincide.

---

### DÍA 6 (Martes - 2 horas): División entera frente a división real y el operador módulo (`%`)

#### 1. Caso práctico narrativo
En el caso guía del IES El Caminàs, el reloj del sistema devuelve una lectura bruta de `7540` segundos transcurridos desde medianoche. El equipo necesita descomponer esa cifra en cuántas horas completas, minutos restantes y segundos finales representa, pero sin usar condicionales. Laia explica:
> *«Este es el problema clásico que resuelve la combinación de la **división entera** y el **operador módulo o resto (`%`)**. Es una de las herramientas matemáticas más potentes que tenemos en algorítmica secuencial»*.

#### 2. Fundamento teórico
* **División entera (`int / int`):** Descarta por completo cualquier residuo o parte decimal. `7 / 2 = 3`.
* **Operador módulo (`%`):** Devuelve exclusivamente el **resto** de una división entera. `7 % 2 = 1`.
* **Descomposición matemática de magnitudes:**
    * Segundos en una hora: $3600$.
    * Segundos en un minuto: $60$.

#### 3. Andamiaje didáctico (PSeInt $\rightarrow$ Java)
* **PSeInt (`pr/pseudocodigo/dia6_modulo_tiempos.psc`):**
```psc
Algoritmo Dia6ModuloTiempos
    Definir segundosTotales, horas, minutos, segundosFinales Como Entero
    
    Escribir "Introduce los segundos totales transcurridos:"
    Leer segundosTotales
    
    // Descomposicion secuencial exacta
    horas <- trunc(segundosTotales / 3600)
    minutos <- trunc((segundosTotales MOD 3600) / 60)
    segundosFinales <- segundosTotales MOD 60
    
    Escribir "Desglose horario:"
    Escribir horas, " horas, ", minutos, " minutos y ", segundosFinales, " segundos."
FinAlgoritmo
```

* **Traducción inmediata a Java en IntelliJ (`pr/src/Dia6ModuloTiempos.java`):**
```java
import java.util.Scanner;

public class Dia6ModuloTiempos {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        
        System.out.print("Introduce los segundos totales transcurridos: ");
        int segundosTotales = scanner.nextInt();
        
        // Uso coordinado de división entera y operador resto (%)
        int horas = segundosTotales / 3600;
        int minutos = (segundosTotales % 3600) / 60;
        int segundosFinales = segundosTotales % 60;
        
        System.out.println("Desglose horario:");
        System.out.println(horas + " horas, " + minutos + " minutos y " + segundosFinales + " segundos.");
        
        scanner.close();
    }
}
```

#### 4. Actividad del día para el proyecto propio
El estudiante aplica el operador módulo (`%`) y la división entera a un problema de descomposición o empaquetado de su proyecto propio (ej. convertir céntimos en euros enteros y céntimos restantes, agrupar productos en cajas completas y unidades sobrantes, o dividir minutos en días y horas).

---

### DÍA 7 (Miércoles - 2 horas): Jerarquía de operadores y conversiones de tipo (*Type Casting*)

#### 1. Caso práctico narrativo
Alba Torres calcula la tasa de ocupación del vestíbulo del instituto. Hay `18` alumnos presentes sobre un aforo de `40`. Escribe: `double ratio = 18 / 40;` y el sistema imprime `0.0`. Alba advierte a Pau y al estudiante:
> *«Java ha evaluado primero la división entre dos enteros (`18 / 40 = 0`) y luego ha convertido ese cero a decimal (`0.0`). Hemos perdido la información por no aplicar una **conversión explícita o casting**. Hoy aprenderemos a gobernar las conversiones de tipos en expresiones mixtas»*.

#### 2. Fundamento teórico
* **Jerarquía de operadores:** Los paréntesis `()` tienen máxima prioridad, seguidos de los unarios (`++`, `--`), multiplicación/división/módulo (`*`, `/`, `%`) y suma/resta (`+`, `-`).
* **Conversión Implícita (Ensanchamiento):** De tipo menor a mayor (`int` $\rightarrow$ `double`). Se realiza de forma segura y automática.
* **Conversión Explícita (*Casting*):** Se fuerza manualmente anteponiendo el tipo destino entre paréntesis: `(double) a / b` o `(int) variableDecimal`. Provoca pérdida de decimales si se pasa de real a entero.

#### 3. Andamiaje didáctico (PSeInt $\rightarrow$ Java)
* **PSeInt (`pr/pseudocodigo/dia7_casting_porcentajes.psc`):**
```psc
Algoritmo Dia7CastingPorcentajes
    Definir presentes, aforoTotal Como Entero
    Definir porcentajeReal Como Real
    Definir porcentajeEnteroTruncado Como Entero
    
    Escribir "Introduce los alumnos presentes:"
    Leer presentes
    Escribir "Introduce el aforo total del vestibulo:"
    Leer aforoTotal
    
    // Forzamos calculo real
    porcentajeReal <- (presentes * 100.0) / aforoTotal
    porcentajeEnteroTruncado <- trunc(porcentajeReal)
    
    Escribir "Porcentaje exacto: ", porcentajeReal, " %"
    Escribir "Porcentaje redondeado a la baja: ", porcentajeEnteroTruncado, " %"
FinAlgoritmo
```

* **Traducción inmediata a Java en IntelliJ (`pr/src/Dia7CastingPorcentajes.java`):**
```java
import java.util.Scanner;

public class Dia7CastingPorcentajes {
    public static void main(String[] args) {
        Scanner entrada = new Scanner(System.in);
        
        System.out.print("Introduce los alumnos presentes: ");
        int presentes = entrada.nextInt();
        
        System.out.print("Introduce el aforo total del vestibulo: ");
        int aforoTotal = entrada.nextInt();
        
        // Casting explícito a (double) en el numerador para forzar división decimal
        double porcentajeReal = ((double) presentes / aforoTotal) * 100.0;
        
        // Casting explícito a (int) para truncar decimales
        int porcentajeEnteroTruncado = (int) porcentajeReal;
        
        System.out.println("Porcentaje exacto: " + porcentajeReal + " %");
        System.out.println("Porcentaje redondeado a la baja: " + porcentajeEnteroTruncado + " %");
        
        entrada.close();
    }
}
```

#### 4. Actividad del día para el proyecto propio
El estudiante diseña un cálculo de porcentaje, ratio o ponderación en su proyecto propio donde intervengan magnitudes enteras que deban devolver un resultado decimal exacto mediante casting explícito `(double)`.

---

### DÍA 8 (Jueves - 2 horas): Laboratorio: Motor de cálculo secuencial del proyecto propio (Paso 2)

#### 1. Caso práctico narrativo
Laia Claramunt revisa el Sprint Backlog del equipo:
> *«Hoy cerramos la segunda semana. Cada uno debe integrar en el repositorio el **motor de cálculo matemático completo de su proyecto propio**, asegurando que utiliza operadores combinados, descomposición con módulo (`%`) y conversiones de tipo correctas sin pérdida de datos»*.

#### 2. Trabajo práctico guiado en el aula
1. Abrir IntelliJ IDEA en el espacio de trabajo.
2. Crear el archivo `pr/pseudocodigo/reto1_calculo.psc`.
3. Crear la clase Java `pr/src/Reto1Calculo.java`.
4. El programa debe implementar:
    * Entrada de datos con `Scanner`.
    * Al menos dos expresiones matemáticas con jerarquía y paréntesis.
    * Una operación con el operador módulo (`%`).
    * Al menos una conversión explícita (*casting*) justificable.
5. Ejecutar con diferentes valores numéricos y verificar en la consola.
6. Realizar commit y push en GitHub:
   ```bash
   git add pr/
   git commit -m "feat(pr): implementar formulas matematicas y conversiones de tipo en reto 1"
   git push
   ```

---

## SEMANA 3: CONSTANTES, LITERALES, SALIDA FORMATEADA Y CIERRE (8 HORAS)

---

### DÍA 9 (Lunes - 2 horas): Constantes inmutables (`final`) y eliminación de números mágicos

#### 1. Caso práctico narrativo
Alba Torres hace una auditoría del código del IES El Caminàs y encuentra números dispersos por los archivos: `0.21`, `60`, `3600`, `"CAMINAS"`. Alba explica a Pau y al estudiante:
> *«Si el cliente decide que el prefijo del QR cambia el curso que viene o cambiamos una tasa, tendremos que buscar ese texto en diez archivos distintos y cometeremos errores. En AzaharTech está prohibido usar 'números mágicos'. Todo valor fijo debe declararse como una **constante inmutable** al principio de la clase»*.

#### 2. Fundamento teórico
* **Constante inmutable en Java:** Se declara con la palabra clave `final`. Su valor no puede ser reasignado una vez inicializado.
* **Convención de estilo:** Identificadores en **MAYÚSCULAS** y palabras separadas por guion bajo: `UPPER_SNAKE_CASE` (*ej. `final int DIAS_SEMANA = 7;`*).
* **Literales tipados:**
    * Entero largo: Sufijo `L` (*`3000000000L`*).
    * Flotante simple: Sufijo `F` (*`3.14F`*).
    * Doble precisión: Por defecto o sufijo `D`.

#### 3. Andamiaje didáctico (PSeInt $\rightarrow$ Java)
* **PSeInt (`pr/pseudocodigo/dia9_constantes.psc`):**
```psc
Algoritmo Dia9Constantes
    Definir PREFIJO_CENTRO Como Cadena
    Definir SEGUNDOS_MAXIMOS_ACCESO Como Entero
    
    PREFIJO_CENTRO <- "CAMINAS"
    SEGUNDOS_MAXIMOS_ACCESO <- 1800
    
    Definir idTicket Como Entero
    idTicket <- 42
    
    Escribir "Prefijo oficial: ", PREFIJO_CENTRO
    Escribir "Tiempo limite de sesion: ", SEGUNDOS_MAXIMOS_ACCESO, " segundos."
    Escribir "Identificador de operacion: ", PREFIJO_CENTRO, "-", idTicket
FinAlgoritmo
```

* **Traducción inmediata a Java en IntelliJ (`pr/src/Dia9Constantes.java`):**
```java
public class Dia9Constantes {
    public static void main(String[] args) {
        // Declaración formal de constantes inmutables
        final String PREFIJO_CENTRO = "CAMINAS";
        final int SEGUNDOS_MAXIMOS_ACCESO = 1800;
        final double VERSION_PROTOCOLO = 1.0;
        
        int idTicket = 42;
        
        // Composición usando constantes seguras
        String codigoGenerado = PREFIJO_CENTRO + "-" + idTicket + "-v" + VERSION_PROTOCOLO;
        
        System.out.println("Constantes de configuracion fijadas correctamente:");
        System.out.println("Codigo de acceso generado: " + codigoGenerado);
        System.out.println("Tiempo maximo de entrada: " + SEGUNDOS_MAXIMOS_ACCESO + " seg.");
    }
}
```

#### 4. Actividad del día para el proyecto propio
El estudiante identifica al menos 3 valores fijos de su proyecto propio (nombres de empresa, tasas de impuestos, capacidades máximas, prefijos identificadores) y los declara como constantes `final` respetando la convención `UPPER_SNAKE_CASE`.

---

### DÍA 10 (Martes - 2 horas): Secuencias de escape y formato en consola

#### 1. Caso práctico narrativo
Pau Ferrer ejecuta su aplicación de acceso y la salida sale apelotonada en una sola línea horizontal. Laia Claramunt le muestra cómo dar formato visual a la consola:
> *«Para que un usuario o conserje entienda la pantalla, los datos deben organizarse con saltos de línea claros, tabulaciones que alineen los textos y comillas cuando citemos nombres de entidades. Esto se consigue con las **secuencias de escape**»*.

#### 2. Fundamento teórico
* **Secuencia de escape:** Carácter especial precedido por la barra invertida (`\`) que Java interpreta como una orden de formato en lugar de texto literal:
    * `\n`: Salto de línea (*Newline*).
    * `\t`: Tabulador horizontal (*Tab*), desplaza el cursor a la siguiente columna tabular.
    * `\"`: Permite escribir comillas dobles sin cerrar la cadena de texto.
    * `\\`: Permite imprimir el carácter de barra invertida.

#### 3. Andamiaje didáctico (PSeInt $\rightarrow$ Java)
* **PSeInt (`pr/pseudocodigo/dia10_escapes.psc`):**
```psc
Algoritmo Dia10Escapes
    Escribir "============================================="
    Escribir "SISTEMA:\tControl de Asistencia QR"
    Escribir "ENTIDAD:\tIES El Caminas"
    Escribir "UBICACION:\tVestibulo principal"
    Escribir "============================================="
    Escribir ""
    Escribir "Estado:\t\"OPERATIVO\""
FinAlgoritmo
```

* **Traducción inmediata a Java en IntelliJ (`pr/src/Dia10Escapes.java`):**
```java
public class Dia10Escapes {
    public static void main(String[] args) {
        // Uso de tabulaciones (\t), saltos (\n) y comillas escapadas (\")
        System.out.println("=============================================");
        System.out.println("SISTEMA:\t\"Control de Asistencia QR\"");
        System.out.println("ENTIDAD:\tIES El Caminas (Castellon)");
        System.out.println("UBICACION:\tVestibulo principal (Edificio A)");
        System.out.println("=============================================\n");
        
        System.out.println("Aviso:\tCompruebe que el lector emite la senal en verde.");
    }
}
```

#### 4. Actividad del día para el proyecto propio
El estudiante diseña la cabecera visual de su proyecto en consola maquetando títulos, subtítulos y datos clave alineados mediante tabulaciones (`\t`) y saltos de línea (`\n`).

---

### DÍA 11 (Miércoles - 2 horas): Salida formateada profesional con `System.out.printf()`

#### 1. Caso práctico narrativo
Alba y Pau presentan el ticket de acceso al IES El Caminàs, pero el tiempo transcurrido aparece como `2.3333333333333335 min` y el precio del carnet como `5.0 euros`. Laia les enseña la herramienta definitiva de salida en Java:
> *«En producción no podemos mostrar 15 decimales. Con **`printf`** controlamos con precisión de relojero cuántos decimales mostramos, el ancho exacto de cada columna y si alineamos a izquierda o derecha. Hoy aprenderemos a generar informes tabulares profesionales»*.

#### 2. Fundamento teórico
* **Método `System.out.printf(formato, argumentos...)`:** Imprime texto formateado según una plantilla.
* **Especificadores de formato comunes:**
    * `%s`: Cadenas de texto (`String`).
    * `%d`: Números enteros (`int`, `long`).
    * `%f`: Números decimales (`double`, `float`).
    * `%c`: Carácter individual (`char`).
    * `%n`: Salto de línea estándar e independiente del sistema operativo.
* **Modificadores de precisión y anchura:**
    * `%.2f`: Redondea a exactamente **2 decimales**.
    * `%-25s`: Reserva 25 caracteres para el texto alineado a la **izquierda**.
    * `%10.2f`: Reserva 10 caracteres totales para el número alineado a la **derecha**.

#### 3. Andamiaje didáctico (PSeInt $\rightarrow$ Java)
* **PSeInt (`pr/pseudocodigo/dia11_printf.psc`):**
```psc
Algoritmo Dia11Printf
    Definir nombre Como Cadena
    Definir tiempoExacto Como Real
    Definir idRegistro Como Entero
    
    nombre <- "Alba Torres"
    tiempoExacto <- 2.33333333
    idRegistro <- 7
    
    Escribir "--- REPORTE FORMATEADO ---"
    Escribir "Registro N. : ", idRegistro
    Escribir "Estudiante  : ", nombre
    Escribir "Tiempo est. : ", redon(tiempoExacto * 100) / 100, " min"
FinAlgoritmo
```

* **Traducción inmediata a Java en IntelliJ (`pr/src/Dia11Printf.java`):**
```java
public class Dia11Printf {
    public static void main(String[] args) {
        String nombre = "Alba Torres";
        double tiempoExacto = 2.33333333;
        int idRegistro = 7;
        double tasaFiabilidad = 99.856;
        
        System.out.println("======================================================================");
        System.out.println("                  INFORME FORMATEADO CON PRINTF                       ");
        System.out.println("======================================================================");
        
        // Control exacto de columnas, enteros con ceros a la izquierda y decimales acotados
        System.out.printf("Registro ID:       #%05d%n", idRegistro);
        System.out.printf("Desarrolladora:    %-25s%n", nombre);
        System.out.printf("Tiempo de estancia: %.2f minutos%n", tiempoExacto);
        System.out.printf("Tasa fiabilidad:    %6.2f %%%n", tasaFiabilidad);
        
        System.out.println("======================================================================");
    }
}
```

#### 4. Actividad del día para el proyecto propio
El estudiante diseña la tabla o ticket final de salida de su proyecto utilizando `printf`, alineando al menos 3 columnas de datos y redondeando los valores monetarios o temporales a dos decimales (`%.2f`).

---

### DÍA 12 (Jueves - 2 horas): Laboratorio de integración: Programa secuencial completo y cierre del Sprint 1

#### 1. Caso práctico narrativo
Es jueves 1 de octubre. Mañana concluye el **Sprint 1**. Laia Claramunt reúne al equipo para el remate final:
> *«Hoy ensamblamos todo lo aprendido durante estas tres semanas en el **programa secuencial definitivo de vuestro proyecto propio**. Debe incluir constantes inmutables, lectura por `Scanner`, cálculos matemáticos precisos con *casting*, salida formateada con `printf` y comentarios limpios de cabecera. Con esto, vuestro módulo de Programación queda sellado al 100 % para la primera entrega oficial»*.

#### 2. Fundamento teórico
* **Comentarios profesionales:**
    * Comentario de cabecera con propósito del archivo, autor y versión.
    * Comentarios de línea (`//`) explicando el *porqué* de una fórmula, no lo evidente.
* **Checklist de calidad del código:** Indentación consistente en IntelliJ (`Ctrl + Alt + L`), sin variables en desuso ni advertencias del compilador.

#### 3. Ensamblaje del Reto 1 Completo del Proyecto Propio
Cada estudiante finaliza su código fuente en `pr/src/Reto1Completo.java` siguiendo la plantilla de calidad corporativa de AzaharTech:

```java
/**
 * Proyecto: [Nombre del Proyecto Elegido de la Bolsa de Proyectos]
 * Consultora: AzaharTech
 * Módulo: Programación (PR) - Sprint 1 (RA1)
 * 
 * Descripción: Programa secuencial integral que captura datos de entrada,
 * realiza cálculos matemáticos de precisión y emite un informe formateado.
 * 
 * @author [Tu Nombre y Apellidos]
 * @version 1.0 (Octubre 2026)
 */
import java.util.Scanner;

public class Reto1Completo {
    public static void main(String[] args) {
        // 1. CONSTANTES DEL SISTEMA (Inmutables)
        final String NOMBRE_ENTIDAD = "AzaharTech Software";
        final double TASA_RECARGO = 0.15;
        final int UNIDADES_BASE = 100;
        
        // 2. DECLARACIÓN DE VARIABLES
        Scanner teclado = new Scanner(System.in);
        String identificadorCliente;
        int cantidadSolicitada;
        double costeUnitario;
        
        // 3. ENTRADA DE DATOS
        System.out.println("=================================================");
        System.out.println("   " + NOMBRE_ENTIDAD + " - SISTEMA DE GESTION   ");
        System.out.println("=================================================");
        System.out.print("Introduce el identificador del cliente: ");
        identificadorCliente = teclado.nextLine();
        
        System.out.print("Introduce la cantidad de unidades: ");
        cantidadSolicitada = teclado.nextInt();
        
        System.out.print("Introduce el coste base por unidad: ");
        costeUnitario = teclado.nextDouble();
        
        // 4. PROCESAMIENTO Y CÁLCULOS SECUENCIALES
        double subtotal = cantidadSolicitada * costeUnitario;
        double recargoCalculado = subtotal * TASA_RECARGO;
        double costeTotal = subtotal + recargoCalculado;
        
        // Uso de división entera y módulo para empaquetado
        int paquetesCompletos = cantidadSolicitada / UNIDADES_BASE;
        int unidadesSueltas = cantidadSolicitada % UNIDADES_BASE;
        
        // 5. SALIDA DE INFORMACIÓN FORMATEADA (printf)
        System.out.println("\n-------------------------------------------------");
        System.out.printf("CLIENTE ID:       %-20s%n", identificadorCliente);
        System.out.printf("PAQUETES (x%d):    %03d completos%n", UNIDADES_BASE, paquetesCompletos);
        System.out.printf("SOBRANTES:        %d unidades sueltas%n", unidadesSueltas);
        System.out.printf("SUBTOTAL:         %10.2f EUR%n", subtotal);
        System.out.printf("RECARGO (%.0f%%):    %10.2f EUR%n", (TASA_RECARGO * 100), recargoCalculado);
        System.out.println("-------------------------------------------------");
        System.out.printf("TOTAL OPERACIÓN:  %10.2f EUR%n", costeTotal);
        System.out.println("=================================================");
        
        teclado.close();
    }
}
```

#### 4. Cierre formal del Sprint 1 en Git
El estudiante comprueba que el pseudocódigo equivalente está en `pr/pseudocodigo/reto1_completo.psc`, ejecuta el programa en IntelliJ para verificar la salida limpia y sella la entrega en GitHub:

```bash
git add pr/
git commit -m "feat(pr): ensamblar programa secuencial completo y cerrar reto 1 del sprint 1"
git push
```

---

### Resumen del Cumplimiento del RA1 en las 24 horas del Sprint 1

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                 BALANCE FINAL DEL RA1 EN EL SPRINT 1 (PR)                   │
├──────────────┬────────────────────────────────────────┬─────────────────────┤
│ Sesiones     │ Contenidos y Competencias RA1          │ Criterios Cubiertos │
├──────────────┼────────────────────────────────────────┼─────────────────────┤
│ Días 1 a 4   │ Estructura de programas, variables,    │ CE 1.a, 1.b, 1.c,   │
│ (Semana 1)   │ tipos primitivos y entrada/salida.     │ CE 1.d, CE 1.e      │
├──────────────┼────────────────────────────────────────┼─────────────────────┤
│ Días 5 a 8   │ Operadores aritméticos, asignación     │ CE 1.g, CE 1.h,     │
│ (Semana 2)   │ compuesta, módulo (%) y casting.       │ CE 1.d, CE 1.e      │
├──────────────┼────────────────────────────────────────┼─────────────────────┤
│ Días 9 a 12  │ Constantes (final), literales, escapes,│ CE 1.f, CE 1.i,     │
│ (Semana 3)   │ salida con printf y entrega completa.  │ CE 1.a, CE 1.b      │
└──────────────┴────────────────────────────────────────┴─────────────────────┘
```