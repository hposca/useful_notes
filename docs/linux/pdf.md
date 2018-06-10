# Reduce size of PDF files

- [compression - How can I reduce the file size of a scanned PDF file? - Ask Ubuntu](http://askubuntu.com/questions/113544/how-can-i-reduce-the-file-size-of-a-scanned-pdf-file)
- [compression - How to reduce the size of a pdf file? - Ask Ubuntu](http://askubuntu.com/questions/207447/how-to-reduce-the-size-of-a-pdf-file)
- [Reducing PDF file-size in Linux | The Road to Elysium](http://jorge.fbarr.net/2012/11/29/reducing-pdf-file-size-in-linux/)
- [Bash Shell: Ignore Aliases and Functions When Running A Command](http://www.cyberciti.biz/faq/ignore-shell-aliases-functions-when-running-command/)

Low quality options:

    gs -dNOPAUSE -dBATCH -sDEVICE=pdfwrite -dCompatibilityLevel=1.4 -dPDFSETTINGS=/screen -sOutputFile=new_file.pdf original_file.pdf
    gs -dNOPAUSE -dBATCH -sDEVICE=pdfwrite -dCompatibilityLevel=1.4 -dPDFSETTINGS=/ebook -sOutputFile=new_file.pdf original_file.pdf

The best balance was found in:

    gs -dNOPAUSE -dBATCH -sDEVICE=pdfwrite -dCompatibilityLevel=1.4 -dPDFSETTINGS=/default -sOutputFile=new_file.pdf original_file.pdf

**Note:** You may need to issue the command with a backslash in front of it if you use 'gs' as an alias:

    \gs -dNOPAUSE -dBATCH -sDEVICE=pdfwrite -dCompatibilityLevel=1.4 -dPDFSETTINGS=/default -sOutputFile=new_file.pdf original_file.pdf

# Splitting and merging PDF files

With hints from:

- [Linux Commando: Splitting up is easy for a PDF file](http://linuxcommando.blogspot.com.br/2013/02/splitting-up-is-easy-for-pdf-file.html)
- [Linux Commando: How to split up PDF files - part 2](http://linuxcommando.blogspot.com.br/2014/01/how-to-split-up-pdf-files-part-2.html)
- [Linux Commando: How to merge or split pdf files using convert](http://linuxcommando.blogspot.com.br/2015/03/how-to-merge-or-split-pdf-files-using.html)

    sudo apt-get update && sudo apt-get install -y pdftk

## Splitting

You can specify page ranges like this:

    pdftk myoldfile.pdf cat 1-2 4-5 output mynewfile.pdf

pdftk has a few more tricks in its back pocket. For example, you can specify a burst operation to split each page in the input file into a separate output file.

    pdftk myoldfile.pdf burst

By default, the output files are named pg_0001.pdf, pg_0002.pdf, etc.

## Merging

pdftk is also capable of merging multiple pdf files into one pdf.

    pdftk pg_0001.pdf pg_0002.pdf pg_0004.pdf pg_0005.pdf output mynewfile.pdf

That would merge the files corresponding to the first, second, fourth and fifth pages into a single output pdf.

# Joining/merging pdf pages/files into a single one

Install `pdfjam`:

```bash
sudo apt-get install -y pdfjam
```

(Maybe, there is the need to install `texlive-latex-recommended` too)

Merge files with:

```bash
pdfjam Page1.pdf Page2.pdf --nup 2x1 --landscape --outfile Page1_2.pdf
```

Merge pages from the same file:

```bash
pdfjam file.pdf --nup 2x1 --landscape --outfile together.pdf
```

All this was possible with the help of https://superuser.com/questions/366490/how-to-merge-multiple-pdf-files-onto-one-page-with-pdftk/750293#750293

# Convert PDFs into JPEGs

## All pages into the same file

```
for i in *.pdf; do convert -density 600 -append $i ${i%.*}.jpg; done
```

With the help of [linux - How to merge images in command line? - Stack Overflow](https://stackoverflow.com/questions/20075087/how-to-merge-images-in-command-line)
