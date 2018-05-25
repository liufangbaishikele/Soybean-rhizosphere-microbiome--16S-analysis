# Qiime2 
----------------
## Intall and get familiar with QIIME
-----------------
## Upload qiime to env

Module load qiime from ACF by ``module load qiime2/2017.11``
Check if qiime2 loaded to the current environment by ``module list`` (during this process anaconda3 is all loaded)
It shows that qiime2 is successfully loaded to working directory.
** -bash: qiime: command not found ** When I type in ``qiime``
Then I tried to load mothur ``module load mothur/1.39.5`` and checked using ``module list`` to make sure correctly uploaded.
When I type in ``mothur`` from my terminal, it poped out an error (mothur: symbol lookup error: /sw/cs400_centos7.3_acfsoftware/anaconda3/4.4.0/centos7.3_gnu6.3.0/anaconda3-4.4.0/lib/libreadline.so.6: undefined symbol: PC)
I thought the problem may caused by the path or environment. So I double checked the module list and found anaconda3 is uploaded. I guess
the problem may comes from this. So I unload anaconda3 by ``module unload anaconda3/4.4.0`` and type in mothur again. It worked!
BUT when I type ``qiime``, the command was still not found.

## Active conda environment

**NOW go back to qiime documentation** look carefully and found that after upload qiime, I still need to active the conda environment by
``source activate qiime2-2017.11`` 
now qiime become functional;)

## knock around the documentations

**``qiime --help``**
Here are the output
```
QIIME is caching your current deployment for improved performance. This may take a few moments and should only happen once per deployment.
Usage: qiime [OPTIONS] COMMAND [ARGS]...

  QIIME 2 command-line interface (q2cli)
  --------------------------------------

  To get help with QIIME 2, visit https://qiime2.org.

  To enable tab completion in Bash, run the following command or add it to
  your .bashrc/.bash_profile:

      source tab-qiime

  To enable tab completion in ZSH, run the following commands or add them to
  your .zshrc:

      autoload bashcompinit && bashcompinit && source tab-qiime

Options:
  --version  Show the version and exit.
  --help     Show this message and exit.

Commands:
  info                Display information about current deployment.
  tools               Tools for working with QIIME 2 files.
  dev                 Utilities for developers and advanced users.
  alignment           Plugin for generating and manipulating alignments.
  composition         Plugin for compositional data analysis.
  dada2               Plugin for sequence quality control with DADA2.
  deblur              Plugin for sequence quality control with Deblur.
  demux               Plugin for demultiplexing & viewing sequence quality.
  diversity           Plugin for exploring community diversity.
  emperor             Plugin for ordination plotting with Emperor.
  feature-classifier  Plugin for taxonomic classification.
  feature-table       Plugin for working with sample by feature tables.
  gneiss              Plugin for building compositional models.
  longitudinal        Plugin for paired sample and time series analyses.
  metadata            Plugin for working with Metadata.
  phylogeny           Plugin for generating and manipulating phylogenies.
  quality-control     Plugin for quality control of feature and sequence data.
  quality-filter      Plugin for PHRED-based filtering and trimming.
  sample-classifier   Plugin for machine learning prediction of sample
                      metadata.
  taxa                Plugin for working with feature taxonomy annotations.
  vsearch             Plugin for clustering and dereplicating with vsearch.

```


## To enable tab completion of command in bash by 
**``source tab-qiime``**

## Have a look at qiime information by 
**``qiime info``**

```
System versions
Python version: 3.5.4
QIIME 2 release: 2017.11
QIIME 2 version: 2017.11.0
q2cli version: 2017.11.0

Installed plugins
alignment 2017.11.0
composition 2017.11.0
dada2 2017.11.0
deblur 2017.11.0
demux 2017.11.0
diversity 2017.11.0
emperor 2017.11.0
feature-classifier 2017.11.0
feature-table 2017.11.0
gneiss 2017.11.0
longitudinal 2017.11.0
metadata 2017.11.0
phylogeny 2017.11.0
quality-control 2017.11.0
quality-filter 2017.11.0
sample-classifier 2017.11.0
taxa 2017.11.0
types 2017.11.0
vsearch 2017.11.0

Application config directory
/nics/d/home/fliu21/.config/q2cli

Getting help
To get help with QIIME 2, visit https://qiime2.org

Citing QIIME 2
If you use QIIME 2 in any published work, you should cite QIIME 2 and the plugins that you used. To display the citations for QIIME 2 and all installed plugins, run:

  qiime info --citations

```
**qiime info --citation** very useful 

## QIIME command structure

```
qiime demux emp-single \
  --i-seqs emp-single-end-sequences.qza \
  --m-barcodes-file sample-metadata.tsv \
  --m-barcodes-category BarcodeSequence \
  --o-per-sample-sequences demux.qza
```
1. First give plugin name - **demux** (in this case)
emp-single is the commands from demux plugin

2. Commands from the plugin
IF I want to know all available commands from this plugin, I could just type in ``qiime demux emp-single`` and tap enter button.
```
Usage: qiime demux [OPTIONS] COMMAND [ARGS]...
Description: This QIIME 2 plugin supports demultiplexing of single-end and
  paired-end sequence reads and visualization of sequence quality
  information.

  Plugin website: https://github.com/qiime2/q2-demux

  Getting user support: Please post to the QIIME 2 forum for help with this
  plugin: https://forum.qiime2.org

  Citing this plugin: No citation available. Cite plugin website:
  https://github.com/qiime2/q2-demux

Options:
  --help  Show this message and exit.

Commands:
  emp-paired  Demultiplex paired-end sequence data generated with the EMP
              protocol.
  emp-single  Demultiplex sequence data generated with the EMP protocol.
  summarize   Summarize counts per sample.
```
3. options for each commands- could be found from help documents.
```
qiime demux emp_single
```
Then hit enter button.

```
Usage: qiime demux emp-single [OPTIONS]

  Demultiplex sequence data (i.e., map barcode reads to sample ids) for data
  generated with the Earth Microbiome Project (EMP) amplicon sequencing
  protocol. Details about this protocol can be found at
  http://www.earthmicrobiome.org/protocols-and-standards/

Options:
  --i-seqs PATH                   Artifact: EMPPairedEndSequences |
                                  EMPSingleEndSequences | RawSequences
                                  [required]
                                  The single-end sequences to be
                                  demultiplexed.
  --m-barcodes-file PATH          Metadata file or artifact viewable as
                                  metadata. This option may be supplied
                                  multiple times to merge metadata  [required]
  --m-barcodes-category TEXT      Category from metadata file or artifact
                                  viewable as metadata  [required]
                                  The sample
                                  metadata category listing the per-sample
                                  barcodes.
  --p-rev-comp-barcodes / --p-no-rev-comp-barcodes
                                  [default: False]
                                  If provided, the barcode
                                  sequence reads will be reverse complemented
                                  prior to demultiplexing.
  --p-rev-comp-mapping-barcodes / --p-no-rev-comp-mapping-barcodes
                                  [default: False]
                                  If provided, the barcode
                                  sequences in the sample metadata will be
                                  reverse complemented prior to
                                  demultiplexing.
  --o-per-sample-sequences PATH   Artifact: SampleData[SequencesWithQuality]
                                  [required if not passing --output-dir]
                                  The
                                  resulting demultiplexed sequences.
  --output-dir DIRECTORY          Output unspecified results to a directory
  --cmd-config PATH               Use config file for command options
  --verbose                       Display verbose output to stdout and/or
                                  stderr during execution of this action.
                                  [default: False]
  --quiet                         Silence output if execution is successful
                                  (silence is golden).  [default: False]
  --help                          Show this message and exit.
```
-------------------------------
## Start analysis using QIIME
-------------------------------
1. First import sequence data (if undemultiplexed, both fastq and barcode is needed) and sample data (if undemultiplexed, barcode catogery is needed)
2. Demultiplex (if already indivisual samples, skip this step)
3. Make a summary of the sequencing depth and quality of sequences
4. Denoise using ``dada2`` OR ``deblur``

**Web based dada2 documentation**

  * First, open [qiime documentation](https://docs.qiime2.org/2017.11/)
  * Second, click the ``Plugins`` menu on the left panel
  * Third: click [dada2: Plugin for sequence quality control with DADA2](https://docs.qiime2.org/2017.11/plugins/available/dada2/)
  
  * Now, I found two function of dada2
  ``(1) demoise-paired``
  ``(2) denoise-single``
  
**Documentation using command line**

  * Look at available functions of dada2, by typing in ``qiime dada2``
  
  IT will give below information

  ```
  Usage: qiime dada2 [OPTIONS] COMMAND [ARGS]...

    Description: This QIIME 2 plugin wraps DADA2 and supports sequence quality
    control for single-end and paired-end reads using the DADA2 R library.

    Plugin website: http://benjjneb.github.io/dada2/

    Getting user support: Please post to the QIIME 2 forum for help with this
    plugin: https://forum.qiime2.org

    Citing this plugin: DADA2: High-resolution sample inference from Illumina
    amplicon data. Benjamin J Callahan, Paul J McMurdie, Michael J Rosen,
    Andrew W Han, Amy Jo A Johnson, Susan P Holmes. Nature Methods 13, 581–583
    (2016) doi:10.1038/nmeth.3869.

  Options:
    --help  Show this message and exit.

  Commands:
    denoise-paired  Denoise and dereplicate paired-end sequences
    denoise-single  Denoise and dereplicate single-end sequences
  ```
  * Look at options of **denoise-paired** by type in ``qiime dada2 denoise-paired``
  Here are the documentation
  ```
  Usage: qiime dada2 denoise-paired [OPTIONS]

  This method denoises paired-end sequences, dereplicates them, and filters
  chimeras.

Options:
  --i-demultiplexed-seqs PATH     Artifact:
                                  SampleData[PairedEndSequencesWithQuality]
                                  [required]
                                  The paired-end demultiplexed
                                  sequences to be denoised.
  --p-trunc-len-f INTEGER         [required]
                                  Position at which forward read
                                  sequences should be truncated due to
                                  decrease in quality. This truncates the 3'
                                  end of the of the input sequences, which
                                  will be the bases that were sequenced in the
                                  last cycles. Reads that are shorter than
                                  this value will be discarded. After this
                                  parameter is applied there must still be at
                                  least a 20 nucleotide overlap between the
                                  forward and reverse reads. If 0 is provided,
                                  no truncation or length filtering will be
                                  performed
  --p-trunc-len-r INTEGER         [required]
                                  Position at which reverse read
                                  sequences should be truncated due to
                                  decrease in quality. This truncates the 3'
                                  end of the of the input sequences, which
                                  will be the bases that were sequenced in the
                                  last cycles. Reads that are shorter than
                                  this value will be discarded. After this
                                  parameter is applied there must still be at
                                  least a 20 nucleotide overlap between the
                                  forward and reverse reads. If 0 is provided,
                                  no truncation or length filtering will be
                                  performed
  --p-trim-left-f INTEGER         [default: 0]
                                  Position at which forward read
                                  sequences should be trimmed due to low
                                  quality. This trims the 5' end of the input
                                  sequences, which will be the bases that were
                                  sequenced in the first cycles.
  --p-trim-left-r INTEGER         [default: 0]
                                  Position at which reverse read
                                  sequences should be trimmed due to low
                                  quality. This trims the 5' end of the input
                                  sequences, which will be the bases that were
                                  sequenced in the first cycles.
  --p-max-ee FLOAT                [default: 2.0]
                                  Reads with number of expected
                                  errors higher than this value will be
                                  discarded.
  --p-trunc-q INTEGER             [default: 2]
                                  Reads are truncated at the
                                  first instance of a quality score less than
                                  or equal to this value. If the resulting
                                  read is then shorter than `trunc_len_f` or
                                  `trunc_len_r` (depending on the direction of
                                  the read) it is discarded.
  --p-chimera-method [none|pooled|consensus]
                                  [default: consensus]
                                  The method used to
                                  remove chimeras. "none": No chimera removal
                                  is performed. "pooled": All reads are pooled
                                  prior to chimera detection. "consensus":
                                  Chimeras are detected in samples
                                  individually, and sequences found chimeric
                                  in a sufficient fraction of samples are
                                  removed.
  --p-min-fold-parent-over-abundance FLOAT
                                  [default: 1.0]
                                  The minimum abundance of
                                  potential parents of a sequence being tested
                                  as chimeric, expressed as a fold-change
                                  versus the abundance of the sequence being
                                  tested. Values should be greater than or
                                  equal to 1 (i.e. parents should be more
                                  abundant than the sequence being tested).
                                  This parameter has no effect if
                                  chimera_method is "none".
  --p-n-threads INTEGER           [default: 1]
                                  The number of threads to use
                                  for multithreaded processing. If 0 is
                                  provided, all available cores will be used.
  --p-n-reads-learn INTEGER       [default: 1000000]
                                  The number of reads to
                                  use when training the error model. Smaller
                                  numbers will result in a shorter run time
                                  but a less reliable error model.
  --p-hashed-feature-ids / --p-no-hashed-feature-ids
                                  [default: True]
                                  If true, the feature ids in
                                  the resulting table will be presented as
                                  hashes of the sequences defining each
                                  feature. The hash will always be the same
                                  for the same sequence so this allows feature
                                  tables to be merged across runs of this
                                  method. You should only merge tables if the
                                  exact same parameters are used for each run.
  --o-table PATH                  Artifact: FeatureTable[Frequency] [required
                                  if not passing --output-dir]
                                  The resulting
                                  feature table.
  --o-representative-sequences PATH
                                  Artifact: FeatureData[Sequence] [required if
                                  not passing --output-dir]
                                  The resulting
                                  feature sequences. Each feature in the
                                  feature table will be represented by exactly
                                  one sequence, and these sequences will be
                                  the joined paired-end sequences.
  --output-dir DIRECTORY          Output unspecified results to a directory
  --cmd-config PATH               Use config file for command options
  --verbose                       Display verbose output to stdout and/or
                                  stderr during execution of this action.
                                  [default: False]
  --quiet                         Silence output if execution is successful
                                  (silence is golden).  [default: False]
  --help                          Show this message and exit.

Error: Missing option: --i-demultiplexed-seqs
Error: Missing option: --p-trunc-len-f
Error: Missing option: --p-trunc-len-r
Error: Missing option: --o-table
Error: Missing option: --o-representative-sequences
Note: When only providing names for a subset of the output Artifacts or
Visualizations, you must specify an output directory through use of the
--output-dir DIRECTORY flag.

```
**Output from dada2 analysis**
  * representative-sequences (this is a file with sequence ID and corresponding sequences)
  * table (very like the count_table of mothur output, with sequence ID and corresponding counts across all samples)
  
  
  
------------------------------------------------------
Analysis of 2017 AgOCU and AgPSC sequence using qiime2
------------------------------------------------------


**Prepare dataset with only two samples fro 2017 sequencing**

```
AgOCU_fresh_1_S41_L001_R1_001.fastq.gz  
AgOCU_fresh_1_S41_L001_R2_001.fastq.gz

AgPSC_fresh_1_S82_L001_R1_001.fastq.gz
AgPSC_fresh_1_S82_L001_R2_001.fastq.gz
```


1. **Import sequence and create a .qza artifact**

```
qiime tools import \
--type "SampleData[PairedEndSequencesWithQuality]" \
--input-path /nics/d/home/fliu21/qiime2_pair_end/Ag_trial_2017_raw_read/ \
--output-path /nics/d/home/fliu21/qiime2_pair_end/Ag_trial_2017_qiime_analysis/Ag_trial_sequence.qza \
--source-format CasavaOneEightSingleLanePerSampleDirFmt
```

Alternatively
*This commands works for me*

```
qiime tools import \
--type 'SampleData[PairedEndSequencesWithQuality]' \
--input-path $PWD/pe-33-manifest \
--output-path /nics/d/home/fliu21/qiime2_pair_end/Ag_trial_2017_qiime_analysis/paired-end-demux.qza \
--source-format PairedEndFastqManifestPhred33
```
2. **Quality control and filtering using dada2**

```
qiime dada2 denoise-paired \
--i-demultiplexed-seqs Ag_trial_sequence.qza \
--p-n-threads 16  \
--p-trim-left-r 0 \
--p-trim-left-f 0 \
--p-trunc-len-r 250 \
--p-trunc-len-f 250 \
--o-representative-sequences Ag_trial_seqs_dada2.qza \
--o-table Ag_trial_table_dada2.qza

```
I have no idea, this step took forever. Something may be wrong with my code

Alternatively, use this 

```
qiime dada2 denoise-paired \
--i-demultiplexed-seqs paired-end-demux.qza \
--p-n-threads 16  \
--p-trim-left-r 0 \
--p-trim-left-f 0 \
--p-trunc-len-r 250 \
--p-trunc-len-f 250 \
--o-representative-sequences Ag_trial_seqs_dada2.qza \
--o-table Ag_trial_table_dada2.qza

```

Because this step is time consuming, so I submited job to ACF. Below is the job scripts. During the first try, got error **RuntimeError: Click will abort further execution because Python 3 was configured to use ASCII as encoding for the environment** In order to solve this problem, I need to make sure what my locale is. Just type in ``locale`` I will see. Based on this [link](http://click.pocoo.org/5/python3/), I added ``export LC_ALL=en_US.UTF-8 export LANG=en_US.UTF-8``

```
#PBS -S /bin/bash
#PBS -A ACF-UTK0032
#PBS -l nodes=1,walltime=6:00:00
#PBS -N Ag_trial_dada2_denoise
#PBS_NNODES 16

module load qiime2/2017.11
source activate qiime2-2017.11
cd $PBS_O_WORKDIR

export LC_ALL=en_US.UTF-8
export LANG=en_US.UTF-8

qiime dada2 denoise-paired \
--i-demultiplexed-seqs paired-end-demux.qza \
--p-n-threads 16  \
--p-trim-left-r 0 \
--p-trim-left-f 0 \
--p-trunc-len-r 250 \
--p-trunc-len-f 250 \
--o-representative-sequences Ag_trial_seqs_dada2.qza \
--o-table Ag_trial_table_dada2.qza

```


3. **Summarize and visualize featureTable and FeatureData**

* Summarize table data

```
qiime feature-table summarize \
--i-table Ag_trial_table_dada2.qza \
--o-visualization Ag_trial_table_dada2.qzv
```
* summarize unique sequence data

```
qiime feature-table tabulate-seqs \
--i-data Ag_trial_seqs_dada2.qza \
--o-visualization Ag_trial_seqs_dada2.qzv
```
4. Generate phylogenetic tree

  * Generate alignment file using sequence data
  ```
  qiime alignment mafft \
  --i-sequences Ag_trial_seqs_dada2.qza \
  --o-alignment Ag_trial_align_dada2.qza
  ```
  * Mask the the alignment file to remove highly variable positions, which are generally considered to add noise to a resulting phylogenetic tree.
  
  ```
  qiime alignment mask \
  --i-alignment Ag_trial_align_dada2.qza \
  --o-masked-alignment Ag_trial_masked_align_dada2.qza
  ```
  * Generate unrooted tree
  
  ```
  qiime phylogeny fasttree --i-alignment Ag_trial_masked_align_dada2.qza --o-tree Ag_trial_unrooted_tree_dada2.qza
  ```
  
  * Creat rooted tree using unrooted tree by midpoint method 
  
  ```
  qiime phylogeny midpoint-root \
  --i-tree Ag_trial_unrooted_tree_dada2.qza \
  --o-rooted-tree Ag_trial_rooted_tree_dada2.qza
  ```




5. **Train greengene reference to generate customized feature classifers**

* Download the greengene dada from this [link][ftp://greengenes.microbio.me/greengenes_release/gg_13_5/gg_13_8_otus.tar.gz]

* Unzip the ``tar.gz`` file using ``tar -xzvf gg_13_8_otus.tar.gz``
* Running qiime command to customize reference

*Generate .qza file using 99_otus.fasta*

```
qiime tools import \
--type 'FeatureData[Sequence]' \
--input-path 99_otus.fasta \
--output-path 99_outs.qza
```
*Generate .qza file using 99_otu_taxonomy.txt*


```
qiime tools import \
--type 'FeatureData[Taxonomy]' \
--source-format HeaderlessTSVTaxonomyFormat \
--input-path 99_otu_taxonomy.txt \
--output-path 99_otu_taxonomy.qza

```

*Extract reference reads based on my primer sequence*

```
qiime feature-classifier extract-reads \
--i-sequences 99_otus.qza \
--p-f-primer CCTACGGGNGGCWGCAG \
--p-r-primer GACTACHVGGGTATCTAATCC \
--o-reads 99-greengene-341-805-ref-seqs.qza
```

*Generate the classifier*
Once all of the above preparation work were done, the next step is to train the classifier using Naive Bayes method
```
qiime feature-classifier fit-classifier-naive-bayes \
--i-reference-reads 99-greengene-341-805-ref-seqs.qza \
--i-reference-taxonomy 99_otu_taxonomy.qza \
--o-classifier 99_greengene_341_805_ref_classifier.qza
```

6. **Gernate taxnomy table using customized classifier**

```
qiime feature-classifier classify-sklearn \
--i-classifier 99_greengene_341_805_ref_classifier.qza \
--i-reads Ag_trial_seqs_dada2.qza \
--o-classification Ag_trial_dada2_taxonomy.qza
```


7. **Export qiime feature-table (as biom)**

```
qiime tools export Ag_trial_table_dada2.qza \
--output-dir Ag_trial_table_dada2_export
```
The output is a biom file, which is not readable using vim.

8. **Add taxonomy information and metadata to OTU biom table** using biom add_metadata function of biom-format package through conda installation ``conda install -c bioconda biom-format ``

  * Copy OTU biom, taxonomy.tsv and metadata.tsv to the same folder 
  * **Remember** to change the header of taxonomy.tsv with #OTUID taxonomy  confidence
  * Double check sampleID from sample_metadata table and that inside of biom, make sure they match with each other and in right order
  * biom add_metadata -i -o --sample-metadata-fp --observation-metadata-fp --sc-separated taxonomy
  * Then the output could be used to be imported to phyloseq using ``import_biom``
  


### Done with qiime unique feature_table and taxonomy analysis

**PICRUST function prediction analysis**

1. Install picrust software in my home directory ``/nics/d/home/fliu21/anaconda2/bin``

  * deactivate qiime2-2017.11  *but still do not have permission for installation on ACF*
  
  ```
  source deactivate
  ```
  * Logout of beacon and re-login
  * conda install -c bioconda picrust
  
2. Mac installation : python 3.5.2  >  anaconda > qiime2-2017.12 

  * Install python 3.5.2 from this [link](https://www.python.org/downloads/)
  * Download Anaconda package for Mac OS system from this [link](https://www.anaconda.com/download/) and double-click to install
  * Follow this documentation for installation of qiime2 within conda environment [link](https://docs.qiime2.org/2017.12/install/native/#install-qiime-2-within-a-conda-environment)

  ```
  conda install wget
  
  wget https://data.qiime2.org/distro/core/qiime2-2017.12-py35-osx-conda.yml
  conda env create -n qiime2-2017.12 --file qiime2-2017.12-py35-osx-conda.yml
  ```
 
3. Mac installation of picrust


 **conda install -c bioconda picrust**

  Error 
  ```
  Solving environment: failed

  UnsatisfiableError: The following specifications were found to be in conflict:
    - picrust
    - pyzmq
  Use "conda info <package>" to see the dependencies for each package.
  ```

   **conda install -c bioconda python=2.7 picrust**
   
  Error
  ```
    UnsatisfiableError: The following specifications were found to be in conflict:
      - clyent -> python=3.5
      - python=2.7
    Use "conda info <package>" to see the dependencies for each package.
  ```
  **Solution**
  
  * ``conda update anaconda``
  * ``conda install -c bioconda picrust``


4. **Generate close_referenced feature table using 99_gg_341_785_seqs as reference sequences**
Following this qiime forum [link](https://forum.qiime2.org/t/picrust-support/1376/10)


  * 99% similarity based
  ```
  qiime vsearch cluster-features-closed-reference \
  --i-sequences Ag_trial_seqs_dada2.qza \
  --i-table Ag_trial_table_dada2.qza \
  --i-reference-sequences 99_greengene_341_805_ref_seqs.qza \
  --p-perc-identity 1 \
  --o-clustered-table Ag_trial_closeRef_99_table_dada2.qza \
  --o-clustered-sequences Ag_trial_closeRef_99_seqs_dada2.qza \
  --o-unmatched-sequences Ag_trial_closeRef_99_unmatched_seqs_dada2.qza
  ```
 

5.  **Normalization** of the OTU table using ``normalize_by_copy_number.py`` from PICRUST 

 *Using close_ref sequence as input for PICRUSTs*
 
  GOT error message
  ```
  Traceback (most recent call last):
  File "/nics/d/home/fliu21/anaconda2/bin/normalize_by_copy_number.py", line 131, in <module>
    main()
  File "/nics/d/home/fliu21/anaconda2/bin/normalize_by_copy_number.py", line 77, in main
    count_table_fh = gzip.open(input_count_table,'rb')
  File "/nics/d/home/fliu21/anaconda2/lib/python2.7/gzip.py", line 34, in open
    return GzipFile(filename, mode, compresslevel)
  File "/nics/d/home/fliu21/anaconda2/lib/python2.7/gzip.py", line 94, in __init__
    fileobj = self.myfileobj = __builtin__.open(filename, mode or 'rb')
IOError: [Errno 2] No such file or directory: '/nics/d/home/fliu21/anaconda2/lib/python2.7/site-packages/picrust/data/16S_13_5_precalculated.tab.gz'
  ```
  * **Solution** - Search and download precalculated 16S data from this [link](http://kronos.pharmacology.dal.ca/public_files/picrust/picrust_precalculated_v1.1.3/13_5/16S_13_5_precalculated.tab.gz)
  
  **NOTE**
  Now in PICRUST1.1.3 version, there is only ``16S_13_5_precalculated.tab.gz`` and older version``18may2012``, BUT not 16S_13_8_precalculated.tab.gz. However, 16S_13_8_gg is the greengene reference I used for taxnomy and closeRef OTU clustering. As a quick practice, I just use 16S_13_8_precalculated.tab.gz (which is not right).
  
  * Now when I run the command it just works fine
  ```
  normalize_by_copy_number.py \
  -i feature-table.biom \
  -o normalized_feature_table.biom 
  ```
6. **Function prediction** using normalized OTU table 
  Similar error pops out. No such file or directory:'/nics/d/home/fliu21/anaconda2/lib/python2.7/site-packages/picrust/data/ko_13_5_precalculated.tab.gz'. Solution is the same, just download KO file from this [link](http://kronos.pharmacology.dal.ca/public_files/picrust/picrust_precalculated_v1.1.3/13_5/ko_13_5_precalculated.tab.gz) to ``/nics/d/home/fliu21/anaconda2/lib/python2.7/site-packages/picrust/data/`` directory using ``wget`` command
  ```
  predict_metagenomes.py -i normalized_feature_table.biom -o metagenome_prediction.biom
  ```
7. Categorize the KO list to pathway information using KEGG_pathway at 3 level

```
categorize_by_function.py -i metagenome_prediction.biom -c KEGG_Pathways -l 3 -o metagenome_prediction_L3.biom
```
  
 






