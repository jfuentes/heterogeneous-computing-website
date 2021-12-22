+++
title = "Test 2"
weight = 3
date = "2019-05-11"
type = "eval"
+++

## Computación Heterogénea

1. El modelo de cómputo SIMD hace referencia a: (5 puntos)
    - Modelo en que un hilo procesa datos individuales
    - Modelo en el que múltiples hilos procesan el mismo dato individual
    - Modelo en el que hilos procesan múltiples datos a la vez
    - Modelo en que múltiples hilos procesan datos individuales

2. De acuerdo a la Taxonomía de Flynn. ¿A qué modelo corresponde el procesamiento encontrado en CPU multi-core? (5 puntos)
    - SISD
    - SIMD
    - MISD
    - MIMD

3. En el lenguaje de programación CUDA para GPUs Nvidia. ¿Cuál es el propósito de la llamada cudaMalloc(...)? (5 puntos)
    - Reservar memoria en el host
    - Lanzar un kernel de ejecución
    - Reservar memoria en el dispositivo GPU
    - Copiar datos al dispositivo GPU

4. En SYCL y DPC++ existen diferentes clases para la gestión de kernels y datos. Indique por cada una de las siguientes clases su finalidad: (12 puntos)

    | Clase          | SRAM     | DRAM    |
    | -------------- | :---:    | :---:   |
    | sycl::device   | &#9744;  | &#9744; |
    | sycl::queue    | &#9744;  | &#9744; |
    | sycl::buffer   | &#9744;  | &#9744; |
    | sycl::handler  | &#9744;  | &#9744; |
    | sycl::accessor | &#9744;  | &#9744; |
    | sycl::range    | &#9744;  | &#9744; |

5. Dado el siguiente código en DPC++. ¿Qué tipos de accessors deberían ser definidos para A y B? (10 puntos)

    ```cpp
    int main(){
        queue q;
        std::vector<int> v1(N);
        std::vector<int> v2(N);
        ...
        buffer buff1(v1);
        buffer buff2(v2);
        q.submit([&](handler& h){
            accessor A(buff1, h, XXXXX);
            accessor B(buff2, h, XXXXX);
            h.parallel_for(N, [=](auto i){
                B[i] = i + A[i];
            });
        }).wait();
    }
    ```

    - A: read_write y B: read_write
    - A: read_only y B: read_only
    - A: read_only y B: write_only
    - A: write_only y B: write_only

6. Dada la definición del siguiente kernel. ¿Cuántos hilos se crearán para la ejecución en paralelo del parallel_for? (10 puntos)

    ```cpp
    h.parallel_for(nd_range<3>(range<3>({4,5,2}), range<3>({4,2,1})), [=](nd_item<3> item){
        auto global_ = item.get_global_id();
        auto local_id = item.get_local_id();
        ...
    });
    ```
    - 48
    - 4
    - 40
    - 3
    - 20

7. Dada la definición del siguiente kernel. ¿Cuántos grupos de trabajo (work-group) se definen para el parallel_for? (10 puntos)

    ```cpp
    h.parallel_for(nd_range<3>(range<3>({4,5,2}), range<3>({4,2,1})), [=](nd_item<3> item){
        auto global_ = item.get_global_id();
        auto local_id = item.get_local_id();
        ...
    });
    ```
    - 40
    - 3
    - 8
    - 1
    - 5

8. Charla GPU Computing: ¿A qué corresponden los ASICs en las nuevas GPUs de Nvidia? (5 puntos)
    - Hardware especializado para ciertas tareas específicas
    - Hardware disponible para ejecutar cómputo de propósito general
    - Nueva caché L1 programable disponible en Raytracing y Tensor cores
    - Streaming Multiprocesors disponibles en el chip

9. Charla GPU Computing: Explique a qué se refiere el término Multi-chip-module en GPUs modernas (8 puntos)

    ---

        Ingrese aquí su respuesta
    
    ---