# R Notes and Resources

# Table of Contents
- [Manuals, how-to's and tutorials](#manuals-how-tos-and-tutorials)
- [Statistics help](#statistics-help)
- [installing](#installing)
- [start R:](#start-r)
- [debugging](#debugging)
- [function (x) {](#function-x-)
- [line1 <- x](#line1---x)
- [line2 <- 2](#line2---2)
- [line3 <- line1 + line2](#line3---line1-line2)
- [return(line3)](#returnline3)
- [}](#)
- [basic commands for working with R](#basic-commands-for-working-with-r)
- ['save.image()' is just a short-cut for 'save my current workspace',](#saveimage-is-just-a-short-cut-for-save-my-current-workspace)
- [i.e., save(list = ls(all=TRUE), file = ".RData").](#ie-savelist-lsalltrue-file-rdata)
- [It is also what happens with q("yes").](#it-is-also-what-happens-with-qyes)
- [to restore this use](#to-restore-this-use)
- [note that ¦ is not the same as |](#note-that-is-not-the-same-as-)
- [also see stringr::str_split](#also-see-stringrstrsplit)
- [or](#or)

# Manuals, how-to's and tutorials
The R manual  
http://cran.r-project.org/doc/manuals/R-intro.html  
  
http://www.ats.ucla.edu/stat/r/   
http://www.ats.ucla.edu/stat/r/faq/   

Comparison to other languages  
http://www.johndcook.com/R_language_for_programmers.html   
http://hyperpolyglot.org/numerical-analysis   

Advanced R programming - profiling and optimization  
http://adv-r.had.co.nz/ ***
list of most useful functions to learn first: http://adv-r.had.co.nz/Vocabulary.html   

  
Making executable reports  
Rstudio markdown for making reports  
http://www.rstudio.com/ide/docs/authoring/using_markdown  
  
Useful Rants  
http://tim-smith.us/arrgh/  
http://www.burns-stat.com/pages/Tutor/R_inferno.pdf  

  
Tutorial/introduction here:  
http://manuals.bioinformatics.ucr.edu/home/programming-in-r  
  
Knitr/Pandoc/Miktex to make PDFs  
http://rprogramming.net/create-html-or-pdf-files-with-r-knitr-miktex-and-pandoc/  
  
Advanced lectures - scoping  
http://www.stat.berkeley.edu/~statcur/Workshop2/Presentations/functions.pdf  
Statistics tutorials with R  
http://ww2.coastal.edu/kingw/statistics/R-tutorials/  

  
Advanced text on probability, R and R graphing and (soon to come ?) statistics and machine learning: http://theanalysisofdata.com/  

# Statistics help  
This page focuses primarily on the R language although some explanations of stat methods has creeped in.  If you are looking for an explanation of a statistical test or method (or need to choose one) I usually go to one of these sites/books  
  
Choosing and Using Statistical Tests - hard copy at home  
  
http://www.biostathandbook.com - John H. MacDonald  
Various books at home on regression, statistical learning etc.  
Various books in  e. library.  
  
Other Statistics sites  
Good sites for exploring distribution  
http://zoonek2.free.fr/UNIX http://www.statmethods.net/advgraphs/probability.html  
  
Based on Quick R and R in Action  
https://www.statmethods.net/  
  
Bioconductor workflows  
http://bioconductor.org/help/workflows/   
See also my walkthrough of one of these in git/analysis   
  
  
# installing  
for mac: http://cran.rstudio.com/ - get updates from here too  
  
installing bioconductor  
source("http://bioconductor.org/biocLite.R")  
biocLite()  
see http://www.bioconductor.org/install/  
to install specific packages:  
source("http://bioconductor.org/biocLite.R")  
 biocLite("limma")  
  

to install package  
use R studio interface which calls  
install.packages("iRefR")  
http://www.statmethods.net/interface/packages.html  
  
```
library()   # see all packages installed  
search()    # see packages currently loaded  
.libPaths() # where does R store libs?  
```
to manually change where R stores libs do this at the shell prompt  
```
export R_LIBS="/home/your_username/R_libs"   
```  
making your own packages and libs  
`source()`
or  
http://www.r-bloggers.com/creating-your-personal-portable-r-code-library-with-github/  
or  
make a package -see above  

  
# start R:  
in windows: double click the icon  
in linux: type R at the prompt  
in mac: use R studio  

run an R script from the command line:  
http://stat.ethz.ch/R-manual/R-devel/library/utils/html/Rscript.html   
Rscript yourScript.R  

or  
https://stat.ethz.ch/R-manual/R-devel/library/utils/html/BATCH.html   
R CMD BATCH yourScript.R  
the output is sent to a file called yourScript.Rout  
cat yourScript.Rout  
  
tutorial:  
`help.start() #starts the help browser  `
start with "An introduction to R"  
start with Appendix A of this document for an example session  
next if you are interested in using graphng capabilities, go to section 12 on graphical procedures or run  
`demo(graphics)`  
  
  
sessionInfo() - will give a list of attached packages and libraries  

  
 
# debugging  
  
```r
debug(function_name)
#make a call to function name and you're in the debugger  
Browser[1]> help  
#do stuff  
undebug(function_name)  
```
if its been a while, take a look at this video  
https://vimeo.com/99375765   
and then this article   
https://support.rstudio.com/hc/en-us/articles/205612627-Debugging-with-RStudio   
and then this article which is more advanced  
http://adv-r.had.co.nz/Exceptions-Debugging.html  
  
** you can't set breakpoints inside .Rmd files so stop trying  **  
and if you are using Rstudio and stepping through an .Rmd file, you are essentially already in a debug mode for that environment  
  
if you want to debug one of the function calls from the .Rmd file (say to some function in some package) then do this:  
  
type   
`debug(function_name)`  
in the console  
then the next time this function is called, you will see your console prompt change to   
``Browser[1]>  ``  
then at this point type help to see the following list of commands 
```  
n          next  
s          step into  
f          finish  
c or cont  continue  
Q          quit  
where      show stack  
help       show help  
<expr>     evaluate expression  
```
  
use these commands to step through the code or evaluate the values of variables in the current environment (i.e. the function you are debugging)  
when you are done, type  
`undebug(function_name) `  
to prevent the debugger from starting when encuntering that function  
  
for more, try  
`?debugger`  
`?browser`  
  
correcting functions after you have debugged them  
you have found an error in a function in some package and want to correct it, see this link  
https://stackoverflow.com/questions/2458013/what-ways-are-there-to-edit-a-function-in-r  
  
you can use  
`fix(function_name)`   
* this will open an editor and allow you to edit the function  
* make sure to document the solution somewhere since this fix is only for your environment and may have to be replicated elsewhere  
`fixInNamespace`  
* might be able to use this to edit functions that are in a package but not exported  
`body()`  
* can be used to replace entire function or specific lines of function. for example, given this function:  
  
```r
foo <- function(x)  
{  
    line1 <- x  
    line2 <- 0  
    line3 <- line1 + line2  
    return(line3)  
}  
```
  
and you want to change the fourth line to   
    line2 <- 2  
  
you can start by retrieving the line numbers using  
`as.list(body(foo))  `  
  
then do  
```r
body(foo)[[3]] <- substitute(line2 <- 2)  
foo  
# function (x) {  
#     line1 <- x  
#     line2 <- 2  
#     line3 <- line1 + line2  
#     return(line3)  
# }  
```
  
* if you want to replace a function body with contents from a file,  
* rewrite the entire function in a new file called fix.R containing  
  
`working_function <- function() {...}   `  
  
```r
source("fix.R")  
body(broken_function) <- body(working_function)  
```
  
you can use a similar strategy to above in order to change the parameters of a function using   
`formals(some_function)`  
see http://adv-r.had.co.nz/Functions.html  
  
  
# basic commands for working with R
quit R:  
`q() `    
  
get help:  
```r
?  
? mean  
```  
you can also use this syntax   
```r
help(mean)  
help.search  
RSiteSearch  
apropos  
```
  
command history:  use the up and down arrows.  
  
  
set working directory  
```r
setwd("I:/myR") # note the quotes  
getwd() #to find what wd is currently set to  
list.files() #  
list.dirs() # 
```
  
  
listing data objects in current workspace:  
`ls()  `
removing data objects:  
`rm(somedataObjectName) ` 
  
downloading data from a url  
* there is poor support for downloading multiple files from a directory - you will need to download one file at a time using the method  below - alternatively, install wget or curl and use  
`system("wget -r http://xxx/xxx")  `  
`system("curl -O http://xxx/xxx")  `  
  
```r
if (!file.exists("data")){ dir.create("data") }  
  
fileUrl <- "http://cancer.sanger.ac.uk/cancergenome/assets/cancer_gene_census.tsv"  
  
download.file(fileUrl,   
destfile="cancer_gene_census.tsv",   
method="curl", quiet = FALSE, mode = "w",  
              cacheOK = TRUE)  
dateDownloaded <- date()  
```
  
  
reading in data from a tab-delimited file  
```r
data.df <- read.table(  
"cancer_gene_census.tsv",  
        header=TRUE,  
        sep="\t",  
        stringsAsFactors=FALSE, #unless you want  
        quote="" #means no quotes - can solve lots of problems  
)  
```
  
**note:**  
> sometimes files can contain quotes or comment characters where you might not expect them.  using `quote=""` and `comment.char=#` (after removing comments you know about at the top of a file) can solve problems and save hours of fun.  Comment characters (like `#`) in the middle of a line will cause an error like "line 12344 does not have the expected number of elements".  The problem is that the problematic line number reported by R's scan function may be incorrect.  
  
**note:**  
the fastest way to remove the first line of a large file might be  
`csplit -k fileName 1 '{1}'`  
but for readability (even though its a little slower) use  
`tail -n +2`  
  
reading data a line at a time  
```r
fh<-file(description="final/en_US/en_US.blogs.txt", open="r")
thisLine <- readLines(con=fh, n=1)  # can read into list if n > 1 or unspecified
thisLine <- gsub(pattern="[^[:alnum:] ]", replacement="", x=thisLine)  # remove non-word chars
close(fh)  
```
  
reading data from a file into a single string  
```r
fileName <- 'foo.R'
x <- readChar(fileName, file.info(fileName)$size)
```
  
reading data from mySql
* see RMySQL package
  
reading/writing data to HDF5 files
* hierarchical data format - see hdfgroup.org
* see rhdf5 package from bioconductor
* files contain tables and their meta data
* data can be read or written to specific rows and columns of a table
  
reading and writing from excel files
* see xlsx2 package and XLConnect package
  
reading and extracting information from XML  
* see XML package  
* http://www.w3schools.com/xml/simple.xml  
* XPath tutorial: http://www.stat.berkeley.edu/~statcur/Workshop2/Presentations/XML.pdf  
XML tutorials:  
* http://www.omegahat.org/RSXML/shortIntro.pdf
* http://www.omegahat.org/RSXML/Tour.pdf (long)
  
reading and writing JSON  
* see jsonlite package
  
reading and writing large tables  
* consider using data.table (much faster and memory efficient than data.frame  
  
web-scraping   
* see httr package - handles parsing of htmp and username and password authentication  
  
CONTINUE FORMATTING HERE
  
browse a table loaded from a file
```r
head(cgcensus)      # look at first 10 rows
tail(cgcensus)      # look at last ten rows
cgcensus[1]         # first column
cgcensus[,1]        # first row
t(cgcensus)         # transpose a data frame 
```
  
saving and loading data from a session:
```r
save(file="variabs.Rdata", list="a","vec", "mat")
load(file="variabs.Rdata")
```
but the above you have to specify the variable to save
  
save an entire session 
```r
save.image(file="mysession.RData")
# 'save.image()' is just a short-cut for 'save my current workspace',
# i.e., save(list = ls(all=TRUE), file = ".RData").
# It is also what happens with q("yes").
# to restore this use
load(file="mysession.Rdata")
```
  
basic math:
`+,-,*, / and ^`, grouped with parentheses
  
functions:
```r
sqrt(2)
sin(pi)
exp(1)
log(10)
```
  
assignment:
```r
a<-5+2
a=2
```
  
list variables: 
`ls()`
  
getting information about a variable (data-type, structure etc):
```r
class(x)
typeof(x) # for vectors
str(x)
attributes()
```
  
conditional operations, boolean logic, comparison operations
```r
a <- 1
b <- 2
if (a > 0) cat("correct")
if (a > 1 | b > a) cat("correct")
# note that ¦ is not the same as |
```
  
strings
splitting strings
```r
x <- "taxid:559292(Saccharomyces cerevisiae S288c)"
strsplit(x, split="[:(]")  # split is a regex; use fixed=TRUE to interpret literally
unlist(strsplit(x, split="[:(]"))[2]  # "559292"
# also see stringr::str_split
```
  
concatenating (collapsing) a vector into a string using paste
```r
x <- c("a","b","c")
paste(x, sep=" ")          # not what you want
paste(x, collapse=" ")     # "a b c"
```
  
vectors:
```r
vec1 <- NULL
vec1 <- c()
vec1 = c(2,3,4)
vec2 = c(2,3,4)
vec3 = c(vec1,vec2)
vec3[2:4]       # elements 2..4
vec3[vec3<4]    # elements less than 4
vec3[-3]        # all elements except 3rd
```
  
lists:
```r
obj = list(1,2,"cat")                 # mixed types
obj = list(a=1,b=2,name="sam")        # named list
obj$name
obj['name']
obj[['name']]
```
  
hashes (requires the **hash** package)  
http://opendatagroup.wordpress.com/2009/07/26/hash-package-for-r/
  
hash assignment
```r
hash()
hash( keys=c('foo','bar','baz'), values=1:3 )
hash( foo=1, bar=2, baz=3 )
hash( c( foo=1, bar=2, baz=3 ) )
hash( list( foo=1, bar=2, baz=3 ) )
hash( c('foo','bar','baz'), 1:3 )
```
  
hash accession
The standard accessors: `[`, `[[`, `$` 
```r
h <- hash( c('foo','bar','baz'), 1:3 )
h[ c('foo','bar') ]
h[[ 'foo' ]]
h$foo
```
  
hash replacement operation
```r
h <- hash( c('foo','bar','baz'), 1:3 )
h[ c('foo','bar') ] <- c( 'fred', 'wilma' )
h[[ 'foo' ]] <- 'dino'
h$foo <- 'bam bam'
```
  
hashes implemented using env (environments)
```r
hash = new.env(hash=TRUE, parent=emptyenv(), size=100L)
assign(key, value, hash)
get(key, hash)
ls(env=hash)  # keys
```
  
how to remove NAs from a vector
```r
v <- v[-which(is.na(v))]
```
  
how to replace NULLs in a list
```r
l <- list(a=c(1:3), b=NULL, c=c(4:6))
lapply(l, function(x) if(is.null(x)){x<-"NA"} else {x})
# or
replace(l, which(is.na(l)), "NA")
```
  
replacing NULLs in a list of lists
```r
l <- list(list(a=1,b=NULL), list(a=2,b=2)) 
lapply(l, function(x) lapply(x, function(x) ifelse(is.null(x), NA, x)))
```
  
cut - making bins from a continuous variable
  
matrix:
```r
mat=matrix(1:9,nrow=3,ncol=3) 
mat[1,3]
mat[,2]
mat[2,]
mat[mat$V1=="3",]
for(i in 1:3){print(mat[i,3])}
mat=cbind(c(1,2,3),c(4,5,6),c(7,8,9))
mat=rbind(c(1,4,7),c(2,5,8),c(3,6,9))
rownames(mat)=c("Obs1", "Obs2", "Obs3")
colnames(mat)=c("Var1", "Var2", "Var3")
```
  
converting a data frame to a vector by first converting to a matrix (matrices)
```r
thisVector = as.vector(as.matrix(unname(thisDataFrame)))
```
  
using names and multidimensional arrays as hashes
```r
a<-array(0,dim=c(2,3))
dimnames(a)<-list(intA=c("P001","P002"), intB=c("P003","P004","P005"))
a["P001","P004"]<-1
a["P001","P004"]
dimnames(a)["intA"]
a["P001",]
a[c("P001","P002"),]
theseRows<-c("P001","P002")
a[theseRows,]
a[c(TRUE,FALSE),]
L = a["P001",]==1
y=(dimnames(a)[["intB"]])
y[L]  # "P004"
```
  
dates  
http://www.statmethods.net/input/dates.html
```r
today <- Sys.Date()
format(today, format="%B %d %Y")
date()
```
  
times
```r
rightNow <-Sys.time()
format(rightNow, format="%H:%M:%S")
```
  
means - arithmetic,  geometric and harmonic  
http://stats.stackexchange.com/questions/23117/which-mean-to-use-and-when
  
*(Excellent explanatory section retained as-is.)*
  
data frames:
```r
thisDataFrame1 = data.frame(cbind(x=1,y=1:10))
df = data.frame(matrix(vector(), 0, 3, dimnames=list(c(), c("Date", "File", "User"))), stringsAsFactors=F)
df <- data.frame(Date=as.Date(character()), File=character(), User=character(), stringsAsFactors=FALSE)
cancerNames2geneids <- data.frame(cancerName=character(), geneid=integer(), stringsAsFactors=FALSE)
```
  
deleting rows using logical indices / numeric indices:
```r
airquality2 <- subset(airquality, !(Day %in% c(5, 7) & Month == 5))
airquality3 <- airquality[-c(2, 7), ]
df = df[-1,]
```
  
delete a column:
```r
df$x = NULL
```
  
Using rbind overwrites column labels — notes preserved.  
  
R Grouping functions: sapply/lapply/apply/tapply/by/aggregate  
http://stackoverflow.com/questions/3505701/r-grouping-functions-sapply-vs-lapply-vs-apply-vs-tapply-vs-by-vs-aggrega
  
using apply
```r
sapply(1:3, function(x) x^2)
lapply(1:3, function(x) x^2)
```
  
tapply
```r
library(datasets)
data(iris)
tapply(iris$Sepal.Length, iris$Species, mean)
```
  
aggregate examples (ToothGrowth) — formulas shown:
```r
aggregate(len ~ supp, data = ToothGrowth, mean)
aggregate(len ~ ., data = ToothGrowth, mean)
aggregate(. ~ supp, data = ToothGrowth, mean)
```
  
useful function for finding out about functions
```r
isGeneric("print")
getOption("defaultPackages")
args(mean)
str(lm)
methods(predict)
search()
```
  
managing and working with packages and libraries  
http://www.ats.ucla.edu/stat/r/faq/packages.htm 
```r
installed.packages()
available.packages()
install.packages("packageName")
search()
sessionInfo()
detach("package:pkg", unload=TRUE)
```
  
Example of detaching packages to resolve conflicts preserved.  
  
printing to screen / variables:
```r
vec1 = c(2,3,4); print(vec1); print("hello")

person <-"Grover"; action <-"flying"
cat("cat-Dimension of the data frame is", person,"\n", sep='\t')
message(sprintf("On %s I realized %s was...\n%s by the street", Sys.Date(), person, action))
```
  
print to a file
```r
cat("Hello",file="outfile.txt",sep="\n")
cat("World",file="outfile.txt",append=TRUE)
write(thisSubjectName, file="outFile", append=TRUE, sep='')
sink("outfile.txt"); print("hello"); cat("\n"); cat("world"); sink()
```
  
printing inside a loop or a function — notes preserved.
  
make an eps figure
```r
postscript(file="./fig2.eps", horizontal=F)
pie(c(1,2,3))
dev.off()
```
  
Getting/setting working dir, read/write tables
```r
getwd(); setwd()
mat=matrix(1:9,nrow=3,ncol=3) 
write.table(mat, "mytable.txt")
thisTable = read.table("mytable.txt")
```
  
Basic plotting — references and links preserved.  
  
Legends / factors example preserved.  
  
Saving plots  
```r
par(mar=c(5,4,4,2)+0.1)
dev.copy(pdf, 'filename.pdf'); dev.off()
png(file="./fig2.png", width=500, height=500); plot(1:10); dev.off()
```
  
Histogram
```r
x <- mtcars$mpg
hist(x, breaks=12, col="red"); rug(x)
hist(x, breaks=12, col="red", freq=FALSE); lines(density(x))
```
  
