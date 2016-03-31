# Short-Awk-Tutorial
The tutorial below is designed to be completed in ~30 minutes by anyone with basic understanding of UNIX. To begin please download the three example files to your working directory: chr7.bed, chr11.bed and chroms.csv.  

## What is awk?
Awk is a data manipulation programming language that exists within UNIX, similar to grep or cut. It is useful for extracting text and quick processing of tabular data. Awk works by processing data one record at a time. Each line in a text file is a record and each record is composed of fields. So if you imagine a table full of data, each row is a record and each column within that row is a field.

Awk commands follow a basic structure of `pattern {action}`. The user gives the program a regular expression to be evaluated `pattern`. If the regular expression turns out to be true, then a command is executed `{action}`. If the user just provides the `{action}` with no pattern to be matched, then awk will run the command on all records. As a quick reminder, a regular expression is just a sequence of characters that define a pattern. 

## Important notes
A few important notes before we begin, most of the commands below are designed to print to screen so that it will be easy to visualize what is happening to our files as we move along, but you can also print all results to a file  by adding this sign `>` and a new output file name at the end of each command ` pattern {action} > newfile`.

Commands to be executed in the terminal will look like this: 
```
echo "I am a command, please type or copy-paste me into your terminal"
```
Comments will be noted by this sign `#`. You do not need to copy these into your terminal, they are just here to give additional information on what the commands are doing.

We will be working with example files in .bed format downloaded and modified from the UCSC genome browser (https://genome.ucsc.edu/). We are using the .bed format because the data istab-delimited, which is exactly the type of data that awk excels at working with. For more on the .bed format see: https://genome.ucsc.edu/FAQ/FAQformat.html#format1. I also want to mention that the content and structure of this tutorial is largely based on the great chapter **Unix Data Tools** in *Buffalo V (2015). Bioinformatics Data Skills. California: O'Reilly Media*. You can learn more about the book here: http://shop.oreilly.com/product/0636920030157.do. 


## So let's get started!
To start, let's take a look at our first example file using cat:
```
cat chr7.bed 
```

This file is in .bed tabular format. There are columns for chromosome name, chromosome start position, chromosome end, position , score, strand, etc... So, how can we replicate the cat function using awk?
```
awk '{ print $0 }' chr7.bed
```
The command above prints all the contents of a file to the screen. `{print}` is the action, but by using `$0` we have omitted a pattern. Instead we are telling awk that we want all fields for each record to be printed.  What if we just wanted to print the first field (column) in our file instead?
```
awk '{ print $1 }' chr7.bed
``` 
Now if we want to save the output of our awk commands to file instead of printing it to the screen we need to use `>` and provide the program with a new file name. We can view our new file using `cat`. Let us leave that file in our directory for now, but can you think of how we could erase it using the command line? 
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
awk '{ print $1" "$2}' chr7.bed    # space delimited
awk '{ print $1"\t"$2}' chr7.bed   # tab delimited
``` 
We can even print out non-contiguous fields!
```
awk '{ print $3" "$1}' chr7.bed    # space delimited
```
What if we wanted to combine the 1st field with the chromosome name, the 2nd  field with the position and field 6 which tells us if the variant is on the positive or negative strand in one tab-delimited file? How would you do this with what you have learned so far?
```
awk '{ print $1"\t"$2"\t"$6}' chr7.bed
```
That works! But if we had a much larger file structuring our command like this would involve a lot of typing. Awk has some built in variables that help modify the output so its not necessary to type in the tab-delimiter symbol `\t` every time. There are many different types of these built in variables. We will use a type called field separators. Specifically, we will use the output field separator `OFS` which specifies what the output should be delimited by. To use the field separator we need to insert the `-v` flag in our command. 
```
awk -v OFS="\t" '{ print $1,$2,$6}' chr7.bed      # notice the commas here
```
You can use the output field separator to specify any symbol. Like for example, if we wanted to separate our data fields with colons we would do this:
```
awk -v OFS=":" '{ print $1,$2,$6}' chr7.bed      # notice the commas here
```
This capability is extremely useful for bioinformatics because sometimes it is necessary to transform data into different formats. Awk can help you do this quite easily.

As we have learned so far, awk assumes all your input data is tab-delimited. If we want to work with something else, like say a .csv file (comma separated values), we have to tell awk that the input field separator will be different. For that we can use the built in variable `FS`.  Let's try it by importing our second example file: chroms.csv
```
awk -v FS="," '{ print $1,$2,$3}' chroms.csv
```
And of course you can combine input and output separator statements.
```
awk -v FS="," '{ print $1,$2,$3}' OFS=":" chroms.csv
``` 
There are many more flags you can use to modify input or output with awk. You can see them all if you type `man awk` into the command line. (Note: To exit the `man` page, type `:q`). 

Another helpful built in variable is `NR`. NR stands for range of lines, or current record and it allows you to select specific lines, so for example lets take a look at our third example file: chr11.bed
```
cat chr11.bed
```
This file has a header, now what if we want to exclude that header and just work with the data columns? We can use NR to do that. Note that in this case I do not need the -v flag
```
awk 'NR!=1 {print $0}' chr11.bed
```
Note here a few new things, mainly the use of this symbol: `!=`. This is a negation statement that means NOT TRUE. In this case we are using it to tell awk to print all fields except those in the first record (or line) which is NR=1. The above command is a good demonstration of how you can use awk with conditional statements. Another example of that is using NR to extract ranges of lines. 
```
awk 'NR >=3 && NR <=5' chr11.bed
```
Here we are telling awk to select any record (lines) equal or above record 3 (`>=`) AND (represented by `&&`) lower or equal to record 5. This is almost like an if condition. In fact, you can use if statements and loops with awk, but I will let you guys explore that on your own later.

Another thing you can do with awk is perform arithmetic operations. This could be useful for example if you are
trying to filter data. Say  that for our chr7.bed file we only want to select genes for analysis that are longer than 1200 base pairs (we are going to go back to chr7.bed file because there is no header in that file, although you can probably imagine how we could do all of the subsequent operations with the chr11.bed file by excluding the header using NR). Let's remind ourselves what this file looks like using cat:
```
cat chr7.bed
```
So as a reminder, the 2nd field in this file provides the chromosome start position of our gene of interest. The 3rd field provides the end position. So if we wanted to see what is the length of each of the genes in chromosome 7 we can use this command to print a list of base pair lengths to the screen:
```
awk '{print $3-$2}' chr7.bed
```
Ok, so there are a few genes that are longer than 1200 bp. Let's filter out the information for the other genes that we are not interested in by using a regular expression.
```
awk '$3-$2 > 1200' chr7.bed       # Note that here we do not need the print statement
```
You can also chain statements, like say for example we only wanted genes on the positive DNA strand (field 4 provides this information) and that are over 1100 base pairs long. 
```
awk '$4 ~/Pos*/ && $3 - $2 > 1100' chr7.bed
```
So here we have done few things, the first is we use a pattern `$4 ~/Pos*/`. This pattern is a regular expression, its matching field 4 to the statement `Pos` and then using `*` which is a wildcard. In other words its saying, in field 4 ($4), any time you see the pattern "Pos" (~/Pos), regardless of what follows (*), select that line. The pattern to be matched is in slashes. Then we have `&&` for AND. So the command is saying, after you match this pattern (i.e. find elements on the positive strand) do something else, which in this case is perform the arithmetic. 

We can even use awk to create new fields in a file based on previously inputed information. For example if we wanted to make a new file that only contains the chromosome name, positions and then the length of each gene we could do:
```
awk -v OFS="\t" '{print $1,$2,$3,$3-$2}' chr7.bed
```
Can you explain what we did here? 

This is just skimming the surface of all that awk can do, you can also use conditional statements, calculate more
complex things like means, or apply user created variables in awk. But I think that with this short tutorial you will
have enough knowledge to be able to search for those commands and apply them.

##References and sources for more information
#####Main source:
Buffalo V. 2015. Bioinformatics Data Skills. California: O'Reilly Media.
#####Secondary sources:
1. http://www.theunixschool.com/2011/05/awk-read-file-and-split-contents.html
2. https://www.gnu.org/software/gawk/manual/html_node/Output-Separators.html
3. https://emb.carnegiescience.edu/sites/default/files/140602-sedawkbash.key_.pdf
4. https://genome.ucsc.edu/FAQ/FAQformat.html
5. https://github.com/stephenturner/oneliners
6. http://intro-prog-bioinfo-2014.wikispaces.com/Session+1.1
7. https://www.codecademy.com/learn/learn-the-command-line
