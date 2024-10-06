Setting Up the Environment
==========================

Before we begin, let's set up our working environment.

Activate the Conda Environment
------------------------------

First, create the environment from the `environment.yml` file:

.. code-block:: bash

    conda env create -f environment.yml

Then, activate the provided conda environment that contains all necessary tools:

.. code-block:: bash

    conda activate metaproteomics_env

Start a SLURM Interactive Session (If Applicable)
-------------------------------------------------

If you're working on a cluster with the SLURM workload manager, start an interactive session:

.. code-block:: bash

    si -c 32 -t 120  # Allocates 32 cores for 120 minutes

Copy Data to Shared Memory for Faster Processing
------------------------------------------------

For improved performance, copy your data to the shared memory directory:

.. code-block:: bash

    cp -r data /dev/shm/metap_practical
    cd /dev/shm/metap_practical
