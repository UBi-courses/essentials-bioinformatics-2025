# 🧮 Practical Session 04 – Comparative Genomics

<svg width="100%" height="4"><rect width="100%" height="4" fill="#d0d7de"/></svg>

## Introduction

---

Comparative genomics links genome differences to evolution and function. Conserved genes point to essential roles, while lineage-restricted or rapidly evolving genes can indicate adaptation. In this practical session we start by surveying the dataset, check origin and reliability, group genes into orthogroups to compare species, and end with a test on protein FtsZ to connect results to function.

We will work with a pre-selected set of complete and reference genomes from Pseudomonadota (formerly known as Proteobacteria). These genomes were obtained from the NCBI GenBank database, from which all encoded proteins were downloaded. The corresponding FASTA files can be found in the `~/essentials_bioinformatics_2025/day4/dataset/` folder.

<svg width="100%" height="4"><rect width="100%" height="4" fill="#d0d7de"/></svg>

## Part 1: Initial Data Exploration

---

### - Count Available Genomes
Before we run any tools, we need a basic sense of the dataset. Open a terminal, go to `~/essentials_bioinformatics/day4/dataset`, and count how many genomes are there. Knowing this number helps us choose an analysis strategy that fits the size of the data.

```bash
cd ~/essentials_bioinformatics_2025/day4/dataset/
ls *.faa | wc -l
```

---

### - Explore Species Diversity
Identify a few species using the taxonomic sampling table in Bash.

```bash
less ../taxonomy_sampling.csv
```

🧩 **Any familiar ones?**

You can press **q** to quit `less`.

🧩 **How many proteins does the smallest genome encode? And the largest?**

```bash
grep -c ">" *.faa | sort -t':' -k2,2n
```

<svg width="100%" height="4"><rect width="100%" height="4" fill="#d0d7de"/></svg>

## Part 2: Genomic Databases and Download Options

---

### - NCBI Genome Database Overview

ℹ️ It is possible to download assembled genomes/proteomes obtained by different research centers and deposited in various public databases. The NCBI (United States), ENA (Europe), and DDBJ (Japan) databases contain annotated and unannotated genomes from organisms across all domains of life and viruses, and they synchronize daily, containing the same information. There are also organism-specific databases; for bacteria, some of the most commonly used are IMG/M (Integrated Microbial Genomes & Microbiomes) and PATRIC/BC-BRC.

---

### - NCBI Data Access Methods

For the NCBI database, there are different ways to obtain genomes, for example:

#### NCBI Datasets (Command-Line Interface)
NCBI Datasets is the latest CLI for downloading genomes and annotations that lets you search and fetch genome assemblies, and download sequences (genomic, protein) and metadata.

```bash
# Example Datasets commands
# 1. Search for P. aeruginosa assemblies
datasets summary genome taxon "Pseudomonas aeruginosa" --reference

# 2. Download the PAO1 reference genome (GCA_000006765.1)
datasets download genome accession GCA_000006765.1 \
  --filename ~/essentials_bioinformatics_2025/day4/pao1_reference.zip
```

More information: https://www.ncbi.nlm.nih.gov/datasets/docs/

#### NCBI Genome Web Interface
Another option is through the NCBI Genome web interface: https://www.ncbi.nlm.nih.gov/datasets/genome/

---

### - Explore NCBI Genome Database
Visit the NCBI Genome website, search for genomes corresponding to the phylum Pseudomonadota, and use the **Filters** to refine your results.

#### Understanding Genome Classifications

ℹ️ The strength of our conclusions depends on assembly and annotation quality. NCBI defines multiple assembly levels:

- Contig: Raw contiguous sequences; may contain gaps and unordered contigs  
- Scaffold: Ordered and oriented contigs linked by paired-read information; may still contain gaps  
- Chromosome: Scaffolded into full chromosome sequences; minimal or no gaps  
- Complete: All replicons (chromosomes and plasmids) finished with no gaps or ambiguities  

Reference genomes are usually high-quality assemblies selected as the standard for a species.

🧩 **How many genomes corresponding to this phylum are available in this database?**

🧩 **How many of these genomes are complete?**

🧩 **How many correspond to genomes assembled from metagenomes?**

---

### - File Types and Formats
Explore the **Download Package** options.

🧩 **What type of information and format does each file type present?**

🧩 **Which file type corresponds to those stored in `~/essentials_bioinformatics_2025/day4/dataset/`?**

<svg width="100%" height="4"><rect width="100%" height="4" fill="#d0d7de"/></svg>

## Part 3: Ortholog Identification

---

### - Why Ortholog Identification Matters

Identifying orthologs across a set of genomes is one of the key tasks in comparative genomics, and it is often challenging. Orthologs are homologous genes separated by speciation events, while paralogs are homologous genes separated by duplication events.

ℹ️ Why is this important?

- Understanding evolutionary relationships: Orthologs arise from speciation events. Identifying them allows reconstruction of species trees and reveals how lineages diverged over time.  
- Functional annotation transfer: Orthologs usually share functions. Annotations from well-studied organisms can be reliably transferred to less-characterized species via orthology.  
- Identifying core vs. accessory genes: Core genes are present in all genomes of a group and typically encode essential functions, while accessory genes vary and often relate to niche adaptation or pathogenicity.
- Studying gene family evolution: Tracking expansions, contractions, and duplications of genes across species uncovers how gene families evolve, adapt, and acquire new functions.  

The difficulties and strategies differ when we compare strains within a species and when we compare bacteria with archaea. In this practical session, we will work at an intermediate level with representative species from different classes within the bacterial phylum Pseudomonadota.

---

### - OrthoFinder Tool

ℹ️ We will use OrthoFinder (https://github.com/davidemms/OrthoFinder), a comparative genomics tool that clusters genes into orthogroups, produces useful count tables and summaries, and works well at this taxonomic level and with our data volume. An orthogroup is the full set of proteins across the species in our dataset that descend from one ancestral gene. This set can include strict orthologs and also extra copies created by duplications inside a lineage.

---

### - OrthoFinder Analysis

We will not run OrthoFinder in class because it might take several hours. We have already executed it on the genomes in the dataset directory using:

```bash
orthofinder -f ~/essentials_bioinformatics_2025/day4/dataset/
```

The complete output is available at /mnt/lab/Data/day4/OrthoFinder/Results_Oct09/. We will use these results to obtain the species-by-orthogroup count table and from it define core sets, single-copy sets, and other variable families.

#### Understanding OrthoFinder Output

```bash
# List the directories generated by OrthoFinder
ls -l /mnt/lab/Data/day4/OrthoFinder/Results_Oct09/ 
```

Some of the output files include:

- Orthogroups/Orthogroups.tsv: Lists all proteins per species in each orthogroup
- Orthogroups/Orthogroups.GeneCount.tsv: Number of proteins per species in each orthogroup

```bash
# Take a look at the format of both files
less /mnt/lab/Data/day4/OrthoFinder/Results_Oct09/Orthogroups/Orthogroups.tsv
# Remember you can press "q" to quit "less"
less /mnt/lab/Data/day4/OrthoFinder/Results_Oct09/Orthogroups/Orthogroups.GeneCount.tsv
```

ℹ️ Orthogroups present in all species approximate the core genome, which is often enriched for housekeeping and essential functions. Orthogroups with exactly one copy in every species are strong markers for species trees, because they minimize problems caused by paralogs. In the next step, we will load the OrthoFinder tables into R and filter them to find the core orthogroups and the single-copy orthogroups across the dataset.

```R
# Read the gene count matrix
counts <- read.delim("/mnt/lab/Data/day4/OrthoFinder/Results_Oct09/Orthogroups/Orthogroups.GeneCount.tsv", check.names=FALSE)

# Extract only the numeric matrix (drop Orthogroup column)
mat <- counts[, 2:(ncol(counts)-1)]

# Total number of orthogroups
nrow(mat)

# Orthogroups present in all species (every column > 0)
sum(apply(mat, 1, function(x) all(x > 0)))

# Orthogroups present in all species exactly one copy (every column == 1)
sum(apply(mat, 1, function(x) all(x == 1)))
```

<svg width="100%" height="4"><rect width="100%" height="4" fill="#d0d7de"/></svg>

## Part 4: FtsZ Protein Case Study

---

We will now focus on the FtsZ protein, which plays an essential role in cell division in the vast majority of known bacteria. We will identify the orthogroup that contains the *E. coli* FtsZ protein (AAC73206.1) and summarize its distribution. Examining its orthogroup lets us confirm the global patterns and notice exceptions that may point to interesting biology.

```R
# Read the orthogroup membership table
ogs <- read.delim("/mnt/lab/Data/day4/OrthoFinder/Results_Oct09/Orthogroups/Orthogroups.tsv", header=FALSE, sep="\t", stringsAsFactors=FALSE)

# Find the row containing the FtsZ protein ID
row_index <- grep("AAC73206.1", apply(ogs, 1, paste, collapse = "\t"))
fts_row <- ogs[row_index, ]

# Extract the orthogroup ID (first column)
fts_og <- fts_row[,1]

# Get counts for this orthogroup
fts_counts <- counts[counts$Orthogroup == fts_og, 2:(ncol(counts)-1)]

# Total proteins in FtsZ orthogroup
sum(fts_counts) 

# Calculate numbers of species by copy number category
# Species lacking FtsZ
sum(fts_counts == 0)
# Species with single copy FtsZ
sum(fts_counts == 1)
# Species with multiple copies
sum(fts_counts > 1)
```

🧩 **What can we say about the distribution of FtsZ in Pseudomonadota?**

<svg width="100%" height="4"><rect width="100%" height="4" fill="#d0d7de"/></svg>

## Part 5: Alignment of FtsZ

---

Molecular sequences must be aligned before they can be used to build phylogenies.

ℹ️ Aligning sequences amounts to finding the nucleotide positions (sites) that we can be confident have the same evolutionary history: they correspond to each other across species and can be considered to have evolved from the same common ancestor. A good clue to identify these sites, which are called homologous, is that they are well conserved, with only a few changes.

ℹ️ While aligning two sequences is “easy”, in the sense that an optimal alignment between two sequences can be found in a reasonable amount of time, optimally aligning multiple sequences is computationally intractable. Multiple sequence alignment is a complex process and many methods have been developed to tackle this challenge.

Today you will be aligning sequences using a modern multiple alignment program called MAFFT with the option L-INS-i (linsi). This option is an Accuracy-oriented method, probably the most accurate; recommended for <200 sequences.

```bash
# Check where you are
pwd

# In the directory ~/essentials_bioinformatics_2025/day4/ create a directory for the alignment and phylogenetic analysis
mkdir phylogenetics
cd phylogenetics/

# Copy the FASTA file for the analysis to the newly created directory
cp /mnt/lab/Data/day4/OrthoFinder/Results_Oct09/Orthogroup_Sequences/OG0000617.fa .

# Before running the alignment check the 'help' option
mafft -help

# Run the alignment
linsi OG0000617.fa > OG0000617.aln

```

### - Visualization of FtsZ alignment

For the alignment visualization we will use AliView, a fast an intituitive alignment viewer and editor.

But, first install it!

```bash
conda install bioconda::aliview

# And run it
aliview
```

🧩 **Are the FtsZ sequences aligned?**

🧩 **Can you identify variant sites?**

### - Trimming

trimAl remove poorly aligned regions from a multiple sequence alignment.

```bash
trimal --help
trimal -in OG0000617.aln -out OG0000617.trim -gt 0.2
```

### - Visualization of trimmed FtsZ alignment

```bash
aliview
```

🧩 **What is the main difference with aligned file?**

<svg width="100%" height="4"><rect width="100%" height="4" fill="#d0d7de"/></svg>

## Part 6: Phylogeny of FtsZ

---

ℹ️ The phylogeny will be obtained using the maximum likelihood method. It is based on the idea that the tree that has the highest probability of producing the sequences at the tips of the tree is the tree that is the “most likely” to be correct: this is called the Maximum Likelihood (ML) Tree.

Likelihood is not the same as probability; it is proportional to the probability that the tree is correct, within the set of possible trees and models being considered.

One major assumption about molecular sequence data is that each site evolves independently. Biologically, this is not always the case, but in practice, this makes things much more tractable, and we still have a good chance of getting the correct tree(s).

Another assumption we make is that the substitution rate – the rate at which changes of nucleotide or aminoacid at a given position in the sequence happen – is only dependent on the current state, i.e., we do not care about how a sequence came to be what it is, only what the sequence is now, to determine what are the probable evolutions of it. This seems much more biologically reasonable and makes this into a Markov process, which in turn enables a lot of calculations to be made simply.

```bash
iqtree -s OG0000617.trim -m Q.pfam+F+R6 -B 10000 -nm 100000 -T 4 -bcor 0.999
```

### - Visualization of FtsZ phylogeny

For the visualization of the phylogeny we will use iTOL, google it or copy and paste the link (https://itol.embl.de/) on a browser (e.g. Chrome).

Within the phylogenetics directory, after iqtree is done, you will find a file named OG0000617.trim.treefile.

Explore it with less, more, nano, etc. 

```bash
more OG0000617.trim.treefile
```

🧩 **What do you see?**

🧩 **Is it a tree?**

🧩 **What is this format called?**

Then upload OG0000617.trim.treefile in iTOL. Using the 'Control panel' in the right up corner you can play with visualization possibilities.

Finally, in the 'Control panel' Datasets tab upload the file ```rename_labels_ftsz.txt```.

🧩 **What changed after adding labels?**

🧩 **What can we say about the evolutionary relationships of FtsZ?**
