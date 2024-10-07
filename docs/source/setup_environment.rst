Setting Up the Environment
==========================

Before we begin, let's set up our working environment.

Activate the Conda Environment
--------------------------------

Start a SLURM Interactive Session (If Applicable)
-------------------------------------------------

Start an interactive session to allocate the necessary resources:

.. code-block:: bash

    si -c 32 -t 120  # Allocates 32 cores for 120 minutes

Activate the provided conda environment that contains all necessary tools:

.. code-block:: bash
    course_path="/mnt/aiongpfs/projects/prospectomics_autumn_course"
    env_path="${course_path}/data/mp_practical/env/metaproteomics_env"


    # Load conda module
    module load lang/Anaconda3/2020.11

    # Ensure conda is properly initialized
    conda init bash
    source ~/.bashrc

    conda activate "${env_path}"

Copy Data to the node's Shared Memory
------------------------------------------------

For improved performance, copy data to the node's shared memory directory:

.. code-block:: bash
    # Define paths
    course_data_path="${course_path}/data/mp_practical"
    databases_path="${course_data_path}/databases"
    raw_data_path="${course_data_path}/ms_raw_data"
    node_memory_path="/dev/shm"

    # Make a directory for the course data
    mkdir -p "${node_memory_path}/mp_practical"

    # Move to the shared memory directory
    cd "${node_memory_path}/mp_practical"

    # Copy the data to the shared memory directory
    rsync -avP "${databases_path}" .
    rsync -avP "${raw_data_path}" .

    # Check the contents of the shared memory directory
    tree
