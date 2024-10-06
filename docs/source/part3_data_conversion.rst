Part 3: Running Searches
========================

Overview
--------

Peptide-spectrum matching is performed using search engines that compare observed spectra with theoretical spectra derived from protein databases.

Task 3: Configure and Run the Search Engine
-------------------------------------------

**Step 1: Inspect the Search Engine Parameter File**

Open the parameter file `search_params.txt` in a text editor:

.. code-block:: bash

    nano search_params.txt

Adjust parameters as necessary, such as enzyme specificity, modifications, and mass tolerances.

**Step 2: Run the Search**

Execute the search engine (e.g., MSFragger, Comet):

.. code-block:: bash

    search_engine -i raw_file.ms2 -d decoy_metagenomic_db.fasta -p search_params.txt -o search_results.tsv

Replace `search_engine` with the actual command for the search engine you are using.

**Step 3: Post-Process Results**

Use a custom script to filter results at a 1% decoy FDR:

.. code-block:: bash

    python ../scripts/fdr_filter.py -i search_results.tsv -fdr 0.01 -o filtered_results.tsv

**Step 4: Adjust FDR Threshold**

Try different FDR thresholds (e.g., 5%) and observe the changes:

.. code-block:: bash

    python ../scripts/fdr_filter.py -i search_results.tsv -fdr 0.05 -o filtered_results_5perc.tsv

Questions
---------

- **Q1**: How many human proteins did you identify?
- **Q2**: Which is the most abundant protein in the sample?
- **Q3**: Are there contaminants present in the results?
- **Q4**: How does adjusting the FDR threshold affect the number of identifications?
