# 🧮 Practical 2BC – Long-Read Transcriptomics QC and Alignment (Google Colab)


## Before starting: Google Colab

In this session we will be using Google Colab. Check out [this](https://www.youtube.com/watch?v=inN8seMm7UI) and [this](https://www.youtube.com/watch?v=FXKMmilL70w) introductory videos of Google Colab.
[Google Colaboratory](https://colab.research.google.com/) is a free Jupyter notebook environment that runs on Google’s cloud servers, letting the user leverage backend hardware like GPUs and TPUs. This lets you do everything you can in a Jupyter notebook hosted in your local machine, without requiring the installations and setup for hosting a notebook in your local machine. All generated Colab notebooks are stored in Google Drive.


### ℹ️ Directory and file operations in Google Colab

To browse the directory structure, you can use the file-explorer pane on the left.

Since a Colab notebook is hosted on Google’s cloud servers, there’s no direct access to files on your local drive (unlike a notebook hosted on your machine) or any other environment by default. However, Colab provides various options to connect to almost any data source you can imagine: GitHub, local file system, Google Drive. See [this blog](https://neptune.ai/blog/google-colab-dealing-with-files) for explanations of these multiple options. However, we are running Colab for R and this reduces our options. We are explaining these.

First, in Colab you can see your **file system explorer**. E.g. locate `/content/sample_data` and download one of the files present in this directory. E.g. navigate to the home and create a new directory. Upload any file into it. Use the upload option at the top of the file-explorer pane to upload any file(s) from your local file system to Colab in the present working directory. 

By default, Google Colab starts in the `/content` directory. This is the root directory where you can perform various operations.

Second, for files already present in **GitHub**, you can clone the entire repository. In case you just have to work with a few files rather than the entire repository, you can load them directly from GitHub without needing to clone the repository to Colab. To do this:

    click on the file in the repository, 
    click on View Raw,
    copy the URL of the raw file, 
    use this URL as the location of your file.


### Use R with Colab

As we mentioned, Colab is native for python. However, there are ways to create R notebooks. For a very simple way, the steps are as follow:

    Open the Google Colab and then click on the New Notebook
    When the new notebook opens, click on the Runtime option that is available from the list of options
    In the dropdown, click on Change Runtime Type
    A dialog box appears. Under the heading Runtime type by default Python3 appears. Click on the arrow and change it to R. After that click on Save.

Execute any command of R. You will get to see the results.

And, paying attention, you will see that these R notebooks show the extension `*.ipynb`. When saving/downloading the notebook, select this extension.


<font color="blue">

### ⛔  CAUTION



One important caveat to remember while using Colab is that the files you upload to it won’t be available forever. Colab is a **temporary environment** with an **idle timeout of 90 minutes** and an **absolute timeout of 12 hours**. This means that the runtime will disconnect if it has remained idle for 90 minutes, or if it has been in use for 12 hours. On disconnection, you lose all your variables, states, installed packages, and files and will be connected to an entirely new and clean environment on reconnecting.

Also, Colab has a disk space limitation of 108 GB, of which only 77 GB is available to the user. While this should be enough for most tasks, keep this in mind while working with larger datasets like image or video data.


</font>

&nbsp;


### General instructions

1. Login to Google/Gmail; you need access to your Drive
2. Try one of the many different ways of opening in Google Colab the notebook for this session
3. Once opened, make your own copy of the notebook (copy to your Drive)
4. Follow the notebook instructions, including accessing your Drive and the Shared Folder Drive


&nbsp;

## Long-Read Transcriptomics: ONT cDNA Pipeline for Differential Expression Analysis

Several workflow management systems have been developed for building bioinformatics pipelines including [Snakemake](https://snakemake.readthedocs.io/en/stable) and [Nextflow](https://www.nextflow.io/). Recently it seems that Nextflow is gaining popularity, not least because of the NF-Core: a large repository of the Nextflow bioinformatics pipelines (https://nf-co.re/pipelines/).


The first (now deprecated) Nanopore’s pipeline for quantitative RNAseq analysis was based on the paper of Love et al 2018 ( https://f1000research.com/articles/7-952/v3 ). It included alignment with minimap2 against a reference transcriptome, followed by counts with Salmon, differential genes expression (DGE) with edgeR and differential transcripts usage (DTU) analysis using R packages DEXSeq and stageR. That pipeline used Snakemake workflow manager. Recently it has been substituted by the **wf-transcriptomes** pipeline, which uses Nextflow and has some additional steps (such as fusion detection with Jaffal, and transcriptome assembly).
If you have to perform Nanopore quantitative RNA-seq analysis you may want to run [wf-transcriptomes](https://github.com/epi2me-labs/wf-transcriptomes), developed by Oxford Nanopore.

Currently the `wf-transcriptomes` pipeline includes the following functionality:

1. Identifying RNA transcripts (using either cDNA or direct RNA reads)
2. Making reference-aided transcriptome assembly
3. Annotating the assembled transcripts
4. Detecting gene fusions
5. Differential gene expression (DGE) analysis
6. Differential transcript usage (DTU) analysis

However, in this practical, we will not use `wf-transcriptomes`; instead, we’ll run a leaner, step-by-step pipeline that better fits Google Colab’s resource limits. This approach makes each stage—QC, alignment, expression quantification, and differential expression analysis—more explicit, allowing students to clearly see and understand what happens at every step of the workflow.

<font color="green">
🧩  To follow this practical session, two Colab notebooks are available: `practico2B.ipynb` and `practico2C.ipynb`. The additional files with the dataset are provided through a Drive shared folder.
<font>

&nbsp;
&nbsp;
&nbsp;