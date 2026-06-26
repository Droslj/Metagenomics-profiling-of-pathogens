# Metagenomics-profiling-of-pathogens
Metagenomic Characterization and Virulence Profiling of Shiga Toxin-producing E. coli (STEC) using nf-core/mag

**Keywords**<br>
Nextflow (nf-core) pipeline orchestration, Cloud infrastructure (Seqera/GCP), Troubleshooting resource allocation for high-memory tasks, Functional annotation, Identifying pathogenic virulence fact[...]

**Platform used**
Infrastructure: Seqera Platform / Google Cloud Platform
Pipeline: nf-core/mag v3.01

**Overview**
This project involved the end-to-end bioinformatic processing of metagenomic short-read data. Data was taken from the following project [1].<br> 
The analysis successfully reconstructed a high-quality genome of Escherichia coli O157:H7 strain and identified critical virulence factors, including hemolysin secretion systems and toxin-antitoxin system

<br>

# Workflow and Methodology overview
<br>
The analysis was performed using the nf-core/mag Pipeline using an orchestrated environment:<br>

 (1) Pipeline Deployment <br>
 Orchestrated nf-core/mag pipeline was used to process multi-sample metagenomic datasets. Pipeline was configured to perform raw read QC, assembly, binning, and annotation.<br>
 
 (2) Cloud Infrastructure & Optimization <br> 
 Managed cloud computing resources via Seqera Platform required optimizing nextflow.config settings (CPUs, RAM, Time) to handle memory-intensive tasks and resolving Google Bucket storage bottlenec[...]

 (3) Genomic Assembly & QC <br>
 Pipeline produces some high-quality assemblies (N50 > 100 Kbp) using SPAdes and MEGAHIT. Results were validated using QUAST and MultiQC reports.<br>
 
 (4) Pathogen Identification <br>
 Escherichia coli O157:H7 from environmental/clinical samples was identified and characterized by analyzing Prodigal gene predictions and EggNOG-mapper functional assignments.<br>
 
 (5) Functional Genomics <br>
 Using characterization of specialized metabolic pathways, including Type I Secretion Systems (Hemolysin) and Toxin-Antitoxin modules, bacterial virulence and mobility was assesed.<br>
 <br>

Complete workflow is provided on Figure 1:<br>
<br>

![Workflow](/Images/Complete_flow.png)

**Figure 1: Complete workflow**



## Quality Control
<br>
Read quality was assessed via FastQC; MultiQC was used to aggregate global run statistics.<br>
The report is available here:<br>
<br>

[View MultiQC Report](https://droslj.github.io/Metagenomics-profiling-of-pathogens/Images/multiqc_report.html)

<br>

## Assembly

Comparative assembly was performed using MEGAHIT and SPAdes.
The report is available here:

[View QUAST Report](https://droslj.github.io/Metagenomics-profiling-of-pathogens/Images/results_v3_Assembly_MEGAHIT_QC_SRR14256425_QUAST_report.html)

## Binning

Metagenome-assembled genomes (MAGs) were recovered using MetaBAT2.


## Annotation

Structural annotation was conducted via Prodigal, with functional and taxonomic assignment performed through EggNOG-mapper. Prodigal identified ~6.000 faa sequences (for one of the samples, the SRR14256425) which I ran through EggNOG mapper.
The report is available here:

![View EggNOG Report](/Images/eggNOG.tabular)

This list contains about 5,800+ annotated genes.

## Key Results & Assembly Statistics

The assembly produced a low-complexity, high-contiguity result consistent with a dominant bacterial isolate.<br> 
The complete list of organisms identified is available here:<br>

![Microorganisms detected in the isolate](/Images/organisms_identified.png)

# Functional profiling of transcripts

In order to tvsualize the functional landscape of the isolate I used the COG_category. EggNOG mapper attempts to assign functionality to all the transcripts in the COG_category column. Due to the design, when a protein has multiple functions, eggNOG-mapper assigns multiple letters to it (like CE, CF, JKL, or OU). So, before visualization, it was neccessary to collapse the categories to primary category. This gives a better overview of the functional landscape of this isolate (see Figure 2).

![Functional landscape](/Images/COG_categories.png)

**Figure 2: Functional landscape of the isolate SRR14256425**

**Comments on Functional landscape**

Following significant categories can be observed in this schematic:<br>
<br>
(1) Metabolic section <br>
Significant blocks representing Amino acid transport, Carbohydrate transport, and Inorganic ion transport show an organism that is heavily optimized to scavenge nutrients rapidly, which is highly characteristic of a gut-dwelling enteric bacterium like Escherichia coli.<br>
<br>
(2) Growth section<br>
Significant portion of the isolate represents sections dedicated to Transcription, Translation, and Replication, recombination and repair which indicates a highly active, rapidly dividing cell population.<br>
<br>
(3) Specialized sections<br>
There are distinct slices for Cell motility and Defense mechanisms which prove that this isn't just a passive microbe; it has the active machinery to move, colonize, and defend itself against host threats or antibiotics.<br>
<br>
## Cell motility

This category contains flagellar and chemotactic genes. Cell motility is an important factor in virulence, and a high number of entries in this section means that the isolate is highly mobile and optimized for colonization.<br> 
This indicates that pathogen contains an intact, multi-operon flagellar regulon scattered across a few highly contiguous assembly blocks. Elements from the following functional categories were detected:<br>
 (1) Flagellar Biogenesis genes (fli & flg genes)<br>
 (2) Chemotaxis genes (che & tar/tsr genes).<br>
<br>
## Virulence / Defense mechanisms
<br>
This category defines the pathogenic potential of the strain. Following elements were detected:<br>
 - Hemolysin Operon (hlyCABD) => Identified a complete Type I Secretion System (T1SS). This system is responsible for the secretion of RTX toxins (hemolysins) that damage host cell membranes<br>
 - Toxin-Antitoxin (TA) Systems => Detection of vagC and vapC modules, suggesting a plasmid-stabilization mechanism that ensures the retention of virulence factors<br>
 - Mobility => Presence of Transposases and Phage Integrases adjacent to virulence genes, indicating high potential for horizontal gene transfer<br>
<br>

# Technical Troubleshooting & Cloud Optimization

A significant portion of this project involved managing cloud-scale computational constraints:<br>

(1) Resource Allocation <br>
Resolving "Out of Memory" (OOM) errors during binning by adjusting nextflow.config to optimize RAM usage on a 112GB GCP instance.<br>

(2) Modular Execution: <br>
Utilizing Nextflow's -resume feature and selective module skipping (skip_gtdbtk) to prioritize functional output over high-resource taxonomic indexing.<br>
<br>

**References**<br>
[1] PulseNet Escherichia coli and Shigella genome sequencing, https://www.ncbi.nlm.nih.gov/bioproject/PRJNA218110 (SRR14256425)
