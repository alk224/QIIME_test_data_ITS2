**ITS2 test data for community amplicon analysis**  

This is ITS2 mock community data I generated on a MiSeq.  

Data has had phix contamination removed.  

Data is paired end reads which have been joined.  

Data is subsampled at 1% of the original data so that it will process quickly.  

I cultured everything myself (7 taxa). One of the taxa I think was contaminated.  

Data has 3 reps per community. Use metadata category "Community" for analysis.  

---

To use the data, clone or download this repository, and unpack it. Within the archive,  
you will find three files containing metadata and multiplexed sequences.  

Unpack command (linux bash or mac OS):  

    tar -xzvf ITS2_mock_testdata.tar.gz  

Files:  

    idx_sub.fq (index sequences)  
    rd_sub.fq (ITS2 sequences)  
    map.ITS.mock.csv (QIIME mapping file in tsv format despite the extension)  

The first read on this run was from the LSU end. This means that after quality filtering,  
you should reverse compliment the reads so they will match against an ITS2 database such  
as UNITE. I also think you should trim them uniformly after reverse complimenting. 200bp  
length seems to work well.  

It would be helpful to have fastx toolkit installed for the trimming step. You can find  
it [here.](http://hannonlab.cshl.edu/fastx_toolkit/) You should also have [fastqc](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/) in place for checking read quality  
prior to making a decision on how much to trim your reads.  

---

To get you started, follow these instructions:  

**Step 1:** Save all data in a directory called "raw_data". If you need to start again, you can  
just copy out some fresh data. In this example, I will rename the files to something  
simpler to type at the same time.  

    mkdir raw_data  
    mv map.ITS.mock.csv ./raw_data/map.txt  
    mv idx_sub.fq ./raw_data/idx.fq  
    mv rd_sub.fq ./raw_data/rd.fq  
  
**Step 2:** Copy the data out into your working directory.  

    cp ./raw_data/* ./  

**Step 3:** Check data with fastqc.  

    fastqc rd.fq  

Once the command completes, view the .html file in the output directory.  

**Step 4:** Trim away the low quality data. Quality starts to decline after 200nt. You could  
be more thorough and choose a slightly longer length, but we will use 200 here.

    fastqx_trimmer -i rd.fq -o rd_200.fq -l 200    

**Step 5:** Demultiplex and quality filter your data into QIIME.  

    split_libraries_fastq.py -i rd_200.fq -b idx.fq -m map.txt -o split_libraries  

**Step 4:** Check the result.  

    cat split_libraries/split_library_log.txt  

**Step 5:** Reverse compliment your output in order to keep orientation with the UNITE  
database. This will save computational time during domain filtering by ITSx and taxonomic  
classification.  

    adjust_seq_orientation.py -i split_libraries/seqs.fna -o split_libraries/seqs_rc.fna -r  

**Step 6:** Rename your seqs files so that if using [akutils](https://github.com/alk224/akutils-v1.2), the pick_otus command  
will select the correct file to process.  

    mv split_libraries/seqs.fna split_libraries/original_seqs.fna  
    mv split_libraries/seqs_rc.fna split_libraries/seqs.fna  

**Step 7:** Enter [akutils](https://github.com/alk224/akutils-v1.2) with the pick_otus command. This step presumes you  
already have a valid akutils config file in place to reference all of your parameters.  
For more information, check out the [akutils wiki](https://github.com/alk224/akutils-v1.2/wiki) and [this tutorial](https://github.com/alk224/akutils-v1.2/wiki/Example:-2x300,-2-loci) as it pertains to ITS2  
sequence data.  

    akutils pick_otus ITS  

Good luck, and thanks for trying out my data!  
