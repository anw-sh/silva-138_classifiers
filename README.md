# silva-138_classifiers
Classifiers trained on different variable regions of Prokaryotic 16S rRNA genes

**Database:** [SILVA release_138 nr99 SSU](https://www.arb-silva.de/fileadmin/silva_databases/release_138/Exports/SILVA_138_SSURef_NR99_tax_silva.fasta.gz)  
**Tutorials:** QIIME 2 - [Feature Classifier](https://docs.qiime2.org/2021.4/tutorials/feature-classifier/) & [RESCRIPt](https://forum.qiime2.org/t/processing-filtering-and-evaluating-the-silva-database-and-other-reference-sequence-data-with-rescript/15494)

## Workflow

#### Downloading SILVA data
```bash
qiime rescript get-silva-data --p-version '138' --p-target 'SSURef_NR99' --p-include-species-labels --o-silva-sequences silva-138-ssu-nr99-seqs.qza --o-silva-taxonomy silva-138-ssu-nr99-tax.qza
```
*Taxonomy file has 7 levels as required and without gaps (all levels are labelled). Not performing the rank propagation step.*

<br>

#### Removing low quality sequences
```bash
qiime rescript cull-seqs --i-sequences silva-138-ssu-nr99-seqs.qza --o-clean-sequences silva-138-ssu-nr99-seqs-cleaned.qza
```

<br>

#### Remove Eukaryotic taxa
```bash
qiime taxa filter-seqs --i-sequences silva-138-ssu-nr99-seqs-cleaned.qza --i-taxonomy silva-138-ssu-nr99-tax.qza --p-exclude 'd__Eukaryota' --p-mode 'contains' --o-filtered-sequences silva138_noEuk_seqs.qza
```

<br>

#### Filter by length
*Archaea, Bacteria & Eukaryota (900, 1200, 1400 bases) - Excluding Eukaryota as it has already been removed.*
```bash
qiime rescript filter-seqs-length-by-taxon --i-sequences silva138_noEuk_seqs.qza --i-taxonomy silva-138-ssu-nr99-tax.qza --p-labels Archaea Bacteria --p-min-lens 900 1200 --o-filtered-seqs silva138_noEuk_AB_seqs.qza --o-discarded-seqs silva138_Euk_seqs_discard.qza
```
*This step did not discard any sequences, as those sequences were filtered in the previous step itself*

<br>

#### Dereplicating 
*Default mode - uniq; Default rank-handles - silva*
```bash
qiime rescript dereplicate --i-sequences silva138_noEuk_AB_seqs.qza --i-taxa silva-138-ssu-nr99-tax.qza --p-threads 12 --o-dereplicated-sequences silva138_noEuk_AB_seqs_uniq.qza --o-dereplicated-taxa silva138_noEuk_AB_tax_uniq.qza
```

___


### V4 amplicons
#### Extracting V4 region
_**Primers:** 515f & 806r_
```bash
qiime feature-classifier extract-reads --i-sequences silva138_noEuk_AB_seqs_uniq.qza --p-f-primer GTGYCAGCMGCCGCGGTAA --p-r-primer GGACTACNVGGGTWTCTAAT --p-n-jobs 12 --o-reads silva138_AB_V4seqs.qza
```
_**Output:** [silva138_AB_V4seqs.qza](https://mega.nz/file/JHgQmRIA#9lZfwBgzGWiWrCPFOJKbZaA21ecqC2K_8dKM2v8WkGQ)_

<br>

#### Dereplicating the target region
```bash
qiime rescript dereplicate --i-sequences silva138_AB_V4seqs.qza --i-taxa silva138_noEuk_AB_tax_uniq.qza --o-dereplicated-sequences silva138_AB_V4seqs_uniq.qza --o-dereplicated-taxa silva138_AB_V4taxa-uniq.qza --p-threads 12
```
_**Dereplicated Sequences:** [silva138_AB_V4seqs_uniq.qza](https://mega.nz/file/Eahi3DZI#EjWrksgKgwGuGH4KWz9QGjIuIwXKSaUBo99ryAhaJzU)_  
_**Dereplicated Taxa:** [silva138_AB_V4taxa-uniq.qza](https://mega.nz/file/5D5CSLDL#sHzTKdAhxd5BOvJ4TT-wwev1l4XAXFcFArH9jnEXcDc)_

<br>

#### Classifying & evaluating with RESCRIPt
Using multiple threads increases memory usage [Ref](https://forum.qiime2.org/t/memoryerror-when-running-feature-classifer-with-pre-trained-classifier/566/3). Tried and sticking with 1 thread and using --p-reads-per-batch option (took approx 24 hrs)
```bash
qiime rescript evaluate-fit-classifier --i-sequences silva138_AB_V4seqs_uniq.qza --i-taxonomy silva138_AB_V4taxa-uniq.qza --p-reads-per-batch 10000 --o-classifier silva138_AB_V4_classifier.qza --o-observed-taxonomy silva138_AB_V4_predicted_taxonomy.qza --o-evaluation silva138_AB_V4_classifier_eval.qzv
```
_**Classifier:** [silva138_AB_V4_classifier.qza](https://mega.nz/file/8S5wjZDC#32T4bG6QsUgHTAjlU0oP0d-51ZbctePwTMM4sWn_oyw)_  
_**Predicted Taxonomy:** [silva138_AB_V4_predicted_taxonomy.qza](https://mega.nz/file/xToi2BBY#Cywghlh5PNfnKg7F18E-6Qw8BkFOUDSaeyNIrrnt9Uw)_  
_**Evaluation:** [silva138_AB_V4_classifier_eval.qzv](https://mega.nz/file/ZDwAnJgL#OVei4pRuHAzH2LVhJmFosQD2xeLAbgPq6xt-iFJ_IM8)_  
