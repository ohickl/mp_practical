Setting Up the Session
==========================

Before we begin, let's set up our working environment.

Task 1: Start a SLURM Interactive Session (If Applicable)
----------------------------------------------------------

Start an interactive session to allocate the necessary resources:

.. code-block:: bash

    si -c 32 -t 120  # Allocates 32 cores for 120 minutes


Task 2: Activate the Conda Environment
---------------------------------------

Activate the provided conda environment that contains all necessary tools:

.. code-block:: bash

    course_path="/mnt/aiongpfs/projects/prospectomics_autumn_course"
    env_path="${course_path}/envs/metaproteomics_env"

    # Load conda module
    module load lang/Anaconda3/2020.11

    # Ensure conda is properly initialized
    conda init bash
    source ~/.bashrc

    conda activate "${env_path}"

Task 3: Copy Data to the node's Shared Memory
---------------------------------------------

For improved performance, copy data to the node's shared memory directory:

.. code-block:: bash

    # Define paths
    course_data_path="${course_path}/data/mp_practical"
    databases_path="${course_data_path}/databases"
    raw_data_path="${course_data_path}/ms_raw_data"
    session_path="/dev/shm"

    # Make a directory for the course data
    mkdir -p "${session_path}/mp_practical"

    # Move to the shared memory directory
    cd "${session_path}/mp_practical"

    # Copy the data to the shared memory directory
    rsync -avP "${databases_path}" .
    rsync -avP "${raw_data_path}" .

    # Check the contents of the shared memory directory
    tree

.. hint::

    Check the section `General Tips` for basic Linux commands.