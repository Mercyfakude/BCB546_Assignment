#UNIX Assignment

##Data Inspection

###Attributes of `fang_et_al_genotypes`

```
These are my codes for inspecting the head and the tail of the file 
  $ cat fang_et_al_genotypes.txt
  $ head fang_et_al_genotypes.txt
  $ head -n 5 fang_et_al_genotypes.txt
  $ tail -n 5 fang_et_al_genotypes.txt
  $ (head -n 2; tail -n 2) < fang_et_al_genotypes.txt
  $ wc fang_et_al_genotypes.txt
  $ du -h fang_et_al_genotypes.txt
  $ tail -n +6 fang_et_al_genotypes.txt | awk -F "\t" '{print NF; exit}'ere is my snippet of code used for data inspection
```
- At first, I tried to inspect the head and the tail of the file but the file is huge I could not detect the head and the tail.
- I then used command 'wc' to inspect the characters of the file. 
- I then used command 'du' -h to inspect the size of the file.
- I then used command 'awk' command to inspect the columns of the file 

By inspecting this file I learned that:
1. the has 2783 number of line  
2. the file has 2783 number of lines, 2744038 words, and 11051939 Mb
3. the file is 6.6M
4. the file has 986 columns

###Attributes of `snp_position.txt`

```
$ head -n 1 snp_position.txt
$ wc snp_position.txt.txt
$ wc snp_position.txt.txt
$ tail -n +6 snp_position.txt | awk -F "\t" '{print NF; exit}'
```

By inspecting this file I learned that:
1. the file has 15 headers     
2. the has 2783 number of line  
3. the file has 984 number of lines, 13198 words, and 82763 Mb
4. the file is 41k
5. the file has 15 columns

##Data Processing

###Maize Data (Processing snp_position.txt)

```
$ cut -f 1,3,4 snp_position.txt > snpmaize_excol.txt
$ less snpmaize_excol.txt
$ sort -k1,1 snpmaize_excol.txt > snpmaize_excol_sorted.txt

Processing fang_et_al_genotypes for maize data
$ grep -E "(ZMMIL|ZMMLR|ZMMMR)" fang_et_al_genotypes.txt > fangmaize_exgeno.txt
$ grep -E "Sample_ID" fang_et_al_genotypes.txt > fangmaize_sampleid.txt
$ cp fangmaize_sampleid.txt fangteosinte_sampleid.txt
$ cat fangmaize_exgeno.txt >> fangmaize_sampleid.txt
$ awk -f transpose.awk fangmaize_sampleid.txt > fangmaize_sampleid_transposed.txt
$ sed 's/Sample_ID/SNP_ID/' fangmaize_sampleid_transposed.txt > fangmaize_sampleid_transposed2.txt
$ sort -k1,1 fangmaize_sampleid_transposed2.txt > fangmaize_sorted.txt
$ join -1 1 -2 1 snpmaize_excol_sorted.txt fangmaize_sorted.txt > fangmaize_joined.txt
awk '$2 ~ /^1$/' fangmaize_joined.txt > chr1_maize.txt 
awk '$2 ~ /^2$/' fangmaize_joined.txt > chr2_maize.txt
awk '$2 ~ /^3$/' fangmaize_joined.txt > chr3_maize.txt
awk '$2 ~ /^4$/' fangmaize_joined.txt > chr4_maize.txt
awk '$2 ~ /^5$/' fangmaize_joined.txt > chr5_maize.txt
awk '$2 ~ /^6$/' fangmaize_joined.txt > chr6_maize.txt
awk '$2 ~ /^7$/' fangmaize_joined.txt > chr7_maize.txt
awk '$2 ~ /^8$/' fangmaize_joined.txt > chr8_maize.txt
awk '$2 ~ /^9$/' fangmaize_joined.txt > chr9_maize.txt
awk '$2 ~ /^10$/' fangmaize_joined.txt > chr10_maize.txt
grep "unknown" fangmaize_joined.txt > maize_unknown.txt
grep "multiple" fangmaize_joined.txt > maize_unknown.txt
sort -k3,3n chr1_maize.txt >chr1_maize_ascending.txt
sed 's/?/-/g' chr1_maize.txt | sort -k3,3nr > maize_chr1_descending.txt


```
1. cut command extracts columns (SNP_ID, Chromosome and Position) from the original file "snp_position.txt" and created a new file to paste the 3 extracted columns:   
2. less' command to confirm that the newly created file "snpmaize_excol.txt" contains the 3 extracted columns so I can join the in later steps
3. I used the 'q' command to escape or quit the new file and go back to my terminal  
4. I then used the sort command to sort the file by the first column  
5. grep -E command to extract the required genotypes from "fang_et_al_genotypes" file and paste to a new file fangmaize_exgeno.txt"
6. grep -E "Sample_ID" command to to extract the samples ID insert them on the newly created maize file "fangmaize_exgeno.txt" 
7. cp to copy the file with extracted headers "fangmaize_sampleid.txt" with the file that will have teosinte headers or samples IDs later on "fangteosinte_sampleid.txt"
8. cat to put the file with extracted headers "fangmaize_sampleid.txt" under the extracted genotype file "fangmaize_exgeno.txt"
9. awk -f transpose.awk to transpose the the headers or sample IDs and insert them in a new file "fangmaize_sampleid_transposed.txt"
10. sed 's/Sample_ID/SNP_ID/'to change the header in the genotype from sample IDs to SNP IDs and insert to a new file "fangmaize_sampleid_transposed2.txt"
11. sort -k1,1 to sort the SNP file by the first column and saved it in a new file "fangmaize_sorted.txt"
12. join command to join the SNP file and the genotypes file and saved them in 
13. awk command command to print out SNP and the records for each chromosome and save them in a in a new file.
14. grep to extract the SNP which are unknown and multiple.
15. sort -k3,3n to sort maize chromosomes with SNPs files in an ascending order and save them in a new file "chr1_maize_ascending.txt" -----> repeated for all 10 chromosome.
16. sed Replace missing data with encoded by the "?" by symbol "-"

 
###Teosinte Data (Processing snp_position.txt)

```
$ cut -f 1,3,4 snp_position.txt > snpteosinte_excol.txt
$ less snpteosinte_excol.txt
$ 'q' command to escape or quit the new file and go back to my terminal.   
$ sort -k1,1 snpteosinte_excol.txt > snpteosinte_excol_sorted.txt

Processing fang_et_al_genotypes (For teosinte)
$ grep -E "(ZMPBA|ZMPIL|ZMPJA)" fang_et_al_genotypes.txt > fangteosinte_exgeno.txt
$ grep -E "Sample_ID" fang_et_al_genotypes.txt > fangteosinte_sampleid.txt
$ cat fangteosinte_exgeno.txt >> fangteosinte_sampleid.txt
$ awk -f transpose.awk fangteosinte_sampleid.txt > fangteosinte_sampleid_transposed.txt
$ sed 's/Sample_ID/SNP_ID/' fangteosinte_sampleid_transposed.txt > fangteosinte_sampleid_transposed2.txt
$ sort -k1,1 fangteosinte_sampleid_transposed2.txt > fangteosinte_sorted.txt
$ join -1 1 -2 1 snpteosinte_excol_sorted.txt fangteosinte_sorted.txt > fangteosinte_joined.txt
$ awk '$2 ~ /^1$/' fangteosinte_joined.txt > chr1_teosinte.txt 
$ awk '$2 ~ /^2$/' fangteosinte_joined.txt > chr2_teosinte.txt
$ awk '$2 ~ /^3$/' fangteosinte_joined.txt > chr3_teosinte.txt
$ awk '$2 ~ /^4$/' fangteosinte_joined.txt > chr4_teosinte.txt
$ awk '$2 ~ /^5$/' fangteosinte_joined.txt > chr5_teosinte.txt
$ awk '$2 ~ /^6$/' fangteosinte_joined.txt > chr6_teosinte.txt
$ awk '$2 ~ /^7$/' fangteosinte_joined.txt > $ chr7_teosinte.txt
$ awk '$2 ~ /^8$/' fangteosinte_joined.txt > chr8_teosinte.txt
$ awk '$2 ~ /^9$/' fangteosinte_joined.txt > chr9_teosinte.txt
$ awk '$2 ~ /^10$/' fangteosinte_joined.txt > chr10_teosinte.txt
$ grep "unkown" fangteosinte_joined.txt > teosinte_unknown.txt
$ grep "mulptiple" fangteosinte_joined.txt > teosinte_unknown.txt
$ sort -k3,3n chr1_teosinte.txt> chr1_teosinte_ascending.txt
$ sed 's/?/-/g' chr1_teosinte.txt | sort -k3,3nr > chr1_teosinte_descending.txt 
```

Here is my brief description of what this code does
1. cut command extracts columns (SNP_ID, Chromosome and Position) from the original file "snp_position.txt" and created a new file to paste the 3 extracted columns:   
2. less' command to confirm that the newly created file "snpteosinte_excol.txt" contains the 3 extracted columns 
3. I used the 'q' command to escape or quit the new file and go back to my terminal  
4. I then used the sort command to sort the file by the first column
1. grep -E command to extract the required genotypes from "fang_et_al_genotypes" file and paste to a new file "fangteosinte_exgeno.txt"
2. grep -E "Sample_ID" command to extract the samples ID insert them on the newly created maize file "fangteosinte_exgeno.txt" 
4. cat to put the file with extracted headers "fangteosinte_sampleid.txt" under the extracted genotype file "fangteosinte_exgeno.txt"
3. awk -f transpose.awk to transpose the the headers or sample IDs and insert them in a new file "fangteosinte_sampleid_transposed.txt"
5. sed 's/Sample_ID/SNP_ID/'to change the header in the genotype from sample IDs to SNP IDs and insert to a new file "fangteosinte_sampleid_transposed2.txt"
6. sort -k1,1 to sort the SNP file by the first column and saved it in a new file "fangteosinte_sorted.txt"
7. join command to join the SNP file and the genotypes file and saved them in a new file "fangteosinte_joined.txt
1. awk command command to print out SNP and the records for each chromosome and save them in a in a new file.
2. grep to extract the SNP which are unknown and multiple.
3. sort -k3,3n to sort maize chromosomes with SNPs files in an ascending order and save them in a new file "chr1_teosinte_ascending.txt" -----> repeated for all 10 chromosome.
4. sed Replace missing data with encoded by the "?" by symbol "-"
