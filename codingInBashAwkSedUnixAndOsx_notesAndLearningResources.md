# Bash, Awk, Sed, Unix and OSX — Notes and Learning Resources



## Resources

Beginners bash tutorial: [Bash Beginners Guide](http://tldp.org/LDP/Bash-Beginners-Guide/html/Bash-Beginners-Guide.html)  
especially:  
- Chapter 3 — on variable expansion and substitution  
- Chapter 4 — on regex  
- Chapter 5.1 — Quoting variables with spaces in them (#hours of fun)

**Reference card** — appendices have the most interesting bits:  
<http://www.tldp.org/LDP/abs/html/refcards.html>

**Advanced bash tutorial**: <http://tldp.org/LDP/abs/html/>

**sed and awk and unix**: <http://www.grymoire.com/Unix/>

**Index of bash commands**: <http://ss64.com/bash/>

**bash manual**: <http://www.gnu.org/software/bash/manual/bashref.html>

**comparison operators**: <http://www.freeos.com/guides/lsst/ch03sec02.html>

**Great command line tutorial for absolute beginners**:  
<http://lifehacker.com/5633909/who-needs-a-mouse-learn-to-use-the-command-line-for-almost-anything>

---

## nohup and redirecting output

```bash
nohup program_name 1>log 2>&1 &
```
- `1>log` means send stdout to `log`  
- `2>&1` means send stderr to the same output as stdout  
- `&` means return my prompt

Another option to `nohup` is **screen** (search this document) or start a process and place it into ‘nohup’-like state using Ctrl‑Z, `bg`, `disown` — see below.

### Placing a running process into nohup
See <http://www.kossboss.com/linux---move-running-to-process-nohup>

```
0. Run some SOMECOMMAND
1. ctrl+z to stop (pause) the program and get back to the shell
2. bg to run it in the background
3. disown -h so that the process isn't killed when the terminal closes
4. Type exit to get out of the shell because now you're good to go as the operation will run in the background in it own process so its not tied to a shell
```

---

## setup workenvironment

```bash
echo 'alias ll="ls -la --color=auto"' >> ~/.bash_profile
```

---

## bash debugger

<http://bashdb.sourceforge.net/>

```bash
sudo yum install bashdb
```

<http://bashdb.sourceforge.net/bashdb.html#Debugger-Command-Reference>

---

## IDE for Bash? (Eclipse plugin)

<http://sourceforge.net/projects/shelled/>

---

## an example script

<http://wiki.centos.org/HowTos/Rotational_backup_with_remote_backup_options>

---

## how to parse command line parameters

### Preferred Method: Using straight bash without getopt[s]

> I originally answered the question as the OP asked. This Q/A is getting a lot of attention, so I should also offer the non-magic way to do this. I'm going to expand upon guneysus's answer to fix the nasty sed and include Tobias Kienzler's suggestion.  
> Two of the most common ways to pass key value pair arguments are:

**Straight Bash Space Separated**  
Usage:
```bash
./myscript.sh -e conf -s /etc -l /usr/lib /etc/hosts
```

```bash
#!/bin/bash
# Use > 1 to consume two arguments per pass in the loop (e.g. each
# argument has a corresponding value to go with it).
# Use > 0 to consume one or more arguments per pass in the loop (e.g.
# some arguments don't have a corresponding value to go with it such
# as in the --default example).
# note: if this is set to > 0 the /etc/hosts part is not recognized ( may be a bug )
while [[ $# > 1 ]]
do
key="$1"

case $key in
    -e|--extension)
    EXTENSION="$2"
    shift # past argument
    ;;
    -s|--searchpath)
    SEARCHPATH="$2"
    shift # past argument
    ;;
    -l|--lib)
    LIBPATH="$2"
    shift # past argument
    ;;
    --default)
    DEFAULT=YES
    ;;
    *)
            # unknown option
    ;;
esac
shift # past argument or value
done

echo FILE EXTENSION  = "${EXTENSION}"
echo SEARCH PATH     = "${SEARCHPATH}"
echo LIBRARY PATH    = "${LIBPATH}"
echo "Number files in SEARCH PATH with EXTENSION:" $(ls -1 "${SEARCHPATH}"/*."${EXTENSION}" | wc -l)
if [[ -n $1 ]]; then
    echo "Last line of file specified as non-opt/last argument:"
    tail -1 $1
fi
```

#### checking command line arguments
```bash
#check that the lastname exists and does not start with -
if [[ ! ${LASTNAME} || $(expr match ${LASTNAME} "-") == 1 ]]; then
	cat 1>&2 <<EOF
You have not supplied the last name of the client.
Try typing
	handbackData -h
to get help.
EOF
	exit 1
fi
```

---

## variable dereferencing (`$VAR` vs `${VAR}` vs `"${VAR}"`)

When should you use these different forms?
```bash
VAR=$VAR1
VAR=${VAR1}
VAR="$VAR1"
VAR="${VAR1}"
```
See this discussion “$VAR vs ${VAR} and to quote or not to quote”:  
<http://unix.stackexchange.com/questions/4899/var-vs-var-and-to-quote-or-not-to-quote>

Why use `${VAR}` format when dereferencing variables:  
<http://stackoverflow.com/questions/8748831/when-do-we-need-curly-braces-in-variables-using-bash>

Why you might sometimes want to use the form `"${VAR}"`:  
<http://tldp.org/LDP/Bash-Beginners-Guide/html/Bash-Beginners-Guide.html> (Chapter 5.1 Quoting variables with spaces in them)

---

## if statement

Hours of fun.

- **Numeric comparison operators** are `-eq`, `-ne`, `-lt`, `-le`, `-gt` or `-ge`. See <http://www.tldp.org/LDP/abs/html/comparison-ops.html>  
- **String comparison operators** are `==`, `!=`, `<`, `>`  
- **File test operators** are `-e`, `-d`, and many more: <http://www.tldp.org/LDP/Bash-Beginners-Guide/html/sect_07_01.html>

> Note spaces inside square brackets.  
> Quote arguments as part of good form in case the evaluated form has a space in it.

```bash
a="qwe rty"
if [ "$a" \< "thisString" ]; then
  # ...
  :
fi
```

```bash
if [ $# -eq 0 ] # note that $# means number of input args
then
  echo "$0 : You must give/supply one integers"
  exit 1
fi
```

```bash
if test $1 -gt 0
then
  echo "$1 number is positive"
else
  echo "$1 number is negative"
fi
```

More hours of fun: to evaluate boolean test constructs see  
<http://tldp.org/LDP/abs/html/testconstructs.html>

```bash
# this does not do what you think
if [ 0 ] || [ 0 ]; then echo TRUE; fi
# [ ] tests for non-null

# instead, you want this
if (( 0 || 0 )); then echo TRUE; fi

# especially important when evaluating the outcome of string comparison tests
USER=bob
# test for presence of variable
if [ $USER ]; then echo NON_NULL; fi
if [ $(expr match $USER "c") ]; then echo STARTS_WITH_C; fi #### WRONG ####
if (( $(expr match $USER "c") )); then echo STARTS_WITH_C; fi ## CORRECT ##

# testing a boolean
if (( $(expr length $USER) == 0 || $(expr match $USER "-")  )); then echo "username is present and does not start with -"; fi
```

Using boolean test conditions inside `[[` will work:
```bash
USER=bob
if [[ $USER && $(expr length $USER) == 3 ]]; then echo BLAP; fi
if [[ $USER && $(expr match $USER "-") != 3 ]]; then echo BLAP; fi 
```

**What is the difference between `[` and `[[`?**

(Discussion marker — left as-is per source material.)

---

## while loop to process lines in a file

```bash
while read p; do
  echo $p
done <peptides.txt
```

### while loop to rename files given a `rename.txt` file

`rename.txt` is a tab‑delimited text file like:
```
oldname1	newname1
oldname2	newname2
```

```bash
while read -r a b; do 
   # Remove echo if satisfied by the output 
   echo mv -- "$a" "$b" 
done <rename.txt
```

---

## for loops

### for loop based on a sequence of strings
```bash
GATK_FTP=ftp://gsapubftp-anonymous@ftp.broadinstitute.org/bundle/2.8/hg19
for f in  hapmap_3.3.hg19.sites.vcf.gz        1000G_omni2.5.hg19.sites.vcf.gz        1000G_phase1.indels.hg19.sites.vcf.gz        Mills_and_1000G_gold_standard.indels.hg19.vcf.gz ; do        echo $f;        curl --remote-name ${GATK_FTP}/${f}; done
```

### for loop based on evaluation of a command
```bash
#!/bin/bash
for i in $( ls ); do
    echo item: $i
done
```

### for loop based on a generated sequence
```bash
#!/bin/bash
for i in `seq 1 10`; do
   echo $i
done
```

### for loop — example 1
```bash
if [ $# -eq 0 ]
then
echo "Error - Number missing form command line argument"
echo "Syntax : $0 number"
echo "Use to print multiplication table for given number"
exit 1
fi
n=$1
for i in 1 2 3 4 5 6 7 8 9 10
do
echo "$n * $i = `expr $i \* $n`"
done
```

---

## arrays

```bash
# array assignment from a string
STRING="one, two, three"

# how to set the internal field separator to comma-space or to tab
# IFS by default will separate on tab or spaces
IFS=', '  # to split on either comma or space
IFS=$'\t' # to split strings on tabs
read -r -a array <<< "$STRING"  #### DONT FORGET QUOTES around $STRING

# To access an individual element:
echo "${array[0]}"

# To iterate over the elements:
for element in "${array[@]}"
do
    echo "$element"
done

# To get both the index and the value:
for index in "${!array[@]}"
do
    echo "$index ${array[index]}"
done

# The last example is useful because Bash arrays are sparse. In other words,
# you can delete an element or add an element and then the indices are not contiguous.
unset "array[1]"
array[42]=Earth

# To get the number of elements in an array:
echo "${#array[@]}"

# As mentioned above, arrays can be sparse so you shouldn't use the length to
# get the last element. Here's how you can in Bash 4.2 and later:
echo "${array[-1]}"
```

---

## for loop — more examples

```bash
PARSESOURCES=

for SOURCE in $SOURCES ; do
    if ! isin "$SOURCE" $EXCLUDEDSOURCES ; then
        PARSESOURCES="${PARSESOURCES}${PARSESOURCES:+ }$SOURCE"
    fi
done
```

The term `${PARSESOURCES:+ }` introduces a space between each term in the list but only when `PARSESOURCES` is defined — if this term in the code were replaced with a space, the list would begin with a space — see <http://www.gnu.org/software/bash/manual/bashref.html#Shell-Parameter-Expansion>

Another example of parameter expansion:
```bash
URL=file://abc/def/file
# the following "${URL#file://}" line means "strip file:// from the beginning of URL (if it exists)"
if [ "${URL#file://}" != "$URL" ]; then # if the URL begins with file://
  :
fi
```

Read more about parameter substitution: <http://tldp.org/LDP/abs/html/parameter-substitution.html#PSUB2>  
Could also use substring matching: <http://tldp.org/LDP/abs/html/moreadv.html#EX45>

### for loop example 3 — rename files
```bash
for FILENAME in *.mitab.06-06-2013.txt ; do NEWFILENAME=`echo $FILENAME | sed 's/06-06-2013/06062013/'` ; mv $FILENAME $NEWFILENAME ; done
```

```bash
# another example
for FILENAME in *.txt ; do 
NEWFILENAME=`echo $FILENAME | cut -d "_" -f 3`; 
NEWFILENAME+=".txt";
echo $NEWFILENAME ; 
done
```

### then zip them up
```bash
for file in `ls -1 *.mitab.*`; do zip "$file".zip "$file"; done;
```

### for a directory full of zipped files, unzip and rename
```bash
mkdir short; mkdir unzipped; 
for FILENAME in *.zip; do 
  unzip $FILENAME -d unzipped; 
done

cd unzipped
for FILENAME in *.txt; do 
  NEWFILENAME=`echo $FILENAME | cut -f 3 -d "_"`; 
  cp $FILENAME ../short/$NEWFILENAME.txt; 
done
```

### for loop example 4
Find all files with a given extension and return a non‑redundant list of all the directories in which they occur:

```bash
for file in `find . *.py -type f`; do echo `dirname $file` >> fileofresults; done;
# or
for file in `locate *.py`; do echo `dirname $file` >> fileofresults; done;

# then do
cat fileofresults | sort -u
```

---

## assigning the output of a command to a variable

Command substitution — <http://www.gnu.org/software/bash/manual/bashref.html#Command-Substitution>

```bash
ALL_SCORES=`cut -f 41 All.mitab.04-07-2015.txt | sort | uniq`
# or
ALL_SCORES=$(cut -f 41 All.mitab.04-07-2015.txt | sort | uniq)
# or — quoting matters only for multi-line commands
ALL_SCORES="$(cut -f 41 All.mitab.04-07-2015.txt | sort | uniq)"
```

### assigning a tab character to a variable and passing it as a parameter
```bash
INPUT_DELIM=$'\t'
cut -d "${INPUT_DELIM}" -f 1 some_tab_delimited_file
```

---

## how to find lines with some value in a specific column

```bash
# use awk to find mitab lines with P in the 41st column
head All.mitab… > tmp
awk 'BEGIN {FS="\t"}{if($41 == "P") print $1}' < tmp
# or pipe it together
head All.mitab… | awk 'BEGIN {FS="\t"}{if($41 == "P") print $1}'
# using a regular expression in awk
awk 'BEGIN{FS="\t"}{if($14 ~ /bind:304633/) print $0}' < All.mitab.04-07-2015.txt
```

```bash
# use grep to find 866 in the second column of a space-delimited line
# -P are for Perl regex
grep -P '^\S+\s+this123\b'
```

```bash
#print the number line number and number of fields per line
awk 'BEGIN{ FS="\t"; }{ print NR, NF; }' < combinedMetadata_2023.02.28.txt | less
```

---

## printing a specific line from a file using mapfile (built-in bash command)

```bash
mapfile -s 42 -n 3 < All.mitab.04-07-2015.txt
printf '%s' "${MAPFILE[2]}"
# or
echo "${MAPFILE[2]}"
```

---

## collating (joining/merging) columns/results from multiple files on a common key

### join

Given the following files:

`tmp0`
```
0
1
2
3
4
5
6
7
```

`tmp1`
```
0	cat
1	has
2	four
3	legs
4	ok
5	tmp1
7	end
```

`tmp2`
```
0	a
1	flower
2	has
3	none
4	ok
6	tmp2
7	end
```

```bash
join -1 1 -2 1 -t $'\t' -a 1 -a 2 -o 0,1.2,2.2 -e "NA" <(sort tmp1) <(sort tmp2)
```

```bash
join -1 1 \ #key for file 1 is in column 1
     -2 1 \ #key for file 2 is in column 1
     -t $'\t' \ # field separator for input/output is a tab
     -a 1     \ # include all lines from file 1 (i.e. left join)
     -a 2     \ # include all lines from file 2 (i.e. right join)
     -o 0,1.2,2.2 \ # format output like this
     -e "NA"      \ # use this string for missing values
     <(sort tmp0) \ #sort file 1 input
     <(sort tmp2)   #sort file 2 before input <() = bash process substitution
```

**WARNING — DO NOT USE `join` To ATTEMPT OUTER JOINS ON MULTIPLE FILES**  
Write to bug-coreutils@gnu.org. The `-e` option is only available if you specify the output option so you can't use it to join columns from multiple files (like you might think you can — hours of fun) — the script below overcomes this shortcoming by first re‑writing all of the output files so that they contain all of the keys and NA in cases where the starting file does not contain an entry for that key.

### implementing a full‑outer join using `join` in bash

The script below does a full outer join on multiple output files that have a common set of keys but where many of the result files will be missing results for some keys.

The result files are from bismark methylation extractor and the files are named `sample_name.bismark.cov.gz`.

The script follows 4 main steps:  
first — make a list of unique keys found in any of the files  
second — rewrite each file as just two columns (a key and a data result)  
third — rewrite files with all keys (and add an NA to those samples that are missing data for a given key)  
fourth — full‑outer join to create a matrix on all keys on all samples

```bash
#!/bin/bash

# collate.sh
#
# collect columns from multiple output files into a key by sample matrix
# based on join
# author: Ian Donaldson - i.donaldson@qmul.ac.uk
#

# usage
# review the params below then
# ./collate.sh
#
# set these parameters
PROJECT_DIR=/data/WHRI-GenomeCentre/idonaldson/bisulfite_dev
INPUT_DIR=${PROJECT_DIR}/results_bismark_consol
OUTPUT_DIR=${PROJECT_DIR}/results_collate
#multiple columns in the input file may be specified to create a compound key column
KEY_COL="1,2"
#only one column in the input file may be specified as the input column
DATA_COL=4
#samples without a data entry for some key will be assigned an NA value (character or string)
NA="NA"
#an extension string can be removed from the name of uncompressed input files to get the sample name
#so if the input files are named like "sample_name.bismark.cov.gz", you could specify ".bismark.cov"
EXTENSION=".bismark.cov"
#these working directories can be left as is
TMP_DIR=${OUTPUT_DIR}/tmp
KEY_FILE=${TMP_DIR}/keys

###
# no changes required beyond this point
###

#all output files will be written to a single sub-directory
if [ ! -e ${OUTPUT_DIR} ]; then mkdir -p ${OUTPUT_DIR}; fi
if [ ! -e ${TMP_DIR} ]; then mkdir -p ${TMP_DIR}; fi

#cp data files to be consolidated to a tmp dir
find ${INPUT_DIR} -name *.bismark.cov.gz | xargs cp -t ${TMP_DIR}/. 
gunzip ${TMP_DIR}/*

# step 1
#rewrite all input data files in two columns (key and data) format
#this allows for creation of compound keys by pasting together multiple columns specified by KEY_COL
for THIS_FILE in $(ls ${TMP_DIR}); do
     cut --output-delimiter '|' -f ${KEY_COL}  ${TMP_DIR}/${THIS_FILE} > ${TMP_DIR}/key.${THIS_FILE} 
     cut                        -f ${DATA_COL} ${TMP_DIR}/${THIS_FILE} > ${TMP_DIR}/data.${THIS_FILE}
     paste ${TMP_DIR}/key.${THIS_FILE} ${TMP_DIR}/data.${THIS_FILE} > ${TMP_DIR}/key_data.${THIS_FILE}
     rm ${TMP_DIR}/data.${THIS_FILE}
     rm ${TMP_DIR}/key.${THIS_FILE}
     rm ${TMP_DIR}/${THIS_FILE}
done

# step 2
#collect a non-redundant list of keys from all input files
#keep the file sorted and unique in place to keep mem requirements low
echo "!key.sample" > ${KEY_FILE}
for THIS_FILE in $(ls ${TMP_DIR}/key_data.*); do
     cut -f 1 ${THIS_FILE} >> ${KEY_FILE}
     sort -u -o ${KEY_FILE} ${KEY_FILE}
done

# step 3
#a. add an entry corresponding to the header (see key.sample in KEY_FILE)
#b. rewrite the files again so that all keys are present in each with NA's for missing data
for THIS_FILE in $(ls  ${TMP_DIR}/key_data.*); do
        # a
        FILE_NAME=$(basename ${THIS_FILE} ${EXTENSION})
        COL_NAME=${FILE_NAME:9}
        echo -e "!key.sample\t${COL_NAME}" >> $THIS_FILE
        # b
	join -1 1 -2 1 -t $'\t' -a 1 -a 2 -o 0,2.2 -e "${NA}" ${KEY_FILE} <(sort ${THIS_FILE}) > "${TMP_DIR}/complete_${FILE_NAME}"
        #wc -l "${TMP_DIR}/complete_${FILE_NAME}"
done

# step 4
COLLATED=${TMP_DIR}/collated
cp ${KEY_FILE} ${COLLATED}
TMP_COLLATED=${TMP_DIR}/tmp_collated
for THIS_FILE in $(ls ${TMP_DIR}/complete_*); do
	join -1 1 -2 1 -t $'\t' ${COLLATED} <(sort ${THIS_FILE}) > ${TMP_COLLATED}
      mv ${TMP_COLLATED}  ${COLLATED}
done

exit
```

Other solutions for collating result columns from multiple files: `awk`, `perl`, `python`, Python `pandas/numpy`, `R`.

---

## Math (arithmetic in bash)

<http://www.bashguru.com/2010/12/math-in-shell-scripts.html>

Use the `$((EXPR))` construct for integer math:
```bash
num=$((num1 + num2))
num=$(($num1 + $num2))       # also works
num=$((num1 + 2 + 3))        # ...
num=$[num1+num2]             # old, deprecated arithmetic expression syntax
```

Bash does not support floating point and requires an external tool:
```bash
num=$(awk "BEGIN {print $num1+$num2; exit}")
num=$(python -c "print $num1+$num2")
num=$(perl -e "print $num1+$num2")
num=$(echo $num1 + $num2 | bc)   # whitespace for echo is important
```

---

## Printing

<http://wiki.bash-hackers.org/commands/builtin/printf>

### here documents
```bash
cat 1>&2 <<EOF
You have not supplied the email of the client.
Try typing
	handbackData -h
to get help.
EOF
```

---

## grep and regex

<http://www.cyberciti.biz/faq/grep-regular-expressions/>

To select all but lines that contain some regex and write to a new file where the regex has a tab in it:

```bash
grep -Pv 'abc\tefg' some_file > some_new_file
# or
grep -v $'abc\tefg' some_file > some_new_file
```

### grep with color in less
```bash
grep --color=always someString inthisFile.txt | less -R
```

### can i grep on a specific column?

Given
```
0,170,3,1,27,170,3,1,27,1490
0,304,2,2,40,304,2,2,40,1473
0,191,1,0,0,191,1,0,0,0
…
```

### how to combine (AND/OR) grep expressions

Given a tmp file with:
```
a
b
c
ab
```

You can pipe greps together to simulate AND:
```bash
grep a tmp | grep b
# -> ab
```

There is no grep AND operator but you can use an expression like:
```bash
grep -E "^[^a]*b" tmp
# -> b
```

You can use extended grep with a pipe to simulate OR:
```bash
grep -E "a|b" tmp
# a
# b
# ab
```
or the equivalent in normal regex:
```bash
grep "a\|b" tmp
```
or specify two expressions:
```bash
grep -e a -e b tmp
```

---

## string operations and regex

```bash
a=1234zipper43231
echo "The string being operated upon is \"$a\"."

# length: length of string
b=`expr length $a`
echo "Length of \"$a\" is $b."

# index: position of first character in substring
#        that matches a character in string
echo "Numerical position of first \"2\" in \"$a\" is \"$b\"."

# substr: extract substring, starting position & length specified
b=`expr substr $a 2 6`
echo "Substring of \"$a\", starting at position 2,\
and 6 chars long is \"$b\"."

# find the index of a substring
b=`expr index "bob" "o"` 
```

String operations using parameter substitution: <http://tldp.org/LDP/abs/html/string-manipulation.html>  
```
${var#*SubStr}  # will drop begin of string upto first occur of `SubStr`
${var##*SubStr} # will drop begin of string upto last occur of `SubStr`
${var%SubStr*}  # will drop part of string from last occur of `SubStr` to the end
${var%%SubStr*} # will drop part of string from first occur of `SubStr` to the end
```

```bash
# replace all returns in a file with tabs
tr '\n' '\t' < in_file > out_file
```

Replace all returns in a file with tabs using the vim editor:
```
:%s/\n/\t/g
```

### listing all the column headers in a tsv or csv
```bash
head -1 adae.csv | awk -v FS="," '{for (i=1;i<=NF;i++) printf "Column "i": " $i"\n"}'
# This will take the first line and split it by ‘comma’ and then we loop over it and print the column name with numbers. 
```

### transposing a tab-delimited file using awk

First replace spaces with underscores:
```bash
tr ' ' '_' < original.file > no.space.file
```

`transpose.awk`:
```awk
{ 
    for (i=1; i<=NF; i++)  {
        a[NR,i] = $i
    }
}
NF>p { p = NF }
END {    
    for(j=1; j<=p; j++) {
        str=a[1,j]
        for(i=2; i<=NR; i++){
            str=str" "a[i,j];
        }
        print str
    }
}
```

Then:
```bash
awk -f transpose.awk no.space.file > outfile
```

Another version of the transpose script:
```bash
#! /bin/sh
# Transpose a matrix: assumes all lines have same number
# of fields

exec awk '
NR == 1 {
        n = NF
        for (i = 1; i <= NF; i++)
                row[i] = $i
        next
}
{
        if (NF > n)
                n = NF
        for (i = 1; i <= NF; i++)
                row[i] = row[i] " " $i
}
END {
        for (i = 1; i <= n; i++)
                print row[i]
}' ${1+"$@"}
```

### transposing a tab-delimited file using perl
```perl
use Text::CSV;
use Array::Transpose; 
my $csv = Text::CSV->new ({ binary => 0, eol => $/,sep_char=>',' });
open my $in, "<", $ARGV[0] or die $!;
while (my $row = $csv->getline ($in)) { push @data, $row; }
close $in;                                                   
@data= transpose \@data;                    # Main transposing operation 
open my $out, ">", "out.txt" or die $!;
for (@data) {$csv->print ($out, $_)}          
close $out;    
```

Check the file using

- view tabs/returns/white-space in less:
```bash
cat -vet filename | less -S
# or
cat -A
```

- in vim:
```
:set hlsearch
:/\s      # spaces and tabs
:/\n      # returns
```

### match
```bash
#  The default behavior of the 'match' operations is to
#  search for the specified match at the BEGINNING of the string.

b=`expr match "$a" '[0-9]*'`               #  Numerical count.
echo Number of digits at the beginning of \"$a\" is $b.
b=`expr match "$a" '\([0-9]*\)'`           #  Note that escaped parentheses trigger substring match.
echo "The digits at the beginning of \"$a\" are \"$b\"."
```

### GREP a particular data in a certain column
How can I GREP a particular data in a certain column. For example, I like to grep the data for column 6 with the value of 52. I should have:
```
0	52	3	3	62	52	3	3	62	1411
0	52	2	2	14	52	2	2	14	1501
0	52	3	4	14	52	3	4	14	1473
0	52	1	3	48	52	1	3	48	1431
```

Code (awk):
```bash
awk -F, '{ if ($6 == 52) print $0 }' input.txt
```

The grep solution looks ugly.
```bash
grep "^[^,]*,[^,]*,[^,]*,[^,]*,[^,]*,52," input.txt
```

### find all of the protein-protein interactions in a mitab file
```bash
awk 'BEGIN {FS="\t";}{if ($21 == "psi-mi:\"MI:0326\"(protein)"  && $22 == "psi-mi:\"MI:0326\"(protein)") print $0;}' < matrixdb_CORE.tab
```

---

## finding the next most likely word (SwiftKey project)

Coursera capstone data science SwiftKey project

```bash
# Very early observations on the Bills game: Offense still struggling but the
grep -oiP 'but the \w+' all.txt | tr '[:upper:]' '[:lower:]' | sort | uniq -c | grep "^\s*[0-9][0-9][0-9]"
```

Using context:
```bash
grep -i 'offense' all.txt | grep -oiP 'but the \w+'| tr '[:upper:]' '[:lower:]' | sort | uniq -c | grep "^\s*[0-9]"
```

So what is the correct answer?
```bash
grep -oiP 'but the (crowd|referees|defense|players)' all.txt | tr '[:upper:]' '[:lower:]' | sort | uniq -c | grep "^\s*[0-9][0-9]"
```

Look ahead and look behind:  
<http://unix.stackexchange.com/questions/13466/can-grep-output-only-specified-groupings-that-match>

### grep — identifying characters by unicode codes — different types of apostrophes (R example)
```r
> grep("\u2019", crps[[1]]$content[2], value=TRUE)
[1] "“I don’t know. Maybe they’re getting too much sun. I think I’m going to cut them way back.” I replied."

> gsub("\u2019", "\'", crps[[1]]$content[2])
[1] "“I don't know. Maybe they're getting too much sun. I think I'm going to cut them way back.” I replied."
```
More about unicode: <http://www.fileformat.info/info/unicode/index.htm>

---

## sed

A simple sed script to substitute text in a script

```bash
# will replace first occurrence on each line of blap with blip
sed 's/blap/blip/' infile
# will replace all occurrences on each line of blap with blip
sed 's/blap/blip/g' infile
```

Remember to use double quotes to pass a variable to a sed script:
```bash
BLAP="blap"
sed "s/$BLAP/blip/" infile
```

```bash
sed s/InnateDB:/innatedb:IDB-/ <file.1.innatedb > file.3.innatedb
```

In‑place replacement (macOS):
```bash
sed -i '.bak' s/foo/bar/g fileIn
```
Replacing the `'.bak'` with an empty `''` for the `-i` parameter means that no backup of the original will be made.

Here's a useful script to do inplace substitutions:
```bash
#!/bin/sh
# replace.sh
#
# find and replace by a given list of files
# usage
# replace.sh foo bar file

find_this="$1"
replace_with="$2"
this_file="$3"

sed -i '' "s/$find_this/$replace_with/" "$this_file" 
```

```bash
# convert the fasta file to one line per sequence and remove fasta headers
cat tmp | sed -r 's/>.*$/9/' | tr -d '\n' | tr '9' '\n' > tmp2
awk '{print length}' > sizes
```

### write a sed script to replace names in the gtf file with more readable gene names

First obtain gtf file for susScr3 (UCSC Genome Table Browser), saved as `susScr3-ensembl.gtf` and `susScr3-ucsc2ensembl-names.txt` in:

```
/data/WHRI-GenomeCentre/data/ref/susScr3/GTF
```

Then make a table of name conversions:
```bash
cut -f 8,9 susScr3-ucsc2ensembl-names.txt | sort -u | head -n -2 > ensembl2name
```
Where the last `head -n -2` removes the original table header and the row with `na    na`  
This table looks like:
```
ENSSSCT00000000003.2	GTSE1
ENSSSCT00000000004.2	TTC38
```

```bash
cd /data/WHRI-GenomeCentre/data/ref/susScr3/GTF

cp susScr3-ensembl.gtf susScr3-ensembl.gtf.bk
cp susScr3-ensembl.gtf tmp
wc -l tmp                    #510144 tmp
wc -l ensembl2name         #21394 ensembl2name

while read THIS_LINE; do
    START=$(date +%s.%N);

    read -d "\t" -a THIS_ARRAY <<< ${THIS_LINE};
    ENSM_NAME=${THIS_ARRAY[0]};
    GENE_NAME=${THIS_ARRAY[1]};
    sed "s/${ENSM_NAME}/${GENE_NAME}/" < tmp > tmp2;
    mv tmp2 tmp

    #END=$(date +%s.%N); DIFF=$(echo "$END - $START" | bc);echo $DIFF;
done < ensembl2name.tmp
```

More on sed:  
<http://www.grymoire.com/Unix/Sed.html>  
<http://www.grymoire.com/Unix/Quote.html>  
<http://www.grymoire.com/Unix/Regular.html>

---

## awk

<http://www.grymoire.com/Unix/Awk.html>

### structure of an awk script
```awk
BEGIN{
#do something once before processing file;
}
{
	#do something once for each record (line) in a file;
}
END {
	#do something once after processing a file;
}
```

### a simple awk script
```awk
# an awk script can be a text file like this

BEGIN{
FS=":";
print "Name\tUserID\tGroupID\tHomeDirectory";
}
{
	print $1"\t"$3"\t"$4"\t"$6;
}
END {
	print NR,"Records Processed";
}
```

Built in awk variables:  
- `FS` = input field separator (default is space)  
- `RS` = record separator (default is `\n`)  
- `NR` = number of records (number of lines when default RS is used "\n")  
- `NF` = number of fields  
- `OFS` = output field separator  
- `ORS` = output record separator

### a very simple awk script written in-line between single quotes
```bash
ls -l | awk '
BEGIN { print "File\tOwner";
FS="\t"; # the default field separator is white-space
 } 
{ print $8, "\t", $3}	
END { print " - DONE -" } 
'
```

Other ways of running this are

1) specify an input file (input.1) and an output file (output.1)
```bash
ls -l > input.1

awk '
BEGIN { print "File\tOwner" } 
{ print $8, "\t", $3}	
END { print " - DONE -" } 
' < input.1 > output.1
```

2) add a line to beginning of above
```
#!/bin/sh
```
then save to a file and run by typing:
```bash
awk -f filename
```

3) make the file itself executable by changing the first line to
```bash
#!/bin/awk -f
```

### retrieve the column number of column with header "Strand"

```bash
head -n 1 $FIRST_FILE | tr -s '\t ' '\n' | nl -nln |  grep "Strand" | cut -f1
# assign to a variable
THIS_COLUMN=$(head -n 1 $FIRST_FILE | tr -s '\t ' '\n' | nl -nln |  grep "Strand" | cut -f1)
```

awk alternative:
```awk
awk -F '\t' -v col='Strand' 'NR==1{for (i=1; i<=NF; i++) if ($i==col) {print i;exit}}' file
```

### find the length of the longest line in a file using wc or awk
```bash
wc -L filename
# or
awk ' { if ( length > x ) { x = length } }END{ print x }' filename
```

---

## sort

Sort on a column:

```
bob     123
alice     1
jeremy  111
```

```bash
sort -k2           # to sort on the second column asciibetically
sort -k2 -n        # to sort numerically
```

<http://ss64.com/bash/>

---

## viewing a tab delimited file as line delimited

```bash
head <file-name> | awk -v OFS='\n' '{$1=$1;print}'
```

For mitab files:
```bash
awk 'BEGIN{FS="\t";OFS="\n";ORS="\n\n*********************\n\n";}{$1=$1;print}'
```

<http://unix.stackexchange.com/questions/28484/how-can-i-convert-tab-delimited-data-to-comma-delimited-data>

---

## wget

(Section marker — link examples included later.)

---

## cron jobs to backup data

```bash
man crontab

crontab -l  # to list current cron jobs
crontab -e  # to edit the crontab file
```

```bash
0 22 * * * cp -r /home/ian /storage2/backups/.
```

This line means at  
- 0 minutes and  
- 22 hours  
- `*` any day‑of‑month (or 1‑31),  
- `*` any month (or 1‑12), and  
- `*` any day‑of‑week or 1=Monday ‑ 7=Sunday,  
then run the command: `cp -r /home/ian /storage2/backups/.`

Apparently any output of a command will be sent to the users registered email.

```bash
0 22 * * * cp -r /home/ian /storage2/backups/.
21 20 * * * /home/ian/bkscript/bkscript
```

---

## email

### setting up email
```bash
sudo vi /etc/aliases
# Scroll down to the person who gets roots email
# root:       marc
# Uncomment the line and change the value to your choice
root     example@yourdomain.com
# You can also send it to existing users as below
root:      username1, username2

newaliases 

# Now send a test Email to check it works properly.
echo "Test Email" | mail -s "This is a test email." externalemail@domain.com
```

---

## timing scripts

1. use the `time` function for single commands like `ls`
```bash
mytime="$(time ( ls ) 2>&1 1>/dev/null )"
echo "$mytime"

# real	0m0.006s
# user	0m0.001s
# sys 	0m0.005s
```

2. use the `date` function and the `bc` function (basic precision calculator)
```bash
START=$(date +%s.%N)
command
END=$(date +%s.%N)
DIFF=$(echo "$END - $START" | bc)
echo $DIFF
```

---

## scp, ssh — secure copy and secure shell — loginless setup

Imagine two machines: `host_src` (source) and `host_dest` (destination) and you want to `ssh` or `scp` without having to login (say from a script).

On host_src:
```bash
ssh-keygen -t rsa
scp ~/.ssh/id_rsa.pub <you>@host_dest:.
```

Then ssh to host_dest:
```bash
cat id_rsa.pub >> ~/.ssh/authorized_keys
chmod 644 ~/.ssh/authorized_keys
```

The above can be done in one step:
```bash
cat .ssh/id_rsa.pub | ssh ian.donaldson@mp60sci03v "cat - >> .ssh/authorized_keys"
```

---

## transferring large data sets

See <http://moo.nac.uci.edu/~hjm/HOWTO_move_data.html#rsync> or  
<http://www.cyberciti.biz/faq/lftp-mirror-example/>

For overview consider using:

- `lftp`  
- **bbcp** (multistreaming protocol — best for entire directories and large transfers, failsafe)

Example:
```bash
cd ~/scratch/1000G 
lftp ftp://ftp.1000genomes.ebi.ac.uk/vol1/ftp/release
mirror -cr 20130502
# -c allows for continued download if interrupted
# -r does not recurse into subdirectories
# 20G was downloaded in less than 5 min!
```

To place data somewhere (reverse mirror):
```bash
lftp ftp://geo:33%259uyj_fCh%3FM16H@ftp-private.ncbi.nlm.nih.gov/fasp/
mirror -Rc idonaldson
```

Browsing with lftp:
```bash
lftp
lftp :~> open -u anonymous,ian.oslo@gmail.com ftp.ensembl.org
```

Problems with proxy and lftp (disable proxy inside lftp):
```
set ftp:proxy
```

Example (trimmed from source text) shows disabling proxy to list directories successfully.

---

## rsync 

- fail‑safe  
- not multi‑streaming  
- block‑level checking for updates  
- better for syncing with remote content that can change (only updates blocks that change)

```bash
rsync -avz me@remote.url:somedir/somefile .
```

---

## wget (examples)

<https://www.labnol.org/software/wget-command-examples/28750/>

Download a file and save it in a specific folder:
```bash
wget --directory-prefix=folder/subfolder example.com
```

---

## compressing files

Use `pigz` — fast parallel compression of files.

---

## find

See also:  
<http://javarevisited.blogspot.co.uk/2011/03/10-find-command-in-unix-examples-basic.html> (good advice on using with `-exec` and `xargs` — see note on `-print0` if filenames might have spaces)  
About prune parameter: <http://www.theunixschool.com/2012/07/find-command-15-examples-to-exclude.html>

Find a file anywhere at or below your current position with blap in the name:
```bash
find . -name '*'
```

Don’t descend into directories you don’t have permission for:
```bash
find . ! -readable -prune -o -name '*blap*'
# or
find . ! -perm u+r,g+r,o+r -prune -o -name '*blap*'
```
`!` means not; `-readable` evaluates to true if the file/directory is readable; `-perm u+r,g+r,o+r` does the same; `-prune` prevents descending; `-o` is OR; `-name` …

Carry out a command on every file returned by find:
```bash
find . -name "A1-AD*.fastq.gz" -exec ls -1 {} \;
# NB space after final }.
```

Carry out a command on every file returned by find using xargs

- removing files
```bash
find . -name "*.tmp" -print | xargs rm –f
```
- grep files
```bash
find . –name "*.txt" –print | xargs grep "Exception"
```

It is possible to access the list item from within xargs:
```bash
find . -name "*.bak" -print0 | xargs -0 -I file mv file ~/old.files
```

To gzip many files, use
```bash
find . -name "*.fastq"  | xargs --verbose -P 32 -n 1 pigz
# -n 1 one argument at a time; -P 32 use 32 processors
```

Read more: <http://javarevisited.blogspot.com/2011/03/10-find-command-in-unix-examples-basic.html#ixzz3rkRSxT2P>

---

## xargs

```bash
seq 1 3 | xargs -n 1 -t ./test.sh
```
where `test.sh` is
```bash
#!/bin/bash
#test.sh
echo $1
```

Flags:  
- `-n 1` = take 1 argument at a time (can be 2,3,....)  
- `-t`   = echo the command before executing it  
- `-p`   = prompt the user before executing  
- `--show-limits` display limits set by OS  
- `-P` set max number of concurrent processes (note stdout may jumble)

**Advanced: write to separate logs**
```bash
seq 1 3 | xargs -n 1 -t -I THIS sh -c "./test.sh 'THIS' > 'THIS'.out"
```

**Parallel processes:**
```bash
seq 1 3 | xargs -P 2 -n 1 -t -I THIS sh -c "./test.sh 'THIS' > 'THIS'.out"
```

**More daring:**
```bash
seq 1 100 | xargs -P 100 -n 1 -t -I THIS sh -c "./test.sh 'THIS' > 'THIS'.out"
```

Alter `test.sh` to make it non-trivial:
```bash
sleep 5s
```

Compare:
```bash
time seq 1 10 | xargs -P 10 -n 1 -t -I THIS sh -c "./test.sh 'THIS' > 'THIS'.out"
time seq 1 10 | xargs -P 1  -n 1 -t -I THIS sh -c "./test.sh 'THIS' > 'THIS'.out"
```

Finally, consider using **parallel** to run jobs in parallel.

### copying large numbers of files 
Suppose you want to move 7200 fastq files that are in various subdirectories:
```bash
nohup find . -name "*_R2.fastq" | xargs --verbose -P 5 -n 2 cp -t /target_dir/. 1>~/nohup.cp.out 2>&1 &
```

Or using find+exec:
```bash
nohup find . -name "*_R2.fastq" exec cp -t /target_dir/. {} + 1>~/nohup.cp.out 2>&1 &
```

To move:
```bash
find . -name "*.fastq.gz" | xargs -I '{}' mv '{}' new_location
```

`{} +` notation will send as many file names to copy to the `cp` command as it can at once as opposed to just one at a time if you used `{} \;`

Trying to use `rsync` with `--exclude/--include` is possible but unwieldy; consider `find ... | xargs mv` after rsync.

Example `rsync` snippet:
```bash
nohup rsync -a             --include 'subdir_exprs'             --include '*_R2.fastq'             source_dir_name              target_dir_name 1>~/nohup.cp.out 2>&1 &
# followed by:
find fastq_demultiplex -name "*_R2.fastq" -exec mv -t . {} +
```

### rsync and use of `/` in command — hours of fun

**Should I add slashes to the end of the file path?**

A trailing slash on the source changes the behavior:

```bash
rsync -av /src/foo /dest       # copies dir by name
rsync -av /src/foo/ /dest/foo  # copies contents into /dest/foo
```

As to the destination, note:
```bash
rsync SRC DEST   # copy SRC file to DEST (file)
rsync SRC DEST/  # create directory DEST and copy SRC into it
```

---

## parallel

```bash
sudo apt install parallel
man parallel # excellent man page
```

Has same params as xargs plus lots more and simplified syntax
```bash
seq 1 10 | parallel -P 10 -n 1 -t -I THIS "./test.sh 'THIS' > 'THIS'.out"
```

---

## screen

```bash
# to start a new session
screen

# do something

# ctrl-a sends commands to screen
# detach from session:
Ctrl-a d

# re-attach to an existing screen session
screen -r
```

Getting help:  
<https://www.rackaid.com/blog/linux-screen-tutorial-and-how-to/>  
`man screen`

