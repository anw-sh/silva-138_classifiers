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
