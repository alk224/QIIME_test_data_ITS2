Andrew Krohn, June 23, 2017

This is ITS2 mock community data I generated on a MiSeq.

Data has had phix contamination removed.

Data is paired end reads which have been joined.

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
it [here.](http://hannonlab.cshl.edu/fastx_toolkit/)

To get you started, follow these instructions:

1) Save all data in a directory called "raw_data". If you need to start again, you can
just copy out some fresh data. In this example, I will rename the files to something
simpler to type at the same time.

    mkdir raw_data  
    mv map.ITS.mock.csv ./raw_data/map.txt  
    mv idx_sub.fq ./raw_data/idx.fq  
    mv rd_sub.fq ./raw_data/rd.fq  
    
