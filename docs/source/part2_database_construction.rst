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

   # Set the needed paths
   course_dir="/mnt/aiongpfs/projects/prospectomics_autumn_course"
   script_dir="${course_dir}/local_tools/scripts"
   template_dir="${course_dir}/local_tools/templates"
   config_template="${template_dir}/se_template.cfg"

   sip_dir="${course_dir}/local_tools/Sipros-Ensemble"
   sip_src="${sip_dir}/Scripts"
   sip_main="${sip_dir}/ReleaseOpenMP"
   export PATH="${sip_src}:${sip_main}:$PATH"

   # Set the Python executables
   py2="${CONDA_PREFIX}/bin/python2.7"  # Using the conda environment's Python 2
   py3="/usr/bin/python3"    # Using the cluster's general Python 3

   # Set the sample name (Replace 'sample1' with your assigned sample)
   SAMPLE="sample1"

   # Set session directories
   session_dir="/dev/shm/metap_practical"
   db_dir="${proj_dir}/db/${SAMPLE}"
   cfg_dir="${proj_dir}/cfg"
   sample_config_file="${cfg_dir}/se_${SAMPLE}.cfg"

   # Database paths
   db_raw="${db_dir}/proteomics.final.faa"
   db="${db_raw%.faa}_sipros_rev.faa"

**Step 2: Create Sipros Database with Decoy Sequences**

Generate the Sipros database with decoy sequences. This step is essential for estimating the False Discovery Rate (FDR) during the search.

.. code-block:: bash

   # Create the database directory if it doesn't exist
   mkdir -p "${db_dir}"

   # Check if the Sipros database already exists
   if [[ ! -f "${db}" ]]; then
     echo "Creating Sipros DB for sample: ${SAMPLE}"

     # Create sample-specific config from the template
     sed -e "s|__DB__|${db_raw}|g" \
         -e "s|__SEARCH_NAME__|${SAMPLE}|g" "${config_temp}" > "${config}"

     # Add Sipros decoys
     "${py2}" "${sip_src}/sipros_prepare_protein_database.py" -i "${db_raw}" \
                                                              -o "${db}" \
                                                              -c "${config}" > "${db_dir}/sipros_prep_db.log" 2>&1

     # Replace commas in FASTA headers with semicolons to avoid issues
     sed -i -e 's/,/;/g' "${db}"

     # Update FASTA path in config
     sed -i -e "s|${db_raw}|${db}|g" "${config}"
   else
     echo "Sipros DB already exists for sample: ${SAMPLE}"
     # Create sample-specific config from the template
     sed -e "s|__DB__|${db}|g" \
         -e "s|__SEARCH_NAME__|${SAMPLE}|g" "${config_temp}" > "${config}"
   fi

**Notes:**

- The `sipros_prepare_protein_database.py` script generates decoy sequences by reversing the protein sequences and appending them to the database.
- Updating the configuration file ensures that the search engine uses the correct database with decoy sequences.

Questions
---------

- **Q1:** Why is it important to generate decoy sequences in the database?
- **Q2:** How do decoy sequences help in estimating the FDR during the search?

---

