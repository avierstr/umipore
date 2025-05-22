# umipore
<mark>This is a work in progress:</mark>

<mark>Demultiplexing based on barcodes , dual barcodes, primers is working good.  
(Demultiplexing based on UMI's is still under construction.)</mark>

Umipore is a tool to demultiplex Oxford Nanopore reads (or sequences from other platforms) based on barcodes, dual barcodes, primers or UMI's (unique molecular identifiers).

(Only works on Linux/Unix and Mac because it uses multiprocessing.  Mac with M1 processor can cause problems because of the multiprocessing)

Requirements:

- Python 3
- edlib: Lightweight, super fast C/C++ library for sequence alignment using edit (Levenshtein) distance ([https://pypi.org/project/edlib/#description](https://pypi.org/project/edlib/#description)) (pip: `python3 -m pip install edlib` or conda: `conda install bioconda::python-edlib`) 
-   biopyton (pip: `pip install biopython` or conda: `conda install biopython` or in linux `sudo apt-get install python3-biopython`)

### Licence
GNU GPL 3.0

### Keywords
amplicon sequencing, MinION, Oxford Nanopore Technologies, barcode, dual barcode, UMI, demultiplexing

### Options:

`-i, --input`: Input **file** in fastq or fasta format.  Also a **folder** can be given as input and will be scanned for .fasta or .fastq files to process.  Make sure the input file(s) is (are) named as .fasta or .fastq because it replaces the extension in parts of the script.

`-o, --outputfolder`: Save the results in the specified outputfolder. Default = folder named as the inputfile.
 
`-min, --minlength`: Minimum readlenght to process.

`-max, --maxlength`: Maximum readlenght to process.  

`-np, --nprocesses`: Number of processors to use.  Default = 4

`-sk, --seq_kit`: Sequencing kit used in experiment (lsk, lsk109, lsk110, lsk114).

`-ns, --no_split`: Split the reads on middle adapter. Default = True.

`-bck, --bc_kit`: Barcode kit(s) used in experiment (nbd, rbk, pbc, pbk, rpb, rab, 16s, pcb).

`-bckn, --bc_kit_numbers`: Barcode numbers from the kit used in experiment.  Default = all 96.

`-bcc, --bc_custom`: File with custom barcodes used in experiment in csv format.

`-tr, --trim`:Trim the barcodes from the reads.  Default = do not trim barcodes.

`-bc_bs, --bc_both_sides`: Search for barcodes on both sides.  Default = True.
 
`-bc_os, --bc_one_side`: Search for barcode on one sides.  Default = False.

### Less important options:

`-sf, --sformat`: File format to save the files.  Default: same as inputfile. (fasta or fastq).

`-sp, --search_part`: Part at begin and end of sequence to search for adapters or barcodes.  Default = 150 bp longer than the adapter or barcode.

`-er, --error`: Percentage error allowed in editdistance for adapters and barcodes. Default = 0.15 (15%).

`-ca, --custom_adapters`: File with adapters from other sequencing technologies in case your data is not from Nanopore Sequencing.  (comma or tab separated csv file) 

### Command examples:

Demultiplex infile.fastq on 8 cores, reads with a minimum length of 500 bp, sequenced with the LSK110 Ligation kit, with custom barcodes and barcodes have to be found on both sides:

`python3 umipore.py -i infile.fastq -np 8 -min 500 -sk lsk110 -bcc custom.csv -bc_bs`

The file with the custom barcodes `custom.csv` is a csv file (comma or tab separated) with 3 columns: name, forward sequence, reverse sequence (5'-3').  The primers in the examples below contain a barcode, flank, gene primer.  
```
BC01,TCGAAGAAAGTTGTCGGTGTCTTTGTGACGACGTTGTAGAGAGTTTGATCMTGGCTCAG,TCGAAGAAAGTTGTCGGTGTCTTTGTGGATGGTCGATGACGGTTACCTTGTTACGACTT
BC02,TCGTCGATTCCGTTTGTAGTCGTCTGTACGACGTTGTAGAGAGTTTGATCMTGGCTCAG,TCGTCGATTCCGTTTGTAGTCGTCTGTGATGGTCGATGACGGTTACCTTGTTACGACTT
BC03,TCGGAGTCTTGTGTCCCAGTTACCAGGACGACGTTGTAGAGAGTTTGATCMTGGCTCAG,TCGGAGTCTTGTGTCCCAGTTACCAGGGATGGTCGATGACGGTTACCTTGTTACGACTT
BC04,TCGTTCGGATTCTATCGTGTTTCCCTAACGACGTTGTAGAGAGTTTGATCMTGGCTCAG,TCGTTCGGATTCTATCGTGTTTCCCTAGATGGTCGATGACGGTTACCTTGTTACGACTT
```
Demultiplex infile.fastq on 8 cores, reads with a minimum length of 500 bp, sequenced with the LSK110 Ligation kit and with the native barcode kit from which I used barcodes 6, 7, 8, 9, 10, 11, 12 and the barcode has to be found on one side:

`python3 umipore.py -i infile.fastq -np 8 -min 500 -sk lsk110 -bck nbd -bckn 6-12 -bc_os`

By default, Umipore will split reads based on the middle adapter from the LSK kit in the command above (turn off with command: `-ns, --no_split`).  It will also search and split reads if it finds the used barcodes in the middle of reads.

### Todo:

### Release notes:

2025-05-22:
- a separate file (adapters.txt) with Nanopore adapter and barcode sequences was needed in the same folder.  Now this information is embedded in the script.
- option added to uses adapters from other sequencing technologies (`-ca, --custom_adapters`).

2024-11-19:
- Improvement in splitting reads based on middle adapters and primers.  When there was a 100% match and a 99% match in the read, it was only splitting the 100% match.  Now it is splitting all.

2024-02-10:
- initial release

(Written with [StackEdit](https://stackedit.io/).)

