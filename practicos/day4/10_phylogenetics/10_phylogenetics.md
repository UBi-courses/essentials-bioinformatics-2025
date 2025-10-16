# 🧮 Practical Session 10 – Phylogenetics

## Part 5: Alignment of FtsZ

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
# Run AliView
aliview
```

🧩 **What is the main difference with aligned file?**

<svg width="100%" height="4"><rect width="100%" height="4" fill="#d0d7de"/></svg>

## Part 6: Phylogeny of FtsZ

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
