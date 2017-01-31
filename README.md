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

There are four folders:
* pdfsall: All the pdfs documents
* pdfs_10kb_tmin: PDF documents smaller than 10k and minimized with tmin to avoid duplicates
* pdfs_30kb_tmin: PDF documents smaller than 10k and minimized with tmin to avoid duplicates
* pdfs_100kb_tmin: PDF documents smaller than 10k and minimized with tmin to avoid duplicates

The minimized has been done with the following command:
```
afl-cmin -i pdfsfolder_10k/ -o pdfsfolder_10k_min/ -t 1000 -m 100 -- src/pdf2swf @@
```

The idea is to skip all the pdfs taking more than 1 second to be processed or more than 100MB of memory.
