---

# AlphaFold Codes and Optimizations

Hi, these are the codes used for running AlphaFold version 2.3.1 on the JHPCE cluster via a Secure SHell (SSH). There are alot of preperations you must make before you can start running the batches. 

## Contents:

- Automatic .fasta files maker from an Excel spreadsheet (via python)
- Automatic .sh & .slurm files maker (via python)
- Automatic iptm_cs file maker (via python)
- Codes needed to for running jobs on alphafold (linux/SSH)

## Making .fasta files  

### 1. Make an Excel spreadsheet for all protein info

a. Make a new Excel Workbook and name the spreadsheet something easy to type (ex, "protein")

b. Name cell A1: "Sequence Name" and B1: "Sequence" 

c. Copy/paste all the sequence names/WP tags in column A and all the amino acid sequences into column B

### 2. Edit Python .fasta writer file

The .fasta writer will create fasta files and make folders with however number of fasta files you want for each folder
- The folders will be named {sm_prot}batch#, and will increase in number as more folders are created
- This way, you will not need to make folders within the SSH system

Grab the file "PYTHON .fasta file writer" and edit the following code within the file: 

```python
INPUT_FILE        = r"Path_to_your_Excel_file.xlsx"  # Right click on your file and copy the path
SHEET_NAME        = "your_sheet_name" # Change this to your excel sheet name (case sensitive!)
COLUMN1           = "Sequence Name"   # Excel column header for sequence names
COLUMN2           = "Sequence"        # Excel column header for sequences
OUTPUT_DIR        = r"path_to_your_desired_directory"  # Path to output directory/file (where u want your files to go)

CONST_HEADER      = ">name_of_prot"  # (ex: ">EGF")
CONST_SEQUENCE    = ("amino_acid_seq") # (ex: "NSDSECPLSHDGYCLHDGVCMYIEALDKYACNCVVGYIGERCQYRDLKWWELR")

SM_PROT           = "T"   # e.g. 'E' for EGF, 'T' for TGFa
FILES_PER_FOLDER  = 15    # how many fasta files per subfolder
```

### 3. Run the code on Python

Just copy and paste the code! 

## Making .sh or .slurm files 

Personalize the following code: 

```python
# === Configuration ===
START_BATCH     = 1    # Starting batch number (inclusive)
END_BATCH       = 5    # Ending batch number (inclusive)
SM_PROT         = "T"  # e.g. "E" for EGF, "T" for TGFa, etc.

USER_NAME       = "(Your username)"     #Input ur SSH/JHPCE username

# Base directory where the code will create a single batch folder
SCRIPT_DIR      = r"C:\path\to\your\output\directory"  # Path to the output folder on your computer
```

### Changing the cluster directories

You can change the paths to the following cluster/ssh directories if you wish to place ur files: 

- For the "run_[]batch[]_wrapper.sh" files, the files are ASSUMED to be placed in your account's main directory, under the "alphafold" folder. However, if you would like to change this, edit the following code on the main code file:

```python
SH_PATH   = "/users/{user_name}/alphafold/"    # On-cluster path to the .sh wrapper files (where u put the .sh files)
```

Additionally, if you want to change the output directory from fastscratch (up to 1Tb of disk space) to your user directory (up to 100 Gb of disk space), change the path in the following code: 

```python
FASTA_DIR_TEMPLATE = "/fastscratch/myscratch/{user_name}/alphafold/fasta_{sm_prot}batch{batch}"
OUT_DIR_TEMPLATE   = "/fastscratch/myscratch/{user_name}/alphafold/outputs_{sm_prot}batch{batch}"
```



