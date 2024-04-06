### V1-V2 amplicons
#### Extracting V1-V2 region
_**Primers:** 27f & 338r_
```bash
qiime feature-classifier extract-reads --i-sequences silva138_noEuk_AB_seqs_uniq.qza --p-f-primer AGAGTTTGATCMTGGCTCAG --p-r-primer TGCTGCCTCCCGTAGGAGT --p-n-jobs 12 --o-reads silva138_AB_V1-V2seqs.qza
```
_**Output:** [silva138_AB_V1-V2seqs.qza](https://mega.nz/file/cCwhxQZZ#DSMxha2NvwgqF5di_Xj1k0MMeB3cxeknrUjJfC_46RM)_

<br>

#### Dereplicating the target region
```bash
qiime rescript dereplicate --i-sequences silva138_AB_V1-V2seqs.qza --i-taxa silva138_noEuk_AB_tax_uniq.qza --o-dereplicated-sequences silva138_AB_V1-V2seqs_uniq.qza --o-dereplicated-taxa silva138_AB_V1-V2taxa_uniq.qza
```
_**Dereplicated Sequences:** [silva138_AB_V1-V2seqs_uniq.qza](https://mega.nz/file/pGxiRTwA#o4erg0w7HgkUE3BMs1ua1o4aecdOIOZ2C76_q0Mu6GM)_  
_**Dereplicated Taxa:** [silva138_AB_V1-V2taxa_uniq.qza](https://mega.nz/file/dbp1yAQI#Y6AwXFv0KKRb_yzLew_iZFM3IDZRh6WI-RODETQV2v4)_

<br>

#### Classifying & evaluating with RESCRIPt
Using multiple threads increases memory usage [Ref](https://forum.qiime2.org/t/memoryerror-when-running-feature-classifer-with-pre-trained-classifier/566/3). Using 1 thread with auto reads per batch (took approx 19 hrs)
```bash
qiime rescript evaluate-fit-classifier --i-sequences silva138_AB_V1-V2seqs_uniq.qza --i-taxonomy silva138_AB_V1-V2taxa_uniq.qza --o-classifier silva138_AB_V1-V2_classifier.qza --o-observed-taxonomy silva138_AB_V1-V2_predicted_taxonomy.qza --o-evaluation silva138_AB_V1-V2_classifier_eval.qzv
```
_**Classifier:** [silva138_AB_V1-V2_classifier.qza](https://mega.nz/file/IOoQUR4S#bra_ukhPfUpjJDwhkBIrmaIWkM53haC8G1zsfnKihck)_  
_**Predicted Taxonomy:** [silva138_AB_V1-V2_predicted_taxonomy.qza](https://mega.nz/file/kTxg1Z6I#80Gtezbctn3Q-e8-9wH-banMtVnnmeG0sXvtrcbAkq0)_  
_**Evaluation:** [silva138_AB_V1-V2_classifier_eval.qzv](https://mega.nz/file/hDxwAJpL#e3NGbARvke3GZVPOkserr-Bz-qOJZ9E0HJYYLZo2QhA)_  
