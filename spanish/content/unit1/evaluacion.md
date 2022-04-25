+++
title = "Test 1"
weight = 7
date = "2019-05-11"
type = "eval"
+++

## Computación Heterogénea

1. Moore's law (ley de Moore) hace referencia a: (7 puntos)
    - El aumento al doble de la frecuencia en procesadores cada cierto tiempo
    - El incremento lineal en el número de cores en un procesador
    - El aumento al doble del número de transistores en procesadores cada cierto tiempo
    - El incremento en potencia generada por un procesador cada cierto tiempo

2. El número de accesos por unidad de tiempo se denomina: (7 puntos)
    - Ancho de banda
    - Ciclos de cómputo
    - Latencia
    - Hit de caché

3. La idea de utilizar jerarquía de memoria en procesadores modernos se basa en: (7 puntos)
    - Evitar que existan conflictos entre cores o unidades de cómputo
    - Aliviar la carga de acceso de la memoria DRAM
    - Accelerar el acceso a datos usando memorias SRAM cercanas a las unidades de cómputo
    - Incrementar las capacidades de almacenamiento agregando SRAM

4. Ordene las memorias de acuerdo a las velocidades de acceso desde una CPU: (14 puntos)
    - DRAM
    - L3
    - Registros
    - L2
    - Disco Duro
    - L1

5. Seleccione a qué tipo de memoria corresponde cada descripción: (14 puntos)
    | Descripción                                   |  SRAM   |  DRAM   |
    | --------------------------------------------- | :------:| :-----: |
    | Tiene menor costo                             | &#9744; | &#9744; |
    | Acceso más rápido                             | &#9744; | &#9744; |
    | Usa más transistores                          | &#9744; | &#9744; |
    | Requiere refrescar memoria                    | &#9744; | &#9744; |
    | Se ubica más cercano al procesador            | &#9744; | &#9744; |
    | También se le conoce como memoria principal   | &#9744; | &#9744; |

6. ¿Qué protocolo de coherencia se aplicó en el siguiente escenario en un CPU multi-core? (7 puntos)

    > En un procesador CPU de 4 cores existen 4 hilos corriendo en cada uno de los cores de forma independiente. La tarea que realizan los hilos es actualizar posiciones de un arreglo compartido en memoria principal. Cuando un hilo cambia un valor de una posición determinada del arreglo, inmediatamente un proceso es encargado de actualizar todas las copias de esa posición del arreglo en las memorias caché de los otros cores.

    - Snooping
    - Actualización
    - Invalidación

7. Seleccione las características en común que tienen las GPUs de Nvidia, AMD e Intel: (7 puntos)

    - [ ] Arquitectura basada muchos cores pequeños (o unidades de cómputo) agrupadas
    - [ ] Cores o unidades de cómputo más complejos que los de CPU
    - [ ] Pueden ejecutar cientos o miles de hilos en paralelo
    - [ ] Incluyen jerarquía de memoria basada en cachés

8. ¿Qué puede definir el programador en un FPGA? (7 puntos)

    - [ ] El tamaño de su memoria interna
    - [ ] La funcionalidad de bloques lógicos
    - [ ] Establecer las rutas de interconexión entre bloques lógicos
    - [ ] El número de transistores del chip
    - [ ] Establecer el clock y reset