# Bienvenido al módulo de Programación
El equipo de estudiantes debe elegir uno de los proyectos que se muestran a continuación.
## Bolsa de proyectos
### Ciberseguridad

1. Simulador de phising — registro de incidencias, activos afectados, nivel de criticidad, estado de resolución (detectado → en análisis → resuelto).
2. Bóveda de contraseñas — catálogo de cuentas/accesos, categorías, fecha de caducidad, nivel de riesgo, histórico de cambios.

### Python / automatización
3. Calculadora paso a paso — biblioteca de scripts, categorías, parámetros configurables, historial de ejecuciones y resultados.
4. Generador de datos de prueba — catálogo de bots, comandos disponibles, usuarios, registro de interacciones.

### Videojuegos y realidad virtual
5. Aventura conversacional — assets, niveles, personajes, versiones/builds, bugs pendientes.
6. Simulador de físicas 2D — salas/escenarios, reservas, participantes, puntuaciones y tiempos.

### Productos software en contenedores
7. Reparto de contenedores en nodos — repositorio de imágenes, versiones, entornos de despliegue (dev/test/prod), estado.
8. Catálogo de servicios + generador de config — catálogo de servicios, versiones, dependencias entre ellos, estado de salud.

### Inteligencia artificial y big data
9. Motor de recomendación — catálogo de datasets, modelos, métricas de rendimiento, versiones.
10. Analizador de reseñas — proyectos de anotación, anotadores asignados, progreso, revisiones.

### Recursos y servicios en la nube
11. Cotizador cloud — instancias, almacenamiento, coste estimado, alertas de uso excesivo.
12. Dashboard de monitorización — proveedores, planes contratados, facturación, usuarios con acceso a cada servicio.

| # Proyecto (especialización) | S1 · RA1 | S2 · RA3 | S3 · RA2 | S4 · RA4 | S5 · RA6 | S6 · RA7 | S7 · RA5-I (GUI) | S8 · RA5-II (ficheros) | Empresa |
|---|---|---|---|---|---|---|---|---|---|
| **1. Simulador de phishing** (Ciberseguridad) | Clasificar 1 mensaje fijo sí/no | Menú que recorre varios mensajes, cuenta aciertos | `Random` para elegir mensaje al azar; `String` (`contains`, `toLowerCase`) para buscar palabras sospechosas; `Scanner` para la respuesta del usuario | Clases `Mensaje`, `Usuario`, `Sesión` | Lista de mensajes + estadísticas de aciertos | Tipos de mensaje (Email/SMS/RedSocial) con pistas distintas | GUI con mensaje y botones sí/no | Historial de partidas en fichero | BD de mensajes/usuarios/resultados; diagramas del flujo de juego |
| **2. Bóveda de contraseñas** (Ciberseguridad) | Evaluar longitud de 1 contraseña | Reglas de fortaleza (mayúsc./dígitos/símbolos) | `String` (`length`, `matches` con regex) para evaluar la contraseña; `LocalDate` para fecha de caducidad | Clases `Credencial`, `PoliticaSeguridad` | Lista de credenciales guardadas, filtrado | Tipos de política (Básica/Estricta/Corporativa) | GUI para añadir/ver credenciales | Bóveda persistida en fichero | BD de credenciales; diagramas del proceso de comprobación |
| **3. Calculadora paso a paso** (Python) | Evaluar suma/resta simple | Precedencia de operadores, tokenizar con bucles | `StringBuilder` para construir la expresión; clases envoltorio `Integer`/`Double` (`parseInt`, `parseDouble`); `Math` (métodos estáticos) para operaciones | Clases `Expresión`, `Token`, `Operador` | Lista de pasos de resolución (histórico) | Tipos de operador con cálculo polimórfico | GUI tipo calculadora con visor de pasos | Historial de cálculos en fichero | BD de histórico; diagramas del proceso de evaluación |
| **4. Generador de datos de prueba** (Python) | Generar 1 dato aleatorio de lista fija | Bucle para generar N datos | `Random` para valores aleatorios; `LocalDate` para fechas; `String.format` para dar formato al dato generado | Clases `Plantilla`, `Campo`, `Registro` | Colección de plantillas y lotes generados | Tipos de campo (Texto/Número/Fecha/Email) polimórficos | GUI para diseñar la plantilla | Exportación de lotes a CSV | BD de plantillas/lotes; diagramas del proceso de generación |
| **5. Aventura conversacional** (Videojuegos) | Texto de intro + 1 decisión | Ramificación de la historia con control de flujo | `Scanner` para las decisiones del jugador; `Random` para eventos aleatorios; `String` para comparar respuestas | Clases `Personaje`, `Escena`, `Objeto` | Inventario del jugador, lista de escenas | Tipos de personaje (Aliado/Enemigo/Neutral) | GUI con texto y botones de decisión | Guardar partida en fichero | BD de partidas; diagramas de estados de la historia |
| **6. Simulador de físicas 2D** (VR/Videojuegos) | Mover 1 bola con variables posición/velocidad | Bucle de simulación con colisión en bordes | `Math` (`sqrt`, `pow`) para cálculos de velocidad/colisión; `Random` para posición/velocidad inicial | Clases `Bola`, `Escenario` | Colección de bolas simultáneas | Tipos de bola (Normal/Pesada/Rebote especial) | GUI que dibuja el movimiento | Exportar estadísticas a fichero | BD de simulaciones; diagramas del ciclo de simulación |
| **7. Reparto de contenedores en nodos** (Contenedores) | Comprobar si 1 contenedor cabe en 1 nodo | Bucle de reparto con reglas simples | `Math` para comprobar capacidad restante; `Random` para generar contenedores de prueba | Clases `Contenedor`, `Nodo`, `Cluster` | Listas de contenedores/nodos, algoritmo de reparto | Tipos de nodo (AltaMemoria/AltaCPU) | GUI con barras de ocupación | Configuraciones de reparto en fichero | BD de clusters/nodos; diagramas del proceso de asignación |
| **8. Catálogo de servicios + generador de config** (Contenedores) | Pedir datos de 1 servicio | Asistente (wizard) paso a paso | `StringBuilder` para construir el fichero de configuración; `LocalDateTime` para la marca de tiempo de la config | Clases `Servicio`, `Configuración` | Catálogo de servicios + historial de configs | Tipos de servicio (Web/BD/Worker) con config polimórfica | GUI tipo wizard | Exportar fichero de configuración | BD de catálogo/configs; diagramas del proceso del asistente |
| **9. Motor de recomendación** (IA/Big Data) | Mostrar 1 recomendación fija | Bucle que compara gustos con catálogo pequeño | `Random` para simular gustos iniciales; `Math` para calcular puntuación de coincidencia | Clases `Item`, `Usuario`, `Etiqueta` | Catálogo + gustos + cálculo de coincidencias | Tipos de ítem (Película/Música/Libro) | GUI para marcar gustos y ver resultados | Catálogo y perfiles en fichero | BD de ítems/valoraciones; diagramas del proceso de recomendación |
| **10. Analizador de reseñas** (IA/Big Data) | Contar palabras +/- en 1 texto | Bucle que analiza varias reseñas | `String` (`split`, `contains`, `toLowerCase`) para contar palabras clave en la reseña | Clases `Producto`, `Reseña`, `Autor` | Reseñas por producto + estadísticas | Tipos de producto con diccionario distinto | GUI para introducir reseñas | Catálogo y reseñas en fichero | BD de productos/reseñas; diagramas del proceso de análisis |
| **11. Cotizador cloud** (Nube) | Coste de 1 recurso (fórmula fija) | Elegir tipo de recurso y aplicar su fórmula | `Math` y clases envoltorio (`Double`) para cálculo de coste; `String.format` para mostrar el importe | Clases `Recurso`, `Presupuesto` | Recursos de un presupuesto + lista de presupuestos | Tipos de recurso (Cómputo/Almacenamiento/Red) polimórficos | GUI para montar un presupuesto | Presupuestos persistidos + comparación | BD de presupuestos/clientes; diagramas del proceso de cotización |
| **12. Dashboard de monitorización** (Nube) | Mostrar 1 valor aleatorio de CPU | Serie temporal + detección de umbral | `Random` para simular lecturas de sensor; `LocalDateTime` para la marca temporal de cada medición | Clases `Sensor`, `Métrica`, `Alerta` | Histórico de métricas + lista de alertas | Tipos de sensor (CPU/Memoria/Red) | GUI tipo dashboard con gráficos | Histórico persistido en fichero | BD de métricas/alertas; diagramas del proceso de monitorización |