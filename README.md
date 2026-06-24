# Metagenomics-profiling-of-pathogens
Metagenomic Characterization and Virulence Profiling of Shiga Toxin-producing E. coli (STEC) using nf-core/mag

**Keywords**<br>
Nextflow (nf-core) pipeline orchestration, Cloud infrastructure (Seqera/GCP), Troubleshooting resource allocation for high-memory tasks, Functional annotation, Identifying pathogenic virulence factors

**Platform used**
Infrastructure: Seqera Platform / Google Cloud Platform
Pipeline: nf-core/mag v3.01

**Overview**
This project involved the end-to-end bioinformatic processing of metagenomic short-read data. Data was taken from the following project [1].<br> 
The analysis successfully reconstructed a high-quality genome of Escherichia coli O157:H7 strain and identified critical virulence factors, including hemolysin secretion systems and toxin-antitoxin modules, indicating a highly pathogenic profile.
<br>
# Workflow and Methodology overview
<br>
The analysis was performed using the nf-core/mag Pipeline using an orchestrated environment:<br>
 (1) Pipeline Deployment <br>
 Orchestrated nf-core/mag pipeline was used to process multi-sample metagenomic datasets. Pipeline was configured to perform raw read QC, assembly, binning, and annotation.<br>
 
 (2) Cloud Infrastructure & Optimization <br> 
 Managed cloud computing resources via Seqera Platform required optimizing nextflow.config settings (CPUs, RAM, Time) to handle memory-intensive tasks and resolving Google Bucket storage bottlenecks.<br>

 (3) Genomic Assembly & QC <br>
 Pipeline produces some high-quality assemblies (N50 > 100 Kbp) using SPAdes and MEGAHIT. Results were validated using QUAST and MultiQC reports.<br>
 
 (4) Pathogen Identification <br>
 Escherichia coli O157:H7 from environmental/clinical samples was identified and characterized by analyzing Prodigal gene predictions and EggNOG-mapper functional assignments.<br>
 
 (5) Functional Genomics <br>
 Using characterization of specialized metabolic pathways, including Type I Secretion Systems (Hemolysin) and Toxin-Antitoxin modules, bacterial virulence and mobility was assesed.<br>
 <br>
## Quality Control
<br>
Read quality was assessed via FastQC; MultiQC was used to aggregate global run statistics.<br>
The report is available here:<br>
<br>
![MultiQC report](/Images/multiqc_report.html)
<br>

## Assembly

Comparative assembly was performed using MEGAHIT and SPAdes.
The report is available here:

![Assembly report]((https://raw.githubusercontent.com/Droslj/Images/Metagenomics-profiling-of-pathogens/main/results_v3_Assembly_MEGAHIT_QC_SRR14256425_QUAST_report.html)

![View QUAST Report](https://raw.githubusercontent.com/Droslj/Metagenomics-profiling-of-pathogens/main/results_v3_Assembly_MEGAHIT_QC_SRR14256425_QUAST_report.html)

## Binning

Metagenome-assembled genomes (MAGs) were recovered using MetaBAT2.

## Annotation

Structural annotation was conducted via Prodigal, with functional and taxonomic assignment performed through EggNOG-mapper.

## Key Results & Assembly Statistics

The assembly produced a low-complexity, high-contiguity result consistent with a dominant bacterial isolate.
    
## Functional Annotation & Virulence Factors
  
Annotation of 5,800+ genes revealed a complex pathogenic toolkit, primarily localized on highly mobile genetic elements:
(1) Hemolysin Operon (hlyCABD) 
I identified complete Type I Secretion System (T1SS) on contig k141_656. This system is responsible for the secretion of RTX toxins (hemolysins) that damage host cell membranes.

(2) Toxin-Antitoxin (TA) Systems 
Detection of vagC and vapC modules, suggesting a plasmid-stabilization mechanism that ensures the retention of virulence factors.

(3) Mobility 
Presence of Transposases and Phage Integrases adjacent to virulence genes, indicating high potential for horizontal gene transfer.

## Technical Troubleshooting & Cloud Optimization

A significant portion of this project involved managing cloud-scale computational constraints:
(1) Resource Allocation 
Resolving "Out of Memory" (OOM) errors during binning by adjusting nextflow.config to optimize RAM usage on 112GB GCP instances.

(2) Modular Execution: 
Utilized Nextflow’s -resume feature and selective module skipping (skip_gtdbtk) to prioritize functional output over high-resource taxonomic indexing.


3. The "Technical Skills" Checklist
You can now honestly add these to your skills section:

Bioinformatics Tools: Nextflow (nf-core), MEGAHIT, SPAdes, MetaBAT2, Prodigal, EggNOG-mapper, QUAST, MultiQC.

Platforms: Seqera (formerly Tower), Google Cloud Platform (GCP).

Languages/Formats: Bash/Linux CLI, GFF3, FASTA/FASTQ, YAML/Nextflow Config.

4. How to Talk About the "Troubleshooting" (The Star Method)
In an interview, if they ask about a challenge, tell the story of the "13-hour bottleneck":
Situation: The pipeline was hanging on heavy binning tasks (COMEBin/MetaBinner) due to resource limits.
Task: I needed to complete the analysis and get the functional results without starting from scratch.
Action: I analyzed the pipeline_info logs, modified the Nextflow configuration to skip failing non-essential modules, and used the -resume feature to salvage the 600+ successfully completed tasks.
Result: The run completed successfully, allowing for the identification of a pO157 virulence plasmid in the sample.

A Final Polish
Since you successfully identified the pO157 virulence markers, you might want to call this project:
"Metagenomic Characterization and Virulence Profiling of Shiga Toxin-producing E. coli (STEC) using nf-core/mag."

**References**
[1] PulseNet Escherichia coli and Shigella genome sequencing, https://www.ncbi.nlm.nih.gov/bioproject/PRJNA218110 (SRR14256425) 
