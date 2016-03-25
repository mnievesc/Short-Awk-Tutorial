# Short-Awk-Tutorial
The tutorial below is designed to be completed in ~40-45 minutes by anyone with basic understanding of UNIX. To begin please download the three example files to your working directory: chr7.bed, chr11.bed and chroms.csv.  

## What is awk?
Awk is a data manipulation programming language that exists within UNIX, similar to grep or cut. It is useful for extracting text and performing quick data processing of tabular data. Awk works by processing data one record at a time. Each line in a text file is a record and each record is composed of fields. So if you imagine a table full of data, each row is a record and each column cell within that row is a field.

Awk commands follow a basic structure of `pattern {action}`. The user gives the program a regular expression to be evaluated: `pattern`. If the regular expression turns out to be true, then a command is executed `{action}`. If the user just provides the `{action}` with no pattern to be matched, then awk will run the command on all records. As a quick reminder, a regular expression is just a sequence of charaacters that define a pattern. 

## Important notes
A few important notes before we begin, most of the commands below are designed to print to screen so that it will be easy to visualize what is happening to our files as we move along, but you can also print all results to a file  by adding this sign `>` and a new output file name at the end of each command ` pattern {action} > newfile`.

Commands to be executed in the terminal will look like this: 
```
echo "I am a command, please type or copy-paste me into your terminal"
```

We will be working with example files in .bed format I downloaded and modified from the UCSC genome browser (https://genome.ucsc.edu/). I picked the .bed format because the data is organized in tab-delimited columnar format which is exactly the type of data that awk excels at working with. For more on the .bed format see: https://genome.ucsc.edu/FAQ/FAQformat.html#format1. I also want to mention that the content and structure of this tutorial is largely based on the great chapter **Unix Data Tools** in *Buffalo V (2015). Bioinformatics Data Skills. O'Reilly Media*.


## So let's get started!
To start, let's take a look at our first example file using cat:
```
cat chr7.bed 
```

This file is in .bed tabular format. There are columns for chromosome name, chromosome start position, chromosome end, position , score, strand, etc... So, how can we replicate the cat function using awk?
```
awk '{ print $0 }' chr7.bed
```
The command above prints all the contents of a file to the screen. `{print}` is the action, but by using `$0` we have omitted a pattern. Instead we are telling awk taht we want all fields for each record to be printed.  What if we just wanted to print the first field (column) in our file instead?
```
awk '{ print $1 }' chr7.bed
``` 
Now if we wanted to save the output of our awk commands to file instead of printing it to the screen we need to use `>` and provide the program with a new file name. We can view our new file using `cat`. Let us leave that file in our directory for now, but can you think of how we could erase it using the command line? 
```
awk '{ print $1 }' chr7.bed > newfile
cat newfile
``` 
We can also use awk to select more than one field in our data file.
```
awk '{ print $1 $2}' chr7.bed
```
Wait, what happened here? Awk assumes we are working with tab-delimited input data, but it does not assume our output is in this format. We have to tell awk how to separate the data and we can do that by specifying it in our command.
```
awk '{ print $1" "$2}' chr7.bed # space delimited
awk '{ print $1"\t"$2}' chr7.bed # tab delimited
``` 





