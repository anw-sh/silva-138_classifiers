### V3 amplicons
#### Extracting V3 region
_**Primers:** 341f & 518r_
```bash
qiime feature-classifier extract-reads --i-sequences silva138_noEuk_AB_seqs_uniq.qza --p-f-primer CCTACGGGNGGCWGCAG --p-r-primer GTATTACCGCGGCTGCTGG --p-n-jobs 12 --o-reads silva138_AB_V3seqs.qza
```
_**Output:** [silva138_AB_V3seqs.qza](https://mega.nz/file/FbxylRaI#OxZmLQU878fiTJI-YvRKwzoOy67HoXEkTGGlg8xP94I)_

<br>

#### Dereplicating the target region
```bash
qiime rescript dereplicate --i-sequences silva138_AB_V3seqs.qza --i-taxa silva138_noEuk_AB_tax_uniq.qza --o-dereplicated-sequences silva138_AB_V3seqs_uniq.qza --o-dereplicated-taxa silva138_AB_V3taza_uniq.qza
```
_**Dereplicated Sequences:** [silva138_AB_V3seqs_uniq.qza](https://mega.nz/file/dSpCFLjb#Lun5LEj6GljOo_YN81Kn9Le14hzuFQzLUhdc4atLtHA)_  
_**Dereplicated Taxa:** [silva138_AB_V3taxa_uniq.qza](https://mega.nz/file/Abw0XbSL#mPBi4jO33aR9njoCY0UaYcdQY8XssQws4eaaP2BBWCk)_

<br>

#### Classifying & evaluating with RESCRIPt
Using multiple threads increases memory usage [Ref](https://forum.qiime2.org/t/memoryerror-when-running-feature-classifer-with-pre-trained-classifier/566/3). Using 1 thread with auto reads per batch (took approx 20 hrs)
```bash
qiime rescript evaluate-fit-classifier --i-sequences silva138_AB_V3seqs_uniq.qza --i-taxonomy silva138_AB_V3taxa_uniq.qza --o-classifier silva138_AB_V3_classifier.qza --o-observed-taxonomy silva138_AB_V3_predicted_taxonomy.qza --o-evaluation silva138_AB_V3_classifier_eval.qzv
```
_**Classifier:** [silva138_AB_V3_classifier.qza](https://mega.nz/file/tD4wELQD#RCdoXEKBIGb07_pMugTesQCLjjfOJu24llJfzFUBKt8)_  
_**Predicted Taxonomy:** [silva138_AB_V3_predicted_taxonomy.qza](https://mega.nz/file/sToynbrB#va9alyidk5pqhVMnqmwuZmUGXKbsY_xBhwei_-tF-K0)_  
_**Evaluation:** [silva138_AB_V3_classifier_eval.qzv](https://mega.nz/file/9OwwmZAK#L0B0TjdCEwJM340xGbs5f_dROOJPGAaSWqiRTkuqPGo)_  
