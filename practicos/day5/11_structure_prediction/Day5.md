# 🧮 Practical 02 – Protein 3D structures and evolution

## Introduction

Holis

![short insert](img/shor-insert.png)  
&nbsp;

Protein 3D structures evolve more slowly than sequence, preserved through evolutionary time and despite sequence signal loss, particularly over long periods, i.e. billions of years. 

Today we will:
A)
- Visualize and examine different structural features of some selected structures of the FtsZ family
- Perform structural alignments using the graphical interface
- Analyse FtsZ family in the light of sequence variation for the FtsZ family, using chimeraX and some pre computed alignments to map over protein stuctures

B)
- Align protein very distant proteins from all Life domains, using sequence and structure
- Compute phylogenetic trees for these sequences and compare tree topologies obtained under different models and alignments

---
## Before we start

Make sure you are in the correct working directory to run this practical.
You will need to do this every day before starting the exercises.
```bash
# Enter the course folder
cd essentials_bioinformatics_2025/

# List the contents
ls

# Go the today's working directory:
cd day5
```

## Files with 3D information from the PDB

### ℹ️ PDB codes, PDB/CIF formats

📘 For details on PDB/CIF formats [https://pdb101.rcsb.org/learn/guide-to-understanding-pdb-data/beginner%E2%80%99s-guide-to-pdbx-mmcif)




### Structure examination

&nbsp;


<font color="green">

### 🧩 Load PDB files of single chains from FtsZ
conda activate day5

cd Datitos/FtsZ/Singlechains

chimerax *pdb &


--- we will play together interactively


</font>

&nbsp;

Content pirulae
- `cosoperez` is coso.  
- `mojochucu` is `.chucu` mojo.  

⛔ Example workflow:
```bash
# only an example, do not run it

# fastq-dump --split-files ERR049934.sra
```
Alternatively, fastq-dump can take the accession number directly, 

✅ downloading and converting on the fly:

```bash
# download only 1 million reads
fastq-dump -X 1000000 --split-files ERR049934
```


Here:

-X 1000000 → download only 1 million reads
--split-files → generate two FASTQ files for paired-end data.

After running fastq-dump, two FASTQ files will be generated in your working directory.

✅
```bash
# You can list them using:
ls

# To preview the first few lines of the first file:
head ERR049934_1.fastq

# To visualize the entire file, use one of the following commands:
more ERR049934_2.fastq
# or
less ERR049934_2.fastq

# Press Ctrl + C (or 'q' if using 'less') to exit the viewer.
```

### ℹ️ Quality control
High-throughput sequencing generates millions of reads, but sequencing technologies are not error-free. Each instrument may introduce different errors. Quality control (QC) is therefore an essential first step.

Phred score
The Phred quality score (Q) measures base call accuracy, that is, how confident the sequencer is in identifying each nucleotide (A, T, C, or G) during the reading process. Each “base call” corresponds to a single position in a read where the sequencing instrument assigns a nucleotide based on fluorescence intensity or signal interpretation.  

📌 The Phred quality score (Q) measures base call accuracy:

$𝑄 = −10log 10(𝑃)$

Where *P* is the probability of error.

* Q10 → 1 error every 10 bases (10% error)
* Q20 → 1 error every 100 bases (1% error)
* Q30 → 1 error every 1000 bases (0.1% error)

👉 Discussion: What is the lowest Phred score you would expect in your reads?


**Sequencing depth** (C) is the average number of times each nucleotide is sequenced:

$𝐶=𝑁L/𝐺$

Where:
N = number of reads
L = read length
G = genome size

From C we can estimate:

Expected coverage: $1−𝑒^-𝐶$
Expected number of contigs: $𝑁𝑒^−𝐶$


###  FastQC
https://www.bioinformatics.babraham.ac.uk/projects/fastqc/

FastQC provides a quick, graphical summary of sequence quality. Instead of inspecting each read, it summarizes distributions, helping us detect:

- Low mean quality
- Positional quality drop-off (e.g. at the 3′ end)
- Overrepresented sequences

✅ To run FastQC:

```bash
fastqc ERR049934_1.fastq ERR049934_2.fastq
```
This generates .html reports viewable in a browser.

✅ To visualize it, you can locate the files using the file browser and open them with a web browser, or use the terminal to open them:
```bash
# example
firefox ERR049934_1.fastqc.html 
```

![FastQC report](img/fastqc.png)
&nbsp;

## Filtering and trimming reads
### Sickle
Sickle filters and trims reads based on quality and length.

✅ Example command for paired-end reads:

```bash
sickle pe -f ERR049934_1.fastq  -r ERR049934_2.fastq -t sanger  -o trimmed_pe1.fastq   -p trimmed_pe2.fastq   -s trimmed_se.fastq  -q 27 -l 90
```

Parameters:

-f / -r → input FASTQ files (forward, reverse)
-t → quality encoding type (e.g. sanger)
-o / -p → trimmed paired reads output
-s → orphan reads output (singletons)
-q → quality threshold
-l → minimum length threshold

✅ After trimming, rerun FastQC to check improvements:

```bash
fastqc trimmed_pe1.fastq trimmed_pe2.fastq
```
Visualize the .html reports viewable in a browser.

<font color="green">

### 🧩 Analyzing the FastQC report

Open the FastQC HTML report generated for your dataset and explore the main quality modules.

1. How does the per-base quality score change along the length of the reads? Are there regions with noticeably lower quality?

2. Does the GC content distribution look uniform or show irregular peaks? What could that indicate?

3. What happens with the sequence length distribution? Are all reads the same length or does it vary across the dataset?

</font>


### Summary of this session
By the end of this practical, you should:

* Understand how paired-end reads are generated
* Be familiar with the FASTQ format
* Be able to download data from SRA using the SRA Toolkit
* Run FastQC for quality control
* Perform trimming and filtering using Sickle