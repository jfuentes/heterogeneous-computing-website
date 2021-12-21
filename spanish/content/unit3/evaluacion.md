+++
title = "Test 3"
weight = 3
date = "2019-05-11"
type = "eval"
+++

## Computación Heterogénea

1. ¿Cuál de las siguientes operaciones no corresponde a una función de reducción en DPC++? (10 puntos)
    - maximum (max)
    - plus (add)
    - minimum (mul)
    - parallel_for

2. Dado el siguiente código. Indicar el orden en el que son ejecutado cada uno de los kernels. (10 puntos)

    ```cpp
    queue q;
    auto A = q.submit([&])(handler& h){});

    auto C = q.submit([&])(handler& h){
        h.depends_on(A);
    });

    auto B = q.submit([&])(handler& h){
        h.depends_on(A);
        h.depends_on(C);
    });

    auto D = q.submit([&])(handler& h){
        h.depends_on(A);
        h.depends_on(B);
    });

    auto E = q.submit([&])(handler& h){
        h.depends_on(C);
        h.depends_on(D);
    });
    ```
    ---

        Ingrese su respuesta

    ---

3. ¿Cuál de los siguientes patrones de diseño considera el uso de datos de vecinos para generar un resultado final? (10 puntos)
    - fork-join
    - map
    - stencil
    - scan
    - recurrencia

4. Indicar si la siguiente afirmación es verdadera o falsa. Una reducción paralela requerirá una menor cantidad de operaciones que una reducción secuencial. (10 puntos)
    - Verdadero
    - Falso

5. Indicar si la siguiente afirmación es verdadera o falsa. Un scan paralelo requerirá una menor cantidad de operaciones que un scan secuencial. (10 puntos)
    - Verdadero
    - Falso

6. Indicar el vector resultante luego de aplicar al vector A una operación Gather con los índices en B: (10 puntos)

    Vector con valores -->  A: [4, 2, 3, 5, 0, 1, 3, 2]  
    Vector con índices -->  B: [5, 1, 1, 0, 3, 2, 4, 0]

    ---

        Ingrese su respuesta

    ---

7. Indicar cuál es el objetivo de agregar padding (o relleno) al programar estructuras AoS y SoA. (10 puntos)

    ---

        Ingrese su respuesta

    ---