# Creating a new dataset

Once we have our clean alignments, the next step is to create datasets for different type of analyses. The main goals here are: 1) to rename specimen names in every alignment file, 2) to subset the data, 3) to count the number of loci per specimen or the number of specimens per locus we have in our datasets, and 4) to concatenate and add missing sequences to specimens in every fasta file.

## Software that we need
We will need to have three [pipelines already installed](https://github.com/pavelm14/Eudaminae_phylogeny/tree/master/bioinformatics/installations) for this part:
- SeqKit: a cross-platform toolkit for phylogenomic fasta manipulation. This will allow us to batch rename taxa in every fasta file.
- SECAPR: a semi-automated pipeline that will allow us to add missing sequences to specimens in every fasta file.
- _catfasta2phyml_: a program written in perl that will allow us to concatenate and get partition files.

## Rename specimens in every fasta file
We will use the SeqKit toolkit to rename taxon name given a list of names. The list should be a tab-delimited file and the names in this file should exactly match the ones in the fasta file. The example file **taxon_list.txt** is in `src` and in my laptop `Hesperiidae/Laboratory/NGS bioinformatics`

```bash
pavelmatos@nympha:~$
cd ~/eudaminae/processed/cleaned_geneious
source ~/.bashrc #activate and use SeqKit from the PATH environment

# we will make a for loop to go through every fasta file in /cleaned_geneious
# note that the file taxon_list.txt is in one directory above, in ~/eudaminae/processed

for file in *.fasta; do
seqkit replace -p "(.+)" -r '{kv}' -k ../taxon_list.txt "$file" > "${file%%.*}"".fas"
done
```

Above, we ran the _replace_ option of SeqKit. This command uses regular expression to rename taxa in fasta files. In the `-p` and `-r` flags we tell the program to entirely replace string having a value by key-value file (taxon_list.txt). More info about the option and flags [here](https://bioinf.shenwei.me/seqkit/usage/#replace).

The new fasta files will have the same name but a different extension. For example, we had the original file `P1.fasta` so we will have the file `P1.fas` with the renamed taxa.

Some programs need to have the fasta files with sequences in one single line. To make it universal, we will reformat the renamed fasta files that have multi-line sequence information. We will simply use bash commands to remove those break lines in the middle of the DNA sequences. In the process, we will move the renamed, single-line-sequences fasta files to a new subfolder cleaned_geneious\renamed.

```bash
pavelmatos@nympha:~/eudaminae/processed/cleaned_geneious$
mkdir renamed

# we will make a for loop to go through every fasta file in /cleaned_geneious
# we will use a awk formula to remove break lines within sequences and 
# move the new fasta files to the folder /renamed

for file in *.fas; do
awk '/^>/ {printf("\n%s\n",$0);next; } { printf("%s",$0);}  END {printf("\n");}' < "$file" > "./renamed/""${file%%.*}"".fasta"
done
```

We used an `awk` script with the first block triggered when a line begins with `>`, that is, the sequence header. We ask the formula to print the header line on its own line. The other lines are skipped and passed to the second block given the `next` argument. The second block prints the sequence without newline, until it reaches the next `>` header. The last, third block will print the final newline (sequence) after the last header line. Script adapted from [here](https://unix.stackexchange.com/questions/346143/understanding-an-awk-formula-that-unwraps-fasta-files).


You can have a look at how many loci a particular specimen has DNA sequence. We use the `grep` command with `-i` (ignore case), `-R` (read all files recursively) and `-l` (print the file name where there is a match).

```bash
pavelmatos@nympha:~/eudaminae/processed/cleaned_geneious$ pwd
grep -iRl ">001" ./ | wc -l

cd ./renamed
grep -iRl "PM-UN-2-2" ./ | wc -l

# the two grep output should give you the same value
```
