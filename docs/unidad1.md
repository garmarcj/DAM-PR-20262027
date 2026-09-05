MÓDULO PROFESIONAL: PROGRAMACIÓN (PR)

SPRINT 1. Algorítmica y fundamentos de programación (3 semanas | 24 horas)

SEMANA 1: FUNDAMENTOS, ESTRUCTURA DEL PROGRAMA Y VARIABLES (8 HORAS)

DÍA 1 (Lunes, 14 de septiembre de 2026 — 2 horas lectivas)

Bloque: Pensamiento computacional, estructura del programa en Java y variables numéricas primitivas (int, double)

- Distribución horaria: 45 minutos de teoría conceptual + 45 minutos de
  andamiaje guiado (PSeInt \rightarrow Java) + 30 minutos de laboratorio
  individual.
- Criterios de Evaluación vinculados: RA1.a, RA1.b, RA1.c, RA1.d, RA1.e.

1. Caso práctico narrativo: El arranque en AzaharTech

Son las once de la mañana en la sede de AzaharTech en Castellón de la Plana.
Tras haber configurado el entorno de desarrollo en la sesión matinal de Entornos
de Desarrollo, la célula de trabajo se traslada al aula técnica de Programación.

Laia Claramunt, supervisora del proyecto, reúne a Alba Torres, a Pau Ferrer y al
nuevo desarrollador junior (el estudiante). En la pizarra no hay código Java
todavía; Laia ha dibujado un esquema elemental compuesto por tres bloques
consecutivos: Entrada \rightarrow Proceso \rightarrow Salida.

«En Entornos de Desarrollo habéis aprendido qué es un entorno integrado y cómo
crear un repositorio en GitHub. Ahora empieza la ingeniería pura: aprender a dar
órdenes precisas a una máquina para que resuelva problemas.

Un ordenador es un procesador de una rapidez abrumadora, pero carece por
completo de criterio o sentido común: solo ejecuta secuencias estrictas de
instrucciones sobre datos almacenados en la memoria RAM.

Para el sistema de control de asistencia del IES El Caminàs (nuestro caso guía),
el primer paso no es dibujar una interfaz bonita, sino almacenar los primeros
datos en memoria: el número de aula asignada y la temperatura del sensor térmico
del vestíbulo. Hoy comprenderemos qué ocurre en la memoria del ordenador cuando
declaramos una variable y escribiremos nuestro primer programa interactivo en
Java».

2. Fundamento teórico: Estructura del programa y variables en memoria

A. El modelo Entrada-Procesamiento-Salida (IPO) y el Pensamiento Computacional

Todo programa informático, desde un script de diez líneas hasta un sistema
bancario internacional, responde al modelo universal de procesamiento de
información:

1.  Entrada (Input): Captura de datos brutos del exterior (teclado, sensores,
    archivos, red).
2.  Procesamiento (Processing): Manipulación de esos datos mediante operaciones
    aritméticas y lógicas sobre variables residentes en la memoria RAM.
3.  Salida (Output): Emisión de los resultados procesados hacia un soporte
    perceptible por el usuario (pantalla, consola, impresora) o hacia otro
    sistema.

[ ENTRADA ]               [ PROCESAMIENTO ]                 [ SALIDA ]
• Teclado     ──►  RAM: [ aula = 104 ]              ──►   Consola:
• Sensor           RAM: [ temp = 21.5 ]                   "Aula: 104 | Temp: 21.5 C"
CPU: (Cálculos / Asignaciones)

B. La anatomía de un archivo fuente en Java

Java es un lenguaje fuertemente tipado y estrictamente orientado a objetos. Esto
impone una estructura jerárquica obligatoria:

// 1. Declaración de la clase (debe llamarse exactamente igual que el archivo en disco)
public class Dia1VariablesNumericas {

    // 2. Método principal: punto de entrada donde comienza a ejecutarse el programa
    public static void main(String[] args) {
        
        // 3. Bloque de instrucciones secuenciales delimitado por llaves { ... }
        
    } // Fin del método main

} // Fin de la clase

- public class NombreClase: Define una unidad de código. El nombre debe
  comenzar en mayúscula y seguir la convención PascalCase (ej.
  RegistroAcceso).
- public static void main(String[] args): Es el método que busca la Máquina
  Virtual de Java (JVM) al arrancar. Sin él, la clase no es ejecutable de
  forma autónoma.
- Sentencias: Cada orden individual en Java debe terminar obligatoriamente con
  un punto y coma (;). Omitirlo provocará un error de compilación inmediato.

C. ¿Qué es una variable y qué sucede en la memoria RAM?

Una variable es un contenedor con nombre que reserva un bloque de bytes
contiguos en la memoria RAM del ordenador para guardar un dato mutable durante
la ejecución del programa.

Declarar una variable implica indicarle al compilador dos parámetros
indispensables:

1.  Identificador (Nombre): Nombre unívoco para referenciar la posición de
    memoria. Sigue la convención camelCase (ej. temperaturaVestibulo).
2.  Tipo de dato: Determina qué clase de información puede albergar y cuántos
    bytes ocupará exactamente en memoria.

D. Tipos primitivos numéricos: int y double

En este primer hito del RA1 nos centramos en los dos tipos de datos numéricos
universales del lenguaje:

┌────────────────────────────────────────────────────────────────────────────────────────┐
│                        TIPOS NUMÉRICOS BÁSICOS EN JAVA                                 │
├──────────────┬──────────────┬────────────────────────┬─────────────────────────────────┤
│ Tipo         │ Espacio RAM  │ Rango de valores       │ Caso de uso representativo      │
├──────────────┼──────────────┼────────────────────────┼─────────────────────────────────┤
│ int          │ 32 bits      │ -2.147.483.648         │ Números de aula, contadores de  │
│ (Entero)     │ (4 bytes)    │ a +2.147.483.647       │ alumnos, identificadores ID.    │
├──────────────┼──────────────┼────────────────────────┼─────────────────────────────────┤
│ double       │ 64 bits      │ Aprox. ±1.7 x 10^308   │ Lecturas térmicas, precios,     │
│ (Real/Coma)  │ (8 bytes)    │ (15 dígitos decimales) │ porcentajes, medias y ratios.   │
└──────────────┴──────────────┴────────────────────────┴─────────────────────────────────┘

E. La clase Scanner para lectura interactiva

Para capturar datos del teclado en la consola de texto, Java proporciona la
clase de utilidad java.util.Scanner, que debe importarse al inicio del archivo:

- Scanner teclado = new Scanner(System.in);: Crea el canal de escucha
  conectado a la entrada estándar (System.in).
- int valorEntero = teclado.nextInt();: Lee el siguiente número entero
  tecleado.
- double valorReal = teclado.nextDouble();: Lee el siguiente número decimal.
- Nota sobre configuración regional: En sistemas operativos configurados en
  castellano/valenciano, Scanner.nextDouble() espera que los decimales se
  introduzcan mediante una coma (,), no un punto.

3. Andamiaje didáctico paso a paso: PSeInt \rightarrow Java

Siguiendo el principio de andamiaje, modelamos el problema del aula y la
temperatura primero en pseudocódigo y lo traducimos inmediatamente a Java.

Paso 1. Diseño algorítmico en PSeInt (pr/pseudocodigo/dia1_variables_numericas.psc)

Algoritmo Dia1VariablesNumericas
// 1. Declaración explícita de tipos de datos en memoria
Definir aula Como Entero
Definir temperaturaVestibulo Como Real

    // 2. Entrada de datos por teclado
    Escribir "=== AZAHARTECH: TERMINAL IES EL CAMINAS ==="
    Escribir "Introduce el numero de aula asignada:"
    Leer aula
    
    Escribir "Introduce la temperatura actual del vestibulo (grados):"
    Leer temperaturaVestibulo
    
    // 3. Salida de datos procesados
    Escribir "--- REGISTRO DE CONFIGURACION ---"
    Escribir "Aula configurada: ", aula
    Escribir "Sensor termico: ", temperaturaVestibulo, " C"
FinAlgoritmo

Paso 2. Traducción inmediata a Java en IntelliJ IDEA (pr/src/Dia1VariablesNumericas.java)

/**
* Módulo: Programación (PR) - Sprint 1 (RA1)
* Unidad: Introducción a la programación y variables
*
* Caso Guía: Registro de parámetros numéricos del IES El Caminàs.
* @author AzaharTech Consulting
  */
  import java.util.Scanner; // Importación de la librería para lectura de teclado

public class Dia1VariablesNumericas {
public static void main(String[] args) {
// Inicialización del lector de entrada estándar
Scanner teclado = new Scanner(System.in);

        // 1. Declaración de variables en memoria
        int aula;
        double temperaturaVestibulo;
        
        // 2. Entrada de datos interactiva
        System.out.println("=== AZAHARTECH: TERMINAL IES EL CAMINAS ===");
        System.out.print("Introduce el numero de aula asignada: ");
        aula = teclado.nextInt(); // Lee un número entero
        
        System.out.print("Introduce la temperatura actual del vestibulo (grados): ");
        temperaturaVestibulo = teclado.nextDouble(); // Lee un número real
        
        // 3. Salida de información por consola
        System.out.println("--- REGISTRO DE CONFIGURACION ---");
        System.out.println("Aula configurada: " + aula);
        System.out.println("Sensor termico: " + temperaturaVestibulo + " C");
        
        // Cierre preventivo del recurso de entrada
        teclado.close();
    }
}

4. Laboratorio práctico: Aplicación a tu proyecto propio

Objetivo de la sesión

Identificar, diseñar y programar la captura de dos variables numéricas
esenciales (una de tipo entero int y otra de tipo decimal double)
correspondientes al proyecto que tu equipo seleccionó de la bolsa de proyectos
(por ejemplo: capacidad de almacén y coste unitario; aforo de sala y tarifa por
minuto; stock de entradas y peso en kilogramos).

Instrucciones paso a paso

1.  Abre tu proyecto en IntelliJ IDEA y sitúate en tu carpeta personal:
    azahartech/nombreEquipo/nombreEstudiante/pr/.
2.  En la subcarpeta pr/pseudocodigo/, crea el archivo dia1_proyecto_propio.psc.
3.  Escribe el algoritmo en PSeInt declarando tus dos variables numéricas,
    capturándolas con Leer y mostrándolas con Escribir. Ejecútalo y comprueba su
    funcionamiento.
4.  En la subcarpeta pr/src/, crea la clase Java Dia1ProyectoPropio.java.
5.  Traduce fielmente tu algoritmo PSeInt a código Java utilizando Scanner y
    System.out.println().
6.  Compila y ejecuta el programa pulsando Ctrl + Shift + F10. Introduce datos
    de prueba en la consola y verifica que la salida coincide con lo esperado.

DÍA 2 (Martes, 15 de septiembre de 2026 — 2 horas lectivas)

Bloque: Tipos de datos alfanuméricos (char, String), valores lógicos (boolean) y gestión de memoria

- Distribución horaria: 45 minutos de teoría conceptual + 45 minutos de
  andamiaje guiado (PSeInt \rightarrow Java) + 30 minutos de laboratorio
  individual.
- Criterios de Evaluación vinculados: RA1.a, RA1.d, RA1.e.

1. Caso práctico narrativo: La identidad del usuario en AzaharTech

Es martes por la mañana en la sala de desarrollo. Pau Ferrer ejecuta el programa
creado el día anterior. Muestra con orgullo cómo el sistema almacena el aula 104
y la temperatura 21.5.

Alba Torres observa la pantalla y plantea una objeción inmediata:

«Pau, los números nos dan contexto físico, pero no nos dicen quién está cruzando
la puerta. El sistema de acceso del IES El Caminàs necesita registrar el nombre
del alumno, la letra de su grupo y comprobar si su matrícula está activa o dada
de baja.

Un sistema informático real debe ser capaz de procesar letras individuales,
textos complejos y estados de verdadero o falso. Hoy completaremos la paleta
básica de tipos primitivos incorporando caracteres (char), cadenas (String) y
valores lógicos (boolean)».

2. Fundamento teórico: Caracteres, Cadenas y Boleanos

┌────────────────────────────────────────────────────────────────────────────────────────┐
│                        TIPOS ALFANUMÉRICOS Y LÓGICOS EN JAVA                           │
├──────────────┬──────────────┬────────────────────────┬─────────────────────────────────┤
│ Tipo         │ Espacio RAM  │ Representación         │ Caso de uso representativo      │
├──────────────┼──────────────┼────────────────────────┼─────────────────────────────────┤
│ char         │ 16 bits      │ Comillas simples: 'A'  │ Letra de grupo ('A', 'B'), letra│
│ (Carácter)   │ (2 bytes)    │ Código Unicode literal │ de DNI ('Z'), opción de menú.   │
├──────────────┼──────────────┼────────────────────────┼─────────────────────────────────┤
│ String       │ Referencia   │ Comillas dobles:       │ Nombre del estudiante, DNI,     │
│ (Cadena)     │ (Variable)   │ "Juan García Pérez"    │ dirección, token de seguridad.  │
├──────────────┼──────────────┼────────────────────────┼─────────────────────────────────┤
│ boolean      │ 1 bit        │ Literales:             │ Matrícula activa (true/false),  │
│ (Lógico)     │ (lógico)     │ true o false           │ torno bloqueado, acceso VIP.    │
└──────────────┴──────────────┴────────────────────────┴─────────────────────────────────┘

A. El tipo primitivo char (Carácter único)

- Almacena exactamente un único símbolo (letra, dígito, signo de puntuación o
  espacio).
- En Java se codifica internamente bajo el estándar Unicode UTF-16 (ocupa 2
  bytes), lo que le permite almacenar caracteres de cualquier alfabeto del
  mundo (incluyendo eñes, acentos o caracteres griegos).
- Regla sintáctica estricta: Se escribe siempre entre comillas simples (' ').
  Escribir 'AB' provocará un error de compilación (demasiados caracteres) y
  escribir "A" provocará un error de incompatibilidad de tipos (las comillas
  dobles representan un objeto String).

B. La clase String (Secuencias de texto)

- Aunque a efectos prácticos se utiliza como un tipo de dato, String es una
  clase que gestiona un array inmutable de caracteres en memoria.
- Se inicializa mediante literales entre comillas dobles: String nombre =
  "Laura Vidal";.
- Permite almacenar textos de cualquier longitud, desde una cadena vacía ("")
  hasta párrafos enteros.

C. El tipo primitivo boolean (Valores lógicos)

- Recibe su nombre en honor al matemático George Boole, padre del álgebra
  booleana.
- Una variable boolean solo admite dos valores literales reservados en
  minúscula: true (verdadero) o false (falso).
- Es la base sobre la que descansan todos los estados de un sistema
  informático (ej. ¿está la puerta abierta? ¿el alumno tiene autorización?).

D. La trampa del buffer de teclado en Java: Mezclar números y cadenas con Scanner

Cuando capturas un número con teclado.nextInt() o teclado.nextDouble() y a
continuación intentas leer un texto completo con teclado.nextLine(), ocurre un
fenómeno clásico de frustración: el programa parece saltarse la lectura del
texto sin dejarte escribir.

- Por qué ocurre: Cuando tecleas 104 y pulsas la tecla Enter, en el buffer de
  entrada del sistema quedan grabados los caracteres '1', '0', '4' y el salto
  de línea generado por la tecla Enter (\n). El método nextInt() consume el
  104, pero deja el \n flotando en el buffer. Cuando invocas a nextLine(),
  este método lee inmediatamente ese \n residual y cree que el usuario ha
  introducido una línea vacía.
- La solución profesional de AzaharTech: Cada vez que leas un número
  (nextInt() o nextDouble()) y la siguiente instrucción sea leer texto
  (nextLine()), debes ejecutar una llamada de limpieza previa:
  int aula = teclado.nextInt();
  teclado.nextLine(); // Instrucción de limpieza: consume el salto de línea residual
  String nombre = teclado.nextLine(); // Ahora sí espera a que el usuario escriba

3. Andamiaje didáctico paso a paso: PSeInt \rightarrow Java

Modelamos el registro de identidad del alumno en PSeInt y lo trasladamos a Java
resolviendo la gestión de buffer y tipos mixtos.

Paso 1. Diseño algorítmico en PSeInt (pr/pseudocodigo/dia2_alfanumericos.psc)

Algoritmo Dia2Alfanumericos
// 1. Declaración de variables alfanuméricas y lógicas
Definir nombreEstudiante Como Cadena
Definir dni Como Cadena
Definir letraGrupo Como Caracter
Definir matriculaActiva Como Logico

    // 2. Entrada de datos
    Escribir "=== REGISTRO DE IDENTIDAD: IES EL CAMINAS ==="
    Escribir "Introduce el nombre completo del estudiante:"
    Leer nombreEstudiante
    
    Escribir "Introduce el DNI del estudiante:"
    Leer dni
    
    Escribir "Introduce la letra del grupo asignado (A, B, C):"
    Leer letraGrupo
    
    // Asignación de estado lógico
    matriculaActiva <- Verdadero
    
    // 3. Salida de datos formateada
    Escribir "---------------------------------------------"
    Escribir "FICHA CREADA CON EXITO:"
    Escribir "Estudiante: ", nombreEstudiante
    Escribir "DNI:        ", dni
    Escribir "Grupo:      ", letraGrupo
    Escribir "Activo:     ", matriculaActiva
FinAlgoritmo

Paso 2. Traducción inmediata a Java en IntelliJ IDEA (pr/src/Dia2Alfanumericos.java)

/**
* Módulo: Programación (PR) - Sprint 1 (RA1)
* Caso Guía: Captura de tipos alfanuméricos y estados lógicos.
* @author AzaharTech Consulting
  */
  import java.util.Scanner;

public class Dia2Alfanumericos {
public static void main(String[] args) {
Scanner teclado = new Scanner(System.in);

        // 1. Declaración de variables
        String nombreEstudiante;
        String dni;
        char letraGrupo;
        boolean matriculaActiva;
        
        // 2. Entrada de datos interactiva
        System.out.println("=== REGISTRO DE IDENTIDAD: IES EL CAMINAS ===");
        System.out.print("Introduce el nombre completo del estudiante: ");
        nombreEstudiante = teclado.nextLine(); // Lee texto con espacios
        
        System.out.print("Introduce el DNI del estudiante: ");
        dni = teclado.nextLine();
        
        System.out.print("Introduce la letra del grupo asignado (A, B, C): ");
        // Capturamos el texto y extraemos el primer carácter con charAt(0)
        letraGrupo = teclado.next().charAt(0);
        
        // Asignación directa de un literal booleano
        matriculaActiva = true;
        
        // 3. Salida de datos por consola
        System.out.println("---------------------------------------------");
        System.out.println("FICHA CREADA CON EXITO:");
        System.out.println("Estudiante: " + nombreEstudiante);
        System.out.println("DNI:        " + dni);
        System.out.println("Grupo:      " + letraGrupo);
        System.out.println("Activo:     " + matriculaActiva);
        
        teclado.close();
    }
}

4. Laboratorio práctico: Aplicación a tu proyecto propio

Objetivo de la sesión

Incorporar a tu proyecto propio de la bolsa de proyectos la gestión de
identidades y estados, declarando al menos una variable String (nombre,
descripción o identificador), una variable char (código de categoría, zona o
letra) y una variable boolean (estado de disponibilidad, validación o
activación).

Instrucciones paso a paso

1.  Crea el archivo pr/pseudocodigo/dia2_proyecto_propio.psc.
2.  Define las variables alfanuméricas y lógicas adaptadas a tu sistema.
3.  Solicita la entrada interactiva de los datos por consola y muestra un
    resumen ordenado.
4.  Crea la clase Java pr/src/Dia2ProyectoPropio.java en IntelliJ.
5.  Traduce el algoritmo gestionando correctamente la captura de cadenas
    (nextLine()) y caracteres individuales (next().charAt(0)).
6.  Ejecuta el programa, introduce nombres compuestos con espacios y comprueba
    que no se produce el error de salto de buffer.

DÍA 3 (Miércoles, 16 de septiembre de 2026 — 2 horas lectivas)

Bloque: Operadores aritméticos elementales (+, -, *), sobrecarga del operador de concatenación y precedencia básica

- Distribución horaria: 45 minutos de teoría conceptual + 45 minutos de
  andamiaje guiado (PSeInt \rightarrow Java) + 30 minutos de laboratorio
  individual.
- Criterios de Evaluación vinculados: RA1.a, RA1.d, RA1.e, RA1.g.

1. Caso práctico narrativo: El fallo de la suma de tiempos en AzaharTech

Es miércoles a mediodía. En la terminal del laboratorio de AzaharTech, Pau
Ferrer muestra una línea de código que ha escrito para calcular el tiempo
acumulado de dos estudiantes que han llegado juntos al vestíbulo:

int tiempoPrimero = 5;
int tiempoSegundo = 3;
System.out.println("Tiempo total registrado: " + tiempoPrimero + tiempoSegundo + " minutos.");

Pau ejecuta el programa y la consola imprime:

Tiempo total registrado: 53 minutos.

Alba Torres suelta una carcajada cómplice:

«Pau, acabas de registrar que dos alumnos que han tardado cinco y tres minutos
han sumado ¡cincuenta y tres minutos de retraso!

En Java, el símbolo más (+) es un operador sobrecargado: si lo pones entre dos
números, suma; pero si lo pones a la derecha de una cadena de texto, convierte
todo lo que tiene a su derecha en texto y lo concatena de izquierda a derecha.

Hoy aprenderemos a dominar los operadores aritméticos elementales, a gobernar la
evaluación secuencial de expresiones y a utilizar los paréntesis () para forzar
a Java a sumar antes de concatenar».

2. Fundamento teórico: Aritmética básica y concatenación

┌────────────────────────────────────────────────────────────────────────────────────────┐
│                        EL COMPORTAMIENTO DUAL DEL OPERADOR '+'                         │
├───────────────────────────────────┬────────────────────────────────────────────────────┤
│ Expresión evaluada                │ Resultado y justificación                          │
├───────────────────────────────────┼────────────────────────────────────────────────────┤
│ 5 + 3                             │ 8 (Operación aritmética entre enteros).            │
├───────────────────────────────────┼────────────────────────────────────────────────────┤
│ "Total: " + 5 + 3                 │ "Total: 53" (Concatenación de izquierda a derecha).│
├───────────────────────────────────┼────────────────────────────────────────────────────┤
│ "Total: " + (5 + 3)               │ "Total: 8" (Los paréntesis fuerzan la suma previa).│
├───────────────────────────────────┼────────────────────────────────────────────────────┤
│ 5 + 3 + " minutos"                │ "8 minutos" (Suma 5+3=8 y luego concatena texto).  │
└───────────────────────────────────┴────────────────────────────────────────────────────┘

A. Operadores aritméticos binarios elementales

- Suma (+): Adición de dos operandos numéricos.
- Resta (-): Sustracción del operando derecho al izquierdo. También opera como
  operador unario de cambio de signo (ej. -valor).
- Multiplicación (*): Producto de dos cantidades numéricas.

B. Reglas de precedencia en expresiones combinadas

Java evalúa las expresiones de izquierda a derecha, pero respetando la jerarquía
matemática universal:

1.  Las operaciones encerradas entre paréntesis () se evalúan siempre en primer
    lugar.
2.  La multiplicación (*) tiene prioridad absoluta sobre la suma (+) y la resta
    (-).
3.  Si aparecen operadores del mismo nivel, se evalúan en orden estricto de
    izquierda a derecha.

Ejemplo de análisis de precedencia:

int resultado = 10 + 2 * 5;     // Multiplica 2*5=10 y luego suma 10 -> Resultado: 20
int resultado2 = (10 + 2) * 5;   // Suma (10+2)=12 y luego multiplica por 5 -> Resultado: 60

C. La semántica de la asignación (=)

El operador = no significa «igualdad matemática» (para la igualdad se usará ==
en el Sprint 3), sino asignación destructiva de valor:

totalAccesos = totalAccesos + 1;

1.  La CPU calcula primero el resultado de la expresión situada a la derecha del
    =: toma el valor actual de totalAccesos y le suma 1.
2.  Una vez calculado el nuevo valor, lo deposita en la posición de memoria
    indicada a la izquierda del =, sobrescribiendo el valor previo.

3. Andamiaje didáctico paso a paso: PSeInt \rightarrow Java

Modelamos el cálculo de accesos parciales del IES El Caminàs, sumando los
registros matutinos y vespertinos y emitiendo una notificación unificada.

Paso 1. Diseño algorítmico en PSeInt (pr/pseudocodigo/dia3_concatenacion.psc)

Algoritmo Dia3Concatenacion
// 1. Declaración de variables
Definir dni Como Cadena
Definir accesosManana Como Entero
Definir accesosTarde Como Entero
Definir totalAccesosDiarios Como Entero
Definir minutosEstanciaBase Como Entero
Definir minutosTotalesConsumidos Como Entero

    // 2. Entrada de datos
    Escribir "=== CONTROL HORARIO: IES EL CAMINAS ==="
    Escribir "Introduce el DNI del usuario:"
    Leer dni
    
    Escribir "Introduce accesos registrados en turno de manana:"
    Leer accesosManana
    
    Escribir "Introduce accesos registrados en turno de tarde:"
    Leer accesosTarde
    
    minutosEstanciaBase <- 50 // Cada sesión lectiva computa 50 minutos
    
    // 3. Procesamiento aritmético secuencial
    totalAccesosDiarios <- accesosManana + accesosTarde
    minutosTotalesConsumidos <- totalAccesosDiarios * minutosEstanciaBase
    
    // 4. Salida con concatenación estructurada
    Escribir "---------------------------------------"
    Escribir "RESUMEN DE ASISTENCIA DIARIA:"
    Escribir "DNI: " + dni
    Escribir "Total de entradas registradas: " + ConvertirATexto(totalAccesosDiarios)
    Escribir "Tiempo lectivo computado: " + ConvertirATexto(minutosTotalesConsumidos) + " minutos."
FinAlgoritmo

Paso 2. Traducción inmediata a Java en IntelliJ IDEA (pr/src/Dia3Concatenacion.java)

/**
* Módulo: Programación (PR) - Sprint 1 (RA1)
* Caso Guía: Operadores aritméticos y concatenación segura de mensajes.
* @author AzaharTech Consulting
  */
  import java.util.Scanner;

public class Dia3Concatenacion {
public static void main(String[] args) {
Scanner teclado = new Scanner(System.in);

        // 1. Declaración de variables
        String dni;
        int accesosManana;
        int accesosTarde;
        int totalAccesosDiarios;
        int minutosEstanciaBase = 50; // Constante conceptual de sesión
        int minutosTotalesConsumidos;
        
        // 2. Entrada de datos
        System.out.println("=== CONTROL HORARIO: IES EL CAMINAS ===");
        System.out.print("Introduce el DNI del usuario: ");
        dni = teclado.nextLine();
        
        System.out.print("Introduce accesos en turno de manana: ");
        accesosManana = teclado.nextInt();
        
        System.out.print("Introduce accesos en turno de tarde: ");
        accesosTarde = teclado.nextInt();
        
        // 3. Procesamiento aritmético secuencial
        totalAccesosDiarios = accesosManana + accesosTarde;
        minutosTotalesConsumidos = totalAccesosDiarios * minutosEstanciaBase;
        
        // 4. Salida concatenada (utilizando paréntesis protectores)
        System.out.println("---------------------------------------");
        System.out.println("RESUMEN DE ASISTENCIA DIARIA:");
        System.out.println("DNI: " + dni);
        System.out.println("Total entradas: " + (accesosManana + accesosTarde)); // Forzamos suma previa
        System.out.println("Total computado: " + minutosTotalesConsumidos + " minutos lectivos.");
        
        teclado.close();
    }
}

4. Laboratorio práctico: Aplicación a tu proyecto propio

Objetivo de la sesión

Implementar en tu proyecto propio un módulo de cálculo que solicite al usuario
dos datos numéricos, realice una suma o multiplicación secuencial protegida
mediante paréntesis y emita un informe descriptivo uniendo texto explicativo y
variables.

Instrucciones paso a paso

1.  Crea el archivo pr/pseudocodigo/dia3_proyecto_propio.psc.
2.  Define los datos de entrada adaptados a tu contexto de negocio.
3.  Implementa al menos una suma y una multiplicación sobre las variables
    numéricas.
4.  Crea la clase Java pr/src/Dia3ProyectoPropio.java.
5.  Traduce el algoritmo a Java asegurando que ninguna concatenación altere los
    resultados matemáticos.
6.  Realiza una prueba introduciendo valores altos para comprobar que el cálculo
    no desborda el tipo int.

DÍA 4 (Jueves, 17 de septiembre de 2026 — 2 horas lectivas)

Bloque: Laboratorio de integración: Reto 1 (Paso 1: Variables y Captura de Datos) del proyecto propio

- Distribución horaria: 30 minutos de modelado de calidad + 90 minutos de
  codificación autónoma tutelada.
- Criterios de Evaluación vinculados: RA1.a, RA1.b, RA1.c, RA1.d, RA1.e,
  RA1.i.

1. Caso práctico narrativo: El primer hito de entrega de la célula

Es jueves por la mañana. Con esta sesión concluye la primera semana de
programación del curso. Laia Claramunt convoca a los desarrolladores frente al
proyector:

«Durante los últimos tres días hemos sentado las bases del lenguaje: sabemos qué
es una clase, cómo declarar variables de distintos tipos en la memoria RAM, cómo
evitar los fallos del buffer de entrada con Scanner y cómo realizar cálculos
aritméticos básicos.

Hoy cerramos la primera semana con una entrega técnica formal. Cada uno de
vosotros debe programar y verificar el primer componente secuencial de su
proyecto de la bolsa de proyectos: el módulo que define la estructura
corporativa, solicita los datos de entrada al usuario, realiza una primera
operación y emite la respuesta formateada.

Al sonar el timbre de las dos horas, el código fuente debe estar compilado,
probado en IntelliJ y publicado en GitHub bajo la carpeta oficial pr/».

2. Estándares de calidad y rúbrica técnica exigida

Para que el programa del Reto 1 (Paso 1) se considere aceptado por AzaharTech,
debe cumplir estrictamente los siguientes cuatro requisitos de ingeniería:

1.  Nomenclatura limpia: Nombre de clase en PascalCase (Reto1Variables),
    variables en camelCase autoexplicativas (tarifaBase, cantidadUnidades), sin
    abreviaturas incomprensibles como x, a1 o temp.
2.  Higiene de código: Indentación uniforme de 4 espacios en cada bloque
    (utilizar el atajo de IntelliJ Ctrl + Alt + L para autoformatear antes de
    entregar).
3.  Gestión de recursos: Declaración y cierre del objeto Scanner al finalizar la
    lectura (teclado.close()).
4.  Comentarios de cabecera: Presencia de un bloque de documentación inicial con
    el nombre del desarrollador, proyecto, fecha y descripción funcional.

3. Taller de integración guiada: Plantilla del Reto 1 (Paso 1)

Cada estudiante traslada a su proyecto el siguiente esquema estructurado de
desarrollo:

Paso 1. Algoritmo secuencial en PSeInt (pr/pseudocodigo/reto1_variables.psc)

Algoritmo Reto1Variables
// 1. Declaración formal de tipos
Definir identificadorProyecto Como Cadena
Definir nombreCliente Como Cadena
Definir unidadesRegistradas Como Entero
Definir valorUnitario Como Real
Definir sistemaOperativo Como Logico
Definir subtotalCalculado Como Real

    // 2. Captura interactiva
    Escribir "================================================="
    Escribir "   AZAHARTECH - SISTEMA DE GESTION DE RETOS      "
    Escribir "================================================="
    Escribir "Introduce el identificador del proyecto:"
    Leer identificadorProyecto
    
    Escribir "Introduce el nombre del cliente:"
    Leer nombreCliente
    
    Escribir "Introduce el numero de unidades registradas:"
    Leer unidadesRegistradas
    
    Escribir "Introduce el valor o tarifa unitaria:"
    Leer valorUnitario
    
    sistemaOperativo <- Verdadero
    
    // 3. Procesamiento aritmético secuencial
    subtotalCalculado <- unidadesRegistradas * valorUnitario
    
    // 4. Emisión de informe
    Escribir "-------------------------------------------------"
    Escribir "INFORME DE CAPTURA INICIAL:"
    Escribir "Proyecto: " + identificadorProyecto + " | Cliente: " + nombreCliente
    Escribir "Unidades: " + ConvertirATexto(unidadesRegistradas)
    Escribir "Subtotal base: " + ConvertirATexto(subtotalCalculado) + " euros."
    Escribir "Estado operativo verificado: " + ConvertirATexto(sistemaOperativo)
    Escribir "================================================="
FinAlgoritmo

Paso 2. Implementación en Java (pr/src/Reto1Variables.java)

/**
* Proyecto: [Nombre de tu Proyecto Elegido de la Bolsa de Proyectos]
* Módulo: Programación (PR) - Sprint 1 (RA1)
*
* Descripción: Módulo secuencial de captura de parámetros de entrada,
* almacenamiento en memoria y cálculo aritmético base.
*
* @author [Tus Apellidos, Tu Nombre]
* @version 1.0 (Semana 1 - Septiembre 2026)
  */
  import java.util.Scanner;

public class Reto1Variables {
public static void main(String[] args) {
// Inicialización de la entrada por teclado
Scanner teclado = new Scanner(System.in);

        // 1. Declaración explícita de variables de distintos tipos
        String identificadorProyecto;
        String nombreCliente;
        int unidadesRegistradas;
        double valorUnitario;
        boolean sistemaOperativo = true;
        double subtotalCalculado;
        
        // 2. Entrada de datos interactiva
        System.out.println("=================================================");
        System.out.println("   AZAHARTECH - SISTEMA DE GESTIÓN DE RETOS      ");
        System.out.println("=================================================");
        System.out.print("Introduce el identificador del proyecto: ");
        identificadorProyecto = teclado.nextLine();
        
        System.out.print("Introduce el nombre del cliente: ");
        nombreCliente = teclado.nextLine();
        
        System.out.print("Introduce el número de unidades registradas: ");
        unidadesRegistradas = teclado.nextInt();
        
        System.out.print("Introduce el valor o tarifa unitaria: ");
        valorUnitario = teclado.nextDouble();
        
        // 3. Procesamiento aritmético secuencial
        subtotalCalculado = unidadesRegistradas * valorUnitario;
        
        // 4. Salida de resultados estructurada
        System.out.println("-------------------------------------------------");
        System.out.println("INFORME DE CAPTURA INICIAL:");
        System.out.println("Proyecto: " + identificadorProyecto + " | Cliente: " + nombreCliente);
        System.out.println("Unidades: " + unidadesRegistradas);
        System.out.println("Subtotal base: " + subtotalCalculado + " euros.");
        System.out.println("Estado operativo verificado: " + sistemaOperativo);
        System.out.println("=================================================");
        
        // Cierre preventivo del recurso
        teclado.close();
    }
}

4. Cierre formal en Git y verificación de la Semana 1

1.  En IntelliJ IDEA, abre la terminal integrada (Alt + F12) o la pestaña Commit
    (Ctrl + K).
2.  Comprueba mediante git status que los dos archivos creados
    (pr/pseudocodigo/reto1_variables.psc y pr/src/Reto1Variables.java) están
    detectados por Git.
3.  Añade los archivos y confirma los cambios aplicando el estándar de
    Conventional Commits:
    git add pr/
    git commit -m "feat(pr): implementar captura de variables y presentacion de datos del proyecto propio"
    git push
4.  Accede a tu repositorio remoto en GitHub desde el navegador y verifica que
    ambos archivos se visualizan correctamente dentro de la carpeta pr/.

Balance pedagógico de la Semana 1 de Programación

Al completar estas primeras cuatro sesiones (8 horas lectivas):

- Has interiorizado el modelo de procesamiento Entrada \rightarrow Proceso
  \rightarrow Salida.
- Conoces la estructura obligatoria de una clase Java y su método main.
- Dominas la reserva y tipado de variables en la memoria RAM con los tipos
  primitivos int, double, char, boolean y la clase String.
- Controlas la entrada de datos con Scanner, previniendo errores de salto de
  buffer.
- Aplicas los operadores aritméticos básicos y la concatenación de cadenas sin
  distorsiones en los cálculos.
- Cuentas en tu repositorio con la primera entrega operativa del Reto 1 de tu
  proyecto propio, 100 % fiel a los contenidos y Criterios de Evaluación del
  RA1.

# MÓDULO PROFESIONAL: PROGRAMACIÓN (PR)

## SPRINT 1. Algorítmica y fundamentos de programación (3 semanas | 24 horas)

---

# SEMANA 2: OPERADORES, EXPRESIONES Y CONVERSIONES DE TIPO (8 HORAS)

---

## DÍA 5 (Lunes, 21 de septiembre de 2026 — 2 horas lectivas)
### Bloque: Operadores de asignación compuesta, operadores unarios de incremento/decremento (`++`, `--`) y trazas de memoria
* **Distribución horaria:** 45 minutos de teoría conceptual + 45 minutos de andamiaje guiado (PSeInt $\rightarrow$ Java) + 30 minutos de laboratorio individual.
* **Criterios de Evaluación vinculados:** RA1.a, RA1.d, RA1.e, RA1.g.

---

### 1. Caso práctico narrativo: El contador del vestíbulo en AzaharTech

Es lunes por la mañana en las oficinas de **AzaharTech**. En la pantalla de pruebas de la sala de desarrollo, **Pau Ferrer** tiene abierto el código de registro de acceso del **IES El Caminàs**. Ha escrito una rutina para actualizar los contadores cada vez que un alumno entra por el torno:

```java
totalAlumnos = totalAlumnos + 1;
minutosAcumulados = minutosAcumulados + tiempoEstancia;
totalAlumnos = totalAlumnos + 1;
```

**Alba Torres** se acerca a su mesa, observa el código y comenta con tono pedagógico:
> *«Pau, tu lógica es correcta y la CPU la ejecutará sin rechistar, pero estás escribiendo código como si estuviéramos en los años setenta. Repetir el nombre de la variable a ambos lados de la asignación ensucia el programa, dificulta la lectura y aumenta el riesgo de erratas tipográficas.*
>
> *En la ingeniería de software profesional utilizamos **operadores de asignación compuesta** y los **operadores de incremento y decremento (`++`, `--`)**. Nos permiten expresar exactamente la misma operación de forma compacta, elegante y optimizada a nivel de compilador.*
>
> *Pero cuidado: el operador `++` es un arma de doble filo si no entiendes la diferencia crítica entre el **pre-incremento** y el **post-incremento**. Hoy aprenderemos a dominar estas instrucciones y realizaremos trazas de memoria rigurosas sobre el papel antes de tocar el teclado»*.

---

### 2. Fundamento teórico: Asignación compuesta, incremento y evaluación secuencial

```
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                        OPERADORES DE ASIGNACIÓN COMPUESTA EN JAVA                      │
├─────────────────────┬───────────────────────────┬──────────────────────────────────────┤
│ Operador Abreviado  │ Expresión Equivalente     │ Significado técnico                  │
├─────────────────────┼───────────────────────────┼──────────────────────────────────────┤
│ x += valor;         │ x = x + valor;            │ Suma 'valor' al contenido actual de x│
├─────────────────────┼───────────────────────────┼──────────────────────────────────────┤
│ x -= valor;         │ x = x - valor;            │ Resta 'valor' al contenido actual x  │
├─────────────────────┼───────────────────────────┼──────────────────────────────────────┤
│ x *= valor;         │ x = x * valor;            │ Multiplica x por 'valor'             │
├─────────────────────┼───────────────────────────┼──────────────────────────────────────┤
│ x /= valor;         │ x = x / valor;            │ Divide x entre 'valor'               │
├─────────────────────┼───────────────────────────┼──────────────────────────────────────┤
│ x %= valor;         │ x = x % valor;            │ Asigna a x el resto de la división   │
└─────────────────────┴───────────────────────────┴──────────────────────────────────────┘
```

#### A. Operadores de Asignación Compuesta
Los operadores de asignación compuesta combinan una operación aritmética binaria con la reasignación destructiva del resultado en la misma variable de memoria.

* **Ventaja de mantenibilidad:** Si la variable tiene un nombre largo y descriptivo (*ej. `totalSegundosAcumuladosTerminal`*), no es necesario duplicar el identificador:
  ```java
  totalSegundosAcumuladosTerminal += segundosLectura;
  ```
* **Casting implícito en la asignación compuesta:** En Java, los operadores compuestos aplican un *casting* implícito al tipo de la variable destino, evitando incompatibilidades menores de tipos.

#### B. Operadores Unarios de Incremento (`++`) y Decremento (`--`)
Son operadores unarios (afectan a un único operando) cuyo propósito exclusivo es sumar o restar exactamente **1** al valor almacenado en una variable numérica entera.

Existen dos modalidades con comportamientos radicalmente distintos cuando forman parte de una expresión:

1. **Post-incremento (`variable++`):**
    * Primero se evalúa o utiliza el valor **actual** de la variable en la expresión.
    * Inmediatamente después de usar el valor, la variable se incrementa en 1 en la memoria RAM.
2. **Pre-incremento (`++variable`):**
    * Primero se incrementa en 1 el valor de la variable en la memoria RAM.
    * Inmediatamente después, se utiliza el **nuevo** valor resultante en la expresión evaluada.

```java
// Ejemplo ilustrativo de la diferencia en memoria:
int a = 10;
int b = a++; // Paso 1: 'b' recibe el valor actual de 'a' (10). Paso 2: 'a' pasa a valer 11.
// Resultado final: a = 11, b = 10

int x = 10;
int y = ++x; // Paso 1: 'x' pasa a valer 11. Paso 2: 'y' recibe el nuevo valor (11).
// Resultado final: x = 11, y = 11
```

> **Directriz de calidad de AzaharTech:** Para evitar errores sutiles de sincronización, utiliza `variable++;` como una instrucción aislada e independiente siempre que sea posible, en lugar de incrustar incrementos dentro de cálculos complejos.

#### C. La Traza de Escritorio (*Desk Checking*)
Una **traza de escritorio** es un ensayo manual y metódico que el programador realiza sobre una tabla para registrar el valor que adopta cada celda de memoria en cada paso secuencial del algoritmo. Es la herramienta fundamental para cazar errores de lógica antes de compilar.

---

### 3. Andamiaje didáctico paso a paso: PSeInt $\rightarrow$ Java

Modelamos el incremento del aforo del vestíbulo del IES El Caminàs paso a paso.

#### Paso 1. Diseño algorítmico en PSeInt (`pr/pseudocodigo/dia5_incrementos.psc`)
```psc
Algoritmo Dia5Incrementos
    // 1. Declaración de variables
    Definir alumnosEnVestibulo Como Entero
    Definir minutosTotales Como Entero
    
    alumnosEnVestibulo <- 0
    minutosTotales <- 0
    
    Escribir "=== SIMULACION DE ENTRADA SECUENCIAL ==="
    Escribir "Estado inicial: ", alumnosEnVestibulo, " alumnos."
    
    // 2. Llegada del primer alumno (añade 1 alumno y 8 minutos)
    alumnosEnVestibulo <- alumnosEnVestibulo + 1
    minutosTotales <- minutosTotales + 8
    
    // 3. Llegada del segundo alumno (añade 1 alumno y 12 minutos)
    alumnosEnVestibulo <- alumnosEnVestibulo + 1
    minutosTotales <- minutosTotales + 12
    
    // 4. Salida de resultados
    Escribir "----------------------------------------"
    Escribir "Alumnos totales procesados: ", alumnosEnVestibulo
    Escribir "Minutos acumulados en acceso: ", minutosTotales
FinAlgoritmo
```

#### Paso 2. Traducción inmediata a Java en IntelliJ IDEA (`pr/src/Dia5Incrementos.java`)
```java
/**
 * Módulo: Programación (PR) - Sprint 1 (RA1)
 * Caso Guía: Asignación compuesta y operadores de incremento.
 * @author AzaharTech Consulting
 */
public class Dia5Incrementos {
    public static void main(String[] args) {
        // 1. Inicialización de contadores y acumuladores en memoria
        int alumnosEnVestibulo = 0;
        int minutosTotales = 0;
        
        System.out.println("=== SIMULACIÓN DE ENTRADA SECUENCIAL ===");
        System.out.println("Estado inicial: " + alumnosEnVestibulo + " alumnos.");
        
        // 2. Llegada del primer alumno: uso de incremento y asignación compuesta
        alumnosEnVestibulo++;      // Incremento compacto: suma 1
        minutosTotales += 8;       // Asignación compuesta: suma 8 al acumulador
        
        // 3. Llegada del segundo alumno
        alumnosEnVestibulo++;
        minutosTotales += 12;
        
        // 4. Salida de datos
        System.out.println("----------------------------------------");
        System.out.println("Alumnos totales procesados: " + alumnosEnVestibulo);
        System.out.println("Minutos acumulados en acceso: " + minutosTotales);
        
        // Demostración explícita de pre y post-incremento
        int turnoActual = 100;
        System.out.println("\n--- COMPORTAMIENTO PRE VS POST ---");
        System.out.println("Turno mostrado con post-incremento: " + (turnoActual++)); // Imprime 100 y luego sube a 101
        System.out.println("Turno en memoria tras la operación:  " + turnoActual);     // Imprime 101
        
        System.out.println("Turno mostrado con pre-incremento:  " + (++turnoActual)); // Sube a 102 y luego imprime 102
    }
}
```

---

### 4. Laboratorio práctico: Aplicación a tu proyecto propio

#### Objetivo de la sesión
Construir una traza de memoria de tu proyecto propio y aplicar operadores de asignación compuesta (`+=`, `-=`) y de incremento (`++`) sobre contadores y acumuladores de tu sistema.

#### Instrucciones paso a paso
1. En tu libreta técnica o en un documento de texto, dibuja una tabla de traza con 3 variables de tu proyecto. Anota su valor inicial (`0`) y cómo cambian tras 4 operaciones secuenciales de incremento y suma compuesta.
2. Abre IntelliJ IDEA en `pr/pseudocodigo/` y crea el archivo `dia5_proyecto_propio.psc`.
3. Diseña el algoritmo secuencial en PSeInt declarando al menos un contador y un acumulador.
4. En `pr/src/`, crea la clase Java `Dia5ProyectoPropio.java`.
5. Traduce el código utilizando los operadores `++`, `--` y `+=`.
6. Compila y ejecuta. Comprueba que los valores finales coinciden exactamente con la traza de escritorio que habías calculado a mano.

---
---

## DÍA 6 (Martes, 22 de septiembre de 2026 — 2 horas lectivas)
### Bloque: La división entera frente a la división real y el operador módulo (`%`)
* **Distribución horaria:** 45 minutos de teoría conceptual + 45 minutos de andamiaje guiado (PSeInt $\rightarrow$ Java) + 30 minutos de laboratorio individual.
* **Criterios de Evaluación vinculados:** RA1.a, RA1.d, RA1.e, RA1.g.

---

### 1. Caso práctico narrativo: El enigma de los segundos en AzaharTech

Es martes por la mañana. En la terminal del laboratorio, **Pau Ferrer** muestra una incidencia detectada en el terminal del **IES El Caminàs**:
El reloj del sistema registra que un alumno ha tardado `185` segundos desde que cruzó la valla exterior hasta que validó su QR en el vestíbulo. Pau necesita mostrar ese tiempo en la pantalla desglosado de forma humana: *«3 minutos y 5 segundos»*.

Pau ha escrito:
```java
int minutos = 185 / 60; // Da 3
```
Y añade confuso:
> *«Sé que 185 dividido entre 60 da 3 minutos enteros, pero ¿cómo saco los 5 segundos que sobran sin usar restas manuales ni condicionales?»*.

**Alba Torres** sonríe y escribe un símbolo en la pizarra: **`%`**:
> *«Te falta el operador más elegante de la aritmética de enteros: el **módulo o resto de la división (`%`)**. Junto con la división entera, te permite descomponer cualquier unidad temporal, monetaria o métrica con precisión absoluta en una sola línea de código»*.

---

### 2. Fundamento teórico: División entera, división decimal y el operador residuo

```
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                        ANATOMÍA DE UNA DIVISIÓN EN ARITMÉTICA JAVA                     │
├────────────────────────────────────────────────────────────────────────────────────────┤
│                       185  │  60    ◄── Divisor (int)                                  │
│       Dividendo (int) ────  ──────                                                     │
│                         5     3     ◄── Cociente: Obtenido con el operador '/'         │
│                         ▲                                                              │
│                         └────────────── Resto/Residuo: Obtenido con el operador '%'    │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

#### A. La dualidad del operador de división (`/`) en Java
En Java, el comportamiento del operador `/` depende estrictamente del **tipo de dato de sus operandos**, no del resultado que esperaría una calculadora:

1. **División entera (`int / int`):**
    * Si ambos operandos son enteros, Java ejecuta una **división euclídea entera**.
    * Cualquier fracción o decimal se descarta por completo (**truncamiento hacia cero**, no redondeo).
    * `185 / 60` resulta **`3`**.
    * `7 / 2` resulta **`3`**.
2. **División en coma flotante (`double / int` o `int / double` o `double / double`):**
    * Si al menos uno de los operandos es decimal (`double` o `float`), Java promociona la operación a división real.
    * `185.0 / 60` resulta **`3.0833333333333335`**.
    * `7.0 / 2` resulta **`3.5`**.

#### B. El operador Módulo o Resto (`%`)
El operador `%` calcula el **residuo exacto que sobra al dividir dos números enteros**.
* En la operación `185 % 60`, como $60 \times 3 = 180$, sobran exactamente **`5`**.
* Propiedades matemáticas y aplicaciones en ingeniería:
    1. **Descomposición de unidades:** Transformar segundos a horas/minutos, o céntimos a euros.
    2. **Detección de paridad:** Un número `n` es par si `n % 2 == 0` (aunque la comprobación condicional se hará en el Sprint 3, la propiedad matemática se fundamenta aquí).
    3. **Comportamiento cíclico:** El resultado de `x % N` siempre estará acotado estrictamente en el rango de $0$ a $N-1$, ideal para turnos o posiciones en ruletas y colas.

#### C. Algoritmo secuencial de descomposición horaria (Horas, Minutos, Segundos)
Para descomponer una cantidad arbitraria de segundos totales en sus tres componentes canónicos sin utilizar ninguna condición:
* $1 \text{ hora} = 3600 \text{ segundos}$.
* $1 \text{ minuto} = 60 \text{ segundos}$.

$$\text{Horas} = \lfloor \text{segundosTotales} / 3600 \rfloor$$
$$\text{Segundos sobrantes de las horas} = \text{segundosTotales} \% 3600$$
$$\text{Minutos} = \lfloor \text{Segundos sobrantes} / 60 \rfloor$$
$$\text{Segundos finales} = \text{segundosTotales} \% 60$$

---

### 3. Andamiaje didáctico paso a paso: PSeInt $\rightarrow$ Java

Modelamos la descomposición de tiempos del IES El Caminàs paso a paso.

#### Paso 1. Diseño algorítmico en PSeInt (`pr/pseudocodigo/dia6_modulo_tiempos.psc`)
```psc
Algoritmo Dia6ModuloTiempos
    // 1. Declaración de variables
    Definir segundosTotales Como Entero
    Definir horas Como Entero
    Definir minutos Como Entero
    Definir segundosRestantes Como Entero
    
    // 2. Entrada de datos
    Escribir "=== CALCULADORA TEMPORAL: IES EL CAMINAS ==="
    Escribir "Introduce el total de segundos transcurridos:"
    Leer segundosTotales
    
    // 3. Descomposición matemática con división entera y módulo
    horas <- trunc(segundosTotales / 3600)
    minutos <- trunc((segundosTotales MOD 3600) / 60)
    segundosRestantes <- segundosTotales MOD 60
    
    // 4. Salida de resultados
    Escribir "--------------------------------------------"
    Escribir "Desglose temporal exacto:"
    Escribir horas, " horas, ", minutos, " minutos y ", segundosRestantes, " segundos."
FinAlgoritmo
```

#### Paso 2. Traducción inmediata a Java en IntelliJ IDEA (`pr/src/Dia6ModuloTiempos.java`)
```java
/**
 * Módulo: Programación (PR) - Sprint 1 (RA1)
 * Caso Guía: División entera y operador módulo (%).
 * @author AzaharTech Consulting
 */
import java.util.Scanner;

public class Dia6ModuloTiempos {
    public static void main(String[] args) {
        Scanner teclado = new Scanner(System.in);
        
        // 1. Declaración de variables
        int segundosTotales;
        int horas;
        int minutos;
        int segundosRestantes;
        
        // 2. Entrada de datos interactiva
        System.out.println("=== CALCULADORA TEMPORAL: IES EL CAMINAS ===");
        System.out.print("Introduce el total de segundos transcurridos: ");
        segundosTotales = teclado.nextInt();
        
        // 3. Procesamiento aritmético secuencial
        horas = segundosTotales / 3600;              // División entera
        minutos = (segundosTotales % 3600) / 60;      // Resto de horas dividido entre 60
        segundosRestantes = segundosTotales % 60;     // Resto final de segundos
        
        // 4. Emisión de resultados
        System.out.println("--------------------------------------------");
        System.out.println("Desglose temporal exacto:");
        System.out.println(horas + " horas, " + minutos + " minutos y " + segundosRestantes + " segundos.");
        
        teclado.close();
    }
}
```

---

### 4. Laboratorio práctico: Aplicación a tu proyecto propio

#### Objetivo de la sesión
Aplicar la combinación de **división entera (`/`) y operador módulo (`%`)** a un problema de descomposición o agrupamiento específico de tu proyecto de la bolsa de proyectos.

#### Ejemplos según la temática de tu proyecto:
* *Proyecto de comercio/ventas:* Descomponer una cantidad de céntimos en euros enteros y céntimos restantes (`euros = totalCentimos / 100; restoCentimos = totalCentimos % 100;`).
* *Proyecto logístico/almacén:* Descomponer unidades de producto en cajas completas de 12 o 24 unidades y unidades sueltas que quedan fuera.
* *Proyecto de reservas/tiempo:* Descomponer minutos de servicio en días, horas y minutos.

#### Instrucciones paso a paso
1. Crea el archivo `pr/pseudocodigo/dia6_proyecto_propio.psc`.
2. Escribe el algoritmo definiendo los datos de entrada, las operaciones de cociente y resto, y la salida por consola.
3. Crea la clase Java `pr/src/Dia6ProyectoPropio.java`.
4. Traduce el algoritmo a Java.
5. Ejecuta pruebas con valores límite (por ejemplo: `0`, un valor menor que el divisor y un valor muy alto) y comprueba que la suma de las partes reconstruye exactamente el total original.

---
---

## DÍA 7 (Miércoles, 23 de septiembre de 2026 — 2 horas lectivas)
### Bloque: Jerarquía de operadores, expresiones matemáticas complejas y conversiones de tipo (*Casting*)
* **Distribución horaria:** 45 minutos de teoría conceptual + 45 minutos de andamiaje guiado (PSeInt $\rightarrow$ Java) + 30 minutos de laboratorio individual.
* **Criterios de Evaluación vinculados:** RA1.a, RA1.d, RA1.g, RA1.h.

---

### 1. Caso práctico narrativo: El cálculo del porcentaje de asistencia

Es miércoles por la mañana. En la pantalla de pruebas, **Pau Ferrer** muestra su nuevo algoritmo para calcular la tasa porcentual de alumnos que han entrado a primera hora al **IES El Caminàs**.

Ha introducido:
* Alumnos matriculados: `40`.
* Alumnos que han escaneado el QR: `38`.

Y ha programado la fórmula:
```java
double porcentajeAsistencia = (alumnosPresentes / alumnosMatriculados) * 100;
System.out.println("Tasa de asistencia: " + porcentajeAsistencia + "%");
```

Al pulsar *Run*, la consola imprime:
```text
Tasa de asistencia: 0.0%
```

Pau exclama desconcertado:
> *«¡Pero si han venido 38 de 40! ¡Debería dar un 95 %! ¿Por qué Java dice cero coma cero?»*.

**Alba Torres** y **Laia Claramunt** se acercan a la pantalla. Laia explica:
> *«Acabas de caer en la trampa más habitual de los tipos primitivos. `alumnosPresentes` es `int` y `alumnosMatriculados` es `int`. Java evalúa primero los paréntesis de izquierda a derecha: divide `38 / 40`, y como es división entera, el resultado es `0`. Luego multiplica `0 * 100`, que sigue siendo `0`. Y finalmente, al asignarlo a la variable `double`, lo convierte en `0.0`.*
>
> *Para solucionar esto debemos dominar la **jerarquía de evaluación** y aplicar una **conversión explícita o casting** que obligue a Java a calcular con decimales desde el primer paso»*.

---

### 2. Fundamento teórico: Precedencia de operadores y conversiones de tipo

```
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                        JERARQUÍA Y PRECEDENCIA DE EVALUACIÓN                           │
├──────────────┬───────────────────────────────────────────┬─────────────────────────────┤
│ Prioridad    │ Operadores                                │ Dirección de evaluación     │
├──────────────┼───────────────────────────────────────────┼─────────────────────────────┤
│ 1. ( )       │ Paréntesis agrupadores                    │ De dentro hacia afuera      │
├──────────────┼───────────────────────────────────────────┼─────────────────────────────┤
│ 2. Unarios   │ ++ , -- , + , - , (tipo) [casting]        │ De derecha a izquierda      │
├──────────────┼───────────────────────────────────────────┼─────────────────────────────┤
│ 3. Aritmética│ * , / , %                                 │ De izquierda a derecha      │
├──────────────┼───────────────────────────────────────────┼─────────────────────────────┤
│ 4. Aritmética│ + , -                                     │ De izquierda a derecha      │
├──────────────┼───────────────────────────────────────────┼─────────────────────────────┤
│ 5. Asignación│ = , += , -= , *= , /= , %=                │ De derecha a izquierda      │
└──────────────┴───────────────────────────────────────────┴─────────────────────────────┘
```

#### A. Las dos caras de la conversión de tipos en memoria

```
 [ byte (1B) ] ──► [ short (2B) ] ──► [ int (4B) ] ──► [ long (8B) ] ──► [ double (8B) ]
 ◄───────────────────────────────────────────────────────────────────────────────────────
   ENSANCHAMIENTO (Widening) -> Automático / Seguro    ESTRECHAMIENTO (Narrowing) -> Casting manual
```

1. **Conversión Implícita (Ensanchamiento / *Widening*):**
    * Ocurre automáticamente cuando se asigna o combina un tipo de menor tamaño en uno de mayor capacidad.
    * No existe riesgo de pérdida de datos.
    * *Ejemplo:*
      ```java
      int contador = 5;
      double decimal = contador; // Java lo convierte de forma transparente a 5.0
      ```

2. **Conversión Explícita (Estrechamiento / *Narrowing / Casting*):**
    * Es obligatoria cuando forzamos al compilador a meter un valor de mayor tamaño o precisión dentro de una variable de menor rango.
    * **Existe riesgo evidente de pérdida de información:**
        * Al pasar de `double` a `int`, los decimales se truncan (se eliminan, no se redondean).
        * Al pasar a un tipo con menos bytes, si el número supera el rango admitido, se produce un desbordamiento (*overflow*).
    * **Sintaxis del Casting:** Se escribe el tipo destino entre paréntesis inmediatamente antes del valor o variable:
      ```java
      double notaExacta = 9.87;
      int notaEntera = (int) notaExacta; // notaEntera contendrá 9 (pierde los decimales)
      ```

#### B. La solución al error de Pau mediante Casting en divisiones
Para solucionar el cálculo del porcentaje de asistencia del IES El Caminàs, forzamos que al menos uno de los operandos enteros sea tratado como `double` antes de dividir:

```java
// Opción A: Casting explícito sobre una variable
double porcentaje = ((double) alumnosPresentes / alumnosMatriculados) * 100.0;

// Opción B: Multiplicar primero por un literal decimal
double porcentaje2 = (alumnosPresentes * 100.0) / alumnosMatriculados;
```
En ambos casos, Java promociona toda la operación a coma flotante de 64 bits y el resultado es el **`95.0 %`** correcto.

---

### 3. Andamiaje didáctico paso a paso: PSeInt $\rightarrow$ Java

Modelamos el cálculo de ratios y porcentajes con conversiones controladas.

#### Paso 1. Diseño algorítmico en PSeInt (`pr/pseudocodigo/dia7_casting_porcentajes.psc`)
```psc
Algoritmo Dia7CastingPorcentajes
    Definir presentes, capacidadTotal Como Entero
    Definir ratioOcupacion Como Real
    Definir ratioTruncadoEntero Como Entero
    
    Escribir "=== CONTROL DE AFORO: IES EL CAMINAS ==="
    Escribir "Introduce los alumnos que han entrado:"
    Leer presentes
    
    Escribir "Introduce el aforo maximo permitido del vestibulo:"
    Leer capacidadTotal
    
    // Forzamos calculo real multiplicando por 100.0
    ratioOcupacion <- (presentes * 100.0) / capacidadTotal
    ratioTruncadoEntero <- trunc(ratioOcupacion)
    
    Escribir "----------------------------------------"
    Escribir "Tasa exacta de ocupacion: ", ratioOcupacion, " %"
    Escribir "Tasa entera (para panel LED): ", ratioTruncadoEntero, " %"
FinAlgoritmo
```

#### Paso 2. Traducción inmediata a Java en IntelliJ IDEA (`pr/src/Dia7CastingPorcentajes.java`)
```java
/**
 * Módulo: Programación (PR) - Sprint 1 (RA1)
 * Caso Guía: Precedencia de operadores y casting explícito.
 * @author AzaharTech Consulting
 */
import java.util.Scanner;

public class Dia7CastingPorcentajes {
    public static void main(String[] args) {
        Scanner teclado = new Scanner(System.in);
        
        // 1. Declaración de variables
        int presentes;
        int capacidadTotal;
        double ratioOcupacion;
        int ratioTruncadoEntero;
        
        // 2. Entrada de datos interactiva
        System.out.println("=== CONTROL DE AFORO: IES EL CAMINAS ===");
        System.out.print("Introduce los alumnos que han entrado: ");
        presentes = teclado.nextInt();
        
        System.out.print("Introduce el aforo maximo permitido: ");
        capacidadTotal = teclado.nextInt();
        
        // 3. Procesamiento con casting explícito (double) para evitar división a 0
        ratioOcupacion = ((double) presentes / capacidadTotal) * 100.0;
        
        // Casting explícito a (int) para eliminar decimales deliberadamente
        ratioTruncadoEntero = (int) ratioOcupacion;
        
        // 4. Salida por consola
        System.out.println("----------------------------------------");
        System.out.println("Tasa exacta de ocupacion: " + ratioOcupacion + " %");
        System.out.println("Tasa entera (para panel LED): " + ratioTruncadoEntero + " %");
        
        teclado.close();
    }
}
```

---

### 4. Laboratorio práctico: Aplicación a tu proyecto propio

#### Objetivo de la sesión
Identificar en tu proyecto propio de la bolsa de proyectos una fórmula que combine magnitudes enteras y deba arrojar un resultado de precisión decimal (porcentajes, tasas, precios unitarios calculados o ratios de ocupación), aplicando casting explícito `(double)` para garantizar la exactitud matemática.

#### Instrucciones paso a paso
1. Crea el archivo `pr/pseudocodigo/dia7_proyecto_propio.psc`.
2. Diseña el cálculo asegurando que intervienen al menos dos variables de entrada enteras.
3. Crea la clase Java `pr/src/Dia7ProyectoPropio.java`.
4. Implementa el cálculo aplicando `(double)` en la división.
5. Imprime el resultado exacto con decimales y, a continuación, aplica un segundo casting `(int)` para mostrar el valor truncado.
6. Verifica con la calculadora que el resultado impreso por pantalla coincide exactamente con la operación matemática real.

---
---

## DÍA 8 (Jueves, 24 de septiembre de 2026 — 2 horas lectivas)
### Bloque: Laboratorio de integración: Reto 1 (Paso 2: Motor de cálculo secuencial y conversiones de tipo)
* **Distribución horaria:** 30 minutos de modelado de calidad + 90 minutos de codificación autónoma tutelada.
* **Criterios de Evaluación vinculados:** RA1.a, RA1.b, RA1.c, RA1.d, RA1.e, RA1.g, RA1.h, RA1.i.

---

### 1. Caso práctico narrativo: La consolidación del motor de cálculo

Es jueves al mediodía en **AzaharTech**. Las dos primeras semanas del Sprint 1 han sentado las bases firmes del módulo de Programación. **Laia Claramunt** convoca a los desarrolladores frente al tablero:

> *«Hoy cerramos formalmente la Semana 2. En la Semana 1 creamos el esqueleto del programa con las variables de entrada. Durante estos últimos tres días habéis aprendido a operar con precisión quirúrgica: domináis los operadores compuestos (`+=`), los incrementos (`++`), la descomposición con módulo (`%`), la precedencia con paréntesis y el casting.*
>
> *La tarea de hoy consiste en ensamblar el **motor de cálculo matemático y transformación secuencial completo de vuestro proyecto propio**.*
>
> *No debe quedar ni una sola operación ambigua. Al finalizar las dos horas, el código fuente debe estar subido a GitHub con su commit convencional correspondiente»*.

---

### 2. Estándares de calidad y rúbrica técnica exigida para el Paso 2

Para que el código del Reto 1 (Paso 2) sea aceptado por el equipo docente y AzaharTech, debe acreditar los siguientes requisitos técnicos:
1. **Ausencia absoluta de bifurcaciones o bucles:** El código debe ser estrictamente secuencial, demostrando que se domina la algorítmica matemática antes de entrar en condicionales.
2. **Casting explícito justificado:** Al menos una división entre variables enteras debe estar promocionada a decimal mediante casting `(double)`.
3. **Uso del operador módulo (`%`):** El algoritmo debe incorporar al menos una descomposición o cálculo de residuo.
4. **Protección de precedencia:** Todas las fórmulas complejas deben utilizar paréntesis `()` explícitos.
5. **Comentarios de bloque:** Cada cálculo relevante debe incluir un comentario explicativo de una línea (`//`).

---

### 3. Taller de integración guiada: Plantilla del Reto 1 (Paso 2)

#### Paso 1. Algoritmo secuencial avanzado en PSeInt (`pr/pseudocodigo/reto1_calculo.psc`)
```psc
Algoritmo Reto1Calculo
    // 1. Declaración de variables
    Definir codigoTransaccion Como Cadena
    Definir unidadesSolicitadas Como Entero
    Definir precioUnitario Como Real
    Definir capacidadPorLote Como Entero
    
    Definir lotesCompletos, unidadesSobrantes Como Entero
    Definir costeSubtotal, costeImpuesto, costeTotalFinal Como Real
    Definir ratioAprovechamiento Como Real
    
    // 2. Entrada de datos
    Escribir "================================================="
    Escribir "     AZAHARTECH - MOTOR DE CALCULO SECUENCIAL    "
    Escribir "================================================="
    Escribir "Introduce el codigo de operacion:"
    Leer codigoTransaccion
    
    Escribir "Introduce el volumen de unidades a procesar:"
    Leer unidadesSolicitadas
    
    Escribir "Introduce el precio unitario base:"
    Leer precioUnitario
    
    capacidadPorLote <- 12 // Capacidad fija de empaquetado por lote
    
    // 3. Cálculos aritméticos y descomposición
    lotesCompletos <- trunc(unidadesSolicitadas / capacidadPorLote)
    unidadesSobrantes <- unidadesSolicitadas MOD capacidadPorLote
    
    costeSubtotal <- unidadesSolicitadas * precioUnitario
    costeImpuesto <- costeSubtotal * 0.21 // IVA 21%
    costeTotalFinal <- costeSubtotal + costeImpuesto
    
    // Ratio con precisión decimal
    ratioAprovechamiento <- ((unidadesSolicitadas - unidadesSobrantes) * 100.0) / unidadesSolicitadas
    
    // 4. Emisión de informe estructurado
    Escribir "-------------------------------------------------"
    Escribir "DESGLOSE DE OPERACION: " + codigoTransaccion
    Escribir "Lotes completos empaquetados: ", lotesCompletos
    Escribir "Unidades sueltas restantes:   ", unidadesSobrantes
    Escribir "Coste base subtotal:          ", costeSubtotal, " euros."
    Escribir "Impuesto aplicado (21%):      ", costeImpuesto, " euros."
    Escribir "Coste final liquidado:        ", costeTotalFinal, " euros."
    Escribir "Porcentaje aprovechamiento:   ", ratioAprovechamiento, " %"
    Escribir "================================================="
FinAlgoritmo
```

#### Paso 2. Implementación completa en Java (`pr/src/Reto1Calculo.java`)
```java
/**
 * Proyecto: [Nombre de tu Proyecto Elegido de la Bolsa de Proyectos]
 * Módulo: Programación (PR) - Sprint 1 (RA1)
 * 
 * Descripción: Motor secuencial de cálculo matemático avanzado,
 * descomposición con operador módulo y casting explícito de tipos.
 * 
 * @author [Tus Apellidos, Tu Nombre]
 * @version 1.0 (Semana 2 - Septiembre 2026)
 */
import java.util.Scanner;

public class Reto1Calculo {
    public static void main(String[] args) {
        Scanner teclado = new Scanner(System.in);
        
        // 1. Declaración de variables
        String codigoTransaccion;
        int unidadesSolicitadas;
        double precioUnitario;
        int capacidadPorLote = 12; // Capacidad estándar por lote
        
        int lotesCompletos;
        int unidadesSobrantes;
        double costeSubtotal;
        double costeImpuesto;
        double costeTotalFinal;
        double ratioAprovechamiento;
        
        // 2. Entrada de datos
        System.out.println("=================================================");
        System.out.println("     AZAHARTECH - MOTOR DE CÁLCULO SECUENCIAL    ");
        System.out.println("=================================================");
        System.out.print("Introduce el código de operación: ");
        codigoTransaccion = teclado.nextLine();
        
        System.out.print("Introduce el volumen de unidades a procesar: ");
        unidadesSolicitadas = teclado.nextInt();
        
        System.out.print("Introduce el precio unitario base: ");
        precioUnitario = teclado.nextDouble();
        
        // 3. Procesamiento y fórmulas matemáticas
        // División entera y operador resto (%)
        lotesCompletos = unidadesSolicitadas / capacidadPorLote;
        unidadesSobrantes = unidadesSolicitadas % capacidadPorLote;
        
        // Operaciones aritméticas combinadas
        costeSubtotal = unidadesSolicitadas * precioUnitario;
        costeImpuesto = costeSubtotal * 0.21; // 21% de IVA
        costeTotalFinal = costeSubtotal + costeImpuesto;
        
        // Casting explícito a (double) para garantizar división decimal precisa
        ratioAprovechamiento = (((double)(unidadesSolicitadas - unidadesSobrantes)) / unidadesSolicitadas) * 100.0;
        
        // 4. Salida de resultados por consola
        System.out.println("-------------------------------------------------");
        System.out.println("DESGLOSE DE OPERACIÓN: " + codigoTransaccion);
        System.out.println("Lotes completos empaquetados: " + lotesCompletos);
        System.out.println("Unidades sueltas restantes:   " + unidadesSobrantes);
        System.out.println("Coste base subtotal:          " + costeSubtotal + " euros.");
        System.out.println("Impuesto aplicado (21%):      " + costeImpuesto + " euros.");
        System.out.println("Coste final liquidado:        " + costeTotalFinal + " euros.");
        System.out.println("Porcentaje aprovechamiento:   " + ratioAprovechamiento + " %");
        System.out.println("=================================================");
        
        teclado.close();
    }
}
```

---

### 4. Cierre formal en Git y sincronización con GitHub

1. En IntelliJ IDEA, abre la terminal integrada (`Alt + F12`) o la pestaña **Commit** (`Ctrl + K`).
2. Verifica el estado del repositorio mediante:
   ```bash
   git status
   ```
3. Comprueba que aparecen los archivos `pr/pseudocodigo/reto1_calculo.psc` y `pr/src/Reto1Calculo.java`.
4. Añade los cambios y realiza el commit convencional:
   ```bash
   git add pr/
   git commit -m "feat(pr): implementar formulas matematicas y conversiones de tipo en reto 1"
   git push
   ```
5. Accede a tu repositorio remoto en GitHub y comprueba que ambos archivos están correctamente registrados con su historial limpio.

---

### Balance pedagógico de la Semana 2 de Programación
Al concluir estas 8 horas lectivas (Días 5 al 8):
* Dominas los **operadores de asignación compuesta** y los **operadores de incremento y decremento (`++`, `--`)**, conociendo los efectos del pre y post-incremento.
* Comprendes la diferencia sustancial entre **división entera** y **división real**, y aplicas con maestría el **operador módulo (`%`)** para descomponer magnitudes.
* Respetas la **jerarquía de operadores** utilizando paréntesis defensivos `()`.
* Gobiernas las **conversiones de tipo implícitas y explícitas (*casting*)**, impidiendo la pérdida de precisión decimal en cálculos críticos.
* El motor matemático del **Reto 1 de tu proyecto propio** está programado, probado y versionado en GitHub, **100 % alineado con los contenidos y criterios del RA1**.

# MÓDULO PROFESIONAL: PROGRAMACIÓN (PR)

## SPRINT 1. Algorítmica y fundamentos de programación (3 semanas | 24 horas)

---

# SEMANA 3: CONSTANTES, LITERALES, SALIDA FORMATEADA (PRINTF) Y CIERRE DEL PROGRAMA SECUENCIAL (8 HORAS)

---

## DÍA 9 (Lunes, 28 de septiembre de 2026 — 2 horas lectivas)
### Bloque: Constantes inmutables (`final`), convención `UPPER_SNAKE_CASE`, literales tipados y eliminación de números mágicos
* **Distribución horaria:** 45 minutos de teoría conceptual + 45 minutos de andamiaje guiado (PSeInt $\rightarrow$ Java) + 30 minutos de laboratorio individual.
* **Criterios de Evaluación vinculados:** RA1.a, RA1.d, RA1.e, RA1.f.

---

### 1. Caso práctico narrativo: La caza de los «números mágicos» en AzaharTech

Es lunes 28 de septiembre a primera hora. La célula de desarrollo de **AzaharTech** encara la última semana del Sprint 1. En el proyector de la sala técnica, **Alba Torres** ha abierto el código fuente del terminal del **IES El Caminàs** y subraya varias líneas con un recuadro rojo:

```java
double costeFinal = totalHoras * 1.21;
int tiempoSegundos = minutosEntrada * 60;
String codigo = "CAMINAS-" + idAlumno;
```

Alba se gira hacia **Pau Ferrer** y hacia el estudiante:
> *«Fijaos en esos valores: `1.21`, `60`, `"CAMINAS"`. En la jerga de la ingeniería del software los llamamos **números mágicos (*magic numbers*) o literales huérfanos**: valores fijos incrustados directamente en mitad de las operaciones matemáticas.*
>
> *¿Qué ocurre si la dirección del IES El Caminàs nos pide mañana cambiar el prefijo del centro a `"CAMINAS-26"`? ¿O qué pasa si el IVA de un servicio se actualiza por normativa legal? Tendríamos que rastrear cientos de líneas de código buscando ese valor a mano, arriesgándonos a cambiar un `60` que era de minutos por un `60` que correspondía a otra cosa.*
>
> *En AzaharTech aplicamos un estándar innegociable: **ningún valor fijo vive en mitad de una fórmula**. Todo dato inmutable debe ser extraído, nombrado y protegido como una **constante formal con la palabra clave `final`** al inicio de la clase»*.

---

### 2. Fundamento teórico: Inmutabilidad, constantes y literales tipados

```
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                        VARIABLE FRENTE A CONSTANTE EN LA MEMORIA RAM                   │
├─────────────────────┬───────────────────────────┬──────────────────────────────────────┤
│ Concepto            │ Declaración en Java       │ Comportamiento en tiempo de ejecución│
├─────────────────────┼───────────────────────────┼──────────────────────────────────────┤
│ Variable mutable    │ int intentos = 3;         │ Su valor puede cambiar con `=`.      │
├─────────────────────┼───────────────────────────┼──────────────────────────────────────┤
│ Constante inmutable │ final int MAX_INTENTOS = 3│ El compilador bloquea cualquier      │
│                     │                           │ intento de reasignación posterior.   │
└─────────────────────┴───────────────────────────┴──────────────────────────────────────┘
```

#### A. La palabra reservada `final` en Java
En Java, una **constante** se define anteponiendo el modificador **`final`** a la declaración del tipo de dato:

```java
final String NOMBRE_INSTITUTO = "IES El Caminàs";
final int SEGUNDOS_POR_MINUTO = 60;
final double FACTOR_IVA = 0.21;
```

* **Garantía del compilador:** Si por descuido escribes `SEGUNDOS_POR_MINUTO = 100;` en cualquier otra línea del programa, el compilador detendrá el proceso de construcción con el error: *«Cannot assign a value to final variable»*.
* **Optimización de rendimiento:** La Máquina Virtual de Java (JVM) optimiza las constantes inmutables en memoria, sustituyendo su referencia directamente por el valor en tiempo de compilación.

#### B. Convención de nomenclatura: `UPPER_SNAKE_CASE`
Para distinguir visualmente y al instante qué identificadores son variables y cuáles son constantes:
* Se escriben **íntegramente en letras MAYÚSCULAS**.
* Las palabras compuestas se separan mediante **guiones bajos (`_`)**.
* *Ejemplos correctos:* `CAPACIDAD_MAXIMA`, `PRECIO_UNITARIO_BASE`, `PREFIJO_TERMINAL`.

#### C. Literales numéricos tipados y sufijos
Un literal es la escritura explícita de un dato en el código fuente. Java asigna tipos por defecto a los literales, lo que exige conocer sus sufijos:
1. **Literales enteros largos (`long`):** Por defecto, Java interpreta cualquier número entero (*ej. `2000`*) como un `int` de 32 bits. Si el número supera el rango de $2.147$ millones, se debe añadir el sufijo **`L`** al final (*ej. `5000000000L`*).
2. **Literales decimales simples (`float`):** Por defecto, cualquier número con coma (*ej. `3.14`*) se interpreta como `double` (64 bits). Para forzarlo a `float` (32 bits), se añade el sufijo **`F`** (*ej. `3.14F`*).
3. **Notación científica:** Java permite escribir números con potencias de diez usando la letra `E` (*ej. `1.5E3` equivale a $1.5 \times 10^3 = 1500.0$*).

---

### 3. Andamiaje didáctico paso a paso: PSeInt $\rightarrow$ Java

Modelamos la parametrización de constantes corporativas del IES El Caminàs paso a paso.

#### Paso 1. Diseño algorítmico en PSeInt (`pr/pseudocodigo/dia9_constantes.psc`)
```psc
Algoritmo Dia9Constantes
    // 1. Declaración conceptual de constantes de configuración
    Definir PREFIJO_CENTRO Como Cadena
    Definir SEGUNDOS_POR_MINUTO Como Entero
    Definir COSTE_EXPEDICION_CARNET Como Real
    
    // Asignación inicial que no debe variar
    PREFIJO_CENTRO <- "CAMINAS"
    SEGUNDOS_POR_MINUTO <- 60
    COSTE_EXPEDICION_CARNET <- 4.50
    
    // 2. Variables de entrada
    Definir dniEstudiante Como Cadena
    Definir minutosLectura Como Entero
    Definir segundosCalculados Como Entero
    Definir tokenGenerado Como Cadena
    
    Escribir "=== AZAHARTECH: PARAMETRIZACION DE CONSTANTES ==="
    Escribir "Introduce el DNI del alumno:"
    Leer dniEstudiante
    Escribir "Introduce los minutos transcurridos:"
    Leer minutosLectura
    
    // 3. Procesamiento usando constantes inmutables
    segundosCalculados <- minutosLectura * SEGUNDOS_POR_MINUTO
    tokenGenerado <- PREFIJO_CENTRO + "-" + dniEstudiante
    
    // 4. Emisión de datos
    Escribir "------------------------------------------------"
    Escribir "Entidad emisora:      ", PREFIJO_CENTRO
    Escribir "Token generado:       ", tokenGenerado
    Escribir "Segundos calculados:  ", segundosCalculados
    Escribir "Tasa carnet asociada: ", COSTE_EXPEDICION_CARNET, " EUR"
FinAlgoritmo
```

#### Paso 2. Traducción inmediata a Java en IntelliJ IDEA (`pr/src/Dia9Constantes.java`)
```java
/**
 * Módulo: Programación (PR) - Sprint 1 (RA1)
 * Caso Guía: Declaración de constantes con 'final' y eliminación de números mágicos.
 * @author AzaharTech Consulting
 */
import java.util.Scanner;

public class Dia9Constantes {
    public static void main(String[] args) {
        // -------------------------------------------------------------
        // 1. CONSTANTES INMUTABLES DEL SISTEMA (UPPER_SNAKE_CASE)
        // -------------------------------------------------------------
        final String PREFIJO_CENTRO = "CAMINAS";
        final int SEGUNDOS_POR_MINUTO = 60;
        final double COSTE_EXPEDICION_CARNET = 4.50;
        final long ID_SESION_GLOBAL = 9500000000L; // Literal long con sufijo L
        
        // -------------------------------------------------------------
        // 2. VARIABLES DE MEMORIA
        // -------------------------------------------------------------
        Scanner teclado = new Scanner(System.in);
        String dniEstudiante;
        int minutosLectura;
        int segundosCalculados;
        String tokenGenerado;
        
        // -------------------------------------------------------------
        // 3. ENTRADA DE DATOS
        // -------------------------------------------------------------
        System.out.println("=== AZAHARTECH: PARAMETRIZACIÓN DE CONSTANTES ===");
        System.out.print("Introduce el DNI del alumno: ");
        dniEstudiante = teclado.nextLine();
        
        System.out.print("Introduce los minutos transcurridos: ");
        minutosLectura = teclado.nextInt();
        
        // -------------------------------------------------------------
        // 4. PROCESAMIENTO SECUENCIAL USANDO CONSTANTES
        // -------------------------------------------------------------
        segundosCalculados = minutosLectura * SEGUNDOS_POR_MINUTO;
        tokenGenerado = PREFIJO_CENTRO + "-" + dniEstudiante;
        
        // -------------------------------------------------------------
        // 5. SALIDA DE DATOS
        // -------------------------------------------------------------
        System.out.println("------------------------------------------------");
        System.out.println("Entidad emisora:      " + PREFIJO_CENTRO);
        System.out.println("Sesión auditoría ID:  " + ID_SESION_GLOBAL);
        System.out.println("Token generado:       " + tokenGenerado);
        System.out.println("Segundos calculados:  " + segundosCalculados);
        System.out.println("Tasa carnet asociada: " + COSTE_EXPEDICION_CARNET + " EUR");
        
        teclado.close();
    }
}
```

---

### 4. Laboratorio práctico: Aplicación a tu proyecto propio

#### Objetivo de la sesión
Identificar en tu proyecto propio de la bolsa de proyectos todos los valores fijos del dominio del problema y refactorizar el código para declararlos como **constantes `final`** al inicio de la clase.

#### Ejemplos según la temática de tu proyecto:
* *Proyecto logístico/almacén:* `final int CAPACIDAD_MAXIMA_PALET = 120;`, `final double IVA_TRANSPORTE = 0.21;`.
* *Proyecto de reservas/hostelería:* `final String NOMBRE_CADENA = "Gran Hotel Castellón";`, `final int HORA_CHECKOUT = 12;`.
* *Proyecto financiero/facturación:* `final double RECARGO_EXPRESS = 15.0;`, `final String MONEDA_BASE = "EUR";`.

#### Instrucciones paso a paso
1. Abre tu proyecto en IntelliJ IDEA y localiza tu archivo `pr/pseudocodigo/dia9_proyecto_propio.psc`.
2. Define al menos 3 constantes representativas utilizando identificadores en mayúsculas.
3. En `pr/src/`, crea la clase `Dia9ProyectoPropio.java`.
4. Traduce el algoritmo a Java empleando la palabra clave `final`.
5. Intenta provocar un error deliberado de compilación asignando un nuevo valor a una de las constantes en mitad del código (`NOMBRE_CONSTANTE = otroValor;`). Observa cómo IntelliJ resalta la línea en rojo impidiendo la compilación.
6. Corrige la línea, compila y ejecuta comprobando que los valores fijos se integran perfectamente en las operaciones matemáticas.

---
---

## DÍA 10 (Martes, 29 de septiembre de 2026 — 2 horas lectivas)
### Bloque: Caracteres no imprimibles, secuencias de escape (`\n`, `\t`, `\"`, `\\`) y maquetación de consola
* **Distribución horaria:** 45 minutos de teoría conceptual + 45 minutos de andamiaje guiado (PSeInt $\rightarrow$ Java) + 30 minutos de laboratorio individual.
* **Criterios de Evaluación vinculados:** RA1.a, RA1.d, RA1.f.

---

### 1. Caso práctico narrativo: El recibo apelotonado de la pantalla de acceso

Es martes por la mañana. **Pau Ferrer** muestra entusiasmado el resumen de fichaje que ha programado. Al ejecutarlo en la consola de IntelliJ, la salida aparece en un único bloque de texto apelotonado:

```text
======================= IES EL CAMINAS ======================= Acceso registrado: Alumno: Laura Vidal Grupo: 1DAM Sala: Aula 104 Nota: Recuerde escanear a la salida
```

Pau comenta:
> *«Para separar esto he tenido que escribir diez `System.out.println()` seguidos con comillas vacías. Además, si quiero que la palabra 'Aula 104' aparezca entre comillas dobles, Java me da error porque piensa que estoy cerrando el texto antes de tiempo»*.

**Laia Claramunt** y **Alba Torres** le muestran la barra invertida (**`\`**) en el teclado:
> *«En programación, los caracteres que no se ven (como el salto de línea o el salto de tabulación) y los caracteres reservados del lenguaje (como las comillas dobles) se representan mediante **secuencias de escape**. Te permiten maquetar salidas limpias, recibos alineados en columnas y cuadros de diálogo en consola con una fracción del código»*.

---

### 2. Fundamento teórico: Secuencias de escape y caracteres de control

```
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                        SECUENCIAS DE ESCAPE UNIVERSALES EN JAVA                        │
├──────────────┬────────────────────────┬────────────────────────────────────────────────┤
│ Secuencia    │ Nombre técnico         │ Acción que produce en la salida de consola     │
├──────────────┼────────────────────────┼────────────────────────────────────────────────┤
│ \n           │ Salto de línea         │ Desplaza el cursor al inicio de la línea       │
│              │ (Newline / Line Feed)  │ siguiente (equivale a pulsar Enter).           │
├──────────────┼────────────────────────┼────────────────────────────────────────────────┤
│ \t           │ Tabulador horizontal   │ Desplaza el cursor a la siguiente parada de    │
│              │ (Horizontal Tab)       │ tabulación (alinea datos en columnas).         │
├──────────────┼────────────────────────┼────────────────────────────────────────────────┤
│ \"           │ Comilla doble escapada │ Imprime el carácter literal " sin cerrar la    │
│              │                        │ cadena de texto del código fuente.             │
├──────────────┼────────────────────────┼────────────────────────────────────────────────┤
│ \'           │ Comilla simple escapada│ Permite representar el carácter ' dentro de un │
│              │                        │ literal de tipo char: '\''                     │
├──────────────┼────────────────────────┼────────────────────────────────────────────────┤
│ \\           │ Barra invertida        │ Imprime el símbolo literal de la barra \ sin   │
│              │                        │ activar una secuencia de escape.               │
└──────────────┴────────────────────────┴────────────────────────────────────────────────┘
```

#### A. El carácter de escape: La barra invertida (`\`)
Cuando el compilador de Java encuentra una barra invertida dentro de una cadena de texto, no la interpreta como una letra común; activa un mecanismo de traducción inmediata que convierte el carácter siguiente en un comando de control de bajo nivel.

#### B. Maquetación tabular con tabuladores (`\t`)
El tabulador horizontal divide la línea de la consola en columnas virtuales equidistantes (habitualmente de 4 u 8 espacios). Colocar un `\t` entre los datos permite alinear etiquetas y valores automáticamente sin tener que contar espacios en blanco manualmente con la barra espaciadora:

```java
System.out.println("DNI:\t" + dni + "\tGRUPO:\t" + grupo);
```

#### C. Inclusión de rutas de archivos y citas en textos
* Si intentas escribir una ruta de Windows en Java: `String ruta = "C:\nuevos\trabajos";`, el compilador dará error porque `\n` lo entenderá como salto de línea y `\t` como tabulador.
* La forma correcta en Java exige **escapar la barra invertida duplicándola**:
  ```java
  String rutaCorrecta = "C:\\nuevos\\trabajos\\datos.txt";
  ```

---

### 3. Andamiaje didáctico paso a paso: PSeInt $\rightarrow$ Java

Modelamos la maquetación del ticket de acceso del IES El Caminàs utilizando secuencias de escape.

#### Paso 1. Diseño algorítmico en PSeInt (`pr/pseudocodigo/dia10_escapes.psc`)
```psc
Algoritmo Dia10Escapes
    Definir nombreEstudiante, dniEstudiante, nombreCentro Como Cadena
    Definir tiempoAccesoSegundos Como Entero
    
    nombreCentro <- "IES El Caminas (Castellon)"
    nombreEstudiante <- "Vidal Soriano, Laura"
    dniEstudiante <- "45123789K"
    tiempoAccesoSegundos <- 185
    
    // Salida estructurada simulando saltos y tabulaciones
    Escribir "=========================================================="
    Escribir "ENTIDAD:\t", nombreCentro
    Escribir "PROTOCOLO:\t\"Escaneo Directo QR en Pantalla\""
    Escribir "=========================================================="
    Escribir ""
    Escribir "DATOS DEL ACCESO:"
    Escribir "DNI:\t\t", dniEstudiante
    Escribir "ALUMNO:\t\t", nombreEstudiante
    Escribir "ESTANCIA:\t", tiempoAccesoSegundos, " seg."
    Escribir ""
    Escribir "NOTIFICACION:\tAcceso verificado.\nRuta servidor:\tC:\\caminas\\logs"
FinAlgoritmo
```

#### Paso 2. Traducción inmediata a Java en IntelliJ IDEA (`pr/src/Dia10Escapes.java`)
```java
/**
 * Módulo: Programación (PR) - Sprint 1 (RA1)
 * Caso Guía: Uso de secuencias de escape (\n, \t, \", \\) para maquetación en consola.
 * @author AzaharTech Consulting
 */
public class Dia10Escapes {
    public static void main(String[] args) {
        // Constantes corporativas
        final String NOMBRE_CENTRO = "IES El Caminàs (Castellón)";
        final String RUTA_SISTEMA = "C:\\caminas\\terminal\\logs";
        
        // Variables de datos
        String nombreEstudiante = "Vidal Soriano, Laura";
        String dniEstudiante = "45123789K";
        int tiempoAccesoSegundos = 185;
        
        // Impresión maquetada con un único bloque estructurado usando escapes
        System.out.println("==========================================================");
        System.out.println("ENTIDAD:\t" + NOMBRE_CENTRO);
        System.out.println("PROTOCOLO:\t\"Escaneo Directo QR en Pantalla\"");
        System.out.println("UBICACIÓN:\tVestíbulo Principal \\ Edificio A");
        System.out.println("==========================================================\n");
        
        System.out.println("DATOS DEL ACCESO:");
        System.out.println("DNI:\t\t" + dniEstudiante);
        System.out.println("ALUMNO:\t\t" + nombreEstudiante);
        System.out.println("ESTANCIA:\t" + tiempoAccesoSegundos + " seg.\n");
        
        System.out.println("ESTADO:\t\t[AUTORIZADO]");
        System.out.println("AUDITORÍA:\tRegistro volcado en: " + RUTA_SISTEMA);
    }
}
```

---

### 4. Laboratorio práctico: Aplicación a tu proyecto propio

#### Objetivo de la sesión
Diseñar la maquetación visual del informe o recibo de tu proyecto propio en la consola utilizando saltos de línea (`\n`), tabuladores de alineación (`\t`), citas entre comillas (`\"`) y barras invertidas (`\\`).

#### Instrucciones paso a paso
1. Abre tu proyecto en IntelliJ IDEA y crea el archivo `pr/pseudocodigo/dia10_proyecto_propio.psc`.
2. Escribe el algoritmo organizando la salida en bloques visuales separados por líneas divisorias (`=====`).
3. Crea la clase Java `pr/src/Dia10ProyectoPropio.java`.
4. Implementa el código en Java utilizando secuencias de escape para alinear etiquetas como *Identificador*, *Fecha*, *Subtotal* y *Estado*.
5. Ejecuta la clase y comprueba que la consola muestra columnas limpias sin desalineaciones tipográficas.

---
---

## DÍA 11 (Miércoles, 30 de septiembre de 2026 — 2 horas lectivas)
### Bloque: Salida formateada profesional con `System.out.printf()` y especificadores de formato
* **Distribución horaria:** 45 minutos de teoría conceptual + 45 minutos de andamiaje guiado (PSeInt $\rightarrow$ Java) + 30 minutos de laboratorio individual.
* **Criterios de Evaluación vinculados:** RA1.a, RA1.b, RA1.e.

---

### 1. Caso práctico narrativo: La presentación del informe ante jefatura

Es miércoles por la mañana. En la sala de reuniones de **AzaharTech**, **Laia Claramunt** proyecta el informe financiero y de aforo que el sistema ha generado para el **IES El Caminàs**.

Pau Ferrer lo ha programado con `System.out.println()` y el resultado en pantalla es el siguiente:
```text
Alumno: Garcia, Juan Tiempo: 2.3333333333333335 min Tasa fiabilidad: 98.756241 % Subtotal: 4.5 euros
Alumno: Lopez, Ana Tiempo: 12.5 min Tasa fiabilidad: 100.0 % Subtotal: 12.0 euros
```

Laia niega con la cabeza y le pide a Alba que tome el teclado:
> *«Pau, si presentamos este informe al equipo directivo del instituto, no causaremos una buena impresión. Los decimales están descontrolados: un alumno tiene quince decimales y otro solo uno. Además, como el nombre 'Garcia, Juan' tiene distinta longitud que 'Lopez, Ana', los números de la columna de la derecha se van desplazando hacia adelante y hacia atrás.*
>
> *En Java profesional, para emitir tablas, recibos e informes utilizamos **`System.out.printf()`**. Nos permite definir una plantilla exacta fijando el ancho de cada columna, la alineación a izquierda o derecha y el número riguroso de decimales con redondeo automático. Hoy aprenderemos a gobernar el formateo de datos como auténticos ingenieros»*.

---

### 2. Fundamento teórico: La instrucción `System.out.printf()` y los especificadores

```
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                        ANATOMÍA DE UNA INSTRUCCIÓN PRINTF                              │
├────────────────────────────────────────────────────────────────────────────────────────┤
│ System.out.printf( "Estudiante: %-20s | Nota: %5.2f | Id: %04d %n", nombre, nota, id ); │
│                     └─────────────────────────────────────────┘     └────────────────┘ │
│                                  Cadena de Formato                      Argumentos     │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

El método **`printf`** (de *print formatted*, impresión formateada) requiere como primer parámetro una **cadena de formato** que contiene texto fijo mezclado con **especificadores de formato** (comienzan con `%`). A continuación, se pasa una lista de variables separadas por comas que se insertan en orden dentro de cada especificador.

#### A. Especificadores de tipo fundamentales
* **`%s`:** Para cadenas de texto (`String`).
* **`%d`:** Para números enteros (`byte`, `short`, `int`, `long`). *Proviene de "decimal integer"*.
* **`%f`:** Para números decimales de coma flotante (`float`, `double`).
* **`%c`:** Para caracteres individuales (`char`).
* **`%b`:** Para valores lógicos (`boolean`).
* **`%n`:** Genera un salto de línea limpio e independiente del sistema operativo (en Windows genera `\r\n` y en Linux/macOS genera `\n`).

#### B. Modificadores de anchura, precisión y alineación
La verdadera potencia de `printf` reside en los modificadores intermedios que se colocan entre el `%` y la letra del tipo:

```
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                        MODIFICADORES DE CONTROL EN PRINTF                              │
├───────────────────┬──────────────────────────────────┬─────────────────────────────────┤
│ Patrón            │ Efecto técnico                   │ Ejemplo de salida               │
├───────────────────┼──────────────────────────────────┼─────────────────────────────────┤
│ %.2f              │ Acota a exactamente 2 decimales  │ 2.333333 se imprime como "2.33" │
├───────────────────┼──────────────────────────────────┼─────────────────────────────────┤
│ %10d              │ Reserva 10 caracteres, alineado  │ "        45"                    │
│                   │ a la DERECHA                     │                                 │
├───────────────────┼──────────────────────────────────┼─────────────────────────────────┤
│ %-15s             │ Reserva 15 caracteres, alineado  │ "Laura Vidal    "               │
│                   │ a la IZQUIERDA                   │                                 │
├───────────────────┼──────────────────────────────────┼─────────────────────────────────┤
│ %05d              │ Reserva 5 dígitos rellenando     │ 42 se imprime como "00042"      │
│                   │ con ceros a la izquierda         │                                 │
├───────────────────┼──────────────────────────────────┼─────────────────────────────────┤
│ %%                │ Imprime el símbolo literal de %  │ "95.00 %"                       │
└───────────────────┴──────────────────────────────────┴─────────────────────────────────┘
```

* **Diferencia entre `%f` y `%.2f`:** `%f` por defecto imprime 6 decimales. `%.2f` redondea matemáticamente el número a dos decimales sin alterar el valor de la variable original en la memoria RAM.

---

### 3. Andamiaje didáctico paso a paso: PSeInt $\rightarrow$ Java

Modelamos la generación de la tabla de accesos del IES El Caminàs comparando el diseño básico con el formateo profesional en Java.

#### Paso 1. Diseño algorítmico en PSeInt (`pr/pseudocodigo/dia11_printf.psc`)
```psc
Algoritmo Dia11Printf
    Definir idRegistro1, idRegistro2 Como Entero
    Definir alumno1, alumno2 Como Cadena
    Definir minutosEstancia1, minutosEstancia2 Como Real
    Definir tasa1, tasa2 Como Real
    
    idRegistro1 <- 1
    alumno1 <- "Garcia Perez, Juan"
    minutosEstancia1 <- 2.3333333
    tasa1 <- 98.756
    
    idRegistro2 <- 25
    alumno2 <- "Lopez Sanz, Ana"
    minutosEstancia2 <- 12.500000
    tasa2 <- 100.000
    
    // En PSeInt la alineación es manual y aproximada
    Escribir "=================================================================="
    Escribir "ID    ALUMNO                       TIEMPO (min)   PUNTUALIDAD (%)"
    Escribir "=================================================================="
    Escribir idRegistro1, "     ", alumno1, "           ", redon(minutosEstancia1*100)/100, "          ", redon(tasa1*100)/100, " %"
    Escribir idRegistro2, "    ", alumno2, "             ", redon(minutosEstancia2*100)/100, "         ", redon(tasa2*100)/100, " %"
    Escribir "=================================================================="
FinAlgoritmo
```

#### Paso 2. Traducción inmediata a Java en IntelliJ IDEA (`pr/src/Dia11Printf.java`)
```java
/**
 * Módulo: Programación (PR) - Sprint 1 (RA1)
 * Caso Guía: Salida formateada avanzada y tablas con System.out.printf().
 * @author AzaharTech Consulting
 */
public class Dia11Printf {
    public static void main(String[] args) {
        // Datos del primer registro
        int idRegistro1 = 1;
        String alumno1 = "Garcia Perez, Juan";
        double minutosEstancia1 = 2.3333333;
        double tasa1 = 98.756;
        
        // Datos del segundo registro
        int idRegistro2 = 25;
        String alumno2 = "Lopez Sanz, Ana";
        double minutosEstancia2 = 12.500000;
        double tasa2 = 100.000;
        
        // Cabecera formal
        System.out.println("======================================================================");
        System.out.println("            INFORME OFICIAL DE ACCESOS: IES EL CAMINÀS                ");
        System.out.println("======================================================================");
        
        // Fila de encabezados de columnas perfectamente alineada
        System.out.printf("%-8s %-25s %15s %15s %n", "ID REG", "ESTUDIANTE", "TIEMPO (min)", "FIABILIDAD");
        System.out.println("----------------------------------------------------------------------");
        
        // Fila 1: ID con ceros a la izquierda (%04d), nombre a la izquierda (%-25s), decimales a 2 posiciones (%.2f)
        System.out.printf("#%04d    %-25s %15.2f %14.2f %%%n", idRegistro1, alumno1, minutosEstancia1, tasa1);
        
        // Fila 2: Misma estructura de columnas asegurando alineación vertical perfecta
        System.out.printf("#%04d    %-25s %15.2f %14.2f %%%n", idRegistro2, alumno2, minutosEstancia2, tasa2);
        
        System.out.println("======================================================================");
    }
}
```

---

### 4. Laboratorio práctico: Aplicación a tu proyecto propio

#### Objetivo de la sesión
Construir una tabla o informe formal de salida para tu proyecto de la bolsa de proyectos utilizando `System.out.printf()`, definiendo al menos 3 columnas con anchos fijos y formateando los datos numéricos a 2 decimales con el símbolo `%` o la moneda correspondiente.

#### Instrucciones paso a paso
1. Crea el archivo `pr/pseudocodigo/dia11_proyecto_propio.psc`.
2. Diseña la estructura de datos que mostrará tu informe de salida.
3. Crea la clase Java `pr/src/Dia11ProyectoPropio.java`.
4. Implementa el método `printf()` definiendo:
    * Una columna de identificador o código alineada a la derecha o con ceros a la izquierda (`%05d`).
    * Una columna de texto descriptivo alineada a la izquierda (`%-20s`).
    * Una columna de importe, peso o tiempo alineada a la derecha y acotada a dos decimales (`%10.2f`).
5. Ejecuta el programa introduciendo textos de distintas longitudes y comprueba que las columnas no se desplazan ni se rompen.

---
---

## DÍA 12 (Jueves, 1 de octubre de 2026 — 2 horas lectivas)
### Bloque: Laboratorio de integración final: Programa secuencial completo del Reto 1 y cierre del Sprint 1 en Programación
* **Distribución horaria:** 30 minutos de estándares de documentación + 90 minutos de ensamblaje final tutelado.
* **Criterios de Evaluación vinculados:** RA1.a, RA1.b, RA1.c, RA1.d, RA1.e, RA1.f, RA1.g, RA1.h, RA1.i.

---

### 1. Caso práctico narrativo: El sellado del código en AzaharTech

Es jueves 1 de octubre por la mañana. Mañana viernes concluye formalmente el primer sprint del curso académico. En la oficina de **AzaharTech**, **Laia Claramunt** convoca a toda la célula de desarrollo frente al proyector:

> *«Equipo, hemos completado las 24 horas del Sprint 1 de Programación. Durante estas tres semanas habéis construido vuestra base técnica desde cero: sabéis qué es un algoritmo, cómo declarar variables en memoria, cómo evitar números mágicos mediante constantes `final`, cómo operar sin perder precisión y cómo emitir informes tabulados profesionales con `printf`.*
>
> *Hoy nos queda la tarea más importante: **ensamblar el programa secuencial definitivo del Reto 1 de vuestro proyecto propio**.*
>
> *El código que dejemos listo hoy será la base sobre la que trabajaremos en el Sprint 2 cuando aprendamos a usar objetos predefinidos. Debe compilar con total limpieza, incluir comentarios formales de ingeniería y quedar guardado en `pr/src/Reto1Completo.java` antes de realizar el commit y push final»*.

---

### 2. Estándares de calidad y comentarios profesionales en Java

Antes de congelar la entrega, el código fuente debe incorporar comentarios estructurados bajo dos modalidades oficiales:

```java
// 1. Comentarios de línea única: explican el motivo de una fórmula concreta
int restoSegundos = totalSegundos % 60; // Extrae los segundos que no completan un minuto

/* 2. Comentarios multilínea de bloque:
   Explican el propósito general de un bloque de cálculos
   o la lógica de negocio aplicada. */

/**
 * 3. Comentario formal de cabecera (Javadoc):
 * Documenta la autoría, versión y propósito global de la clase.
 */
```

#### Rúbrica de aceptación del Reto 1 Completo:
1. **100 % Secuencial:** No contiene instrucciones condicionales (`if`, `switch`) ni bucles (`while`, `for`), respetando estrictamente el alcance del **RA1**.
2. **Constantes inmutables:** Toda configuración o valor fijo del sistema está declarado como `final` en mayúsculas (`UPPER_SNAKE_CASE`).
3. **Entrada interactiva:** Captura datos reales mediante la clase `Scanner`.
4. **Fórmulas de precisión:** Utiliza operadores aritméticos, el operador módulo (`%`) y casting explícito `(double)` cuando procede.
5. **Salida formateada:** Emite el informe final mediante `System.out.printf()`.
6. **Higiene:** Indentación uniforme de 4 espacios y cierre del recurso `Scanner.close()`.

---

### 3. Taller de integración guiada: El Programa Secuencial Completo

Cada estudiante ensambla en su espacio de trabajo el programa definitivo adaptado a los datos y reglas de su **proyecto elegido de la bolsa de proyectos**:

#### Paso 1. Algoritmo secuencial completo en PSeInt (`pr/pseudocodigo/reto1_completo.psc`)
```psc
Algoritmo Reto1Completo
    // -------------------------------------------------------------
    // 1. CONSTANTES DEL SISTEMA
    // -------------------------------------------------------------
    Definir NOMBRE_EMPRESA Como Cadena
    Definir PREFIJO_EXPEDIENTE Como Cadena
    Definir CAPACIDAD_LOTE Como Entero
    Definir TASA_GESTION Como Real
    
    NOMBRE_EMPRESA <- "AzaharTech Solutions"
    PREFIJO_EXPEDIENTE <- "EXP-2026"
    CAPACIDAD_LOTE <- 24
    TASA_GESTION <- 0.15 // 15% de gastos de gestión
    
    // -------------------------------------------------------------
    // 2. DECLARACIÓN DE VARIABLES
    // -------------------------------------------------------------
    Definir codigoCliente Como Cadena
    Definir unidadesDemandadas Como Entero
    Definir precioBaseUnitario Como Real
    
    Definir lotesCompletos, unidadesRestantes Como Entero
    Definir costeSubtotal, costeRecargo, costeTotal Como Real
    Definir ratioAprovechamiento Como Real
    
    // -------------------------------------------------------------
    // 3. ENTRADA DE DATOS
    // -------------------------------------------------------------
    Escribir "========================================================="
    Escribir "   ", NOMBRE_EMPRESA, " - GESTION DE RETO 1              "
    Escribir "========================================================="
    Escribir "Introduce el codigo identificativo del cliente:"
    Leer codigoCliente
    
    Escribir "Introduce el volumen de unidades demandadas:"
    Leer unidadesDemandadas
    
    Escribir "Introduce el precio unitario base (EUR):"
    Leer precioBaseUnitario
    
    // -------------------------------------------------------------
    // 4. PROCESAMIENTO MATEMÁTICO SECUENCIAL
    // -------------------------------------------------------------
    // Descomposición con división entera y módulo
    lotesCompletos <- trunc(unidadesDemandadas / CAPACIDAD_LOTE)
    unidadesRestantes <- unidadesDemandadas MOD CAPACIDAD_LOTE
    
    // Cálculos económicos
    costeSubtotal <- unidadesDemandadas * precioBaseUnitario
    costeRecargo <- costeSubtotal * TASA_GESTION
    costeTotal <- costeSubtotal + costeRecargo
    
    // Cálculo porcentual
    ratioAprovechamiento <- ((unidadesDemandadas - unidadesRestantes) * 100.0) / unidadesDemandadas
    
    // -------------------------------------------------------------
    // 5. SALIDA DE DATOS FORMATEADA
    // -------------------------------------------------------------
    Escribir "---------------------------------------------------------"
    Escribir "EXPEDIENTE:       ", PREFIJO_EXPEDIENTE, "-", codigoCliente
    Escribir "LOTES COMPLETOS:  ", lotesCompletos, " (de ", CAPACIDAD_LOTE, " uds)"
    Escribir "UNIDADES SUELTAS: ", unidadesRestantes
    Escribir "SUBTOTAL BASE:    ", costeSubtotal, " EUR"
    Escribir "GASTOS GESTION:   ", costeRecargo, " EUR"
    Escribir "TOTAL OPERACION:  ", costeTotal, " EUR"
    Escribir "APROVECHAMIENTO:  ", redon(ratioAprovechamiento * 100) / 100, " %"
    Escribir "========================================================="
FinAlgoritmo
```

#### Paso 2. Implementación final en Java (`pr/src/Reto1Completo.java`)
```java
/**
 * Proyecto: [Nombre del Proyecto Elegido de la Bolsa de Proyectos]
 * Consultora: AzaharTech Software Consulting
 * Módulo: Programación (PR) - Sprint 1 (RA1)
 * 
 * Descripción: Programa secuencial integral que captura parámetros de entrada,
 * realiza cálculos aritméticos con división entera y módulo, aplica conversiones
 * de tipo (casting) y emite un informe estructurado mediante printf.
 * 
 * @author [Tus Apellidos, Tu Nombre]
 * @version 1.0 (Octubre 2026)
 */
import java.util.Scanner;

public class Reto1Completo {
    public static void main(String[] args) {
        // -------------------------------------------------------------
        // 1. CONSTANTES INMUTABLES DEL SISTEMA (Configuración)
        // -------------------------------------------------------------
        final String NOMBRE_EMPRESA = "AzaharTech Solutions";
        final String PREFIJO_EXPEDIENTE = "EXP-2026";
        final int CAPACIDAD_LOTE = 24;
        final double TASA_GESTION = 0.15; // 15% de gastos operativos
        
        // -------------------------------------------------------------
        // 2. DECLARACIÓN DE VARIABLES DE MEMORIA
        // -------------------------------------------------------------
        Scanner teclado = new Scanner(System.in);
        String codigoCliente;
        int unidadesDemandadas;
        double precioBaseUnitario;
        
        int lotesCompletos;
        int unidadesRestantes;
        double costeSubtotal;
        double costeRecargo;
        double costeTotal;
        double ratioAprovechamiento;
        
        // -------------------------------------------------------------
        // 3. CAPTURA INTERACTIVA DE DATOS
        // -------------------------------------------------------------
        System.out.println("=========================================================");
        System.out.println("   " + NOMBRE_EMPRESA + " - GESTIÓN DE RETO 1            ");
        System.out.println("=========================================================");
        System.out.print("Introduce el código identificativo del cliente: ");
        codigoCliente = teclado.nextLine();
        
        System.out.print("Introduce el volumen de unidades demandadas: ");
        unidadesDemandadas = teclado.nextInt();
        
        System.out.print("Introduce el precio unitario base (EUR): ");
        precioBaseUnitario = teclado.nextDouble();
        
        // -------------------------------------------------------------
        // 4. PROCESAMIENTO SECUENCIAL Y OPERACIONES MATEMÁTICAS
        // -------------------------------------------------------------
        // Descomposición de unidades con división entera y operador módulo
        lotesCompletos = unidadesDemandadas / CAPACIDAD_LOTE;
        unidadesRestantes = unidadesDemandadas % CAPACIDAD_LOTE;
        
        // Operaciones aritméticas y porcentajes
        costeSubtotal = unidadesDemandadas * precioBaseUnitario;
        costeRecargo = costeSubtotal * TASA_GESTION;
        costeTotal = costeSubtotal + costeRecargo;
        
        // Casting explícito a (double) para evitar pérdida de decimales
        ratioAprovechamiento = (((double)(unidadesDemandadas - unidadesRestantes)) / unidadesDemandadas) * 100.0;
        
        // -------------------------------------------------------------
        // 5. SALIDA DE INFORMACIÓN FORMATEADA PROFESIONAL (printf)
        // -------------------------------------------------------------
        System.out.println("\n=========================================================");
        System.out.println("               INFORME ECONÓMICO Y LOGÍSTICO             ");
        System.out.println("=========================================================");
        System.out.printf("EXPEDIENTE:        %s-%s%n", PREFIJO_EXPEDIENTE, codigoCliente);
        System.out.printf("LOTES COMPLETOS:   %04d lotes (capacidad: %d uds)%n", lotesCompletos, CAPACIDAD_LOTE);
        System.out.printf("UNIDADES SUELTAS:  %d unidades%n", unidadesRestantes);
        System.out.println("---------------------------------------------------------");
        System.out.printf("SUBTOTAL BASE:     %12.2f EUR%n", costeSubtotal);
        System.out.printf("GASTOS GESTIÓN:    %12.2f EUR (%.0f%%)%n", costeRecargo, (TASA_GESTION * 100));
        System.out.printf("TOTAL OPERACIÓN:   %12.2f EUR%n", costeTotal);
        System.out.println("---------------------------------------------------------");
        System.out.printf("APROVECHAMIENTO:   %11.2f %%%n", ratioAprovechamiento);
        System.out.println("=========================================================");
        
        // Cierre preventivo del recurso de entrada
        teclado.close();
    }
}
```

---

### 4. Cierre formal en Git y preparación para la evaluación del Sprint 1

1. Abre la terminal integrada de IntelliJ IDEA (`Alt + F12`) o el panel **Commit** (`Ctrl + K`).
2. Comprueba mediante `git status` que los dos archivos finales (`pr/pseudocodigo/reto1_completo.psc` y `pr/src/Reto1Completo.java`) están preparados.
3. Añade los archivos y confirma los cambios aplicando el estándar de **Conventional Commits**:
   ```bash
   git add pr/
   git commit -m "feat(pr): ensamblar programa secuencial completo y cerrar reto 1 del sprint 1"
   git push
   ```
4. Accede a tu repositorio remoto en GitHub desde el navegador y verifica que ambos archivos están correctamente registrados dentro de la carpeta `pr/`.
5. Mañana viernes, durante la sesión de Entornos de Desarrollo, sellarás todo el repositorio con la etiqueta de versión oficial **`v0.1.0-sprint1`**.

---

### Balance curricular final del Sprint 1 de Programación (24 horas lectivas)

```
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                   CUMPLIMIENTO DE CRITERIOS DE EVALUACIÓN DEL RA1                      │
├─────────────────┬──────────────────────────────────────────┬───────────────────────────┤
│ Bloque / Semana │ Criterios Oficiales Cubiertos            │ Contenido Trabajado       │
├─────────────────┼──────────────────────────────────────────┼───────────────────────────┤
│ SEMANA 1        │ • CE 1.a: Estructura de programas        │ Clases, método main,      │
│ (Días 1 al 4)   │ • CE 1.b: Creación de proyectos          │ variables primitivas      │
│                 │ • CE 1.c: Uso del IDE                    │ (int, double, char,       │
│                 │ • CE 1.d, CE 1.e: Variables y tipos      │ boolean, String) y Scanner│
├─────────────────┼──────────────────────────────────────────┼───────────────────────────┤
│ SEMANA 2        │ • CE 1.g: Operadores y expresiones       │ Asignación compuesta,     │
│ (Días 5 al 8)   │ • CE 1.h: Conversiones implícitas        │ división entera, operador │
│                 │   y explícitas (casting)                 │ módulo (%) y jerarquía    │
├─────────────────┼──────────────────────────────────────────┼───────────────────────────┤
│ SEMANA 3        │ • CE 1.f: Constantes (final) y literales │ Inmutabilidad, escapes,   │
│ (Días 9 al 12)  │ • CE 1.i: Comentarios de código          │ salida con printf         │
│                 │ • CE 1.a, 1.e: Salida formateada         │ y Reto 1 Completo         │
└─────────────────┴──────────────────────────────────────────┴───────────────────────────┘
```

Con estas 12 sesiones de 2 horas (24 horas lectivas en total), el estudiante cuenta con un manual didáctico completo, riguroso y **100 % fiel a los contenidos y criterios del RA1**, dominando los fundamentos del lenguaje Java de forma secuencial antes de abordar las estructuras condicionales y los bucles en los siguientes sprints del curso.