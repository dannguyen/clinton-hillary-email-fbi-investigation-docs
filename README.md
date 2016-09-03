# OCR copy of the 2016-2016 FBI Investigation into Hillary Clinton's emails

The FBI did a Friday document dump of [a redacted 58-page summary](http://www.nytimes.com/interactive/2016/09/02/us/politics/document-Hillary-Clinton-FBI-Investigation.html) of its interview and investigation into Secretary Hillary Clinton's email system ([NYT story here](http://www.nytimes.com/2016/09/03/us/politics/hillary-clinton-fbi.html)).

Unlike [the State Department](https://foia.state.gov/Search/Collections.aspx), the FBI did not run its document through optical character recognition, which makes it impossible to do a computerized search for text keywords. So this repo contains the results of running the FBI PDF through [ABBYY FineReader Pro (for Mac)](https://www.abbyy.com/support/finereader_pro_for_mac/sr/).

Here's the OCR'ed PDF for your convenience: 

[clinton-hillary-fbi-investigation.pdf](//dannguyen.github.io/clinton-hillary-email-fbi-investigation-docs/clinton-hillary-fbi-investigation.pdf)

And for those of you who like to grep, here's the plaintext output from running [Poppler pdftotext](https://poppler.freedesktop.org/) (with `-layout`) on that PDF: 

[clinton-hillary-fbi-investigation.txt](//dannguyen.github.io/clinton-hillary-email-fbi-investigation-docs/clinton-hillary-fbi-investigation.txt)


# Preview

The `-layout` option for __pdftotext__ is handy because it allows you to take some advantage of the visual layout of the plaintext when doing a regex search. Here's a screenshot of [page 31](https://github.com/dannguyen/clinton-hillary-email-fbi-investigation-docs/blob/master/pdf-individual-pages/0031.pdf):

![Page 31 of the FBI docs](http://i.imgur.com/l0ChqVG.jpg)

And here's the [plaintext representation of page 31](https://github.com/dannguyen/clinton-hillary-email-fbi-investigation-docs/blob/master/txt-individual-pages/0031.txt), though I've trimmed some whitespace for brevity. Notice how the markings for exemptions, e.g. `b1`, `b7C`, etc, are positioned on the right-side of their respective text lines, roughly corresponding to how it _looks_ in the scanned page:


(Note that you'll have to scroll horizontally using the scrollbar that follows the plaintext, as Github's web view is too narrow to show the plaintext within its HTML container)

```

                                                                                                                                          bl
                                                                                                                                          b3
                                                                                                                                          b7E


 worried “someone [was] hacking into her email” given that she received an e-mail from a known
                                                                                                                                        b6
 _____ associate containing a link to a website with pornographic material.554 There is no
                                                                                                                                        b7C
 additional information as to why Clinton was concerned about someone hacking into her e-mail
 account, or if the specific link referenced by Abedin was used as a vector to infect Clinton1s
 device I_________________________________________________________
r.u*




                                                                                 |Open source
                                                                                                                                         bl
 information indicated, if opened, the targeted user's device may have been infected, and                                                b3
 information would have been sent to at least three computers overseas, including one in
 Russia.560’56]                                                                                 I


 I).       (U//FOUO) Potential Loss of Classified Information

 (U//FOUO) On March 11, 2011, Boswell sent a memo directly to Clinton outlining an increase
 since January 2011 of cyber actors targeting State employees' personal e-mail accounts.563 The
 memo included an attachment which urged State employees to limit the use of personal e-mail
 for official business since “some compromised home systems have been reconfigured by these
 actors to automatically forward copies of all composed e-mails to an undisclosed recipient.”564
 Clinton's immediate staff was also briefed on cybersecurity threats in April and May 2011.565

 (S//QC/NF
                                                                                                                                              bl
                                                                                                                                              b3

                                                                                                                                          bl
                                                                                                                                          b3
                                                                                                                                          b6
                                                                                                                                          b7C
                                                                                                                                          b7E


 mmmm(U) In order for malicious executables to be effective, the targeted host device has to have the correct program/applications
 installed. If, for example, the host is running an older version of Adobe but the exploit being used is newer, there is a chance the
 host will not be infected because the exploit was unable to execute using the older version of the program.
 'm m (U) A “drop” account, in this case, is an e-mail account controlled by foreign cyber actors and which serves as the recipient
 of auto-forwarded e-mails from victim accounts.

                                                           Page 31 of 47
                                                                                                                                               bl
                                                                        OFORF
                                                                                                                                               b3
                                                                                                                                               b7E

```

If you use a grep-like tool such as [__ack__](http://beyondgrep.com/), you can quickly locate and tally up the exemptions by page and line number:

```sh
ack 'b[A-Z\d]{2,3} *$' txt-individual-pages/*.txt
```




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
