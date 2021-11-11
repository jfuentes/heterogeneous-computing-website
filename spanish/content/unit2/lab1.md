+++
title = "Lab 1"
weight = 1
date = "2019-05-12"
type = "practice"
+++

# Lab 1
 

**Profesor**: Joel Fuentes

**Ayudantes:** Daniel López, Sebastián González

## 1 Descripción


En este laboratorio Ud. deberá implementar un programa en DPC++ que contenga uno o más
kernels que realicen operaciones sobre vectores. El objetivo es que las operaciones en vectores se
ejecuten en paralelo en un acelerador (CPU multi-core, GPU o FPGA).
Considere que existen 3 vectores A, B y C. Las operaciones a realizar en paralelo sobre estos
vectores son:
1. Asignar valores consecutivos incrementales al vector A.
2. Asignar valores consecutivos decrementales al vector B.
3. Realizar operaciones aritméticas entre A y B guardando el resultado en C. Si el índice de los
vectores es par se debe realizar suma, si el índice es impar se debe realizar resta.

  


La Tabla 1.1 muestra un ejemplo de los vectores A, B y C luego de realizar las operaciones
descritas anteriormentes. Por ejemplo, la operación en el índice 0 corresponde a la suma C[0] =
A[0] + B[0] resultando 9. La operación en el índice 1 corresponde a la resta C[1] = A[1] 􀀀 B[1]
resultando -7.
Para la implementación del kernel se recomienda el uso de buers, accessors y parallel_for. En
el siguiente link encontrará una Wiki con detalles sobre su uso en DPC++: 
http://www.face.ubiobio.cl/~jfuentes/classes/ch/unit2/dpcpp

|Indices|0|1|2|3|4|5|6|7|8|9|
|--- | --- | ---|--- | --- | ---|--- | --- | ---| --- |---|
|A|0|1|2|3|4|5|6|7|8|9|
|B|9|8|7|6|5|4|3|2|1|0|
C|9|-7|9|-3|9|-1|9|5|9|9|

> Tabla 1.1: Ejemplo vector A, B y C.

 
Para probar su implementación, se recomienda ejecutarlo en servidores Devcloud. En el siguiente
link encontrará una Wiki con detalles sobre la conguración del entorno de desarrollo Devcloud y
compilación con DPC++: 
http://www.face.ubiobio.cl/~jfuentes/classes/ch/unit2/devcloud
