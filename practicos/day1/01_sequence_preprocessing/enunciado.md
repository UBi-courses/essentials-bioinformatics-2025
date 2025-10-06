# Practical 01 – Sequencing Reads and Quality Control

## Introduction
In a genome sequencing project using Illumina technology, the DNA of the target organism is fragmented into millions of small pieces during library preparation. The resulting sequences (reads) vary between ~36 and 300 nucleotides in length, depending on the platform. These reads can be **single-end** or **paired-end**.


Paired-end reads are generated when both ends of DNA fragments are sequenced (the fragments are longer than the reads themselves). Sequencing proceeds inward from both ends, producing two reads per fragment with a known approximate insert size (more precisely, a random variable with a known distribution). This additional information from paired-end reads is very useful during the genome assembly process.  


![short insert](img/shor-insert.png)  
&nbsp;

Genome assembly refers to the process of taking a set of short DNA sequences (reads) that represent a random collection of the genome, and “stitching” them together to create a representation of the original chromosomes. The goal of an assembler is to produce long contiguous sequences (contigs) from these reads. This is a computationally complex problem, further complicated by the presence of repetitive sequences in genomes.

In this guide we will:
- Obtain sequencing reads  
- Inspect and evaluate read quality  
- Filter and trim reads to improve quality if necessary  

For each step of the protocol, different tools are available, each with strengths and weaknesses. The suggested tools can be replaced with alternatives depending on user preference or the specific problem.

---

## Data retrieval and manipulation

### SRA Toolkit: `fastq-dump` and `prefetch`
NCBI-SRA (https://www.ncbi.nlm.nih.gov/sra) is a public repository for primary sequence data, especially from second- and third-generation sequencing technologies. NCBI provides the **SRA Toolkit**, which facilitates data download and conversion into different formats.

- `prefetch` downloads an SRA accession in its native `.sra` binary format.  
- `fastq-dump` converts `.sra` files into FASTQ format.  

Example workflow:
```bash
# only an example, do not run it
prefetch SRR8922830
fastq-dump --split-files SRR8922830.sra
```
Alternatively, fastq-dump can take the accession number directly, downloading and converting on the fly:

```bash
# download only 1 million reads
fastq-dump -X 1000000 --split-files SRR8922830
```


Here:

-X 1000000 → download only 1 million reads
--split-files → generate two FASTQ files for paired-end data

### FASTQ format
Each read is encoded in four lines:

1. @ + identifier (with sequencing metadata)
2. The raw nucleotide sequence
3. \+ (optionally repeating the identifier)
4. Quality string (Phred scores encoded in ASCII)

Each base in line 2 has a corresponding quality score in line 4.

### Quality control
High-throughput sequencing generates millions of reads, but sequencing technologies are not error-free. Each instrument may introduce different errors. Quality control (QC) is therefore an essential first step.

Phred score
The Phred quality score (Q) measures base call accuracy:

$𝑄 = −10log 10(𝑃)$

Where *P* is the probability of error.

* Q10 → 1 error every 10 bases (10% error)
* Q20 → 1 error every 100 bases (1% error)
* Q30 → 1 error every 1000 bases (0.1% error)

👉 Discussion: What is the lowest Phred score you would expect in your reads?

Sequencing depth
Sequencing depth (C) is the average number of times each nucleotide is sequenced:

$𝐶=𝑁L/𝐺$

Where:
N = number of reads
L = read length
G = genome size

From C we can estimate:

Expected coverage: $1−𝑒^-𝐶$
Expected number of contigs: $𝑁𝑒^−𝐶$


### FastQC
FastQC provides a quick, graphical summary of sequence quality. Instead of inspecting each read, it summarizes distributions, helping us detect:

- Low mean quality
- Positional quality drop-off (e.g. at the 3′ end)
- Overrepresented sequences

To run FastQC:

```bash
fastqc SRR8922830_1.fastq SRR8922830_2.fastq
```
This generates .html reports viewable in a browser.

![FastQC report](img/fastqc.png)
&nbsp;

## Filtering and trimming reads
### Sickle
Sickle filters and trims reads based on quality and length.

Example command for paired-end reads:

```bash
sickle pe -f SRR8922830_1.fastq  -r SRR8922830_2.fastq -t sanger  -o trimmed_pe1.fastq   -p trimmed_pe2.fastq   -s trimmed_se.fastq  -q 27 -l 90
```

Parameters:

-f / -r → input FASTQ files (forward, reverse)
-t → quality encoding type (e.g. sanger)
-o / -p → trimmed paired reads output
-s → orphan reads output (singletons)
-q → quality threshold
-l → minimum length threshold

After trimming, rerun FastQC to check improvements:

```bash
fastqc trimmed_pe1.fastq trimmed_pe2.fastq
```

### Summary of this session
By the end of this practical, you should:

* Understand how paired-end reads are generated
* Be familiar with the FASTQ format
* Be able to download data from SRA using the SRA Toolkit
* Run FastQC for quality control
* Perform trimming and filtering using Sickle