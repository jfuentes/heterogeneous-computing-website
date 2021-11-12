+++
title = "Lab 1"
weight = 1
date = "2019-05-12"
type = "practice"
+++

**Profesor**: Joel Fuentes

**Ayudantes:** Daniel López, Sebastián González

## Descripción


En este laboratorio Ud. deberá implementar un programa en DPC++ que contenga uno o más
kernels que realicen operaciones sobre vectores. El objetivo es que las operaciones en vectores se
ejecuten en paralelo en un acelerador (CPU multi-core, GPU o FPGA).
Considere que existen 3 vectores A, B y C. Las operaciones a realizar en paralelo sobre estos
vectores son:
1. Asignar valores consecutivos incrementales al vector A.
2. Asignar valores consecutivos decrementales al vector B.
3. Realizar operaciones aritméticas entre A y B guardando el resultado en C. Si el índice de los
vectores es par se debe realizar suma, si el índice es impar se debe realizar resta.

  


La Figura 1 muestra un ejemplo de los vectores A, B y C luego de realizar las operaciones
descritas anteriormentes. Por ejemplo, la operación en el índice 0 corresponde a la suma C[0] =
A[0] + B[0] resultando 9. La operación en el índice 1 corresponde a la resta C[1] = A[1] 􀀀 B[1]
resultando -7.
Para la implementación del kernel se recomienda el uso de buffers, accessors y parallel_for. En
el siguiente link encontrará una Wiki con detalles sobre su uso en DPC++: http://www.face.ubiobio.cl/~jfuentes/classes/ch/unit2/dpcpp

<p align="center">
  <img src="../../images/vectors.png" style="height:40%;width:40%">
</p>

<center>Figura 1: Ejemplo vector A, B y C.</center>

 
Para probar su implementación, se recomienda ejecutarlo en servidores Devcloud. En el siguiente
link encontrará una Wiki con detalles sobre la configuración del entorno de desarrollo Devcloud y
compilación con DPC++: http://www.face.ubiobio.cl/~jfuentes/classes/ch/unit2/devcloud
