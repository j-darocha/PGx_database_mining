#PharmGKB African data extraction:

#PharmGKB variant related data is arranged in four files. Descriptions for these are available in VARIANT_ANNOTATIONS_README.pdf

#The studyparams.txt file contains info related to the ethnicity of the samples studied:

###
pops_pgkb.txt shows the distribution of samples. Its unclear why this file doenst have PubmedID in it. Here are the numbers based on unique entries:
     22 
    517 African American/Afro-Caribbean
     55 American
    427 Central/South Asian
      2 Central/South Asian, African American/Afro-Caribbean
      2 COS-1 cells
      6 dysgenic myotubes
   6174 East Asian
   6812 European
     31 in vitro
      1 in-vitro
    258 Latino
      1 Lymphoblastoid Cell Lines
   4703 Mixed Population
    338 Near Eastern
      4 Non human cell line
      4 Not applicable.
      1 Not applicable - study carried out in BHK cells
      2 Not applicable - study carried out in Xenopus oocytes
      3 Not applicable - study was carried out in PBE cells
      4 Not applicable - study was carried out using rectal organoids
      4 Oceanian
      1 Race(s)
    354 Sub-Saharan African
   6534 Unknown
###

#I sliced the studyparams using grep with a list of the ones I wanted (AF_list) {African American/Afro-Caribbean Sub-Saharan African}: 

grep -f AF_list study_parameters.tsv > African_AfAm_studyparams.txt

#(grep -v Asian to exlude the two       2 Central/South Asian, African American/Afro-Caribbean)

#Once you have this, slice out the "StudyParameters" ID

awk '{print $1}' African_AfAm_studyparamsh.txt > Afs_AM_studyparamslist.txt

#Use this to extract the values from the three other variant files
grep -f Afs_AM_studyparamslist.txt var_drug_ann.tsv > Afs_Ams_vardrug.txt
grep -f Afs_AM_studyparamslist.txt var_pheno_ann.tsv > Afs_Ams_varpheno.txt 
grep -f Afs_AM_studyparamslist.txt var_fa_ann.tsv > Afs_Ams_var_fa.txt 

#Put the headers on
head -n1 var_drug_ann.tsv > h_vardrug.txt
head -n1 var_fa_ann.tsv > h_varfa.txt
head -n1 var_pheno_ann.tsv > h_pheno.txt


cat h_vardrug.txt Afs_Ams_vardrug.txt > Afs_Ams_vardrug_h.txt 
cat h_varfa.txt Afs_Ams_var_fa.txt > Afs_Ams_var_fa_h.txt 
cat h_pheno.txt Afs_Ams_varpheno.txt > Afs_Ams_varpheno_h.txt 

#Prep Near Eastern IDs using pbID manually looked at by team to see if samples African

grep -f NE_pbiDs.txt var_drug_ann.tsv > NE_NorthAfs_vardrug.txt
grep -f NE_pbiDs.txt var_pheno_ann.tsv > NE_NorthAfs_varpheno.txt
grep -f NE_pbiDs.txt var_fa_ann.tsv > NE_NorthAfs_var_fa.txt

#Problem - mistake ID clearly included pbID 28135763
#The study participants in this are turkish - mistake of content team who did review
#Maybe there are more - we could redo in time

grep -v 28135763 NE_NorthAfs_vardrug.txt > NE_NorthAfs_vardrug_clean.txt 
grep -v 28135763 NE_NorthAfs_varpheno.txt > NE_NorthAfs_varpheno_clean.txt
#(NE_var_pheno_is_blank)

#And just merge them back up

cat Afs_Ams_vardrug_h.txt NE_NorthAfs_vardrugh_clean.txt > African_NorthAf_and_AF_Amer_PharmGKB_vardrug.txt
cat Afs_Ams_varpheno_h.txt NE_NorthAfs_varpheno_clean.txt > African_NorthAf_and_AF_Amer_PharmGKB_varpheno.txt
cat Afs_Ams_var_fa_h.txt NE_NorthAfs_var_fa.txt > African_NorthAf_and_AF_Amer_PharmGKB_varfa.txt


# The African_NorthAf_and_AF_Amer_PharmGKB_vardrug.txt is our table of interest. But as headers are linked in the other databases, they can be also used too. 
