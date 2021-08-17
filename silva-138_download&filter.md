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
