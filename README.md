# s y n t e n y - B L A S T  Synteny blast is a tool to identify conserved genetic context(s) around a homologues of a protein of interest.

#Usage:
genbank_skinner.py ./*/*.gbff > MY_DATABASE.faa

makeblastdb -in  MY_DATABASE.faa -input_type fasta -dbtype prot

python .\FIND_ME_NEW_PROTEINS.py -input /path/to/sequence.faa -database /path/to/DB.faa -output /path/to/folder  --rps_db /path/to/rpsBLSTdb

#Notes:
It takes ~5-10 minutes to run a normal query with all streptomyces as the DB.  Much of that time is in running rpsBLAST so if you don't specify that DB it will run faster.

For the curious or frustrated, it functions as follows:
1.  Print multiple fasta file from genbank files using genbank_skinner.py.  This preserves context info associated with the CDS.

2.  Make the resulting fasta into a blast db.  Not automated currently.

3.  Search query seq. against DB.  Important! Warning! If the query seq is not in the DB, results may be messed up.

4.  Parse results.  For all good hits, store proteins on either side of the hit.  Write these to a new fasta file, where the names contain info on the protein itself as well as the progenitor

5a.  Make a secondary blast db and run all v. all blast

5b.  Optionally, run rpsBLAST to tag proteins with CDD, pfam, or other domain annotations.

6.  Parse those results, keeping the context info associated with each sequence.

7.  Print tabular output.

8.  Make cluster-level and protein-level networks; write gml files.


#terminology:
seed sequence:        The original protein used as the query for the first blast search.  Generally, a protein you are interested in.  Potentially YFP.

cluster_ID            ID of protein homologous to the seed sequence.

cluster_member_ID     ID of protein located within n coding sequences of cluster_ID protein.

homologue_ID          ID of protein with homology to cluster_member_ID.


#Todo:
   Switch making of the networks to before the printing the tables

   Input:  Sanitize the inputs to get rid of special characters, add routine for killing redundancy in species names.

   Output:  Add function to generate graphical output.  Ideally, can generate aligned graphical output from multiple input files.

   Clustering:  Kmodes clustering based on PFAM domains, or alternately based on the protein level graph output.  Color the cluster network.

   Cytoscape:  Hyperlink from node to tabular output or graphics?


#Make new DBs:
  full bacteria DB

  Diverse test db for running quick checks

#Usability and beautification:
  Add writing of intermediate outputs

  improved user input

#Performance enhancement:
  DIY fasta parser?  Actually right now most of the time is running BLAST or rpsBLAST, I think.

  Parsing the 2ndary BLAST results may also be a performance limit in cases with a large number of very similar clusters.

#Aesthetics:
  alternate strategy vs. the ungodly ID storage approach?

  Does multigeneblast do similar? Can I use their custom DBs?
