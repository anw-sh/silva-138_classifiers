### V3-V4 amplicons
#### Extracting V3-V4 region
_**Primers:** 341f & 805r_
```bash
qiime feature-classifier extract-reads --i-sequences silva138_noEuk_AB_seqs_uniq.qza --p-f-primer CCTACGGGNGGCWGCAG --p-r-primer GACTACHVGGGTATCTAATCC --p-n-jobs 12 --o-reads silva138_AB_V3-V4seqs.qza
```
_**Output:** [silva138_AB_V3-V4seqs.qza](https://mega.nz/file/1LQkVT4T#rzedpymo5dhnp9b6g39t5AOZW31bI2bYVidIalseK2I)_

<br>

#### Dereplicating the target region
```bash
qiime rescript dereplicate --i-sequences silva138_AB_V3-V4seqs.qza --i-taxa silva138_noEuk_AB_tax_uniq.qza --o-dereplicated-sequences silva138_AB_V3-V4seqs_uniq.qza --o-dereplicated-taxa silva138_AB_V3-V4taxa_uniq.qza
```
_**Dereplicated Sequences:** [silva138_AB_V3-V4seqs_uniq.qza](https://mega.nz/file/9XQShbLB#o_QZ7F-rtDdzPShHbCTptUWfLwBaRB3Ui1LC2bwWaYs)_  
_**Dereplicated Taxa:** [silva138_AB_V3-V4taxa_uniq.qza](https://mega.nz/file/BaZmTZxT#Hzq5wHXpTU-68xf-413ifWNAfwCB2pp8hxSTgE4ulYM)_

<br>

#### Classifying & evaluating with RESCRIPt
Using multiple threads increases memory usage [Ref](https://forum.qiime2.org/t/memoryerror-when-running-feature-classifer-with-pre-trained-classifier/566/3). Using 1 thread with auto reads per batch (took approx 30 hrs)
```bash
qiime rescript evaluate-fit-classifier --i-sequences silva138_AB_V3-V4seqs_uniq.qza --i-taxonomy silva138_AB_V3-V4taxa_uniq.qza --o-classifier silva138_AB_V3-V4_classifier.qza --o-observed-taxonomy silva138_AB_V3-V4_predicted_taxonomy.qza --o-evaluation silva138_AB_V3-V4_classifier_eval.qzv
```
_**Classifier:** [silva138_AB_V3-V4_classifier.qza](https://mega.nz/file/BeYUhb7A#G5AMi0IheVzJGGptlZhZM2cs6p9RO0FS-iWGF3fb1os)_  
_**Predicted Taxonomy:** [silva138_AB_V3-V4_predicted_taxonomy.qza](https://mega.nz/file/gbImBRiC#-S1yT08NhaOI1wScibaAdv_4jHBPbrZaaLnjjT8II48)_  
_**Evaluation:** [silva138_AB_V3-V4_classifier_eval.qzv](https://mega.nz/file/VaRWDbKD#3OJyipfi1MM5eeFU1BxqMQuKuY33xX1USmykRsLGn-o)_  
