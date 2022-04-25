+++
title = "DPC++"
weight = 8
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

Pasos de un programa en DPC++
1. Crear cola de dispositivo
2. Crear buffers
3. Encolar kernel(s)
5. Crear accessadores
6. Especificar función del kernel

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
  queue <nombre cola>;
```

#### Single_task
Permite definir un kernel que ejecute una sola tarea o hilo.

```cpp
  h.single_task([=]) {
	  ...
  });
```

#### Parallel_for
Permite definir kernel paralelo que será ejecutado por muchos hilos dependiendo del elementos de procesamiento. En su versión simplificada, el compilador realiza la división del trabajo (iteraciones) en los hilos.

```cpp
  h.parallel_for(range(N) , [=](id<1> idx ) {
	  ...
  });
```
Como se pueden dar cuenta es como una función lambda, la función toma dos argumentos, el primero se llama rango el cual especifica el número de elementos para lanzar en cada dimensión y el segundo es una función del kernel que se ejecutará para cada índice del rango.
Nota: Pueden utilizar para varias dimensiones, para dos dimensiones sería:
```cpp
  // Usando range: NxM elementos de procesamiento
  h.parallel_for(range(N, M), [=](id<2> item) {
	  ...
  });

  // Usando nd_range: NxM elementos de procesamiento y cada grupo de 1x1
  h.parallel_for(nd_range<2>(range<2>({N, M}), range<2>({1, 1})), [=](nd_item<2> item) {
	  ...
  });
```

##### Range
Es una abstracción que describe el número de elementos (hilos o work-items) en cada dimensión. Puede contener 1, 2 o 3 números y está determinada por:
- **range**: describe el espacio de iteración de la ejecutación paralela.
- **item**: representa una instancia individual de una función kernel . Expone funciones adicionales sobre las propiedades del la ejecución (rango, id, etc.).

##### ND_Range
Permite de expresar paralelismo con alto nivel de precisión en múltiples dimensiones. Recibe como parámetros ranges que definen la organización en el espacio global los hilos y sus grupos. Su funcionalidad se puede expresar mediante las clases nd_range y nd_item.
- **nd_range**: representa un la configuración del grupo de ejecución usando rango global y local para cada work group.
- **item**: representa una instancia individual de una función kernel . Expone funciones adicionales sobre las propiedades del la ejecución (rango, id, etc.

El siguiente ejemplo muestra la definición de un ND_Range de 3 dimensiones cuya organización de hilos (work-items) es 12x8x16 y donde estos hilos están organizados en grupos de 6x2x4.

```cpp
  nd_range<3>(range<3>({12, 8, 16}), range<3>({6, 2, 4}))
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
		for(int i = 0; i < size; i++)
		std::cout << "data" << i << " = " << A[i]  << "\n";
		
		return 0;
	}
```