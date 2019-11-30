# Human reference genome, version hg19

    wget "ftp://hgdownload.soe.ucsc.edu/goldenPath/hg19/bigZips/hg19.2bit"
	wget "ftp://hgdownload.soe.ucsc.edu/goldenPath/hg19/bigZips/md5sum.txt"

I then edited md5sum.txt to remove all lines except hg19.2bit and ran

    md5sum -c md5sum.txt

The result confirms that hg19.2bit is okay.

I also downloaded twoBitToFa and faToTwoBit from

    http://hgdownload.soe.ucsc.edu/admin/exe/linux.x86_64/

and installed in my bin directory. bcftools needs the index, which
requires a fasta file compressed with bgzip:

    twoBitToFa hg19.2bit stdout | bgzip -c > hg19.fa.gz
	samtools faidx hg19.fa.gz

Within hg19.2bit, the chromosomes are sorted in numerical
order. Legofit files, on the other hand, sort chromosomes in lexical
order. One could use numerically-sorted fasta files with legofit,
because the fast files are indexed, and bcftools could find the
appropriate chromosome. But this would involve a lot of seeking back
and forth within the file. It should speed things up to sort the fasta
files in lexical order. The job below also removes "chr" from the name
of each chromosome.

    sbatch sort.slr

This creates hg19-sort.fa.gz, compressed with bgzip. Finally, index
the sorted file:

    samtools faidx hg19-sort.fa.gz



