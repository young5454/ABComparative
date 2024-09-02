# ABComp : Assembly Polishing and Bacterial Whole-genome Comparison Pipeline for Multi-group Clinical Isolates

[![Snakemake](https://img.shields.io/badge/snakemake-≥6.3.0-brightgreen.svg)](https://snakemake.github.io)


A Snakemake workflow for `<description>`

# Getting Started
## Directory structure and file names
ABComp starts with two main type of input data, the draft assembly of clinical isolates and Illumina paired-end short reads. In order for ABComp to run smoothly, the input data should be saved in a proper format of both directory structure and file names. 

The genomes and reads should be saved inside the workspace directory as below :

```
0.Assembly
└── {group}_{strain}
    ├── genome
    │   └── {group}_{strain}.fasta
    └── reads
        ├── {group}_{strain}_1.fastq.gz
        └── {group}_{strain}_2.fastq.gz
```

⚠️ Make sure all the file extensions strictly follow the names (`.fasta`, `.fastq.gz`) provided above. Also, the fastq read files should be gzipped beforehand. The directory names should **NOT** be altered.

⚠️ Make sure your `{group}` and `{strain}` names do **NOT** contain a ‘_’ inside. The name formats are intended to have the unique ‘_’ so that during the Snakemake run, the group and strain wildcard info are automatically extracted and saved. Also, the file names should perfectly match the names provided in the configuration file (please see section “Configuration file”).

The assembly polishing block will create a directory `trimmed/` inside each `{group}_{strain}/` folder after running adapter trimming, and save the trimmed reads. Also, the polished assemblies will be saved inside the `genome/` directory with a name format of `{group}_{strain}_polished.fasta`. 

⚠️ If you have your assemblies ready and plan to run only the downstream comparative analysis block, please save all of your genome assemblies with the name format of `{group}_{strain}_polished.fasta` inside the `genome/` directory.

## Configuration file
ABComp requires two configuration files for running the pipeline. The yaml files can be found in the `config/` directory. 

`config.yml` is a default configuration setting for the overall Snakemake run. Make sure you specify the correct parameters and directory names of your preference.

`groups_original.yml` is a configuration file for the complete group-strain information of your clinical isolates. Below is an example yaml file of a 2-group, 5-strain setting :

```
NONMDR:
    - B0112
    - C0234
    - C3455
MDR:
    - B0232
	- D0991
```
⚠️ Make sure that the group and strain names perfectly match with the draft assembly name format : `{group}_{strain}.fasta`. For example, the B0112 strain should have the directory and file name of `NONMDR_B0112`.

## Conda environments
ABComp suggests single conda environment for a single software for avoiding potential dependency errors. Before running the pipeline, make sure you have all of the environments set with the provided yaml files. The yaml file of each individual environment can be found in `workflow/envs/`.

ABComp provides a single bash script `create_envs.sh` that creates all of the required conda environments at once. You can run the script with the following command :
```
bash create_envs.sh 
```

## Pre-downloaded databases
ABComp requires pre-downloaded databases for running **BUSCO** assembly assessment and **EggNOG-mapper**. Please refer to the website or Github of each software for more information :

**BUSCO** : https://busco.ezlab.org/

**EggNOG-mapper** : https://github.com/eggnogdb/eggnog-mapper

### BUSCO lineages
The BUSCO lineages