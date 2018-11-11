# Retrieving-genome-sequence-data-using-SeqinR
Dengue virus 1, complete genome  NCBI Reference Sequence: NC_001477.1
getncbiseq <- function(accession)
{
  require("seqinr") # this function requires the SeqinR R package
  # first find which ACNUC database the accession is stored in:
  dbs <- c("genbank","refseq","refseqViruses","bacterial")
  numdbs <- length(dbs)
  for (i in 1:numdbs)
  {
    db <- dbs[i]
    choosebank(db)
    # check if the sequence is in ACNUC database 'db':
    resquery <- try(query(".tmpquery", paste("AC=", accession)), silent = TRUE)
    
    if (!(inherits(resquery, "try-error"))) {
      queryname <- "query2"
      thequery <- paste("AC=", accession, sep="")
      query2 <- query(queryname, thequery)
      # see if a sequence was retrieved:
      seq <- getSequence(query2$req[[1]])
      closebank()
      return(seq)
    }
    closebank()
  }
  print(paste("ERROR: accession",accession,"was not found"))
}

##to retrieve a sequence from the
##NCBI Nucleotide database, such as the sequence for
##the DEN-1 Dengue virus (accession NC_001477):

dengueseq <- getncbiseq("NC_001477")

#The variable dengueseq is a vector containing the nucleotide sequence (10735 character). Each element of the vector contains one
#nucleotide of the sequence.
#to subset a certain sequence
dengueseq[1:50]
