Part 3: Running Searches
=======================

Overview
--------

Peptide-spectrum matching is performed using search engines that compare observed spectra with theoretical spectra derived from protein databases. In this part, you will configure and run the Sipros search engine.

Task 3: Configure and Run the Sipros Search Engine
--------------------------------------------------

**Note:** The conda environment is already set up on the cluster, and you have access to all required tools and paths.

**Step 1: Set Up Environment Variables and Paths**

Ensure that the environment variables and paths from Part 2 are set. If you have not closed your terminal session since Part 2, these variables should still be available.

.. code-block:: bash

   # If necessary, set the sample name and paths again
   SAMPLE="sample1"  # Replace 'sample1' with your sample name

   proj_dir="/dev/shm/metap_practical"
   script_dir="${proj_dir}/scripts"
   cfg_dir="${proj_dir}/cfg"
   config="${cfg_dir}/se_${SAMPLE}.cfg"

   ms_data="${proj_dir}/data/ms2/${SAMPLE}"
   out_dir="${proj_dir}/output/${SAMPLE}_acetyl_ptm"

**Step 2: Create Output Directory**

Create the output directory where the search results will be stored.

.. code-block:: bash

   mkdir -p "${out_dir}"

**Step 3: Run Sipros Search**

Execute the Sipros search for your sample.

.. code-block:: bash

   echo "Running Sipros search for sample: ${SAMPLE}"
   Sipros_OpenMP -w "${ms_data}" \
                 -c "${config}" \
                 -o "${out_dir}" > "${out_dir}/sipros_search.log" 2>&1

**Notes:**

- The `-w` option specifies the directory containing the MS data files (`.ms2` files).
- The `-c` option specifies the configuration file.
- The `-o` option specifies the output directory.

**Step 4: Process Search Results**

Process the search results to generate peptide XML files and apply initial filtering.

.. code-block:: bash

   echo "Processing Sipros search results for sample: ${SAMPLE}"
   runSiprosFiltering_make_pepxml.sh -in "${out_dir}" \
                                     -o "${out_dir}" \
                                     -c "${config}" > "${out_dir}/sipros_filter.log" 2>&1

**Notes:**

- The `runSiprosFiltering_make_pepxml.sh` script processes the Sipros output and generates `.pepxml` files for downstream analysis.
- Ensure that this script is accessible in your `PATH` or provide the full path to it.

**Step 5: Converge to 1% Protein FDR**

Use the provided Python script to adjust the FDR threshold and converge to a 1% protein FDR.

.. code-block:: bash

   echo "Converging to 1% protein FDR for sample: ${SAMPLE}"
   "${py3}" "${script_dir}/converge_to_protein_decoy_fdr.py" "${out_dir}" "${sip_src}" "${config}" 1.0 >> "${out_dir}/sipros_filter_converge.log" 2>&1

**Notes:**

- The `converge_to_protein_decoy_fdr.py` script iteratively adjusts the filtering thresholds to achieve the desired protein-level FDR.
- The `1.0` at the end specifies the target FDR percentage (1%).

Questions
---------

- **Q1:** What does the Sipros search engine do with the MS data and the protein database?
- **Q2:** Why is it necessary to process the search results before interpreting them?
- **Q3:** How does adjusting the FDR threshold affect your search results?

---

