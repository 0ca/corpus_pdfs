# corpus_pdfs
A set of pdf documents used during the fuzzing process

The pdfs are coming from the following sources:
* https://github.com/mozilla/pdf.js 
* https://github.com/euske/pdfminer
* https://github.com/smalot/pdfparser
* https://github.com/galkahana/PDF-Writer
* https://github.com/modesty/pdf2json
* https://github.com/codeborne/pdf-test
* https://github.com/DmitryRumiantsev/Pdf-test-examples
* https://github.com/veraPDF/veraPDF-corpus
* https://www.pdfa.org/wp-content/until2016_uploads/2011/08/isartor-pdfa-2008-08-13.zip
* https://archive.is/o/vZvxX/www.pdflib.com/fileadmin/pdflib/Bavaria/2009-04-03-Bavaria-pdfa.zip
* https://github.com/openpreserve/format-corpus/tree/master/pdfCabinetOfHorrors
* https://github.com/qpdf/qpdf/tree/master/qpdf/qtest/qpdf

There is a folder for each binary we've minimized for. The pdfall folder contains all of the pdfs which can be used to minimize with other binaries.

The pdfs_all folder also contains the uncompressed pdfs obtained with:
```
find . -type f -exec qpdf --stream-data=uncompress {} {}_uncompressed.pdf \;
```

The redo the corpus (for a different software or because we have more pdfs), it is necessary to follow the next steps:
```
rm -rf pdfs_all_* &&
mkdir pdfs_all_10k &&
mkdir pdfs_all_100k &&
mkdir pdfs_all_1M &&
mkdir pdfs_all_10M &&
find corpus_pdfs/pdfs_all/ -size -10k  -exec cp "{}" pdfs_all_10k/ \; &&
find corpus_pdfs/pdfs_all/ -size -100k -size +10k -exec cp "{}" pdfs_all_100k/ \; &&
find corpus_pdfs/pdfs_all/ -size -1000k -size +100k -exec cp "{}" pdfs_all_1M/ \; &&
find corpus_pdfs/pdfs_all/ -size -10000k -size +1000k -exec cp "{}" pdfs_all_10M/ \; &&
rm -rf corpus_pdfs/poppler/10k_min/ &&
rm -rf corpus_pdfs/poppler/100k_min/ &&
rm -rf corpus_pdfs/poppler/1M_min/ &&
rm -rf corpus_pdfs/poppler/10M_min/ &&
afl-cmin -t 1000 -m 100 -i pdfs_all_10k/ -o corpus_pdfs/poppler/10k_min/ -- poppler-latest/utils/pdftocairo @@ &&
afl-cmin -t 1000 -m 100 -i pdfs_all_100k/ -o corpus_pdfs/poppler/100k_min/ -- poppler-latest/utils/pdftocairo @@ &&
afl-cmin -t 1000 -m 100 -i pdfs_all_1M/ -o corpus_pdfs/poppler/1M_min/ -- poppler-latest/utils/pdftocairo @@ &&
afl-cmin -t 1000 -m 100 -i pdfs_all_10M/ -o corpus_pdfs/poppler/10M_min/ -- poppler-latest/utils/pdftocairo @@
```

It's very important to do first the size filtering and then the corpus minimization.

The idea is to skip all the pdfs taking more than 1 second to be processed or more than 100MB of memory.
