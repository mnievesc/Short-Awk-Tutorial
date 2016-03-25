# Short-Awk-Tutorial
The tutorial below is designed to be completed in ~40-45 minutes by anyone with basic understanding of UNIX. To begin please download the three example files to your working directory: chr7.bed, chr11.bed and chroms.csv.  

## What is awk?
Awk is a data manipulation programming language that exists within UNIX, like grep or cut. It is useful for extracting text and performing quick data processing of tabular data. Awk works by processing data a record at a time, each line in a text file is a record and each record is composed of fields. So if you imagine a table full of data, each row is a record and each columnar cell within that row is a field.

Awk commands follow a basic structure of `pattern {action}` where the user gives the program a regular expression to be evaluated. If the regular expression turns out to be true, then a command is executed. As a quick review, a regular expression is just a sequence of characters that define a pattern, so for example if I said `find apples` that would be a regular expression that would tell my program to go find me some apples. 

## So let's get started!
A few important notes before we begin, most of the commands below are designed to print to screen so that it will be easy to visualize what is happening to our files as we move along, but you can also print all results to a file  by adding this sign `>` and a new output file name at the end of each command ` pattern {action} > newfile`. 

Commands will look like this: 
```
echo "I am a command, please type or copy-paste me into your terminal"
```

We will be working with exampe files in .bed format I downloaded and modified from the UCSC genome browser (https://genome.ucsc.edu/). I picked the .bed format because the fiels are organized in tab delimited column format which is exactly the type of data that awk excels at working with. For more on the .bed format see: https://genome.ucsc.edu/FAQ/FAQformat.html#format1. I also want to mention that the content and structure of this tutorial is largely based on the great chapter **Unix Data Tools** in *Buffalo V (2015). Bioinformatics Data Skills. O'Reilly Media*.


