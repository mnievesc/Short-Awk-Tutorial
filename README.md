# Short-Awk-Tutorial
The tutorial below is designed to be completed in ~40-45 minutes by anyone with basic understanding of UNIX. To begin please download the three example files to your working directory: chr7.bed, chr11.bed and chroms.csv.  

## What is awk?
Awk is a data manipulation programming language that exists within UNIX, like grep or cut. It is useful for extracting text and performing quick data processing of tabular data. Awk works by processing data a record at a time, each line in a text file is a record and each record is composed of fields. So if you imagine a table full of data, each row is a record and each columnar cell within that row is a field.

Awk commands follow a basic structure of pattern {action} where the user gives the program a regular expression to be evaluated and if it turns out to be true, then a command is executed. A regular expression is just a sequence of characters that define a pattern, so for example if I said "find apples" that would be a regular expression that would tell my program to go find me some apples. 
