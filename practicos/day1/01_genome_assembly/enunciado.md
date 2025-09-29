# Practical 01 – Genome Assembly

## Introduction
In a genome sequencing project using Illumina technology, the DNA of the target organism is fragmented into millions of small pieces during library preparation. The resulting sequences (or reads) vary between 36 and 250 nucleotides in length. These reads can be single-end or paired-end.

Paired-end reads are generated when we sequence both ends of DNA fragments (which are much longer than the reads themselves). The sequencing proceeds inward from both ends, producing two reads per fragment, with a known approximate insert size (more precisely, a random variable with a known distribution). This additional information from paired-end reads is very useful during the genome assembly process.

Genome assembly refers to the process of taking a set of short DNA sequences (reads) that represent a random collection of the genome, and “stitching” them together to create a representation of the original chromosomes. The goal of an assembler is to produce long contiguous sequences (contigs) from these reads. This is a computationally complex problem, further complicated by the presence of repetitive sequences.

In this guide we will see how to:
- Obtain reads  
- Inspect the data and assess its quality  
- Filter and trim reads to improve quality if necessary  

For each step of the protocol, different tools are available, each with strengths and weaknesses. Instead of the suggested tools, other alternatives can be used depending on user preferences or the type of problem to be addressed.

---

## Data retrieval and manipulation

### SRA Toolkit: `fastq-dump` and `prefetch`
NCBI-SRA (https://www.ncbi.nlm.nih.gov/sra) is a bioinformatics database that serves as a public repository for primary sequence data, especially from second- and third-generation sequencing technologies. NCBI provides a set of tools, the **SRA Toolkit**, to facilitate access and download of files, as well as conversion into different formats.

The `prefetch` command takes as input an SRA accession number and downloads the file in the `.sra` binary format used by SRA. We then convert the `.sra` file into the desired format. To convert to FASTQ, we use `fastq-dump`:

```bash
fastq-dump SRR8922830.sra --split-files
