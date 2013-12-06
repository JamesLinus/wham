Purpose:
------
WHole-genome Aligment Metricts (wham BAM) is a suite of tools designed to identify anomalies in binary alignment/mapping files (BAM).  

The toolkit has two programs:

### wham 

Wham is designed to look for anomalies within a single bam file

### whamy

Whamy is designed to look for anomalies across groups of bams.  

Installing:
-----

###Dependencies:

  Cmake:
    Make your life easier and install Cmake.  No action should be required under most linux distributions. 
    If yes then use a package manager to install cmake.

  Boost - At least 1.42:
    1.  Boot devel should be installed via Yum or other package manager.
  LibZ:
    1. No action should be required under most linux distributions.

###Bamtools:

1.  git clone https://github.com/pezmaster31/bamtools
2.  cd bamtools
3.  mkdir build
4.  cd build
5.  cmake ..
6.  sudo make install


###wham BAM:
   
1.  git clone git://github.com/jewmanchue/wham.git
2.  cd wham
3.  mkdir build
4.  cd build
5.  cmake ..
6.  make install
7.  cd ..
8.  rm -rf build

Testing:
-----

sh /test/run-test.sh

Basic usage:
-----

### wham

  wham mybam.bam mybam.bai scaffoldX > stdout 2> stderr

## whamy

Whamy requires sorted BAMs.  It is also ideal to remove the duplicates, but not required.  If the scaffold flag 
is not set whamy will run the whole genome.

  whamy --target a.bam,b.bam,c.bam --background d.bam,e.bam,f.bam --scaffold chr1 > stdout 2> stderr

Output:
-----

A tab delimited text file.
Columns:

1. Seqid.
2. Position in the pileup. 
3. Number of "bad reads" covering the position in the pileup.
4. Number of reads covering the position in the pileup.
5. Fraction of "bad reads"
6. Probability of observing K bad reads out of N trials (See Details)
7. Global fraction of bad reads.



Details:
-----

The Samtools flags are hashed across the entire scaffold.  From this the global fraction
of bad reads is calculated.  Then the scaffold is "pileup."  For each position with 
coverage the CDF of the binomial is calculated where:

K = number of bad reads (column 4)
N = number of reads  (column 5)
P = global fraction of bad reads (column 7)

Bad reads include: Mate not mapped, and same strand.  Further information will be added
soon.  IE: the insert size etc....
