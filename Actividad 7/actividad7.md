## UNIVERSIDAD SAN CARLOS DE GUATEMALA ##
## FACULTAD DE INGENIERIA ##
## ESCUELA DE CIENCIAS Y SISTEMAS ##
## Sistemas operativos 1##
## SECCIÓN A ##

### Actividad 1 ###

- **Nombre:** Mynor Francisco Morán García   **Carne:** 201603232

>>


# Completely Fair Scheduler (CFS) en Linux

El **Completely Fair Scheduler (CFS)** es el planificador de procesos predeterminado en Linux, introducido a partir del kernel 2.6.23. Su objetivo principal es garantizar una distribución equitativa del tiempo de CPU entre los procesos, independientemente de su prioridad.

## Características Principales

### 1. Equidad Justa
- CFS se basa en el concepto de "justicia total", buscando repartir el tiempo de CPU de manera equitativa entre todos los procesos.
- En lugar de asignar rebanadas de tiempo fijo, CFS calcula cuánto tiempo ha utilizado cada proceso y ajusta el tiempo futuro de CPU para equilibrar el uso entre todos los procesos.

### 2. Cola de Red-Black Tree
- Los procesos listos para ejecutarse se organizan en un árbol autoequilibrado de tipo **Red-Black Tree**, lo que permite realizar búsquedas, inserciones y eliminaciones de forma eficiente (O(log n)).
- Los nodos del árbol representan los procesos, organizados por la cantidad de "tiempo virtual" que han consumido.

### 3. Tiempo Virtual y Repartición
- CFS asigna a cada proceso un **tiempo virtual**, que incrementa proporcionalmente al uso de CPU real.
- Los procesos con menor tiempo virtual obtienen más tiempo de CPU en las decisiones de planificación para mantener una equidad en el reparto del tiempo de CPU.

### 4. Prioridades
- Las prioridades de los procesos se gestionan a través del valor de **niceness**, que va de -20 (mayor prioridad) a 19 (menor prioridad).
- Los procesos con mayor prioridad incrementan su tiempo virtual más lentamente, permitiendo que se ejecuten por más tiempo.

### 5. Tiempo de Desalojo (Granularidad)
- CFS no define rebanadas de tiempo fijas. En su lugar, decide cuándo intercambiar los procesos basado en la **granularidad**, que es el tiempo mínimo que un proceso puede ejecutar antes de ser preemptado.
- Esto permite que el cambio entre procesos sea fluido y equitativo.

### 6. Límites para Tareas en Tiempo Real
- CFS también gestiona tareas en tiempo real, pero establece límites para evitar que monopolizen la CPU.
- Las tareas en tiempo real tienen su propia gestión separada del resto de procesos, pero CFS retoma el control una vez que esas tareas finalizan.

### 7. Multiprocesador (SMP) y Carga Equilibrada
- En sistemas con múltiples núcleos, CFS intenta equilibrar la carga entre los diferentes procesadores para evitar que un núcleo se sature mientras otros permanecen inactivos.

### 8. Latencia de Planificación
- El parámetro de **latencia de planificación** controla el tiempo máximo que un proceso puede esperar antes de ser planificado.
- Si hay muchos procesos, la latencia puede aumentar para reducir el cambio excesivo de contexto.

## Funcionamiento General

Cuando un proceso está listo para ejecutarse, CFS:
1. Lo agrega al **Red-Black Tree**.
2. Calcula su tiempo virtual y lo compara con otros procesos en la cola.
3. Selecciona el proceso con el menor tiempo virtual para ejecutarse, otorgándole tiempo de CPU.
4. Ajusta el tiempo virtual del proceso después de ejecutar una fracción de su tiempo.
5. Si es necesario, cambia a otro proceso para mantener la equidad.

En resumen, **CFS** es un planificador sofisticado que proporciona tiempo de CPU justo entre todos los procesos, con un sistema de prioridades dinámico y eficiente para manejar procesos de manera escalable.
