Part 3: Running Searches
=======================

Overview
--------

Peptide-spectrum matching is performed using search engines that compare observed spectra with theoretical spectra derived from protein databases. In this part, you will configure and run the Sipros search engine.

Task 1: Set Up the Sipros Configuration File
--------------------------------------------------

**Step 1: Inspect the Sipros configuration file**

Open your Sipros configuration file and inspect the parameters. Ensure that the paths to the protein database and other necessary files are correctly set.

.. hint::

   .. toggle::

      Open and inspect the configuration file using a text editor. 
      Example using nano:
      `nano "${config_temp}"`
      Example using vim:
      `vim "${config_temp}"`

- **Q1:** What parameters are available in the Sipros configuration file?
- **Q2:** Which parameters do you think will have the most significant impact on the search results?
- **Q3:** Which parameters will have the most significant impact on the search speed?
- **Q4:** Why are some possible parameters not set by default?

Task 2: Prepare paths and files for search

**Step 1: Set Paths**

.. code-block:: bash

   # Paths to Sipros funtionalities
   sip_src="${sip_dir}/Scripts"
   sip_main="${sip_dir}/ReleaseOpenMP"

   # Make Sipros functionalities available in the PATH
   export PATH="${sip_src}:${sip_main}:$PATH"

   # We already prepares the config path in a previous task: ${config_temp}
   # We already prepared the ms2 data path in a previous task: ${ms2_path}

   # Path to the output directory
   out_dir="${session_path}/mp_practical/sipros_output"

   # Ensure the output directory exists
   mkdir -p "${out_dir}"

Task 2: Run Sipros Search

**Step 1: Check the Sipros command**

Inspect the Sipros command to ensure that the paths to the MS data, configuration file, and output directory are correctly set.

.. code-block:: bash

   Sipros_OpenMP -h

.. hint::

   Even though we just have a single filem, we need to input the directory containing the MS data files to avoid errors.

**Step 2: Run the search**

Execute the Sipros search for your sample.

.. hint::

   .. toggle::

      .. code-block:: bash

         Sipros_OpenMP -w "${ms2_path}" \
                       -c "${config_temp}" \
                       -o "${out_dir}"

**Step 3: Inspect the Sipros Output File**

Check the output directory to ensure that the Sipros search generated the expected files.

.. hint::

   .. toggle::

      .. code-block:: bash

         sipros_out_file=$(ls "${session_path}/mp_practical/sipros_output"/*)

         less -RS "${sipros_out_file}"

- **Q5:** What information does the output of the Sipros search contain?
- **Q6:** What information is missing?

**Step 4: Process Search Results**

Process the search results to generate PSM, Peptide and Protein files.

Check the help command of provided Python script to process the search results.

.. code-block:: bash

   # Path to the script directory
   script_dir="${course_dir}/scripts/mp_practical"

   # Check the help command
   "${py3}" "${script_dir}/converge_to_protein_decoy_fdr.py" -h

Use the provided Python script to adjust the FDR threshold and converge to a 1% protein FDR.

.. hint::

   .. toggle::

      .. code-block:: bash

         "${py3}" "${script_dir}/converge_to_protein_decoy_fdr.py" "${out_dir}" "${sip_src}" "${config_temp}" 1.0

- **Q6:** What was the final protein FDR?
- **Q7:** Why did we arrive at this threshold?
