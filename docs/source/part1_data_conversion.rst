Part 1: Data Conversion
=======================

Overview
--------

Mass spectrometry data often comes in vendor-specific formats (e.g., Thermo `.raw` files). To process these files with most bioinformatics tools, we need to convert them into open formats like `mzML`, `MGF`, or `MS2`.

Task 1: Convert Thermo `.raw` File to `.ms2` Format
---------------------------------------------------

**Step 1: Navigate to the Data Directory and Activate Environment (If Not Already Done)**

.. code-block:: bash

    # Path to the shared memory directory
    node_memory_path="/dev/shm"

    # Move to the shared memory directory
    cd "${node_memory_path}/mp_practical"

    # Activate the provided conda environment
    course_path="/mnt/aiongpfs/projects/prospectomics_autumn_course"
    env_path="${course_path}/data/mp_practical/env/metaproteomics_env"

    # Load conda module
    module load lang/Anaconda3/2020.11

    # Ensure conda is properly initialized
    conda init bash
    source ~/.bashrc

    conda activate "${env_path}"

**Step 2: Explore the ThermoRawFileParser Tool**

Use the help command to understand how to use the tool:

.. code-block:: bash

    ThermoRawFileParser --help

**Step 3: Convert the Raw File**

Use the appropriate command-line options to convert `raw_file.raw` to `raw_file.mgf`.

.. hint::
    Look for options that specify input (`-i`), output (`-o`), and format (`-f`).

    .. toggle::
        
        .. code-block:: bash

            ThermoRawFileParser -i="${raw_data_path}/M11-01_V2/E4.raw" -o="${mgf_path}" -f=0 -g -m=0

Questions
---------

- **Q1**: What does each option in the command you used mean?
- **Q2**: Why do we need to convert the raw file before analysis?

Task 2: Explore the Created MGF File
------------------------------------

**Step 1: List the Contents of the MGF Directory**

Navigate to the directory containing the converted MGF files and list its contents:

.. code-block:: bash

    ls -lh "${mgf_path}"

**Step 2: View the MGF File**

Use a text viewer like `less` or `cat` to open and explore the contents of the MGF file:

.. code-block:: bash

    less "${mgf_path}/E4.mgf"

- **Q3**: What do you see in the MGF file?
- **Q4**: What information about the spectra is present in the MGF file?

**Step 3: Count the Number of Spectra**

Count the number of spectra in the MGF file.

.. hint::
    .. toggle::
        Look for lines starting with `TITLE` and count them.

        .. code-block:: bash

            grep "^TITLE" "${mgf_path}/E4.mgf" | wc -l

**Step 4: Identify Charge States**

Identify the different charge states present in the MGF file.

.. hint::
    .. toggle::
        Look for lines starting with `CHARGE` and list unique values.

        .. code-block:: bash

            grep "^CHARGE" "${mgf_path}/E4.mgf" | sort | uniq

Questions
---------

- **Q5**: How many spectra did you find in the MGF file?
- **Q6**: What charge states are present in the MGF file?

**Step 5: Convert MGF File to MS2 Format**

Use the provided script to convert the `mgf` file to `ms2` format.

.. code-block:: bash

    # Path to the conversion script
    conversion_script_path="${course_path}/scripts/mgf_to_ms2_converter.sh"

    # Run the conversion script
    bash "${conversion_script_path}" "${mgf_path}/E4.mgf" "${ms2_path}/E4.ms2"

- **Q7**: What command did you use to convert the MGF file to MS2 format?
- **Q8**: What differences do you observe between the MGF and MS2 files?

**Step 6: Compare MGF and MS2 Files**

Compare the contents of the MGF and MS2 files to understand the differences.

.. code-block:: bash

    # View the MS2 file
    less "${ms2_path}/E4.ms2"

- **Q9**: What information is present in the MS2 file that is not in the MGF file?
- **Q10**: Why might different formats be useful for different types of analysis?