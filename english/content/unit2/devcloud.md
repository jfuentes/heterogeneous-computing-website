+++
title = "DevCloud"
weight = 9
date = "2019-05-11"
type = "wiki"
+++

## VS Code + DevCloud Configuration

The step-by-step guide to connect to DevCloud using VS Code is available [here](https://devcloud.intel.com/oneapi/get_started/baseToolkitSamples/).

1. Setting up the SSH Config
    1. Download the auto config file customized for your account, available on the guide's website.    
    2. Open a terminal that supports bash in the location of the downloaded file and execute the following command: `bash setup-devcloud-access-XXXXX.txt`, where XXXXX is your user's ID.
    3. For safety reasons, remove the downloaded file.
    4. If the bash command was unsuccessful, try out the guide's manual configuration section.
2. Download VS Code and execute it.
3. Install the `Remote - SSH` extension.
4. Open a terminal inside the program and run the command `ssh devcloud`.


## Code compilation and running


### Compilation

Compilation in DevCloud follows almost the same steps a regular C code compilation would, with the difference that the dpcpp compiler must be used, and additional parameters corresponding to openCL and SYCL must be included. An example of a expected compile command would be as follows:

>`dpcpp <options> <file name> -lOpenCL -lsycl`

### Running code

To run the compiled files, we will use DevCloud's interactive mode. Interactive mode will assign us a compute node, depending on which one is requested. We can request different types of nodes: Multi-core CPU, GPU, FPGA and MPI.

When entering DevCloud, the session will be named after your username and a login, for example: \[u84751@login-2]. After requesting a node and being assigned one, the name of your session will change with the ID of the assigned node, for example: \[u84751@s001-141]. It is important to note that you must wait for a node to be assigned to you, depending on the availability of the system.

To request a node, we will use the shell command qsub, and we will pass it the parameters -I (uppercase i) and -l (lowercase L), to indicate that we want to request an interactive session and that we want to fully use the node, respectively. Also, we will tell qsub that we want 2 processes per node with :ppn=2. Here are the compute nodes that can be requested along with their commands:

1. Multi-core CPU
    >`qsub -I -l nodes=1:xeon:ppn=2`
2. GPU
    >`qsub -I -l nodes=1:gpu:ppn=2`
3. FPGA
    >`qsub -I -l nodes=1:fpga:ppn=2`
4. MPI

    To run MPI applications in DevCloud, multiple nodes in interactive nodes must be requested using the qsub command. For example, the following command requests two GPU computing nodes:
    >`qsub -I -l nodes=2:gpu:ppn=2 -d .` 

    The following command gets the configurations of all requested nodes:
    >`pbsnodes | grep properties | sort –u`

    Once all nodes have been asigned, the following command can be used to fetch the nodes' names:
    >`qstat –xf`

After requesting a node, the clinfo command can be used to get more information about the assigned node.
For more information about the qsub command and the nodes that can be requested, follow the next [link](https://devcloud.intel.com/oneapi/documentation/job-submission/).
