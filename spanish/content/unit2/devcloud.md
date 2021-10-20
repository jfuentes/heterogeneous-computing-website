+++
title = "Wiki DevCloud"
weight = 3
date = "2019-05-11"
+++

## Configuración VS Code + DevCloud

La guía paso a paso de conexión a DevCloud con VS Code está disponible [aquí](https://devcloud.intel.com/oneapi/get_started/baseToolkitSamples/), en inglés.

1. Prepara la conexión SSH
    1. Descarga el archivo de configuración automatizada correspondiente a tu usuario, disponible en el link de la guía.
    2. Abre una terminal que soporte bash en la carpeta donde se descargó el archivo y ejecuta el siguiente comando: `bash setup-devcloud-access-XXXXX.txt`, donde XXXXX es tu ID de usuario.
    3. Elimina el archivo descargado por razones de seguridad.
    4. Si por alguna razón hubo problemas con esta configuración, prueba la sección de configuración manual en la guía.
2. Descargar VS Code y ejecutarlo.
3. Instalar la extensión `Remote - SSH`.
4. Abrir una terminal dentro del programa y conectarse escribiendo `ssh devcloud`.


## Compilación y ejecución de programas


### Compilación

La compilación dentro del DevCloud se asemeja a la compilación normal de un programa C, con la diferencia que debe utilizarse el compilador dpcpp e incluir los parámetros de compilación correspondientes a openCL y SYCL. Un ejemplo de compilación en la consola se vería de la siguiente manera:

>`dpcpp <opciones> <nombre del archivo> -lOpenCL -lsycl`

### Ejecución

Para ejecutar los archivos compilados, utilizaremos el modo interactivo de DevCloud. El modo interactivo nos asignará un nodo de cómputo, según cuál le sea solicitado. Podemos solicitar diferentes tipos de nodos, los cuales son CPU multi-core, GPU, FPGA, y MPI.

Al ingresar al DevCloud, la sesión tendrá por nombre tu nombre de usuario y un login, por ejemplo: \[u84751@login-2]. Luego de pedir un nodo, y que se te sea asignado uno, el nombre de tu sesión cambiará con la ID del nodo asignado, por ejemplo: \[u84751@s001-141]. Es importante notar que debes esperar a que se te asigne un nodo, dependiendo de la disponibilidad del sistema.

Para solicitar un nodo, utilizaremos el comando qsub del shell, y le pasaremos los parámetros -I (i mayúscula) y -l (L minúscula), que indican que queremos solicitar una sesión interactiva y que queremos utilizar completamente el nodo, respectivamente. Además, le indicaremos a qsub que queremos 2 procesos por nodo con :ppn=2. A continuación, se presentan los nodos de cómputo que se pueden solicitar junto con sus comandos:

1. Multi-core
    >`qsub -I -l nodes=1:xeon:ppn=2`
2. GPU
    >`qsub -I -l nodes=1:gpu:ppn=2`
3. FPGA
    >`qsub -I -l nodes=1:fpga:ppn=2`
4. MPI

    Para correr aplicaciones MPI en DevCloud se deben solicitar múltiples nodos en modo interactivo con el comando qsub. Por ejemplo, el siguiente comando solicita dos nodos de cómputo GPU:
    >`qsub -I -l nodes=2:gpu:ppn=2 -d .` 

    El siguiente comando obtiene todas las configuraciones de los nodos en DevCloud:
    >`pbsnodes | grep properties | sort –u`

    Una vez que los nodos sean asignados, se puede utilizar el siguiente comando para obtener los nombres de los nodos:
    >`qstat –xf`

Luego de solicitar un nodo, puede utilizarse el comando clinfo para obtener información sobre el nodo asignado.
Para más información sobre qsub y los nodos que se pueden solicitar, consulte el siguiente [link](https://devcloud.intel.com/oneapi/documentation/job-submission/).
