+++
title = "Lab 1"
weight = 1
date = "2019-05-12"
type = "practice"
+++

# Lab 1

  

**Professor**: Joel Fuentes

**Assistants:** Daniel López, Sebastián González

## 1 Description

In this lab you must implement a DPC++ program that contains one or more kernels that perform operations on vectors. The goal is for vector operations to be run in parallel on an accelerator (multi-core CPU, GPU, or FPGA).

Consider that there are 3 vectors A,B and C. The operations to be carried out in parallel on these vectors are:

  

1. Assign incremental consecutive values to vector A.

2. Assign decremental consecutive values to vector B.

3. Perform arithmetic operations between A and B by saving the result in C. If the index of the vectors is even you must perform addition, if the index is odd you must perform subtraction.

  

**Table 1.1** shows an example of vectors A, B and C after performing operations described above.

For example, the operation on index 0 corresponds to the sum C[0] = A[0] + B[0] resulting in 9. The operation in index 1 corresponds to the subtraction C[1] = A[1] - B[1] resulting in -7.

For kernel implementation, the use of buffers, accessors, and parallel_for is recommended. In the following link will find a Wiki with details about its use in DPC++:

http://www.face.ubiobio.cl/~jfuentes/classes/hc/unit2/dpcpp

  

|Index|0|1|2|3|4|5|6|7|8|9|

|--- | --- | ---|--- | --- | ---|--- | --- | ---| --- |---|

|A|0|1|2|3|4|5|6|7|8|9|

|B|9|8|7|6|5|4|3|2|1|0|

C|9|-7|9|-3|9|-1|9|5|9|9|

> Table 1.1: Example vectors A, B and C.

  

To test your deployment, we recommend running it on Devcloud servers. In the following link you will find a Wiki with details about the configuration of the Devcloud development environment and compiling with DPC++:

  

http://www.face.ubiobio.cl/~jfuentes/classes/hc/unit2/devcloud/
