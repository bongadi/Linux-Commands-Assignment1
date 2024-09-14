# Linux-Commands-Assignment1
This assignment is linked to the Introduction of common Linux commands 

This dataset is for a FASTA file, create your new working directory, and a fasta file and copy the following sequences to do the assignment. The assingment will focus on advanced usage of `grep`, `sed`, `awk`, `wc`, and scripting.

Dataset: sample_sequences.fasta
```
>Sequence1
ATCGTAGCTAGCTAGCTCGATGCGTAGCTAG
>Sequence2
CGTAGCTAGATCGTAGCTGACTGACTGATCG
>Sequence3
GCTAGTCGATCGATCGTACGTACGTAGCAGT
>Sequence4
CTGACTGATCGTACGTAGCTAGCATCGTAGC
>Sequence5
ATCGTAGCATCGTACGTGACTAGCTGACTAG
>Sequence6
TGACTGACTGACGCTGACTGCTAGTCGATGC
>Sequence7
GTCGATCGATCGTAGCTGACGTCGATCGTAG
>Sequence8
CGTAGCTAGATCGTAGCTGACTGACTGATCG
>Sequence9
ATCGTAGCTAGCTAGCTCGATGCGTAGCTAG
>Sequence10
GCTAGTCGATCGATCGTACGTACGTAGCAGT
```
## Question 1: Sequence Analysis
Using `grep` and `awk`, find the number of sequences in the file where:

The sequence contains the motif GCTAG.
The length of the sequence is more than 30 nucleotides.
Write a single command that combines these conditions and outputs the total number of sequences matching both.

## Question 2: Sequence Modification
Using `sed`, replace all occurrences of the motif CGTA with GCAT in the sample_sequences.fasta file. However, ensure the modification is only applied to sequences where the line starts with ATCG. After applying the changes, save the result to a new file called modified_sequences.fasta.

## Question 3: Scripting for Statistics
Write a bash script called sequence_stats.sh that:

Takes the sample_sequences.fasta file as input.
- Calculates the total number of sequences in the file.
- Counts how many sequences contain the pattern ATCGT.
- Outputs the percentage of sequences that contain ATCGT compared to the total number of sequences.
- Ensure that the script accepts the file as an argument when running.

# Answers
#### Answer 1:
To find sequences that contain GCTAG and are longer than 30 nucleotides, use this combination of grep and awk:
```
grep -B1 "GCTAG" sample_sequences.fasta | awk '/^>/ {header=$0} !/^>/ && length($0) > 30 {print header}'
```
This command will:

Use `grep` -B1 "GCTAG" to find the sequences that contain the motif GCTAG and include the header before it.
Use `awk` to filter sequences longer than 30 nucleotides and print their headers.
To count the number of such sequences:
```
grep -B1 "GCTAG" sample_sequences.fasta | awk '/^>/ {header=$0} !/^>/ && length($0) > 30 {print header}' | wc -l
```
#### Answer 2:
To replace CGTA with GCAT only in sequences that start with ATCG and save the result to a new file:
```
sed '/^ATCG/s/CGTA/GCAT/g' sample_sequences.fasta > modified_sequences.fasta
```
This command ensures that sed only modifies lines that start with ATCG (/^ATCG/) and replaces CGTA with GCAT.

Answer 3:
Here’s the sequence_stats.sh script:
```
#!/bin/bash

# Check if a file was provided as an argument
if [ -z "$1" ]; then
  echo "Please provide a FASTA file as input."
  exit 1
fi

# Input file
input_file=$1

# Count the total number of sequences (lines starting with '>')
total_sequences=$(grep -c "^>" "$input_file")

# Count the number of sequences containing 'ATCGT'
sequences_with_pattern=$(grep -B1 "ATCGT" "$input_file" | grep -c "^>")

# Calculate the percentage
if [ $total_sequences -eq 0 ]; then
  percentage=0
else
  percentage=$(echo "scale=2; ($sequences_with_pattern/$total_sequences)*100" | bc)
fi

# Output results
echo "Total sequences: $total_sequences"
echo "Sequences with ATCGT: $sequences_with_pattern"
echo "Percentage: $percentage%"
```
This script takes a FASTA file as input.
It calculates the total number of sequences using grep -c "^>".
It finds how many sequences contain ATCGT using grep -B1 "ATCGT" and counts the headers.
The percentage of sequences containing ATCGT is calculated and displayed.
