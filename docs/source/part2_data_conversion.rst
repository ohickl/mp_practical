Part 2: Database Construction
=============================

Overview
--------

Creating an appropriate protein database is crucial for peptide identification. In metaproteomics, databases can be derived from metagenomic data, public repositories, or a combination of both.

Task 2: Build Protein Databases
-------------------------------

**Step 1: Combine Databases**

- **Metagenomic Database**: Provided as `metagenomic.fasta`.
- **Human Proteome**: Provided as `human_proteome.fasta`.
- **CRAP Contaminant Database**: Provided as `contaminants.fasta`.

Combine these databases into one:

.. code-block:: bash

    cat metagenomic.fasta human_proteome.fasta contaminants.fasta > combined_metagenomic_db.fasta

Repeat the process to create a combined SwissProt database:

.. code-block:: bash

    cat swissprot.fasta human_proteome.fasta contaminants.fasta > combined_swissprot_db.fasta

**Step 2: Inspect Databases Using SeqKit**

Install SeqKit if not already installed:

.. code-block:: bash

    conda install -c bioconda seqkit

Get basic statistics:

.. code-block:: bash

    seqkit stats combined_metagenomic_db.fasta
    seqkit stats combined_swissprot_db.fasta

Visualize size distribution (if `hist` command is available):

.. code-block:: bash

    seqkit fx2tab -nl combined_metagenomic_db.fasta | cut -f2 | hist
    seqkit fx2tab -nl combined_swissprot_db.fasta | cut -f2 | hist

**Step 3: Add Decoy Sequences**

Use the provided script to append decoy sequences for false discovery rate (FDR) estimation:

.. code-block:: bash

    python ../scripts/decoy_generator.py combined_metagenomic_db.fasta decoy_metagenomic_db.fasta
    python ../scripts/decoy_generator.py combined_swissprot_db.fasta decoy_swissprot_db.fasta

Questions
---------

- **Q1**: How different in size and size distribution are the SwissProt and metagenomic databases?
- **Q2**: Is the human proteome a large contributor to the combined database size?
- **Q3**: Why do we add the human proteome and the CRAP contaminant database?
