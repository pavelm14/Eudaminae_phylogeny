# Creating a new dataset

Once we have our clean alignments, the next step is to create datasets for different type of analyses. The main goals here are: 1) to rename specimen names in every alignment file, 2) to subset the data, 3) to count the number of loci per specimen or the number of specimens per locus we have in our datasets, and 4) to concatenate and add missing sequences to specimens in every fasta file.

##### Table of Contents  
- [Software](#software-that-we-need)  
1. [Rename taxa](#1-rename-specimens-in-every-fasta-file)
   - [Reformat fasta sequences](11-multi--to-single-line-sequences-in-fasta-files)
2. [Count loci per specimen](2-count-number-of-sequences-across-fasta-files)
3. [Extract sequences](3-extract-sequences-of-interest-from-multiple-fasta-files)
   - [Count specimens per locus](31-count-the-number-of-specimens-per-locus)
4. [Add missing sequences](4-add-missing-sequences-to-fasta-files)

## Software that we need
We will need to have three [pipelines already installed](https://github.com/pavelm14/Eudaminae_phylogeny/tree/master/bioinformatics/installations) for this part:
- SeqKit: a cross-platform toolkit for phylogenomic fasta manipulation. This will allow us to batch rename taxa in every fasta file.
- SECAPR: a semi-automated pipeline that will allow us to add missing sequences to specimens in every fasta file.
- _catfasta2phyml_: a program written in perl that will allow us to concatenate and get partition files.

## 1. Rename specimens in every fasta file
We will use the SeqKit toolkit to rename taxon name given a list of names. The list should be a tab-delimited file and the names in this file should exactly match the ones in the fasta file. The example file `taxon_list.txt` is in [src](https://github.com/pavelm14/Eudaminae_phylogeny/tree/master/bioinformatics/src) and in my laptop `Hesperiidae/Laboratory/NGS bioinformatics`

The structure of the fasta file and the list file should be:
```bash
pavelmatos@nympha:~$
cd ~/eudaminae/processed/cleaned_geneious

cat P1.fasta
# >001
# GTCAGTCAGTCAGTCA
# >002
# TGACTGACTGACTGAC

cat ../taxon_list.txt
# Bioinf_code	code
# 001	PM-UN-2-2_Butleria_bissexguttatus_
# 002	PM-UN-41_Bungalotis_astylos_
```

We will use a for loop to go through every fasta file in the folder `/cleaned_geneious`.

```bash
pavelmatos@nympha:~/eudaminae/processed/cleaned_geneious$
#activate and use SeqKit from the PATH environment
source ~/.bashrc #source /storage/plzen1/home/pavelmatos/.bashrc

# note that the file taxon_list.txt is in one directory above, in ~/eudaminae/processed

for file in *.fasta; do
seqkit replace -p "(.+)" -r '{kv}' -k ../taxon_list.txt "$file" > "${file%%.*}"".fas"
done
```

Above, we ran the _replace_ option of SeqKit. This command uses regular expression to rename taxa in fasta files. In the `-p` and `-r` flags we tell the program to entirely replace string having a value by key-value file (taxon_list.txt). More info about the option and flags [here](https://bioinf.shenwei.me/seqkit/usage/#replace).

The new fasta files will have the same name but a different extension. For example, we had the original file `P1.fasta` so we will have the file `P1.fas` with the renamed taxa.

### 1.1. Multi- to single-line sequences in fasta files
Some programs need to have the fasta files with sequences in one single line. To make it universal, we will reformat the renamed fasta files that have multi-line sequence information. We will simply use bash commands to remove those break lines in the middle of the DNA sequences. In the process, we will move the renamed, single-line-sequences fasta files to a new subfolder `/cleaned_geneious/renamed`.

```bash
pavelmatos@nympha:~/eudaminae/processed/cleaned_geneious$
mkdir renamed

# we will make a for loop to go through every fasta file in /cleaned_geneious
# we will use a awk formula to remove break lines within sequences and 
# move the new fasta files to the folder /renamed

for file in *.fas; do
awk '/^>/ {printf("\n%s\n",$0);next; } { printf("%s",$0);}  END {printf("\n");}' < "$file" > "./renamed/""${file%%.*}"".fasta"
done

rm *.fas #remove every multi-line renamed sequences
```

We used an `awk` script with the first block triggered when a line begins with `>`, that is, the sequence header. We ask the formula to print the header line on its own line. The other lines are skipped and passed to the second block given the `next` argument. The second block prints the sequence without newline, until it reaches the next `>` header. The last, third block will print the final newline (sequence) after the last header line. Script adapted from [here](https://unix.stackexchange.com/questions/346143/understanding-an-awk-formula-that-unwraps-fasta-files).

## 2. Count number of sequences across fasta files
You can count how many loci a particular specimen has DNA sequence. We use the `grep` command with `-i` (ignore case), `-R` (read all files recursively) and `-l` (print the file name where there is a match).

```bash
pavelmatos@nympha:~/eudaminae/processed/cleaned_geneious$ pwd
grep -iRl ">001" ./ | wc -l

cd ./renamed
grep -iRl "PM-UN-2-2" ./ | wc -l

# the two grep output should give you the same value
```

For convenience we will move the renamed, single-line sequences to `/cleaned_geneious`, and the original sequences to a new folder `/v0.1`.

```bash
pavelmatos@nympha:~/eudaminae/processed/cleaned_geneious/renamed$ pwd
cd ..
mkdir v0.1
mv *.fasta ./v0.1
mv ./renamed/*.fasta .
rm -r renamed
```

### 2.1. Scaling-up the count across files
You can scale up the code above to count loci in a number of specimens of interest across multiple fasta files. For example, we are interested only on the taxa that will go to our FINAL DATASET. We will use a list with the voucher code of taxa that should match part of the header in the fasta files.

The `final_dataset.txt` is in [src](https://github.com/pavelm14/Eudaminae_phylogeny/tree/master/bioinformatics/src):

```bash
pavelmatos@nympha:~/eudaminae/processed/cleaned_geneious$
# note that the file final_dataset.txt is in one directory above, in ~/eudaminae/processed
# the structure of the file is

cat ../final_dataset.txt
# 187-ADW
# 509-ADW
# 27-ADW

while read -r line; do
c=`grep -iRl "$line" ./*.fasta | wc -l`
echo -e "$line\t$c"
done < ../final_dataset.txt > loci_per_sample.txt
```

In the code above, we search for every line containing voucher codes in the `final_dataset.txt`, and go through every fasta file. We search and count the number of times a voucher code appears (we have only one voucher code per fasta file, no duplicated sequences). Finally, we print the voucher code followed by the counts, and save the list in the file `loci_per_sample.txt`.

## 3. Extract sequences of interest from multiple fasta files
Let's now extract the sequences that will go to our final dataset. We will use the same file `final_dataset.txt` as a reference.

```bash
pavelmatos@nympha:~/eudaminae/processed/cleaned_geneious$
mkdir ../final_dataset

for file in *.fasta; do
while read -r line; do
grep -A1 "$line" "$file"
done < ../final_dataset.txt > "../final_dataset/""$file"
done

```

In the code above, we indicate to go through every fasta file in the current directory `/cleaned_geneious` (renamed and single-line sequences). Then we read every line of the file `final_dataset.txt`, each line having a voucher code of interest for the final dataset. After that, we pick up every line containing the voucher code (i.e., the header in fasta files) and one line after it (the sequence). Finally, we save the taken headers with their respective sequences to files having the same name (e.g., `P1.fasta`) in the folder `/final_dataset`.

### 3.1. Count the number of specimens per locus
We counted above the number of loci per taxon. Now we will cound the number of sequences in the final dataset per locus.

```bash
pavelmatos@nympha:~/eudaminae/processed/cleaned_geneious$
cd ../final_dataset

for file in *.fasta; do
c=`grep ">" "$file" | wc -l`
echo -e "${file%%.*}\t$c"
done > samples_per_locus.txt
```

In the code above, we go through every fasta file in the folder `/final_dataset` and count the number of sequences in every file (locus). Print the locus name (if the file is `P1.fasta` the locus name is P1) with the number of sequences, and save it in the file `samples_per_locus.txt`.

## 4. Add missing sequences to fasta files
When using programs such as BEAST we need to have the same number of taxa in every locus file. We also need to have missing sequences filled with "?" or "N" when using the concatenation method in, for example, IQTREE.

We will use the `add_missing_sequences` function from the SECAPR pipeline to add missing sequences to every fasta alignment.

```bash
pavelmatos@nympha:~/eudaminae/processed/final_dataset$
#activate SECAPR from the PATH environment and conda
source ~/.bashrc #source /storage/plzen1/home/pavelmatos/.bashrc
source activate secapr_env

cd ..
secapr add_missing_sequences --input final_dataset/ --output final_dataset/complete_alignments
```
