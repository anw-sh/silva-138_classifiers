## Workflow

### Downloading SILVA data
```bash
qiime rescript get-silva-data --p-version '138' --p-target 'SSURef_NR99' --p-include-species-labels --o-silva-sequences silva-138-ssu-nr99-seqs.qza --o-silva-taxonomy silva-138-ssu-nr99-tax.qza
```
*Taxonomy file has 7 levels as required and without gaps (all levels are labelled). Not performing the rank propagation step.*

<br>

### Removing low quality sequences
```bash
qiime rescript cull-seqs --i-sequences silva-138-ssu-nr99-seqs.qza --o-clean-sequences silva-138-ssu-nr99-seqs-cleaned.qza
```

<br>

### Remove Eukaryotic taxa
```bash
qiime taxa filter-seqs --i-sequences silva-138-ssu-nr99-seqs-cleaned.qza --i-taxonomy silva-138-ssu-nr99-tax.qza --p-exclude 'd__Eukaryota' --p-mode 'contains' --o-filtered-sequences silva138_noEuk_seqs.qza
```

<br>

### Filter by length
*Archaea, Bacteria & Eukaryota (900, 1200, 1400 bases) - Excluding Eukaryota as it has already been removed.*
```bash
qiime rescript filter-seqs-length-by-taxon --i-sequences silva138_noEuk_seqs.qza --i-taxonomy silva-138-ssu-nr99-tax.qza --p-labels Archaea Bacteria --p-min-lens 900 1200 --o-filtered-seqs silva138_noEuk_AB_seqs.qza --o-discarded-seqs silva138_Euk_seqs_discard.qza
```
*This step did not discard any sequences, as those sequences were filtered in the previous step itself*

<br>

### Dereplicating 
*Default mode - uniq; Default rank-handles - silva*
```bash
qiime rescript dereplicate --i-sequences silva138_noEuk_AB_seqs.qza --i-taxa silva-138-ssu-nr99-tax.qza --p-threads 12 --o-dereplicated-sequences silva138_noEuk_AB_seqs_uniq.qza --o-dereplicated-taxa silva138_noEuk_AB_tax_uniq.qza
```
_**Dereplicated Sequences:** [silva138_noEuk_AB_seqs_uniq.qza](https://mega.nz/file/xaITGA7L#V9BtsBC9Zd61K0YLgEYJBNpc4WP4fuN4go01sneRhoI)_  
_**Dereplicated Taxa:** [silva138_noEuk_AB_tax_uniq.qza](https://mega.nz/file/IeZnxQ4L#MBreCSxvIa5WG1bSJy7brqo8glfhjDL87KYjqAcSEkw)_

### Classifying & evaluating with RESCRIPt
Generating a full length classifier
> This step was performed with `qiime_2023.5` on a Fedora 39 server

```bash
qiime rescript evaluate-fit-classifier --i-sequences silva138_noEuk_AB_seqs_uniq.qza --i-taxonomy silva138_noEuk_AB_tax_uniq.qza --o-classifier silva138_noEuk_AB_classifier.qza --o-observed-taxonomy silva138_noEuk_AB_predicted_taxonomy.qza --o-evaluation silva138_noEuk_AB_classifier_eval.qzv --p-n-jobs 10
```
_**Classifier:** [silva138_noEuk_AB_classifier.qza](https://mega.nz/file/4GAkjKyK#-sKjy5z8BoO-FKvqYRZQm3zPUL4xKcJT4w5nmqil7Jk)_  
_**Predicted Taxonomy:** [silva138_noEuk_AB_predicted_taxonomy.qza](https://mega.nz/file/0ao0QY7B#hzLrBzrn4pZgPRAKYGOYJBFC89Co7TNEc2yIvogKM_c)_  
_**Evaluation:** [silva138_noEuk_AB_classifier_eval.qzv](https://mega.nz/file/wL5CSBRJ#GVaWH9FbRSemePdF9y1AM2CTiPyd1WzHq9rdz2gUFKU)_

___

- Workflow for [**V1-V2** classifier](V1-V2_classifier.md)
- Workflow for [**V3** classifier](V3_classifier.md)
- Workflow for [**V3-V4** classifier](V3-V4_classifier.md) 
- Workflow for [**V4** classifier](V4_classifier.md)
