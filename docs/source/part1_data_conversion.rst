Part 1: Data Conversion
=======================

Overview
--------

Mass spectrometry data often comes in vendor-specific formats (e.g., Thermo `.raw` files). To process these files with most bioinformatics tools, we need to convert them into open formats like `mzML`, `MGF`, or `MS2`.

Task 1: Convert Thermo `.raw` File to `.ms2` Format
---------------------------------------------------

**Step 1: Navigate to the Data Directory**

.. code-block:: bash

    cd /dev/shm/metap_practical/data

**Step 2: Explore the ThermoRawFileParser Tool**

Use the help command to understand how to use the tool:

.. code-block:: bash

    mono ThermoRawFileParser.exe --help

**Step 3: Convert the Raw File**

Use the appropriate command-line options to convert `raw_file.raw` to `raw_file.ms2`.

*Hint*: Look for options that specify input (`-i`), output (`-o`), and format (`-f`).

.. code-block:: bash

    mono ThermoRawFileParser.exe -i=raw_file.raw -o=./output/ -f=0 -g -m=0

Questions
---------

- **Q1**: What does each option in the command you used mean?
- **Q2**: Why do we need to convert the raw file before analysis?
