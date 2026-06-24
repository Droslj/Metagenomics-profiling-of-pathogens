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


## Key Results & Assembly Statistics

The assembly produced a low-complexity, high-contiguity result consistent with a dominant bacterial isolate.<br> 
The complete list of organisms identified is available here:<br>

![View EggNOG Report](/Images/organisms_identified.tabular)

## Functional Annotation & Virulence Factors

Annotation of 5,800+ genes revealed a complex pathogenic toolkit, primarily localized on highly mobile genetic elements:

(1) Hemolysin Operon (hlyCABD) 
I identified complete Type I Secretion System (T1SS) on contig k141_656. This system is responsible for the secretion of RTX toxins (hemolysins) that damage host cell membranes.

(2) Toxin-Antitoxin (TA) Systems 
Detection of vagC and vapC modules, suggesting a plasmid-stabilization mechanism that ensures the retention of virulence factors.

(3) Mobility 
Presence of Transposases and Phage Integrases adjacent to virulence genes, indicating high potential for horizontal gene transfer.

# The "Big Three" Domains

You have successfully detected life from across the biological spectrum:

(1) Bacteria: The vast majority of your 5,800 genes belong here (Proteobacteria, Firmicutes, Actinobacteria).

(2) Viruses: You found Myoviridae, Siphoviridae, and Podoviridae. These are Bacteriophages—viruses that infect and kill the bacteria in your sample.

(3) Eukaryota/Metazoa: Seeing "Metazoa" (animals) and "Opisthokonta" (fungi/animals) usually indicates DNA from the host (if this was a clinical sample) or microscopic multicellular life in the environment.

# The Dominant "Pathogen" Signature

The entries labeled "155864.EDL933" are the most important for your earlier discovery.

EDL933 is the specific name of the original "reference" strain for E. coli O157:H7.

The fact that you have multiple specific EDL933 sub-strains listed confirms that your "Big Winner" bacterium is indeed this specific foodborne pathogen.

## Technical Troubleshooting & Cloud Optimization

A significant portion of this project involved managing cloud-scale computational constraints:

(1) Resource Allocation 
Resolving "Out of Memory" (OOM) errors during binning by adjusting nextflow.config to optimize RAM usage on a 112GB GCP instance.

(2) Modular Execution: 
Utilizing Nextflow's -resume feature and selective module skipping (skip_gtdbtk) to prioritize functional output over high-resource taxonomic indexing.


**References**
[1] PulseNet Escherichia coli and Shigella genome sequencing, https://www.ncbi.nlm.nih.gov/bioproject/PRJNA218110 (SRR14256425)
