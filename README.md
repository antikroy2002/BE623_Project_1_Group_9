# BE 623 Biocomputing Project 1 Group 9
# Members
Antik Roy -Gene Name - MT-CO3
           Gene Type - Mitochondrial gene
Subhi Tiwari -Gene Name - MYC gene
            -Gene Type - Nuclear gene
# WorkFlow
Gene was searched on NCBI using API
Query was refined using RefSeq filter
Sequences were downloaded in .fasta and .gb format
Key fields and headers of the files were checked and saved into headers.txt
Then CDS region from the downloaded sequence was extracted
The CDS region was translated with a manual codon table 
The translated sequence was then compared with the deposited GenBank Protein for sequence similarity and length similarity
The difference,if there was any, was checked and position of difference was determined 
The obtained results were cross verified using BioPython
The results were committed and pushed to the GitHub

# Obtained results :
Total nucleotide length
Number of codons
Protein length
First 30 amino acids residue

# Codes and commands used 
# Gene was searched on NCBI using API and Query was refined using RefSeq filter
https://eutils.ncbi.nlm.nih.gov/entrez/eutils/esearch.fcgi?
db=nucleotide&term=MYC[gene]+AND+human[orgn]

https://eutils.ncbi.nlm.nih.gov/entrez/eutils/esearch.fcgi?
db=nucleotide&term=HBB[gene]+AND+human[orgn]+AND+refseq

# Sequences were downloaded in .fasta and .gb format and Key fields and headers of the files were checked and saved into headers.txt

https://eutils.ncbi.nlm.nih.gov/entrez/eutils/efetch.fcgi?
db=nucleotide&id=NM_002467&rettype=fasta&retmode=text

grep ">" sequence.gb>>headers.txt
grep ">" mycprotein.fasta>>headers.txt
grep "LOCUS" sequence.gb>>headers.txt

#  CDS region from the downloaded sequence was extracted
seq = ""

for line in open("sequence.fasta"):
    if not line.startswith(">"):
        seq += line.strip()

cds = seq[363:1728]
print(cds)
print("total nucleotide :" , len(cds))
print("number of codons:", len(cds)/3)


# CDS region was translated with a manual codon table 

protein = ""
for i in range(0, len(cds), 3):
    codon = cds[i:i+3]
    protein= protein+ gcode[codon]

print("Protein length:", len(protein))
print("First 30 residues:", protein[:30])


# The translated sequence was then compared with the deposited GenBank Protein for sequence similarity and length similarity


c="MDFFRVVENQQPPATMPLNVSFTNRNYDLDYDSVQPYFYCDEEENFYQQQQQSELQPPAPSEDIWKKFELLPTPPLSPSRRSGLCSPSYVAVTPFSLRGDNDGGGGSFSTADQLEMVTELLGGDMVNQSFICDPDDETFIKNIIIQDCMWSGFSAAAKLVSEKLASYQAARKDSGSPNPARGHSVCSTSSLYLQDLSAAASECIDPSVVFPYPLNDSSSPKSCASQDSSAFSPSSDSLLSSTESSPQGSPEPLVLHEETPPTTSSDSEEEQEDEEEIDVVSVEKRQAPGKRSESGSPSAGGHSKPPHSPLVLKRCHVSTHQHNYAAPPSTRKDYPAAKRVKLDSVRVLRQISNNRKCTSPRSSDTEENVKRRTHNVLERQRRNELKRSFFALRDQIPELENNEKAPKVVILKKATAYILSVQAEEQKLISEEDLLRKRREQLKHKLEQLRNSCA"
print(protein==c)
print(len(protein),len(c))

# The difference,if there was any, was checked and position of difference was determined 
difference=0
for i in range(min(len(protein),len(c))):
    if protein[i]!=c[i]:
         difference=difference +1
        
         print("total difference= " ,difference+1)
         print("protein:",protein[i])
         print("c:",c[i])
         break


# The obtained results were cross verified using BioPython
from Bio.Seq import Seq
bio_protein=Seq(cds).translate()
print(bio_protein[0:30])
bio_protein=Seq(cds).translate()

if protein==bio_protein:
    print("yes")
else :
    print("no")
