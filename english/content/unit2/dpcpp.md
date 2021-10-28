+++
title = "DPC++"
weight = 1
date = "2019-05-12"
type = "wiki"
+++

  

Enlaces con material de interés:  

- [Libro Data Parallel C++](https://link.springer.com/content/pdf/10.1007%2F978-1-4842-5574-2.pdf)
- [Ejemplos en C++](https://github.com/oneapi-src/oneAPI-samples/tree/master/DirectProgramming/C%2B%2B)
- [Ejemplos de DPC++](https://github.com/oneapi-src/oneAPI-samples/tree/master/DirectProgramming/DPC%2B%2B)

  

Siempre utilizar en el código:
```cpp
  #include <CL/sycl.hpp> 
  Using namespace  sycl;
```

### 1. Gestión de Memoria
------

#### Malloc
Para gestionar la memoria utilizar enfoque basado en punteros, más específicamente Malloc.
Por ejemplo, crear locación de memoria para compartir:

```cpp
  char *ejemplo = malloc_shared<char>(variable,queue);
```
En SYCL se puede utilizar 3 tipos: *malloc_host*, *malloc_shared* y *malloc_device*:

```Cpp
  int* host_array = malloc_host(N, Q);
  int* shared_array = malloc_shared(N, Q);
  int* device_array = malloc_device(N, Q);
```
Características:
Type | Description | Accessible on host? | Accessible on device? | Located on
---|---|---|---|---
**device**| Allocation in device memory| No | Yes | device
**host** | Allocations in host memory| Yes|Yes| host
**shared**| Allocations shared between host and device|Yes|Yes|can migrate back and forth

<br/><br/>

#### Buffer

Otra opción  es  la utilización  de  buffers: 
La forma más fácil de declararlos es indicando en su constructor la fuente de datos: arreglo, vector, puntero, etc.

Ejemplo:
```cpp
  buffer  my_buffer(my_data);
```

Para acceder a estos se debe utilizar un "accessor"
Ejemplo: 
```cpp
  accessor my_accessor(my_buffer, h)
```

| Tipos de accesos | Descripción|
| ---|---|
| **read_only** | Read only access | 
| **write_only** | Write only access |
| **read_write** | Read and write access |

&NewLine;
&NewLine;

### 2. Implementación y gestión de Kernels
------
#### Colas (Queue)
Las colas (queue) nos permiten conectar con los dispositivos (device), con ellas enviamos kernels para ejecutar trabajo y mover datos.
 
```cpp
  Queue <nombre cola>;
```


#### Parallel_for
Permite definir un bucle paralelo que será ejecutado por muchos hilos dependiendo del número de iteraciones. En su versión simplificada, el compilador realiza la división del trabajo (iteraciones) en los hilos.

```cpp
  h.parallel_for(range{N} , [=](id<1> idx ) {
```
Como se pueden dar cuenta es como una función lambda, la función toma dos argumentos, el primero se llama rango el cual especifica el número de elementos para lanzar en cada dimensión y el segundo es una función del kernel que se ejecutará para cada índice del rango.
Nota: Pueden utilizar para varias dimensiones, para dos dimensiones sería:
```cpp
  h.parallel_for(range{N, M}, [=](id<2> idx)
```

 Un ejemplo completo en código:
```cpp
   
    #include <CL/sycl.hpp>
    #include <array>
    #include <iostream>
    using namespace sycl;
   
    int main(){
		constexpr int size = 16;
		std::array<int, size> data;
		//Se crea una cola
		queue Q;
		//Creación del buffer
		buffer B { data };

		Q.submit([&](handler& h){
			accessor A{B,h};
			h.parallel_for(size, [=](auto& idx){
				A[idx] = idx;
			});
		});

		host_accessor A{B};
		for(int i=0; i<size; i++)
		std::cout << "data" << i << " = " << A[i]  << "\n";
		
		return 0;
	}
```