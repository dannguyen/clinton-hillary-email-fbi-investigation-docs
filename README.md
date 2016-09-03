# OCR copy of the 2016-2016 FBI Investigation into Hillary Clinton's emails

The FBI did a Friday document dump of [a redacted 58-page summary](http://www.nytimes.com/interactive/2016/09/02/us/politics/document-Hillary-Clinton-FBI-Investigation.html) of its interview and investigation into Secretary Hillary Clinton's email system ([NYT story here](http://www.nytimes.com/2016/09/03/us/politics/hillary-clinton-fbi.html)).

Unlike [the State Department](https://foia.state.gov/Search/Collections.aspx), the FBI did not run its document through optical character recognition, which makes it impossible to do a computerized search for text keywords. So this repo contains the results of running the FBI PDF through [ABBYY FineReader Pro (for Mac)](https://www.abbyy.com/support/finereader_pro_for_mac/sr/).

Here's the OCR'ed PDF for your convenience: 

[clinton-hillary-fbi-investigation.pdf](//dannguyen.github.io/clinton-hillary-email-fbi-investigation-docs/clinton-hillary-fbi-investigation.pdf)

And for those of you who like to grep, here's the plaintext output from running [Poppler pdftotext](https://poppler.freedesktop.org/) (with `-layout`) on that PDF: 

[clinton-hillary-fbi-investigation.txt](//dannguyen.github.io/clinton-hillary-email-fbi-investigation-docs/clinton-hillary-fbi-investigation.txt)





# Technical deets

After using ABBYY FineReader to create an [OCR'ed copy of the PDF](clinton-hillary-fbi-investigation.pdf), I ran this shell code to use poppler's __pdftotext__ to convert the entire PDF into [one text file](clinton-hillary-fbi-investigation.txt), and then to convert the individual PDF pages into [individual text files](txt-individual-pages):


```sh

pdftotext -layout clinton-hillary-fbi-investigation.pdf

mkdir -p txt-individual-pages

find pdf-individual-pages -name '*.pdf' \
| while read -r fname; do
    newname=$(echo "$fname" | sed 's/pdf/txt/g')
    pdftotext -layout "$fname" "$newname"
done
```
