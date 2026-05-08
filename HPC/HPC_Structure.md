# 2. HPC Systems

## 2.1. Structure concepts

HPC - High Performance Computing

- Node: single processor

- Cluster: collection of interconnected clusters

- Instance: from where you are connecting (UGent, KU Leuven, VIB ...), may change access permissions and configurations

- Session: your login to your user directory of the system

- Job: a task to be performed by a specifi cluster 

#### Tiers

0. Tier 0 - EU level

1. Tier 1 - Vlaanderen level

2. Tier 2 - Institution level

UGent does not charge for use in Tier 2. Tiers 1 and 0 require project submission.

#### File Systems

| System  | Capacity  | Performance    | Storage    | Jobs execution |
| ------- | --------- | -------------- | ---------- | -------------- |
| Home    | Low       | Low            | No data    | No             |
| Data    | High      | No performance | Long-term  | No             |
| Scratch | Very high | Very high      | Short-term | Yes            |

General purpose and how to access in VSC:

- **Home**: configurations and user preferences
  
  ```bash
  $VSC_HOME
  ```

- **Data**: files and data storage
  
  ```bash
  $VSC_DATA
  ```

- **Scratch**: local connection to run jobs
  
  ```bash
  $VSC_SCRATCH
  ```

General remarks for VSC Tier 2:

- 25 GB capacity for Data and Scratch systems

- Scratch storage is is erased after 1 week - 1 month

- Scratch access is only possible through your instance

#### Nodes

A node is a single machine that performs a specific task:

- CPU: for fast processing

- GPU: for parallelized tasks (image processing, machine learning)

- Memory: data-intensive tasks

- Debug interactive

The more resources used for a job the less its priority in the HPC queue.

**Optimize resources**:

- Run small scale verision of your task first

- Look into its log file `<jobname.logID>`

- Choose right amount of nodes and their capacity  

## 2.2. Connecting to VSC

#### Create dedicated SSH for VSC

Generate SSH key then add it to your VSC account: https://account.vscentrum.be/django//

```bash
ssh-keygen -t ed25519 -C "your_email@example.com" -f ~/.ssh/<ed25519_vsc>
```

#### Open OnDemand

- Interactive way to connect and run jobs

- Good to see available options for newbies

Access UGent Tier-2:

- Enter link: https://login.hpc.ugent.be/

- Authorize access and **keep the firewall tab open**

- Initiate a session already with job proprieties, and then run scripts from inside

#### WSL/Linux Terminal

- Make sure your SSH key is accessible through the working directory

- Run in terminal:
  
  ```bash
  ssh <your_vsc_id>@login.hpc.ugent.be
  ```

- Search for available clusters and select a partition
  
  ```bash
  module av cluster/ # Available cluster partitions
  ```
  
  ```bash
  module swap cluster/<partition> # Select given partition
  ```

- Get general info from system and from the partition
  
  ```bash
  info # General
  ```
  
  ```bash
  sinfo # Partition you're in
  ```

- Check for available modules from a keyword, load them and check loaded modules. Note that `python` and `bash` are always already loaded.
  
  ```bash
  module av <tool_keyword>
  ```
  
  ```bash
  module load <tool_extension>
  ```
  
  ```bash
  module list
  ```

- Unload modules
  
  ```bash
  module unload <tool_extension>
  ```
  
  ```bash
  module purge # All modules
  ```

- Edit a batch file to run a job
  
  ```bash
  nano <slurm_job_file>
  ```

- Run a slurm batch file
  
  ```bash
  sbatch <slurm_job_file>
  ```

- See the queue of your jobsa and the possible status
  
  ```bash
  squeue
  ```
  
  | R      | Running    |
  | ------ | ---------- |
  | **CD** | Completed  |
  | **S**  | Suspended  |
  | **PD** | Pending    |
  | **CG** | Completing |

#### General format of a Slurm batch file to run

```bash
#!/bin/bash
#SBATCH --job-name=<My_job
#SBATCH --mail-user=<my_mail@mail.com>
#SBATCH --mail-type=end #### NONE, BEGIN, END, FAIL, REQUEUE, ALL
#SBATCH --partition=doduo
#SBATCH --mem=4G
#SBATCH -e "slurm_%x_%j.error"  ## %x takes job name; %j takes job ID
#SBATCH -o "slurm_%x_%j.out"
#SBATCH --time=01:00:00
#SBATCH --reservation=vibrepdata_1

# Define variables
DOCKER_IMG="docker://hello-world"

# Best practice to purge all loaded modules first
module purge

# Pull minimal Docker image using Singularity
singularity pull $DOCKER_IMG

# Sleep for 40 seconds so there is time to see the job via `squeue`
echo 'Going to sleep for 40 seconds...'
sleep 40

# Execute the hello world image. The Docker deamon will create a new container wich has an executable inside that will display text in your terminal:
singularity run hello-world_latest.sif

# Sleep 5 seconds before creating report
sleep 5

# Creating resources report
echo "=== Report resources usage ==="
sacct -j $SLURM_JOBID  --format=jobid,partition,elapsed,state,totalcpu,maxrss,averss

### Meaning of each collum in the report (more can be added looking into the manual of sacct command)
#    --format= JobID,\  # job id
#    JobName,\  # job name
#    Partition,\  # Partition during Job run
#    State,\      # Completed; Failed; Out of Memory; Timeout; Cancelled
#    Elapsed,\  # Real Run time
#    TotalCPU,\  # Number of allocated CPUs
#    MaxRSS,\  # Peak real RAM used (number used to adjust mem request)
#    AveRSS\  # Average memory used over time
```
