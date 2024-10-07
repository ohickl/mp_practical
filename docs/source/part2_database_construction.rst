Part 2: Database Construction
=============================

Overview
--------

Creating an appropriate protein database is crucial for peptide identification. In metaproteomics, databases can be derived from metagenomic data, public repositories, or a combination of both. In this part, you will prepare the databases for the search, including generating decoy sequences.

Task 2: Build and Prepare Protein Databases
-------------------------------------------

**Note:** All required tools and paths are provided. You do not need to create a conda environment; the environment is already set up on the cluster. You will use the provided paths to access Sipros and other necessary tools.

**Step 1: Set Up Environment Variables and Paths**

Set up the necessary environment variables and paths. Replace `/path/to` with the actual paths provided on the cluster.

.. code-block:: bash

   ### Set the needed paths
   course_dir="/mnt/aiongpfs/projects/prospectomics_autumn_course"
   script_dir="${course_dir}/scripts"

   sip_dir="${course_dir}/tools/Sipros-Ensemble"

   template_dir="${sip_dir}/templates"
   config_template="${template_dir}/se_template.cfg"

   sip_src="${sip_dir}/Scripts"
   sip_main="${sip_dir}/ReleaseOpenMP"
   export PATH="${sip_src}:${sip_main}:$PATH"

   ### Set the Python executables
   # The conda environment's Python 2 is used for Sipros
   py2="${CONDA_PREFIX}/bin/python2.7"
   # The cluster's general Python 3 is used for other scripts
   py3="/usr/bin/python3"

   ### Set a sample name
   SAMPLE="sample1"

   ### Set session directories
   # Path to write the databse(s) to
   db_dir="${session_path}/mp_practical/databases"
   # Path to write the config file to
   cfg_dir="${session_path}/cfg"

   # Ensure the config directory exists
   mkdir -p "${cfg_dir}"

   # We will copy the config template for SiprosEnsemble to the session directory
   # Dont worry about Sipros and the config template for now, we will get to that later
   config_temp="${cfg_dir}/se_template.cfg"

   cp "${config_template}" "${config_temp}"

**Step 2: Choose and inspect databases**

.. code-block:: bash

   # Check the contents of the database directory
   ls -lSh "${databases_path}"

   # Inspect a database
   less -RS "${databases_path}/db_01.faa"

   # Use seqkit to get statistics on the database(s)
   seqkit -h

.. hint::
    Use `seqkit stats` to get statistics on databases

    .. toggle::

        .. code-block:: bash

             # Get statistics on the database
             seqkit stats -j 32 "${databases_path}"/*

Questions
---------

- **Q1:** How is the information in the database organized?
- **Q2:** How do the different databases differ in size and content?

**Step 3: Create Sipros Database with Decoy Sequences**

Inspect the config file and update the database path.

.. code-block:: bash

   ### Database paths
   # Path to the chosen raw database
   db_raw="${db_dir}/<CHOOSE_A_DATABSE_HERE>"

   # Path to the Sipros database
   db="${db_raw%.faa}_sipros_rev.faa"

   # Create the sessions database directory if it doesn't exist
   mkdir -p "${db_dir}"

   # Modify the config file to include the database path
   # Example using nano:
   # nano "${config_temp}"
   # Example using vim:
   # vim "${config_temp}"
   # Alternatively, you can use `sed` to replace placeholders in the template with the actual paths
   # Datbase path placeholder
   # Search name (for the search name): __SEARCH_NAME__

.. hint::

    In the config file, look for parameter `FASTA_Database` and replace the placeholder `__DB__` with the path to the raw database.

    Use the following sed command to automatically create the sample-specific config file:

    .. toggle::

        .. code-block:: bash

             sed -i -e "s|__DB__|${db_raw}|g" e "s|__SEARCH_NAME__|${SAMPLE}|g" "${config_temp}"

Generate the Sipros database with decoy sequences. This step is essential for estimating the False Discovery Rate (FDR) during the search.

.. code-block:: bash

   # Check the help command for the decoy database generation script
   "${py2}" "${sip_src}/sipros_prepare_protein_database.py"

   # Select the correct paths and create the Sipros database

.. hint::

    .. toggle::

        .. code-block:: bash

         "{py2}" "${sip_src}/sipros_prepare_protein_database.py" -i "${db_raw}" \
                                                                 -o "${db}" \
                                                                 -c "${config_temp}" 


Replace commas in the FASTA headers with semicolons to avoid issues during the search and update the config file with the new database path.

.. code-block:: bash

   # Replace commas in FASTA headers with semicolons to avoid issues
   sed -i -e 's/,/;/g' "${db}"

   # Update FASTA path in config


.. hint::

    Open the config file manually and replace the database path with the new database path.

    Use `sed` to replace the database path in the config file:

    .. toggle::

        .. code-block:: bash

             sed -i -e "s|${db_raw}|${db}|g" "${config_temp}"

Questions
---------

- **Q3:** Why is it important to generate decoy sequences in the database?
- **Q4:** Inspect the new database file. What changes do you observe?


.. hint::

    .. toggle::

      Use seqkit to inspect the new database file:

      .. code-block:: bash

           seqkit stats "${db}"

**Notes:**

- The `sipros_prepare_protein_database.py` script generates decoy sequences by reversing the protein sequences and appending them to the database.
- Updating the configuration file ensures that the search engine uses the correct database with decoy sequences.
