# Repository of useful Slurm tools

Wrappers around Slurm commands to make them more user-friendly and provide additional functionality.

## Setup and Installation

```bash
git clone ....
cd slurm-scripts
```

add the directory to your PATH

```bash
export PATH=$PATH:$PWD
```

to make it permanent add the above line to your `.bashrc` or `.bash_profile` file

```bash
echo 'export PATH=$PATH:$PWD' >> ~/.bashrc
```

The commands will be available in your terminal after you restart it or run the following command
  
```bash
source ~/.bashrc
```

## Features

To see a list of available commands, run the following command:

```bash
# alternatively, you can run <path-to-slurm-scripts>/slurm-scripts
❯ slurm-scripts

Available Commands found:
        acctusage
        cqueue
        gnodes
        gpuinfo
        jobhist
        jobinfo
        myaccount
        mypartitions
        myqueue
        nodeinfo
        qalloc
        qbuild
        slurm-scripts
```


## 1)  `acctusage`

The `acctusage` is a shell script that reports Slurm account usage by users or accounts
- It provides options to specify users, project accounts, start date/time, and end date/time
- The script retrieves account usage information from Slurm and displays it in a formatted table, including login, accounts, cluster, resource, and usage
- It also provides CPU/GPU usage in minutes
- The script is designed to be run from the command line and provides a help option to display usage information

```bash
❯ acctusage
--------------------------------------------------------------------
Slurm account usage from 2024-05-06 to 2024-05-13
--------------------------------------------------------------------
     User         Account   Cluster       Resource             Usage
--------- --------------- --------- -------------- -----------------
t119797u+          bhklab    h4huhn            cpu      155 
t119797u+          bhklab    h4huhn       gres/gpu        0 
t119797u+          bhklab    h4huhn            mem    39744 
t119797u+          bhklab    h4huhn        billing      155 
--------------------------------------------------------------------
*CPU/GPU usage in minutes
```

#### Usage

To use the `acctusage` script, you can run it from the command line with the following syntax:

```
acctusage [-h] [-u <users>] [-a <accounts>] [-s <startdate>] [-e <enddate>]
```

Here are the options and their descriptions:

- `-h`: Display help
- `-u`: Specify users
  - When only using users option, default value is current user
  - When only using accounts option, default value is all users in accounts
  - Multiple users can be specified with comma separation
- `-a`: Specify project accounts
  - When only using users option, default value is all accounts
  - When only using accounts option, default value is all users in accounts
  - Multiple accounts can be specified with comma separation
- `-s`: Specify start date/time
  - Default value is 7 days ago
- `-e`: Specify end date/time
  - Default value is current date/time


## 2) `cqueue`

The `cqueue` script is a utility that displays job queue information. 

- It provides options to filter the output based on partitions and users. 
- The script uses the `squeue` command to retrieve the job queue information.
- The output includes job ID, user, partition, name, state, time, nodes, and reason.

```bash
❯ cqueue
-----------------------------------------------------------------------------------------
      Job ID       User     Job Name  Partition    State     Elapsed     Nodelist(Reason)
------------ ---------- ------------ ---------- -------- ----------- --------------------
11716027     t119797uhn /cluster/pro        all  PENDING        0:00          (BeginTime)
11716028     t119797uhn /cluster/hom        all  PENDING        0:00          (BeginTime)
```

#### Usage

To use the `cqueue` script, you can run it from the command line with the following syntax:

```
cqueue [-h] [-p <partitions>] [-u <users>]
```

Here are the options and their descriptions:

- `-h`: Display help
- `-p`: Specify partitions
  - When only using partitions option, default value is all partitions
  - When only using users option, default value is all users in partitions
  - Multiple partitions can be specified with comma separation
- `-u`: Specify users
  - When only using users option, default value is current user
  - When only using partitions option, default value is all partitions
  - Multiple users can be specified with comma separation


## 3) `gnodes`

The `gnodes` app is a shell script that reports Slurm node information.

- It provides options to filter the output based on node names, partitions, and states.
- The script retrieves node information from Slurm and displays it in a formatted table, including node name, state, CPUs, GPUs, memory, and partition.
```bash
Symbol | Meaning
      . | Available core
      _ | Allocated core
      O | Loaded core
      ! | Load is significantly higher than allocated core count
      ? | Load is unknown

Symbol | Meaning
      * | Unallocated GPU
      G | Allocated GPU
```

```bash
gnodes -p himem

+- himem - 38 cores & 220GB & max time 7-00:00:00 ------+-------------------------------------------------------+-------------------------------------------------------+
| node100   11G  ......!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!! | node51    6G  ............_______OOOOOOOOOOOOOOOOOOO | node70    6G  .......______OOOOOOOOOOOOOOOOOOOOOOOOO |
| node101    0G  _______OOOOOOOOOOOOOOOOOOOOOOOOOOOOOOO | node52   12G  ..............OOOOOOOOOOOOOOOOOOOOOOOO | node71   34G  .............OOOOOOOOOOOOOOOOOOOOOOOOO |
| node102    6G  .___OOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOO | node53    7G  ......._____OOOOOOOOOOOOOOOOOOOOOOOOOO | node72   10G  ..........________OOOOOOOOOOOOOOOOOOOO |
| node103   11G  .........!!!!!!!!!!!!!!!!!!!!!!!!!!!!! | node54   35G  ....________________________________OO | node73    4G  ............______OOOOOOOOOOOOOOOOOOOO |
| node104    4G  .........._______OOOOOOOOOOOOOOOOOOOOO | node55    4G  ......!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!! | node74    4G  ..!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!! |
| node105   11G  .OOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOO | node56    5G  ........OOOOOOOOOOOOOOOOOOOOOOOOOOOOOO | node75   70G  ................OOOOOOOOOOOOOOOOOOOOOO |
| node106   12G  ........___OOOOOOOOOOOOOOOOOOOOOOOOOOO | node57   13G  .......________OOOOOOOOOOOOOOOOOOOOOOO | node76   12G  .......OOOOOOOOOOOOOOOOOOOOOOOOOOOOOOO |
| node107   15G  ....!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!! | node58   14G  ........OOOOOOOOOOOOOOOOOOOOOOOOOOOOOO | node77   12G  ........___OOOOOOOOOOOOOOOOOOOOOOOOOOO |
| node108    7G  ...OOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOO | node59   10G  ....____________OOOOOOOOOOOOOOOOOOOOOO | node79   10G  .....________OOOOOOOOOOOOOOOOOOOOOOOOO |
| node109   13G  ..........OOOOOOOOOOOOOOOOOOOOOOOOOOOO | node60    6G  .________OOOOOOOOOOOOOOOOOOOOOOOOOOOOO | node80   53G  .............___________OOOOOOOOOOOOOO |
| node110    7G  ..........!!!!!!!!!!!!!!!!!!!!!!!!!!!! | node61    6G  ..OOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOO | node81    7G  .......__OOOOOOOOOOOOOOOOOOOOOOOOOOOOO |
| node111   12G  ........._________OOOOOOOOOOOOOOOOOOOO | node62    6G  ...._______________________________OOO | node82   12G  ..!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!! |
| node112    9G  .......OOOOOOOOOOOOOOOOOOOOOOOOOOOOOOO | node63   13G  ................___OOOOOOOOOOOOOOOOOOO | node83   11G  ..................OOOOOOOOOOOOOOOOOOOO |
| node113    0G  ___________________OOOOOOOOOOOOOOOOOOO | node64   11G  ..____________OOOOOOOOOOOOOOOOOOOOOOOO | node84   13G  ......___OOOOOOOOOOOOOOOOOOOOOOOOOOOOO |
| node141    6G  .............!!!!!!!!!!!!!!!!!!!!!!!!! | node65   12G  .____________OOOOOOOOOOOOOOOOOOOOOOOOO | node92   62G  .............._____OOOOOOOOOOOOOOOOOOO |
| node47    0G  _______________OOOOOOOOOOOOOOOOOOOOOOO | node66    8G  ...........!!!!!!!!!!!!!!!!!!!!!!!!!!! | node93    6G  ....!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!! |
| node48    0G  !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!! | node67   15G  ..........._________OOOOOOOOOOOOOOOOOO | node94   13G  ......OOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOO |
| node49    7G  ....._____OOOOOOOOOOOOOOOOOOOOOOOOOOOO | node68    0G  OOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOO | node95    6G  ......OOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOO |
| node50    0G  OOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOO | node69   11G  .OOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOO |                                                       |
+-------------------------------------------------------+-------------------------------------------------------+-------------------------------------------------------+

+- himem - 38 cores & 354GB & max time 7-00:00:00 ------+-------------------------------------------------------+-------------------------------------------------------+
| node118  175G  ........................____OOOOOOOOOO | node123    0G  OOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOO | node128  266G  ...............................______O |
| node119    0G  ______________OOOOOOOOOOOOOOOOOOOOOOOO | node124    0G  OOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOO | node129    0G  _______OOOOOOOOOOOOOOOOOOOOOOOOOOOOOOO |
| node120    6G  ._____________________________OOOOOOOO | node125  342G  ..............................___OOOOO | node131    0G  OOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOO |
| node121    0G  _____________OOOOOOOOOOOOOOOOOOOOOOOOO | node126  354G  ...................................... | node140    0G  OOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOO |
+-------------------------------------------------------+-------------------------------------------------------+-------------------------------------------------------+

+- himem - 94 cores & 1476GB & max time 7-00:00:00 -------------------------------------------------------------+
| node130  860G  .............._______________________________________________________________________________O |
+---------------------------------------------------------------------------------------------------------------+
```

## 4) `gpuinfo`

The `gpuinfo` script is a utility that displays GPU information.

- It provides options to filter the output based on GPU IDs and states.
- The script uses the `sinfo` command to retrieve the GPU information.
- The output includes GPU ID, state, memory, and partition.

```bash
❯ gpuinfo
------------------------------------------------
Slurm GPU information
------------------------------------------------
There are a total of 75 GPUs [up]
4 a40 gpus
8 a100 gpus
31 p100 gpus
32 v100 gpus
------------------------------------------------
There are a total of 75 GPUs [accessible]
4 a40 gpus
8 a100 gpus
31 p100 gpus
32 v100 gpus
------------------------------------------------
Usage by user:
None
------------------------------------------------
There are a total of 75 GPUs [available]
a100: 8 available 
v100: 32 available 
p100: 31 available 
a40: 4 available 
------------------------------------------------

```

#### Usage

To use the `gpuinfo` script, you can run it from the command line with the following syntax:

```bash
usage: gpuinfo [-h] [-p PARTITION] [-v]
```

Here are the options and their descriptions:

- `-h`: Display help
- `-p`: Specify partitions
- `-v`: Verbose output



## 5) `jobhist`

The `jobhist` script is a utility that displays job history information.

- It provides options to filter the output based on partitions, users, and time range.
- The script uses the `sacct` command to retrieve the job history information.
- The output includes job ID, user, partition, name, state, time, nodes, and reason.

```bash
❯ jobhist
---------------------------------------------------------------------------------------------------------
          Startdate        Job ID     Job Name  Partition      State     Elapsed Nodes CPUs   Memory GPUs
------------------- ------------- ------------ ---------- ---------- ----------- ----- ---- -------- ----
2024-05-12T17:01:59      11716028 /cluster/ho+        all   REQUEUED    00:04:27     1    1     256M 0
2024-05-13T09:16:55      11716027 /cluster/pr+        all   REQUEUED    00:00:02     1    1     256M 0
```

#### Usage

To use the `jobhist` script, you can run it from the command line with the following syntax:

```
jobhist [-h] [-p <partitions>] [-u <users>] [-s <startdate>] [-e <enddate>]
```

Here are the options and their descriptions:

- `-h`: Display help
- `-p`: Specify partitions
  - When only using partitions option, default value is all partitions
  - When only using users option, default value is all users in partitions
  - Multiple partitions can be specified with comma separation


## `jobinfo`

The `jobinfo` script is a utility that displays detailed job information.

- It provides options to filter the output based on job IDs.
- The script uses the `scontrol` command to retrieve the job information.
- The output includes job ID, user, partition, name, state, time, nodes, and reason.

```bash
Job ID               : 11716028
Job name             : /cluster/home/t119797uhn/.local/bin/ncdu
User                 : t119797uhn
Account              : bhklab
Working directory    : /cluster/home/t119797uhn
Cluster              : h4huhn
Partition            : all
Nodes                : 1
Nodelist             : None assigned
Tasks                : --
CPUs                 : 0
GPUs                 : --
State                : PENDING (BeginTime)
Exit code            : --
Submit time          : 2024-05-12T17:06:26
Start time           : --
End time             : --
Wait time            : --
Reserved walltime    : 1-00:00:00
Used walltime        : --
Used CPU walltime    : --
Used CPU time        : --
% User (computation) : --
% System (I/O)       : --
CPU efficiency       : --
Reserved memory      : 256M
Max memory used      : --
Memory efficiency    : --
Max disk write       : --
Max disk read        : --
```

### Usage

To use the `jobinfo` script, you can run it from the command line with the following syntax:

```
jobinfo <job_id>
```


