# UNIX assignment

##Inspection
###File size
$ ls -lh fang_et_al_genotypes.txt 
-rw-r--r--  1 mahsa  staff    11M Sep 26 10:11 fang_et_al_genotypes.txt

$ ls -lh snp_position.txt 
-rw-r--r--  1 mahsa  staff    81K Sep 26 10:11 snp_position.txt

###Columns
$ awk -F "\t" '{print NF; exit}' fang_et_al_genotypes.txt 
986

$ awk -F "\t" '{print NF; exit}' snp_position.txt 
15

###Rows
$ grep -c "[^ \\n\\t]" snp_position.txt 
984

$ grep -c "[^ \\n\\t]" fang_et_al_genotypes.txt 
2783

###Other inspections

$ head fang_et_al_genotypes.txt

$ head snp_position.txt

$ tail fang_et_al_genotypes.txt

$ tail snp_position.txt

$ cat fang_et_al_genotypes.txt

$ cat snp_position.txt

$ less fang_et_al_genotypes.txt

$ less snp_position.txt

$ more fang_et_al_genotypes.txt

$ more snp_position.txt



##Processing
###extracting maize and teosinte

$ grep -v ZMP fang_et_al_genotypes.txt > maize_genotypes.txt

$ grep -v ZMM fang_et_al_genotypes.txt > teosinte_genotypes.txt

###Transpose of Maize and Teosinte

$ awk -f transpose.awk teosinte_genotypes.txt > transposed_teosinte_genotypes.txt

$ awk -f transpose.awk maize_genotypes.txt > transposed_maize_genotypes.txt

###Sorting Mazie and Teosinte

$ sort -k1,1 -k2,2n transposed_teosinte_genotypes.txt > sorted_teosinte.txt

$ sort -k1,1 -k2,2n transposed_maize_genotypes.txt > sorted_maize.txt

###Cutting SNP_ID, Chromosome and position columns from the snp_position.txt

$ cut -f 1,3,4 snp_position.txt > cut_snp_position.txt

###Sorting cut_snp_position.txt 

$ sort -k1,1 -k2,2n cut_snp_position.txt > sorted_cut_snp_position.txt


###Joining Teosinte and Maize with snp_position

$ join -1 1 -2 1 -t $'\t' sorted_teosinte.txt sorted_cut_snp_position.txt > joined_sorted_teosinte.txt

$ join -1 1 -2 1 -t $'\t' sorted_maize.txt sorted_cut_snp_position.txt > joined_sorted_maize.txt

###The chromosome and position columns were in the end of file so I needed to bring them to $2 and $3

$ awk -F "\t" '{print NF; exit}' joined_sorted_maize.txt
1810

$ awk -F'\t' -v OFS="\t" '{$2=$1809 OFS $2;$1809=""}7' joined_sorted_maize.txt > K2_joined_sorted_maize.txt 

$ awk -F'\t' -v OFS="\t" '{$3=$1811 OFS $3;$1811=""}7' k2_joined_sorted_maize.txt > K3_K2_joined_sorted_maize.txt

$ awk -F "\t" '{print NF; exit}' joined_sorted_teosinte.txt 
1212

$ awk -F'\t' -v OFS="\t" '{$2=$1211 OFS $2;$1211=""}7' joined_sorted_teosinte.txt > K2_joined_sorted_teosinte.txt

$ awk -F'\t' -v OFS="\t" '{$3=$1213 OFS $3;$1213=""}7' k2_joined_sorted_teosinte.txt > K3_K2_joined_sorted_teosinte.txt

###Making 10 files based on chromosome number in maize and teosinte

$ for i in {1..10}; do awk '$2 == '$i'' K3_K2_joined_sorted_teosinte.txt > chr"$i"_teosinte.txt; done

$ for i in {1..10}; do awk '$2 == '$i'' K3_K2_joined_sorted_maize.txt > chr"$i"_maize.txt; done

 

###Replacing ?/? with ? in Maize and Teosinte

$ sed 's|?/?|?|g' chr1_maize.txt > sed?_chr1_maize.txt

$ sed 's|?/?|?|g' chr9_teosinte.txt > sed?_chr9_teosinte.txt

I ran the code above for all the seperate chromosome files of maize and teosinte 



###Sorting based on increasing positions in Maize and Teosinte

$ sort -k3,1n sed?_chr1_teosinte.txt > inc_sed?_chr1_teosinte.txt

$ sort -k3,1n sed?_chr9_maize.txt > inc_sed?_chr9_maize.txt

I ran the code above for all the seperate chromosome files of maize and teosinte


###Replacing ?/? with - in Maize and Teosinte

$ sed 's|?/?|-|g' chr1_maize.txt > sed-_chr1_maize.txt

$ sed 's|?/?|-|g' chr9_teosinte.txt > sed-_chr9_teosinte.txt

I ran the code above for all the seperate chromosome files of maize and teosinte


###Sorting based on decreasing positions in Maize and Teosinte

 $ sort -k3,1nr sed-_chr9_maize.txt > dec_sed-_chr9_maize.txt
 
 
 $ sort -k3,1nr sed-_chr9_teosinte.txt > dec_sed-_chr9_teosinte.txt
 
 I ran the code above for all the seperate chromosome files of maize and teosinte
 
 
 ###Multiple and unknown positions 
 
 
$ grep unknown K3_K2_joined_sorted_teosinte.txt > unknown_teosinte.txt

$ grep unknown K3_K2_joined_sorted_maize.txt > unknown_maize.txt

$ grep multiple K3_K2_joined_sorted_maize.txt > multiple_maize.txt

$ grep multiple K3_K2_joined_sorted_teosinte.txt > multiple_teosinte.txt

$ awk '$2 != "multiple"' multiple_teosinte.txt > final_multiple_teosinte.txt

$ awk '$2 != "multiple"' multiple_maize.txt > final_multiple_maize.txt

