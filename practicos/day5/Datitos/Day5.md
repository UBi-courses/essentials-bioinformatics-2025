# 🧮 Practical 05 – Protein 3D structures and evolution

## Introduction


![short insert](img/shor-insert.png)  
&nbsp;

Protein 3D structures evolve more slowly than sequence, preserved through evolutionary time and despite sequence signal loss, particularly over long periods, i.e. billions of years. 

Today we will:
A)
- Visualize and examine different structural features of some selected structures of the FtsZ family
- Perform structural alignments using the graphical interface within chimeraX
- Analyze FtsZ 3D structures in the light of sequence variation of the family, using chimeraX and some pre computed sequence alignments to map over protein structures

B)
- Compute multiple alignments of very distant proteins from all Life domains, using sequence and structure
- Compute phylogenetic trees for these alignments and compare tree topologies obtained under different models
- Examine tree topologies and support for sequence- and structure-based aminoacid alignments, and structural alignments using the 3Di alphabet

---
## Before we start

Make sure you are in the correct working directory to run this practical.
You will need to do this every day before starting the exercises.
```bash
# Enter the course folder
cd essentials_bioinformatics_2025/day5

# List the contents
ls

#bring today's stuff to this folder
cp -rp /mnt/lab/Data/day5/Datitos/ .
```
#########################
##(A)
## Files with 3D information from the PDB

### ℹ️ PDB codes, PDB/CIF formats

📘 For details on PDB/CIF formats [https://pdb101.rcsb.org/learn/guide-to-understanding-pdb-data/beginner%E2%80%99s-guide-to-pdbx-mmcif)



### Structure examination

&nbsp;


<font color="green">

### 🧩 Load PDB files of single chains from FtsZ
conda activate day5

cd Datitos/FtsZ/SingleChains/

chimerax *pdb &


--- we will play together interactively





</font>

&nbsp;



##(B)

⛔  workflow:
```bash

# cd Datitos/Fusexins

```

✅ alignment of multiple structures with foldmason, generating aminoacid and 3di alphabet alignments:


```bash
# 
foldmason easy-msa *pdb FusexinsFoldmason_results.afa tmpFolder --report-mode 1
```


✅ alignment of multiple sequences with mafft, but using the 3di alphabet

```bash
# You can align the 3Di sequences obtained from foldmason (gaps removed for you in a new file), but providing mafft a new scoring matrix, different from PAM or BLOSUM (remember Daniela's talk?)

linsi --aamatrix mat3di.out Fsxnseqs3Di.fasta > Fsxnseqs3Di_linsi.afa

trimal -in Fsxnseqs3Di_linsi.afa -out Fsxnseqs3Di_linsi_gt01.fa -gt 0.1

iqtree3 -s Fsxnseqs3Di_linsi_gt01.fa -pre Fsxnseqs3Di_linsi_gt01TREE -m MFP -mset LG,WAG,EX_EHO,PMB,Q.3Di.AF,Q.3Di.LLM,Q.LG,Q.pfam,Q.pfam-gb -nt AUTO -bb 1000 -alrt 1000

#Compute and compare sequence trees and structural trees (both aminoacids and 3di)

linsi FsxnseqsAA.fasta > FsxnseqsAA_linsi.afa

trimal -in FsxnseqsAA_linsi.afa -out FsxnseqsAA_linsi_gt01.fa -gt 0.1

iqtree3 -s FsxnseqsAA_linsi_gt01.fa -pre FsxnseqsAA_linsi_gt01TREE -m MFP -mset LG,WAG,EX_EHO,PMB,Q.3Di.AF,Q.3Di.LLM,Q.LG,Q.pfam,Q.pfam-gb -nt AUTO -bb 1000 -alrt 1000

.
```

#that was for sequence alignments, now use the structural alignments from FoldMason

iqtree3 -s FOLDMASON_AA.fa -pre FOLDMASON_AA_TREE -m MFP -mset LG,WAG,EX_EHO,PMB,Q.3Di.AF,Q.3Di.LLM,Q.LG,Q.pfam,Q.pfam-gb -nt AUTO -bb 1000 -alrt 1000

iqtree3 -s FOLDMASON_3Di.fa -pre FOLDMASON_3DiTREE -m MFP -mset LG,WAG,EX_EHO,PMB,Q.3Di.AF,Q.3Di.LLM,Q.LG,Q.pfam,Q.pfam-gb -nt AUTO -bb 1000 -alrt 1000


### ℹ️ Now play with the four trees, compare topologies and supports. Remember we're dealing with homology beyond sequence recognition. The story is nice. 


### 🧩  


</font>


### Summary of this session
By the end of this practical, you should be happy


