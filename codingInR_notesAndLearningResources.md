### Manuals, how-to's and tutorials
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

### Statistics help  
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
  
  
### installing  
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

  
### start R:  
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

  
 
### debugging  
  
```debug(function_name)
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
  
```
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
```
> body(foo)[[3]] <- substitute(line2 <- 2)  
> foo  
function (x)   
{  
    line1 <- x  
    line2 <- 2  
    line3 <- line1 + line2  
    return(line3)  
}  
```
  
* if you want to replace a function body with contents from a file,  
* rewrite the entire function in a new file called fix.R containing  
  
`working_function <- function() {...}   `  
  
and then do  
```
source("fix.R")  
body(broken_function) <- body(working_function)  
```
  
you can use a similar strategy to above in order to change the parameters of a function using   
formals(some_function)  
see http://adv-r.had.co.nz/Functions.html  
  


### basic commands for working with R
quit R:  
`q() `    
  
get help:  
```
?  
? mean  
```  
you can also use this syntax   
```
help(mean)  
help.search  
RSiteSearch  
apropos  
```
  
command history:  use the up and down arrows.  
  
  
set working directory  
```
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
  
`if (!file.exists("data")){ dir.create("data") }  `  
  
`fileUrl <- "http://cancer.sanger.ac.uk/cancergenome/assets/cancer_gene_census.tsv"  ` 
  
``
download.file(fileUrl,   
destfile="cancer_gene_census.tsv",   
method="curl", quiet = FALSE, mode = "w",  
              cacheOK = TRUE)  
dateDownloaded <- date()  
```
  
  
reading in data from a tab-delimited file  
```
data.df <- read.table(  
"cancer_gene_census.tsv",  
		header=TRUE,  
		sep="\t",  
		stringsAsFactors=FALSE, #unless you want  
		quote="" #means no quotes - can solve lots of problems  
)  
```
  
`note: `  
> sometimes files can contain quotes or comment characters where you might not expect them.  using quote="" and comment.char=# (after removing comments you know about at the top of a file) can solve problems and save hours of fun.  Comment characters (like #) in the middle of a line will cause an error like "line 12344 does not have the expected number of elements".  The problem is that the problematic line number reported by R's scan function may be incorrect.  
  

`note: `  
the fastest way to remove the first line of a large file might be
`csplit -k fileName 1 '{1}'`
but for readability (even though its a little slower) use
`tail -n +2`

reading data a line at a time  
`fh<-file(description="final/en_US/en_US.blogs.txt", open="r")`  
`thisLine <- readLines(con=fh, n=1)`  
can read into list if n is > 1 or unspecified 
could use lapply  
remove any non-word character  
`thisLine <- gsub(pattern="[^[:alnum:] ]", replacement="", x=thisLine)`  
`close(fh)`  
  
reading data from a file into a single string  
`fileName <- 'foo.R'`
`x <- readChar(fileName, file.info(fileName)$size)`

  
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
head(cgcensus) 	# look at fisrt 10 rows
tail(cgcensus) 	#look at last ten tows
cgcensus[1]		#first column
cgcensus[,1]		#first row

t(cgcensus)		#transpose a data frame 


saving and loading data from a session:
save(file="variabs.Rdata", list="a","vec", "mat")
load(file="variabs.Rdata")
but the above you have to specify the variable to save

save an entire session 
save.image(file="mysession.RData")
'save.image()' is just a short-cut for 'save my current
     workspace', i.e., 'save(list = ls(all=TRUE), file = ".RData")'.
     It is also what happens with 'q("yes")'.
to restore this use
load(file="mysession.Rdata")


basic math:
+,-,*, / and ^, grouped with parentheses

functions:
sqrt(2)
sin(pi)
exp(1)
log(10)

assignment:
a<-5+2
or
a=2

list variables: 
ls()

getting information about a variable (data-type, structure etc):
class(x)
typeof(x) # for vectors
str(x)
attributes()


conditional operations, boolean logic, comparison operations

a <- 1
b <- 2
if (a > 0) cat("correct")
if (a > 1 | b > a) cat("correct")
#note that ¦ is not the same as |


strings
splitting strings
x <- "taxid:559292(Saccharomyces cerevisiae S288c)"
strsplit(x, split="[:(]" ) 
#note: split is a regex
#use fixed=TRUE to interpret split literally
#[[1]]
#[1] "taxid"                           "559292"                          #"Saccharomyces cerevisiae S288c)"
#a list is returned - use unlist to return a vector
unlist(strsplit(x, split="[:(]" ))[2]
#[1] "559292"
#also see stringr package and str_split

#concatenating (collapsing) a vector into a string using paste
#this does not work
x <- c("a","b","c")
paste(x, sep=" ")
x
#[1] "a" "b" "c"
#instead use this
paste(x, collapse=" ")
x
#[1] "a b c"




vectors:
vector initialization
vec1 <- NULL;
vec1 <- c();
vectors assignment:
vec1 = c(2,3,4)
vec2 = c(2,3,4)
vec3 = c(vec1,vec2)
vector retrieval:
vec3
vec3[2:4] - elements 2, 3 and 4
vec3[vec<4] - elements less than 4
vec3[-3] - all elements except 3rd

lists:
obj = list(1,2,"cat") # can have elements of different types
#lists can be named
obj = list(a=1,b=2,name="sam") # - note use of plain quotes
#list retrieval - three syntaxes
obj$name
obj['name']
obj[['name']]


hashes (requires the hash package)
http://opendatagroup.wordpress.com/2009/07/26/hash-package-for-r/

hash assignment
hash()
hash( keys=c('foo','bar','baz'), values=1:3 )
hash( foo=1, bar=2, baz=3 )
hash( c( foo=1, bar=2, baz=3 ) )
hash( list( foo=1, bar=2, baz=3 ) )
hash( c('foo','bar','baz'), 1:3 )


hash accession
The standard accessors: [, [[, $ 
h <- hash( c('foo','bar','baz'), 1:3 )
h[ c('foo','bar') ]
h[[ 'foo' ]]
h$foo

hash replacement operation
h <- hash( c('foo','bar','baz'), 1:3 )
h[ c('foo','bar') ] <- c( 'fred', 'wilma' )
h[[ 'foo' ]] <- 'dino'
h$foo <- 'bam bam'


hashes implemented using env (environments)
hash = new.env(hash=TRUE, parent=emptyenv(), size=100L)
assign(key, value, hash)
get(key, hash)
#We can even get the keys from the hash with the ls function:
ls( env=hash )

#how to remove NAs from a vector
v <- v[-which(is.na(v))]

#how to replace NULLs in a list
l <- list(a=c(1:3), b=NULL, c=c(4:6))
lapply(l, function(x) if(is.null(x)){x<-"NA"} else {x})

#or use the replace function
l <- list(a=c(1:3), b=NULL, c=c(4:6))
replace(l, which(is.na(l)), "NA")


#replacing NULLs in a list of lists
l <- list(list(a=1,b=NULL), list(a=2,b=2)) 
lapply(l, function(x) lapply(x, function(x) ifelse(is.null(x), NA, x))) 


cut - making bins from a continuous variable



matrix:
mat=matrix(1:9,nrow=3,ncol=3) 
mat[1,3] - note row, column format
mat[,2]
mat[2,]
mat[mat$V1=="3",] -retrieve the row where the first column (V1) is equal to three
mat[mat$V1=="3",] -quotes are not necessary?
for(i in 1:3){print(mat[i,3])}
mat=cbind(c(1,2,3),c(4,5,6),c(7,8,9))
mat=rbind(c(1,4,7),c(2,5,8),c(3,6,9))
column and row headings:
rownames(mat)=c("Obs1", "Obs2", "Obs3")
colnames(mat)=c("Var1", "Var2", "Var3")


converting a data frame to a vector by first converting to a matrix (matrices)

thisVector = as.vector(as.matrix(unname(thisDataFrame)))
  


using names and multidimensional arrays as hashes
declaration
> a<-array(0,dim=c(2,3))
> a
     [,1] [,2] [,3]
[1,]    0    0    0
[2,]    0    0    0

assigning dim names
> dimnames(a)<-list(intA=c("P001","P002"), intB=c("P003","P004","P005"))
> a
      intB
intA   P003 P004 P005
  P001    0    0    0
  P002    0    0    0

assignment
> a["P001","P004"]<-1
> a
      intB
intA   P003 P004 P005
  P001    0    1    0
  P002    0    0    0

recall
> a["P001","P004"]
[1] 1

recalling names for all interactors
> dimnames(a)["intA"]
$intA
[1] "P001" "P002"


retrieving rows
a["P001",] 				#using a row name

a[c("P001","P002"),] 		#using a list of row names

theseRows<-c("P001","P002")	#using a list of row names in a variable
a[theseRows,]

a[c(TRUE,FALSE),]			#using a logical vector
P003 P004 P005 
   0    1    0 


retrieving interactor names using logical vectors
> L = a["P001",]==1
> L
 P003  P004  P005				#a logical vector 
FALSE  TRUE FALSE 

> y=(dimnames(a)[["intB"]])			#note the double brackets
> y
[1] "P003" "P004" "P005"
> y[L]
[1] "P004"					#the name of the interactor

dates
http://www.statmethods.net/input/dates.html
today <- Sys.Date()
format(today, format="%B %d %Y")
"June 20 2007"

date()


times
rightNow <-Sys.time()
format(rightNow, format="%H:%M:%S")


means - arithmetic,  geometric and harmonic
http://stats.stackexchange.com/questions/23117/which-mean-to-use-and-when
arithmetic data for comparing levels 
	mean of the values
geometric means are used for comparing ratios/multiplicative diffs
	inverse log of the mean of the logs 
harmonic means for comparing rates
	reciprocal of the mean of the reciprocals 

A mean is a central point in a set of measurements (it minimizes the sums of the differences between that point and all other points in the set).  A mean is used to characterize a set of measurements (usually for the purpose of comparing two or more sets of measurements.  So, for example, if we wanted to compare the height of girls with the height of boys in a class we would naturally compare the mean heights of the two groups.  
But what if we wanted to compare two cells that each have a set of three genes that are overexpressed 3X, 10X and 7X in one cell and 5X, 5X and 7X.  Which cell has the highest level of overexpression? Now we can't simply take the arithmetic mean because there are three different measurements made on a multiplicative scale (where the multiple is hidden).  Instead we could take the log of each of the measurements, take the arithmetic mean of these logs and then the reciprocal log of this mean (because we are interested in the total multiplicative differences between the two cells); this is the geometric mean.  The alternative would be to convert the fold-overexpression back to total counts and then take the arithmetic mean but this may not be possible if we don't have the total counts.  Make a spreadsheet for this. https://www.mathsisfun.com/numbers/geometric-mean.html



		absolute expression		
base		cell_1	cell_2		
10	gene_1	30	10	⇐ each cell is base x factor	
100	gene_2	100	300		
10000	gene_3	30000	10000		
		3	1	⇐ factors	
		1	3		
		3	1		
arithmetic mean	2.333333333	1.666666667		1.4
arithmetic mean abs	10043.33333	3436.666667		2.922405432
geometric mean	2.080083823	1.44224957		1.44224957
					


note how geometric means are an accurate reflection of the factors even though they are computed using only the factors


Finally, what what if we wanted to compare two drivers that each drive back and forth between two cities at different speeds (e.g. driver one drives from A to B at 50 Km/hr and then from B to A at 100 Km/hr while driver two drives from A to B at 75 Km/hr and then from B to A at 75 Km/hr.  Which driver is faster?  The answer is non-intuitive - if you just take the arithmetic means of their speeds you would conclude that they are both the same.  But if you knew the the distance from A to B is 100 Km.  Driver A's speed is 200/(2 + 1)= 66.6 while Drivers B speed is 200(100/75 + 100/75)= 75.  But what if this distance is hidden.  We want to compare ratios where the dividend is hidden. So, we could take the reciprocals of each of the measurements, then take the arithmetic mean of these reciprocals and then take the reciprocal of this mean; this is the harmonic mean.

data frames:
Data frames contain several variables (columns) and several experimental units (rows). Try:
thisDataFrame1 = data.frame(cbind(x=1,y=1:10))
thisDataFrame
#create a data frame without specifying column data types
df = data.frame(matrix(vector(), 0, 3, dimnames=list(c(), c("Date", "File", "User"))), stringsAsFactors=F)
#or specify the column data types like this
df <- data.frame(Date=as.Date(character()),
                 File=character(), 
                 User=character(), 
                 stringsAsFactors=FALSE)
another example
cancerNames2geneids <- data.frame(cancerName=character(), geneid=integer(), stringsAsFactors=FALSE);


deleting rows using logical indices:
length(airquality$Day)
airquality2 <- subset(airquality, !(Day %in% c(5, 7) & Month == 5))
deleting rows using numerical indices
length(airquality$Day)
airquality3 <- airquality[-c(2, 7), ]
If you'd like to actually delete the first row from a data.frame, you can use negative indices like this:
df = df[-1,]


If you'd like to delete a column from a data.frame, you can assign NULL to it:
df$x = NULL

Using rbind overwrites column labels
rbind requires that all arguments have the same column names!!!!
#initialize the data frame
cancerNames2geneids <- data.frame(cancerName=character(), geneid=integer(), stringsAsFactors=FALSE);

#or initialize the dataframe this way

cancerNames2geneids <- data.frame(matrix(ncol = 2, nrow = 0), stringsAsFactors=F)

#or this way

cancerNames2geneids = data.frame(matrix(vector(), 0, 2, dimnames=list(c(), c("cancerName", "geneid"))), stringsAsFactors=F)

#there seems to be no sense to adding column names when declaring a data-frame since they are overwritten by the rbind function and have to be readded

#instead, initialize the df with a row of fake data before using rbind then remove the fake row later - like this

cancerNames2geneids <- data.frame(cancerName="some_name", geneid=1111, stringsAsFactors=FALSE);

#use rbind to add rows ….

#it is essential that all added rows have the ***same*** column names
#other wise you see errors like 
#"Error in if (facCol[jj]) { : missing value where TRUE/FALSE needed"
#using unname on the rows before adding them will not work
#failing to use:
#colnames(thisRow) <- c("colName1", "colName2")
#before adding the row will lead to errors and hours of fun


#then remove the first fake row
cancerNames2geneids = cancerNames2geneids[-1,]


R Grouping functions: sapply vs. lapply vs. apply. vs. tapply vs. by vs. aggregate
the apply functions are covered well in this post
http://stackoverflow.com/questions/3505701/r-grouping-functions-sapply-vs-lapply-vs-apply-vs-tapply-vs-by-vs-aggrega


using apply
see http://www.r-bloggers.com/using-apply-sapply-lapply-in-r/
examples
sapply(1:3, function(x) x^2)
#[1] 1 4 9

lapply is very similar, however it will always return a list rather than a vector:

lapply(1:3, function(x) x^2)
#[[1]]
#[1] 1
#
#[[2]]
#[1] 4
#
#[[3]]
#[1] 9

tapply - apply a function FUN to subsets of data x as specified by some factor variable f (t means type)
tapply(x, f, FUN)
library(datasets)
data(iris)
tapply(iris$sepal.Length, iris$Species, mean)

aggregate - merge rows together sharing some common key in one column and using some function - for e.g. this could be used to consolidate data from multiple entries (rows) for the same sample as indicated by the entry in the "sample_name" column - you might add or average the data in adjacent columns
e.g. using the ToothGrowth built-in data set

ToothGrowth
    len supp dose
1   4.2   VC  0.5
2  11.5   VC  0.5
11 16.5   VC  1.0
12 16.5   VC  1.0

31 15.2   OJ  0.5
32 21.5   OJ  0.5
41 19.7   OJ  1.0
42 23.3   OJ  1.0


aggregate(x=ToothGrowth[,c(1,3)], by=list(ToothGrowth[,2]), FUN=mean)
x are the data column to be aggregated
by is the column forming the key to aggregate on
FUN is the function to apply to all rows in a column for original rows with the same 'by' key
you can also aggregate by a key that covers more than one column
aggregate(x=ToothGrowth[,1], by=list(ToothGrowth[,2],ToothGrowth[,3]), FUN=mean)

the above can also be expressed using formula notation
aggregate(len ~ supp, data = ToothGrowth, mean)
and
aggregate(len ~ ., data = ToothGrowth, mean)
and
aggregate(. ~ supp, data = ToothGrowth, mean)


useful function for finding out about functions

is.Generic
getOption("defaultPackages")
args(functionName)
str(functionName)
methods(functionName) - what functions could a generic function called functionName call. e.g. methods(predict)
search() - returns a list of packages where function definitions are searched for


managing and working with packages and libraries
http://www.ats.ucla.edu/stat/r/faq/packages.htm 
installed.packages() - get a list of installed packages - anything that says "recommended" or "NA" is installed but not loaded
available.packages() - not installed - large list
install.packages("packageName")

search() - to get a list of packages where function definitions are searched for
sessionInfo() - will show attached and loaded (but not attached packages
detach("package:packageName", unload=TRUE)- can be useful when debugging -unloading is only an attempt - it could fail if the package is required for some other currently loaded package

when you see this error
Error in sqliteExecStatement(con, statement, bind.data) : 
  RS-DBI driver: (expired SQLiteConnection)
 try
> search()
 ".GlobalEnv"           
"package:GOSemSim"      
"package:BiocInstaller" 
"package:AnnotationDbi" 
"package:GenomeInfoDb"  
"package:IRanges"       
"package:S4Vectors"    
"package:Biobase"       
"package:BiocGenerics"  
"package:parallel"      
"package:stats4"        
"package:RSQLite"       
"package:DBI"           
"tools:rstudio"        
"package:stats"         
"package:graphics"      
"package:grDevices"     
"package:utils"         
"package:datasets"      
"package:methods"       
"Autoloads"            
"package:base"         

detach("package:GOSemSim", unload=TRUE)
detach("package:BiocInstaller", unload=TRUE)
detach("package:AnnotationDbi", unload=TRUE)
detach("package:GenomeInfoDb", unload=TRUE)
detach("package:RSQLite", unload=TRUE)
detach("package:DBI", unload=TRUE)

then

source("http://bioconductor.org/biocLite.R")
biocLite("GOSemSim")
library(GOSemSim)
geneSim("241", "251", ont = "MF", organism = "human", measure = "Wang", combine = "BMA")
Loading required package: org.Hs.eg.db
Loading required package: AnnotationDbi
Loading required package: GenomeInfoDb

Attaching package: ‘AnnotationDbi’


#####
What is the difference between loading and attaching a package

Sometimes attaching things to your search path causes conflicts, if there are identically named objects in two or more attached items. You'll be warned, but you may later want a reminder of what the conflicts were. Do this... 
conflicts()

dealing with library conflicts - an example
there is an interesting example here of how the order of loading libraries can matter
the "select" function from the AnnotatioDb package stopped working with error
Error in select(GO.db, keys = "GO:1990237", columns = c("GOID", "TERM",  : 
  unused arguments (keys = "GO:1990237", columns = c("GOID", "TERM", "ONTOLOGY"), keytype = "GOID")

I say "stopped working" because the call was composed correctly and had worked before.  The above cryptic message doesnt tell you that 
its not trying to call the "select" function you think you are trying to call.  In the case above, the phrase "unused arguments"
should be the tip-off that you need to check this
try this
> select
function (obj) 
UseMethod("select")
<bytecode: 0x10e0389c0>
<environment: namespace:MASS>

the problem was solved by doing this…
detach("package:MASS")  # this effectively drops the library and function definitions asociated with it so that prior function definitions are no longer masked


remove.packages("packageName") 
help(package="packageName") - help and dataset descriptions
sessionInfo()


printing to screen:
vec1 = c(2,3,4)
print(vec1)
print("hello")

printing using variables
  person <-"Grover"
  action <-"flying"
  #simplest
  cat("cat-Dimension of the data frame is", person,"\n", sep='\t')
  #using sprintf
  message(sprintf("On %s I realized %s was...\n%s by the street", Sys.Date(), person, action))
  cat(sprintf("cat - On %s I realized %s was...\n%s by the street", Sys.Date(), person, action))
  print(sprintf("On %s I realized %s was...\n%s by the street", Sys.Date(), person, action))





print to a file
using cat - preferred
cat("Hello",file="outfile.txt",sep="\n")
cat("World",file="outfile.txt",append=TRUE)

using write - use this inside loops - see below
write(thisSubjectName, file=outFile, append=TRUE, sep='')

using sink
sink("outfile.txt")
print("hello")
cat("\n")
cat("world")
sink()

using writeLines - dont use this, i cant get it to append to a file
use write instead
#must have a connection
fileConn<-file("output.txt")
writeLines(c("Hello","World"), fileConn)
close(fileConn)



printing inside a loop or a function
implicit printing is turned off inside loops and functions (since these belong to different environments) so
	shapiro.test(DF[,y])
will not print anything

instead, explicitly print using either cat or print


for(y in 1:2) {
	#preferred - 
someText<-paste("Shapiro Wilks Test for column", y)
write(someText, file="outFile", append=TRUE, sep='')

#or
cat(someText, "and some more text", sep = " ")

    		print(shapiro.test(DF[,y]))
		#or - not preferred - ok for stdout but for 
#printing to a file needs a file connection
		#and i cant seem to get this to append to a file
		writeLines(someText
}

also consider combining the above with a call to 
flush.console()
or you could paste everything into a string and then print that string after the loop finishes; e.g.:
built_up_text=paste(c(built_up_text, stringFromThisIteration,"\n"), collapse='' )



#make a eps figure
postscript(file="./fig2.eps", horizontal=F)
pie(c(length(unique(irefindex_curr_ecoli$icrigid)), length(unique(irefindex_curr_yeast$icrigid)), length(unique(irefindex_curr_celegans$icrigid)), length(unique(irefindex_curr_fly$icrigid)), length(unique(irefindex_curr_mouse$icrigid)), length(unique(irefindex_curr_rat$icrigid)), length(unique(irefindex_curr_human$icrigid))), labels=c("E. coli", "S. cerevisiae", "C. elegans", "D. melanogaster", "M. musculus", "R. norvegicus", "H. sapiens"))
dev.off()


Getting and setting the working directory:
getwd()
setwd()

Writing and reading data to/from disk:
mat=matrix(1:9,nrow=3,ncol=3) 
write.table(mat, "mytable.txt")
thisTable = read.table("mytable.txt")
thisTable
rownames(thisTable)
colnames(thisTable)
?read.table - note many options
?write.table - notes many options

Editing data using the R editor
thisNewTable = edit(thisTable)





Basic plotting
References
http://www.statmethods.net/graphs
R Graphics Cookbook (my collection)
Data Analysis and Graphics Using R - By Example (my collection)

#setting graphical parameters
?par
opar <- par() # to save current parameters
par(opar)     # to restore them
par(mfrow=c(2,2)) # to make a figure composed of 2 by 2 sub-figures
din=c(4,4) #device size in inches w x h - you cannot set this
pin=c(4,4) #plot size in inches w x h - you can set this
fin=c(4,4) #figure size in inches w x h
one of the best desc of different graphical parameters in R is
http://research.stowers-institute.org/efg/R/Graphics/Basics/mar-oma/
#graphical parmas can also be reset by calling
dev.off()
#default margins
par(mar=c(5,4,4,2) +0.1)

#basic tutorial on making plots/figures/images, adding lines, legends, axes, margins and saving them to disk
http://www.harding.edu/fmccown/r/

making legends based on factor variables
trickier than you might think
note that factor variables cannot be cast as integers or numerics
instead use nlevels(someFactorVariable) which returns an integer
say you have a data frame with five points
theseData <- cbind(c(1,2,3,4,5), c(1,2,3,4,5)) 
say you have a boolean vector x indicating membership in some group
membership<-c(1,1,0,1,0)
membership.f <- factor(membership, labels=c("in_group", "not_in_group"))
#note the levels and mapping 
str(theseGroups)
# Factor w/ 2 levels "in_group","not_in_group": 2 2 1 2 1
#to avoid confusion, specify both levels and labels when making the factor variable - if you dont, R will unique and sort the membership vector before assigning labels to values so that 0,1 will get labels in_group and out_of_group respectively!!!
membership.f <- factor(membership, levels=c(1,0), labels=c("in_group", "not_in_group"))
plot(theseData, col=membership.f)
legend(x="topleft", legend=levels(membership.f), fill=1:nlevels(membership.f))








Saving plots
see http://www.stat.berkeley.edu/~s133/saving.html

if you have already made an image on screen, you can save it to disk using the R studio figure viewer export button or…
par(mar=c(5,4,4,2)+0.1, pin=(7,5)) #may have to play with these
dev.copy(pdf, 'filename.pdf')
#or
dev.copy(png, 'filename.pdf')
dev.off() # to flush the image to file


if you dont already have the figure and want to make it straight away to a file that can be imported into a talk
#make a pdf figure
png(file="./fig2.pdf", width=500, height=500)
plot(....)
dev.off()


#the equivalent command for making a pdf is
pdf(filename='figures/name_of_fig.png')
plot(...)
dev.off()



also look inside functions for parameter settings (heatmap has CexRow and cexCol that masks cex.axis)


Histogram

#histogram
x <- mtcars$mpg
hist(x, breaks=12, col="red")
rug(x)

#histogram with density line
x <- mtcars$mpg
hist(x, breaks=12, col="red", freq=F)
rug(x)
lines(density(x))

#histogram with bin midpoints labelled and freqs printed above bar
nbins <- 40
x<-spikes2GeneRatio.ord
histInfo <- hist(x, breaks=nbins, col="red", freq=F, xaxt='n')
text(x=histInfo$mids, y=histInfo$density, labels=histInfo$counts, pos=3, offset=0.1, cex=0.5)
axis(1, at=histInfo$mids, labels=histInfo$mids, las=2, cex.axis=0.5)
rug(spikes2GeneRatio.ord)
lines(density(spikes2GeneRatio.ord))


Density plot

#density plots
xd<-density(x)
plot(xd)
polygon(xd, col="red", border="blue")
abline(h=0.02)
abline(v=20)

# compare density distributions of some variable for different groups (levels)
install.packages("sm")
library(sm)
# create value labels 
cyl <- mtcars$cyl
mpg <- mtcars$mpg
cyl.f <- factor(cyl, levels= c(4,6,8), labels = c("4 cylinder", "6 cylinder", "8 cylinder")) 
# plot densities 
sm.density.compare(mpg, cyl, xlab="Miles Per Gallon")
title(main="MPG Distribution by Car Cylinders")
# add legend via mouse click
colfill<-c(2:(2+length(levels(cyl.f)))) 
legend(locator(1), levels(cyl.f), fill=colfill)

Scatter plot

#scatter plot
plot(x=1:10, y=(1:10)^2)
plot(x=1:10, y=(1:10)^2, type="p", col="red", main="My Graph", pch="*")
#try also type="l", "b", ?plot
plot(x=sample(1:10, 5), y=rep(1,5), type="h", col="red", main="My Graph", pch="*")
demo(graphics) - use this for a tutorial


Barplot
#barplot
tmp<-rbind(sumsERCC, sumsOrgGenes)
mid_points<-barplot(tmp, main="total counts from genes versus ERCC spike-ins for each sample", xlab="sample", ylab="total counts", col=c("darkblue", "red"), xaxt='n')
legend("bottomright", rownames(tmp), fill=c("darkblue", "red"), cex=0.7, inset=0.1)
axis(1, at=mid_points, labels=colnames(tmp), las=2, cex.axis=0.3)


 



Boxplot
boxplot(log2(samplesXgenes[,which(colnames(samplesXgenes)=="CXCL8")]+1) ~ isCluster1, ylab="log2 CXCL8 counts", xlab="isCluster1", outpch=NA)

Stripchart
x<-mtcars$mpg
stripchart(x, method="stack", xlab="x", pch=1, offset=1, cex=2)
also try method=jitter

how to add points to a boxplot using stripchart and jitter (jiggle)
x<-c(1,2,3,4,5,6,3,4,5,6)
isGroup1<-c(1,1,1,1,1,0,0,0,0,0)
boxplot(x~isGroup1, outpch=NA)
stripchart(x~isGroup1, vertical=TRUE, method="jitter", pch=20, col=c("red","blue"), add=TRUE)
#add labels to a few select points - note x, y inversion because of vertical above
text(x<-as.integer(isGroup1)+1, y<-x, labels=c("", "blip", "", "", "", "", "", "", "blap", "")


What do the whiskers mean in a box plot
MS excel whiskers show the maximum and minimum data points in the set (0 and 100th percentile) unless these data points fall outside of +/- 1.5 IQRs from the upper (75th percentile) or lower end (25th percentile) of the box; in these cases, the whiskers represent the highest and lowest data points that are within the 1.5 IQR boundaries. Anything beyond these boundaries is drawn as a 'Tukey' outlier point.

R
Compare this to the default R boxplot where the upper whisker represents the smaller of the max point in the data set or the demarcation of the 1.5 IQR + the 75th percentile.  See https://www.r-bloggers.com/about-boxplot/ and https://en.wikipedia.org/wiki/Box_plot.
From Wiki:
“… the bottom and top of the box are always the 25th and 75th percentile (the lower and upper quartiles, respectively), and the band near the middle of the box is always the 50th percentile (the median). But the ends of the whiskers can represent several possible alternative values…”
In R’s default boxplot{graphics} code,

upper whisker = min(max(x), Q_3 + 1.5 * IQR) 
lower whisker = max(min(x), Q_1 – 1.5 * IQR)

where IQR = Q_3 – Q_1, the box length.
So the upper whisker is located at the *smaller* of the maximum x value and Q_3 + 1.5 IQR, 
whereas the lower whisker is located at the *larger* of the smallest x value and Q_1 – 1.5 IQR.




Image
# Notice that image interprets the z matrix as a table of f(x[i], y[j]) values, so that the x axis corresponds to row number and the y axis to column number, with column 1 at the bottom, i.e. a 90 degree counter-clockwise rotation of the conventional printed layout of a matrix.

myMatrix <- matrix(rnorm(1e+05), 1000, 5)
thisDataFrame <- as.data.frame(myMatrix)
thisDataFrame[,3] <- 1
thisDataFrame[600:700,] <- 2
image(x=1:dim(thisDataFrame)[1], y=1:dim(thisDataFrame)[2], z=as.matrix(thisDataFrame) )

#you will also see this nomenclature to plot a data frame and turn it the right way round
image(t(thisDataFrame)[,nrow(thisDataFrame):1])
#yuck

#and how scalable is it
myMatrix <- matrix(rnorm(1e+05), 1000, 1000)
thisDataFrame <- as.data.frame(myMatrix)
thisDataFrame[,300:400] <- 1
thisDataFrame[600:700,] <- 2
image(t(thisDataFrame)[,nrow(thisDataFrame):1])

how to identify points on a scatter plot
plot(x, y) # scatterplot
identify(x, y, labels=row.names(mydata)) # identify points 
#click on the plot, press stop (or ESC), labels appear
#example, with a PCA plot
plot(project, main="PC1 versus PC2", col=colCode+4)
identify(project, labels=row.names(project), pos=2, col="red", cex=0.5)




adding labels to a plot
#in the case below, project consists of two columns taken to be the x and y param of text respectively
plot(project, main="a scatter-plot of some data frame")
text(project, labels=row.names(project), pos=2, col="red", cex=0.5)

working with visualizing graphs
try this
http://datastorm-open.github.io/visNetwork
https://cran.r-project.org/web/packages/visNetwork/vignettes/Introduction-to-visNetwork.html



how to identify outliers

there are a few methods, i have only used ESD
1.	ESD - anything beyond +/- 3sd from the mean
2.	IQR method - Q1-1.5xIQR is lower bound (aka Tukey outlier)

visually check with histogram and boxplots
also see outlier package
also see https://rexplorations.wordpress.com/2015/09/05/simple-outlier-detection-in-r/ and dixon.test() or chisq.out.test() 
could use this to scan across bins of a variable (use cut)

also see
http://www.itl.nist.gov/div898/handbook/eda/section3/eda35h.htm 

how to do a chi-squared test, hypergeometric test, fisher test

tbl<-table(isNonZero,isCluster1)
tbl

#chi-squared test - Choosing and using statistical tests p. 200
#the expected number in each cell should be greater than or equal to 1
#no more than 20% of the expected values should be less than 5
#only perform on frequency data - never on percentages or otherwise transformed data
#there should be a sample size of at least 50
chisq.test(tbl)

#hypergeometric test
?phyper
#to calculate the probability of obtaining the observed number of successes or more….
q=tbl[2,2]-1   # successes (white balls) in sample (taken from urn)-1
		    # note the minus 1 because of the wording of the
               # question
m=sum(tbl[,2]) # number white balls in urn
n=sum(tbl[,1]) # number of black balls in urn
k=sum(tbl[2,]) # size of sample drawn from urn
phyper(q,m,n,k,lower.tail=FALSE)
or
1-phyper(q,m,n,k,lower.tail=TRUE)
or
sum(dhyper(q+1,m,n,k))
or
1-sum(dhyper(0:q, m,n,k))




how to assess correlation

var - just a wrapper for cov
cov
cor




how to cluster data

hclust

knn 


correcting for multiple hypothesis testing

my notes on FDR - false discovery rate
http://irefindex.org/wiki/images/c/c4/Working_with_Gene_Lists_and_Over-representation_analysis.pdf
and also here for  an excel spreadsheet version of B-H correction
http://www.biostathandbook.com/multiplecomparisons.html 

a quick explanation that i am always forgetting

the corrected p-value is obtained by first sorting the uncorrected p-values least to greatest then applying:

p-value * number_of_tests / rank

to intuitively understand this, recognize that

p-value * number of tests 

is just the number the_number_of_expected_false_discoveries

and the rank can be thought of as the_number_of_items_we_have_looked_at_so_far

when proceeding from top to bottom on our sorted list of results

so now

	the_number_of_expected_false_discoveries
	________________________________________
	the_number_of_items_we_have_looked_at_so_far

is going to be our false discovery rate at that point in the list.

Note that there is a second step in the B-H process where you examine each of these corrected p-values and replace it with any p-values below it that are smaller - this step just forces the output of the process to be monotonic


how to make a heat map

given a set of samples S each with a set of measured properties P in an SxP matrix called M

0. consider normalization of the data (e.g. scale, log etc.)

1. use some distance metric to calculate a distance matrix DM for all pairwise combinations of samples:

DM <- dist(M, method="euclidean")
	other methods might be maximum, manhattan, canberra, binary, or
     minkowski

this will calculate the euclidean distance matrix but there are multiple other possibilities

DM <- cor(M, method="pearson") 
other methods might be kendall or spearman

note that DM will be a 

2. then make the heatmap of the distance matrix 

heatmap(DM)

or a little fancier

library("RColorBrewer")
hmcol = colorRampPalette(brewer.pal(10,"RdBu"))(256)
hmcol = rev(hmcol)
heatmap(DM, sym=TRUE, col=hmcol, distfun=as.dist)


more advanced heatmaps
more on advanced heatmaps (heatmap.2 and heatmap.3)
http://sebastianraschka.com/Articles/heatmaps_in_r.html 
https://www.biostars.org/p/18211/ 

if yo want a legend or more advanced annotation of columns and rows, dont use heatmap (use heatmap.2 instead)



RColorBrewer
making lots of colors - say 60 different ethnicities

http://moderndata.plot.ly/create-colorful-graphs-in-r-with-rcolorbrewer-and-plotly/ - for all of the ethnicities
library(RColorBrewer)
thisPalette = colorRampPalette(brewer.pal(11,"Set1"))(length(levels(sampleEthn)))


then in the plot statement use
col=thisPalette[sampleEthn]




how to do pca (principle component analysis)

given a set of samples S each with a set of measured properties P in an SxP matrix called M

0. measurements should be continuous variables (for categorical data, use MFA).  PCA is commonly used for genetic because it is assumed that the 0,1,2 encodings can be interpreted numerically under an additive gene-dosage model

1. scale/transform data where appropriate
a. log-transform http://stats.stackexchange.com/questions/164381/why-log-transforming-the-data-before-performing-principal-component-analysis 	
b. use co-variance or correlation matrix
http://stats.stackexchange.com/questions/53/pca-on-correlation-or-covariance 
2. find principal components
princ <- prcomp(M)
summary(princ) #to see contribution of each PC to variance
plot(princ) #to see a scree plot
3. examine a projection - make a biplot
     project <- predict(princ, newdata=(M))[,1:2]
	plot(project)
4. repeat above for diff PCs (replace 1:2 with c(1,3) for example)
5. examine loading (aka weighting) of first (second, third etc.) PCs
	thisOrder<-order(abs(princ$rotation[,1]))
	component_1 <- princ$rotation[thisOrder,1]
	component_1[1:40]
	plot(abs(component_1[1:40], xaxt="n")
	axis(1, at=1:40, labels=names(component_1)[1:40], las=2,    
        cex.axis=0.5)

really good tutorial on PCA

http://www.sthda.com/english/wiki/principal-component-analysis-in-r-prcomp-vs-princomp-r-software-and-data-mining 

how are SVD and PCA related(Jeff Leek) - https://youtu.be/KyaJLeTLZMo 

Centering, scaling, and transformations: improving the biological information content of metabolomics data
an intelligent discussion on whether to scale or transform biological data prior to PCA
http://www.ncbi.nlm.nih.gov/pmc/articles/PMC1534033/

should you scale or log transform before PCA?
PCA constructs orthogonal - mutually uncorrelated - linear combinations that (successively) explains as much common variation as possible. Actually, PCA can be done based on the covariance matrix as well as the correlation matrix, not only the latter one. Scaling the data matrix such that all variables have zero mean and unit variance (also known as "normalizing", "studentizing", "z-scoring"), makes the two approaches identical. This is because the covariance between two normalized variables *is* the correlation coefficient.

Should you log-transform?

If you have variables that always get positive numbers, such as length, weight, etc., and that shows much more variation with higher values (heteroscedasticity), a log-normal distribution (i.e., normal after log-transformation) might be a clearly better description of the data than a normal distribution. In such cases I would log-transform before doing PCA. Log-transforming that kind of variables makes the distributions more normally distributed, stabilizes the variances, but also makes your model multiplicative on the raw scale instead of additive. That is of course the case for all types of linear models, such as t-tests or multiple regression. That's worth a thought when you interpret the results.

Should you scale the data to mean = 0, var = 1?

This depends on your study question and your data. As a rule of thumb, if all your variables are measured on the same scale and have the same unit, it might be a good idea *not* to scale the variables (i.e., PCA based on the covariance matrix). If you want to maximize variation, it is fair to let variables with more variation contribute more. On the other hand, if you have different types of variables with different units, it is probably wise to scale the data first (i.e., PCA based on the correlation matrix).




imputing missing values
https://bioconductor.org/packages/release/bioc/manuals/impute/man/impute.pdf

for the purposes of data exploration and quick analysis, impute.knn can be used

#given some data matrix
dataMatrix
dataMatrix2 <- dataMatrix
#put some holes in it
dataMatrix2[sample(1:100, size=40, replace=F)] <- NA
#and then fill them back in with impute
library(impute)
dataMatrix3 <- impute.knn(dataMatrix2)$data


Creating a function: 

suma <- function(a,b){a+b}
suma(1,2)

suma2 <- function(a,b){print(a);print(b);a+b}
suma2(2,3)

#default parameter values
suma2 <- function(a,b=0){print(a);print(b);a+b}
suma2(a=2)


if statement:
if(1>0) { 
print ("hello")
}
else {
	#do nothing
}

for loop:
for (i in 1:5){print(i)}
for (i in list("a","b",TRUE)){print(i)}


Scripting:
Write commands to a text file and save as myScrpt.R
#lines beginning with a # are comments and are ignored by the R interpretor
To run the script:
source(file=myScript.R)


Debugging
traceback: prints out the function call stack after an error occurs; does nothing if there’s no error
debug: flags a function for “debug” mode which allows you to step through execution of a function one line at a time
browser: suspends the execution of a function wherever it is called and puts the function in debug mode
trace: allows you to insert debugging code into a function a specific places
recover: allows you to modify the error behavior so that you can browse the function call stack

Debugging segmentation faults
http://r.789695.n4.nabble.com/segfault-debugging-td4651490.html
using gdb
1.	add something to your script to get it to pause (or to wait for some file to be created, or some other signal), then figure out the process number using ps -aux
2.	attach gdb to this process id using: gdb --pid=PID 
3.	gdb will interrupt the process. restart it. 
4.	send the signal to your process to get out of its loop (see 1)
5.	hopefully see the segfault in action. 

using valgrind (only on linux)
1.	narrow error down to a few lines in a script
2.	R -d valgrind -f myscript.R

valgrind slows down evaluation a lot so you need a quick way (say less than 10's of seconds) of reproducing the bug 






Profiling code

Advanced R programming - profiling and optimization
http://adv-r.had.co.nz/ ***



Sys.time()
startTime <- Sys.time()
Sys.sleep(1)
endTime <- Sys.time()
difftime(startTime, endTime)
elapsedTime <- endTime - startTime
format(elapsedTime, format="%H:%M:%S")

system.time(<insert code here that you want to profile>)
returns: proc_time, user_time and wall_time. if user_time is > wall_time then CPU may be spending a lot of time waiting.  if user_time < wall_time your machine has multiple cores and is using them

multi-threaded libs include vecLib, ATLAS, ACML, mkl and parallel (a parallel processing lib)

Rprof()
example:
Rprof()
## some code to be profiled
Rprof(NULL) #stop profiling
output is automatically capture to Rprof.out (can be configured) which prints out call stack every 0.02 s (not very useful by itself
then type
summaryRprof() - this returns two objects
1)	$by.total - gives time spent in each function as a fraction of runtime
2)	$by.self - gives time spent in each function as a fraction of runtime but removes lower functions that it calls

output to a file



Bioconductor:
http://www.bioconductor.org/packages/release/Software.html




Bioconductor
Accessing the db 

library(GO.db)
help(package="GO.db")
help(GO.db)
ls("package:GO.db")
GO()
# Quality control information for GO:
# 
# 
# This package has the following mappings:
# 
# GOBPANCESTOR has 25193 mapped keys (of 25193 keys)
# GOBPCHILDREN has 14497 mapped keys (of 25193 keys)
# GOBPOFFSPRING has 14497 mapped keys (of 25193 keys)
# GOBPPARENTS has 25193 mapped keys (of 25193 keys)
# GOCCANCESTOR has 3232 mapped keys (of 3232 keys)
# GOCCCHILDREN has 1070 mapped keys (of 3232 keys)
# GOCCOFFSPRING has 1070 mapped keys (of 3232 keys)
# GOCCPARENTS has 3232 mapped keys (of 3232 keys)
# GOMFANCESTOR has 9602 mapped keys (of 9602 keys)
# GOMFCHILDREN has 1930 mapped keys (of 9602 keys)
# GOMFOFFSPRING has 1930 mapped keys (of 9602 keys)
# GOMFPARENTS has 9602 mapped keys (of 9602 keys)
# GOOBSOLETE has 1838 mapped keys (of 1838 keys)
# GOTERM has 38028 mapped keys (of 38028 keys)

GO_dbInfo()
#or
GO.db
# GODb object:
# | GOSOURCENAME: Gene Ontology
# | GOSOURCEURL: ftp://ftp.geneontology.org/pub/go/godatabase/archive/latest-lite/
# | GOSOURCEDATE: 20130907
# | Db type: GODb
# | package: AnnotationDbi
# | DBSCHEMA: GO_DB
# | GOEGSOURCEDATE: 2013-Sep12
# | GOEGSOURCENAME: Entrez Gene
# | GOEGSOURCEURL: ftp://ftp.ncbi.nlm.nih.gov/gene/DATA
# | DBSCHEMAVERSION: 2.1

GO_dbfile()
#1] "/Library/Frameworks/R.framework/Versions/3.0/Resources/library/GO.db/extdata/GO.sqlite"

GO_dbschema()
# CREATE TABLE metadata (
#   name VARCHAR(80) PRIMARY KEY,
#   value VARCHAR(255)
# );
# CREATE TABLE go_ontology (
#   ontology VARCHAR(9) PRIMARY KEY,               -- GO ontology (short label)  
#   term_type VARCHAR(18) NOT NULL UNIQUE          -- GO ontology (full label)
# );
# ...

3

#1. as a list indexed by GOIDs
xx <- as.list(GOTERM)
if(length(xx) > 0){
        # Get the TERMS for the first elent of xx
        GOID(xx[[1]]);
        Term(xx[[1]]);
        Synonym(xx[[1]]);
        Secondary(xx[[1]]);
        Definition(xx[[1]]);
        Ontology(xx[[1]]);
    }

head(names(xx))
xx$"GO:0000001"


recall

#get a list of current entrez gene ids
#using org.Hs.eg to get gene name and symbol from geneid
x <- org.Hs.egGENENAME;
# Get the gene names that are mapped to an entrez gene identifier
allCurrentGeneIds <- as.integer(mappedkeys(x));
head(allCurrentGeneIds);
# Convert to a list
geneid2name <- as.list(x[allCurrentGeneIds])
geneid2name$"5430"

#2. using the select method built into the underlying DBI interface
help(select)

columns(GO.db)
#[1] "GOID"       "TERM"       "ONTOLOGY"   "DEFINITION"

keytypes(GO.db)
[1] "GOID"       "TERM"       "ONTOLOGY"   "DEFINITION"

keys <- head( keys(GO.db) );
keys;

select(GO.db, keys=keys, columns=c("GOID", "TERM", "ONTOLOGY", "DEFINITION"), keytype="GOID");

#3. using dbGetQuery and regular expressions

liketok <- "complex"
gcon = GO_dbconn()
query <- paste("select go_id from go_term where term like '%", liketok, "%'", sep = "")
results <- as.character(dbGetQuery(gcon, query )[[1]])
details <- select(GO.db, keys=results, columns=c("GOID", "TERM", "ONTOLOGY"), keytype="GOID");
details <- details[details$ONTOLOGY == "CC",]
complexGoIds <- details$GOID



liketok <- "complex"
termlikeToTags = function (liketok) {
  require(GO.db)
  gcon = GO_dbconn()
  query <- paste("select go_id from go_term where term like '%", liketok, "%'", sep = "")
  results <- as.character(dbGetQuery(gcon, query )[[1]])
}


>>
>> if you want to use full regular expressions
>>
>> reToTags = function (retok, ...) {
>>     require(GO.db)
>>     allt = dbGetQuery(GO_dbconn(), "select  go_id, term from go_term")
>>     inds = grep(retok, as.character(allt[,"term"]), value=FALSE, ...)
>> # will error if value set at call
>>     if (length(inds)>0) return(as.character(allt[inds,"go_id"]))
>>     stop("retok did not grep to any term")
>> }
>>
>>> length(reToTags("transcription"))
>>
>> [1] 372
>>>
>>> length(termlikeToTags("transcription"))
>>
>> [1] 372
>>>
>>> length(reToTags("transcrip.*"))
>>
>> [1] 411
>>>
>>> length(termlikeToTags("transcrip.*"))
>>
>> [1] 0

I'd suggest you read the vignettes at
http://www.bioconductor.org/help/bioc-views/release/bioc/html/AnnotationDbi.html,
particularly 
http://www.bioconductor.org/packages/2.7/bioc/vignettes/AnnotationDbi/inst/doc/AnnotationDbi.pdf

http://www.bioconductor.org/packages/release/bioc/manuals/AnnotationDbi/man/AnnotationDbi.pdf

https://stat.ethz.ch/pipermail/bioconductor/2011-February/038100.html


BioMart
Working with biomaRt to retrieve and convert gene and protein accessions, names and identifiers.

start here, then follow the example code below:
http://www.ensembl.org/info/data/biomart/biomart_r_package.html
then
http://www.bioconductor.org/packages/release/bioc/vignettes/biomaRt/inst/doc/biomaRt.html#using-select 
and the full manual
http://www.bioconductor.org/packages/release/bioc/manuals/biomaRt/man/biomaRt.pdf

some example code

#set up biomaRt
source("http://bioconductor.org/biocLite.R");
biocLite("biomaRt");
library(biomaRt);
listMarts();

library(biomaRt)
mart<- useDataset("btaurus_gene_ensembl", useMart("ensembl"))

ensembl_genes<- c("ENSBTAG00000026199", "ENSBTAG00000014685") ## etc...

getBM(
  filters= "ensembl_gene_id", 
  attributes= c("ensembl_gene_id", "external_gene_id", "entrezgene", "description"),
  values= ensembl_genes,
  mart= mart)


# Note: if the function useMart runs into proxy problems you should set
# your proxy first before calling any biomaRt functions. You can do this using
# the Sys.putenv command:
# Sys.putenv("http\_proxy" = "http://my.proxy.org:9999")

#thisMart=useMart("unimart"); #abandoned because conversion from UniProt ID to Entrez Gene id was not supported
thisMart=useMart("ensembl"); 


listDatasets(thisMart);
thisMart = useDataset("hsapiens_gene_ensembl",mart=thisMart); 
#31          hsapiens_gene_ensembl             Homo sapiens genes (GRCh37.p13)      GRCh37.p13

#examine filters by hand for UniProt Ids - a fairly large list so grep is required
filters = listFilters(thisMart);
head(filters);
dim(filters);
filters;
filters[c(grep("uniprot", filters[,2], ignore.case = TRUE, perl = TRUE)), ];
#uniprot_swissprot
#uniprot_sptrembl
#accession

#examine attributes for gene ids - a fairly large list so grep is required
attributes = listAttributes(thisMart)
head(attributes);
dim(attributes);
attributes;
attributes[c(grep("uniprot", attributes[,2], ignore.case = TRUE, perl = TRUE)), ];
attributes[c(grep("trembl", attributes[,2], ignore.case = TRUE, perl = TRUE)), ];
attributes[c(grep("entrezgene", attributes[,2], ignore.case = TRUE, perl = TRUE)), ];
#uniprot_swissprot
#entrezgene

#compose an example query
#P24928, RPB1_HUMAN, 5430
theseUniprotIds=c("RPB1_HUMAN");
getBM(attributes=c("entrezgene", "uniprot_swissprot"), filters="uniprot_swissprot" , values=theseUniprotIds, mart=thisMart); #works






#######################################




Finding sample data distributed with packages
data() - lists all data sets available to you
data(package="somePackageName") - lists data sets distributed with somePackageName
data(USJudgeRatings) - loads data set called USJudgeRatings
names(USJudgeRatings) - returns all variable names used in USJudgeRatings data set


Descriptive statistics

x=1:20
sum(x)
mean(x)
cumsum(x)
range(x)
var(x)
sqrt(x)
sqrt (var(x)) - standard deviation
sd(x) - standard deviation




Regular expressions:
Try the following code and find out what is doing:
> grep("[a-z]", letters)
> txt <- c("arm", "foot", "lefroo", "bafoobar",
"gorilla", "armadillo", "far", "tate")
> i <- grep("foo",txt)
> txt[i]
> i=grep("^l.*",txt)
> txt[i]
> i=grep("^f[oa]",txt)
> txt[i]
> i=grep("^[fg][oa]",txt)
> txt[i]
> i=grep(".*o$",txt)
> txt[i]
> i=grep("^...o{2}b..$",txt)
> txt[i]

> txt <- c("The", "licenses", "for", "most", "software",
"are", "designed", "to", "take", "away", "your",
"freedom", "to", "share", "and", "change", "it.", "",
"By", "contrast,", "the", "GNU", "General", "Public",
"License", "is", "intended", "to", "guarantee", "your",
"freedom", "to", "share", "and", "change", "free",
"software", "--", "to", "make", "sure", "the",
"software", "is", "free", "for", "all", "its", "users")
> ot <- sub("[b-e]",".", txt)
> ot2 <- sub("[b-e]{2}","..", txt)



######################################



#probability distribution
x<-0:1
f<-dbinom(x, size=1, prob=.1)
plot(x,f,xlab="x",ylab="density",type="h",lwd=5)

set.seed(100)
x<-rbinom(100, size=1, prob=.1)
hist(x)

x<-seq(-4,4,0.1)
f<-dnorm(x, mean=0, sd=1)
plot(x,f,xlab="x",ylab="density",lwd=5,type="l")

x<-rnorm(100, mean=0, sd=1)
hist(x)

#quantiles
q90<-qnorm(0.90, mean = 0, sd = 1)
x<-seq(-4,4,.1)
f<-dnorm(x, mean=0, sd=1)
plot(x,f,xlab="x",ylab="density",type="l",lwd=5)
abline(v=q90,col=2,lwd=5)
#v means vertical

#descriptive statistics
x<-rnorm(100, mean=0, sd=1)
quantile(x)
quantile(x,probs=c(0.1,0.2,0.9))

x<-rnorm(100, mean=0, sd=1)
mean(x)
median(x)
IQR(x)
var(x)
sd(x)
summary(x)

#box plot
x<-rnorm(100, mean=0, sd=1)
boxplot(x)

#qqplot
x<-rnorm(100, mean=0, sd=1)
qqnorm(x)
qqline(x)

x<-rt(100,df=2)
qqnorm(x)qqline(x)
x<-seq(-4,4,.1)
f1<-dnorm(x, mean=0, sd=1)
f2<-dt(x, df=2)
plot(x,f1,xlab="x",ylab="density",lwd=5,type="l")
lines(x,f2,xlab="x",ylab="density",lwd=5,col=2)

x<-rnorm(100, mean=0, sd=1)
y<-rnorm(100, mean=0, sd=1)
qqplot(x,y)
x<-rt(100, df=2)
y<-rnorm(100, mean=0, sd=1)
qqplot(x,y)



Nice output to files - using kable

kable(thisDataFrame[ 1:10, 1:5 ])
#note explicit use of print required inside functions/loops - hours of fun
print(kable(thisDataFrame[ 1:10, 1:5 ]))


Nice output to files - using gridExtra

To print a dataframe as a table

library(gridExtra) 
pdf("data_output.pdf", height=11, width=8.5) grid.table(mtcars) 
dev.off()



If your data doesn't fit on the page, you can reduce the text size grid.table(mtcars, gp=gpar(fontsize=8))

adding a title

grid.arrange(tableGrob(mtcars, gp=gpar(fontsize=6)), main="Main Title Here.")


if rows dont fit on one page

library(gridExtra); 
maxrow = 30; 
npages = ceiling(nrow(iris)/maxrow); 
pdf("iris_pages.pdf", height=11, width=8.5); 
for (i in 1:npages) {
idx = seq(1+((i-1)*maxrow), i*maxrow); 
grid.newpage(); 
grid.table(iris[idx, ])
}; 
dev.off()

Using Rstudio and knitr

\documentclass{article} 
\begin{document} 
<<myTable,results='asis'>>= 
library(xtable) 
tab <- read.table(text = 'Date County Trade 1/1/2012 USA 5 1/1/2012 Japan 4 1/2/2012 USA 10 1/3/2012 Germany 15',header = TRUE) print(xtable(tab),hline.after=c(2,3)) 
## print.xtable have many smart options 
@ 
\end{document}


 
Using Rstudio and knitr - .Rmd (R markdown) document

#making a table of contents - add this to the beginning of the document
---
title: "someTitle"
author: "Ian Donaldson"
date: "dd/mm/yyyy"
output: 
  html_document:
    pandoc_args:
    - +RTS
    - -K64m
    - -RTS
    toc: yes
---


#setting global options for code chunk markup
`r opts_chunk$set(cache=TRUE, echo=TRUE, results="hide")`

#a code chunk looks like this
```{r codeChunkName1, echo=FALSE, cache=TRUE}

####
#set options for this code chunk in the brackets above
#http://yihui.name/knitr/options#chunk_options
####

#echo=FALSE -- if you want to hide source code
#cache=TRUE -- to cache results
#eval=FALSE -- if you dont want to run the code
#results='hide' --if you want to hide the output
#comment=NA     --to supress ## in front of output


#Rcode

```#end of code-chunk - these are back-ticks

#heading 1
##heading 2
###heading3

```{r codeChunkName2, fig.width=7, fig.height=10, results='hold', cache=TRUE}

#codeChunkNames must be unique in the document
#fig.width=7,     -- set the width of the figure 
#fig.height=10,   -- set the height of the figure
#results='hold'   -- dont print results until after printing and       
                     running all instructions in this code chunk

```

#to render rmarkdown from the commandline use:
rmarkdown::render("input.Rmd")


#to extract code from an .Rmd file use
purl("test.Rmd", output = "test2.R", documentation = 2)
https://www.rforge.net/doc/packages/knitr/knit.html
a more generalized approach that uses sed to extract any code from between ``` and ``` in any markdown file would be:
sed -n '/^```/,/^```/ p' < input.file | sed '/^```/ d' > output.file
You can find an explanation of how to use line ranges by pattern, and the 'p' and 'd' commands starting here:
http://www.grymoire.com/Unix/Sed.html#uh-29


Knitr resources
R markdown and Rstudio http://rmarkdown.rstudio.com/ 
Knit home http://yihui.name/knitr/ 
R markdown cheet sheet: http://www.rstudio.com/wp-content/uploads/2015/02/rmarkdown-cheatsheet.pdf 
Yihui Xie's knitr examples https://github.com/yihui/knitr-examples
Using knitr with bash and other languages: https://github.com/yihui/knitr-examples/blob/master/106-polyglot.Rmd
Other engines: python, perl, C++, bash
http://yihui.name/knitr/demo/engines/




 
Shiny Apps in 2 minutes

Start a new project in Rstudio of type shiny app
There will be two files ui.R and server.R
In ui.R you create the user interface.  User input is captured using *Input functions (like textInput or sliderInput).  An example is 

textInput(inputId="userSays", label="Enter a few words", value = "")

The inputId parameter is used to refer to this value in the server.R script.

Output is generated using *Output functions (like textOutput).  An example is

textOutput("iSay")

where "iSay" refers to a variable that is created in server.R

A simple server.R file could be


###
library(shiny)      
source("mylib.R")


shinyServer(function(input, output) {
 output$iSay<-renderText({predictNextWord(inputString=input$userSays, cutoff=5, all=FALSE)})
})

###

-	There is really only one function (shinyServer).
-	The output variable is called output$iSay
-	renderText is one of the render* functions used to create output content - these functions take one argument (an R statement enclosed in curly braces - so renderText({...})
-	predictNextWord is a function defined in mylib.R
-	input$userSays is a variable defined in ui.R
-	code outside of the shinyServer function will be executed once

if your app relies on a dataset, load it by including another file in the same directory called global.R with the following line

load("myData")

Test your app locally by typing: runApp()

Deploy your app by typing: 
install.packages('devtools')
devtools::install_github('rstudio/shinyapps')
library(shinyapps)
deployApp()

Where to go for further info on shiny apps:

I have a shinyio account here:https://www.shinyapps.io (use Google pasword to login).  My shiny apps are here
http://iandonaldson.shinyapps.io/wordPredict 
https://iandonaldson.shinyapps.io/shinyRegressionToy/ 
with code here
https://github.com/iandonaldson/shinyRegressionToy 
More functionality with shiny dashboard:
http://rstudio.github.io/shinydashboard/ 
Search for this book on it ebooks
Web Application Development with R Using Shiny 
Cool blog here:
http://www.joyofdata.de/blog/illustration-of-principal-component-analysis-pca/#more-1582


***Best brief tutorial is*** http://shiny.rstudio.com/articles/shinyapps.html

A longer tutorial is here
http://shiny.rstudio.com/tutorial/

Trying to make a different layout then start here:http://shiny.rstudio.com/articles/layout-guide.html and look at the gallery here: 

If you need more, dig around the articles, gallery and documentation here: http://shiny.rstudio.com/ 




 

##################################################################################################################

optimizing R
from http://www.noamross.net/blog/2013/4/25/faster-talk.html 
also see Advanced R programming - profiling and optimization
http://adv-r.had.co.nz/ ***




using R -studio server
http://www.noamross.net/blog/2013/4/25/faster-talk.html
The most user-friendly way to use cloud computing is to set up a server to run RStudio, which can then be accessed by a web browser and works just like it would on your desktop. Bioconductor has a step by step guide to starting an Amazon Web Instance that they’ve pre-configured this way. There are a couple of other useful guides to doing the same thing by Louis Aslett (including a video) and Karthik Ram. Read all of these and you should be able to set yourself up in an hour or two.

Parallel packages
various packages that do so e.g.multicore, snow,snowfall, doParallel,foreach, plyr,…

In practice, this means that you want to “parallelize” your code at the highest, or chunkiest level. If you have a series of nested loops, for instance, you will want to run the highest-level loop in parallel.

 limit the parallel processes to computational tasks, and avoid things like reading/writing to disk or downloading files

library(plyr)
p.flag=FALSE  # Change to TRUE if using parallel processing
.
.
.
 aaply(seq(1,10000,100), function(x) rnorm(1, mean=x+(1:100), 
      .parallel=p.flag)
Compiling functions
R has compiling ability, which can speed up functions by a factor or 3 or 4 in some cases. Compiling functions just requires calling cmpfun() in the base compiler package. Here’s an example:
library(compiler)
f <- function(n, x) for (i in 1:n) x = (1 + x)^(-1)
g <- cmpfun(f)

library(microbenchmark)
compare <- microbenchmark(f(1000, 1), g(1000, 1), times = 1000)

library(ggplot2)
autoplot(compare)

JIT auto-compiling

You can also enable just-in-time compiling in R, which will automatically compile EVERY function the first time that it is run. This will slow down R quite a bit at first, as each function must be compiled before it is run the first time, but but then speed it up later. Just add this to the start of your script
library(compiler)
enableJIT(1)
The numeric argument in enableJIT specifies the “level” of compilation. At level 2 and 3 more functions are compiled. In a few rare cases enableJIT will slow down code, particularly, if most of it is already pre-compiled or written in C, and/or if the code creates functions repeatedly, which then need to be compiled every time. This is more likely to happen with enableJIT(2) or enableJIT(3), though these have the potential to speed up code more, as well.


benchmarking
Traditionally, system.time(replicate()) could be used to time functions, but the microbenchmarkpackage is a better option for benchmarking individuals calls and functions. If you have identified a function that is called many times in your code and needs to be speeded up, you can write or try out several different versions, using microbenchmark to compare them.

profiling

Profiling examines your code to determine what parts of it are running slow. This is the essential tool for getting the best bang for your buck in speeding up code.
R’s code profiler samples the “call stack” at regular intervals. The call stack is the list of the current function running, the function that called it, and all the functions that those. By examining these samples, you can find which functions are taking up the largest amount of time, either because they are slow or are called many times.
You activate the profiler with Rprof("filename") which writes each sample of the call stack to “filename”. So do this:
Rprof("file.out")
Some_slow_code()
Rprof(NULL)
summaryRprof("file.out")
Rprof(NULL) stops sampling and summaryRprof("file.out") gives you a summary of the results. 
Here’s an example of profiling a random walk function (from a talk by Duncan Temple Lang):
rw2s1 <- function(n) {
    xpos = ypos = numeric(n)
    xdir = c(TRUE, FALSE)
    pm1 = c(1, -1)
    for (i in 2:n) if (sample(xdir, 1)) {
        xpos[i] = xpos[i - 1] + sample(pm1, 1)
        ypos[i] = ypos[i - 1]
    } else {
        xpos[i] = xpos[i - 1]
        ypos[i] = ypos[i - 1] + sample(pm1, 1)
    }
    list(x = xpos, y = ypos)
}

Rprof("out.out")
for (i in 1:1000) pos = rw2s1(1000)
Rprof(NULL)
summaryRprof("out.out")
## $by.self
##              self.time self.pct total.time total.pct
## "sample.int"      7.62    47.04       7.62     47.04
## "rw2s1"           5.28    32.59      16.20    100.00
## "sample"          2.90    17.90      10.52     64.94
## "-"               0.30     1.85       0.30      1.85
## "+"               0.04     0.25       0.04      0.25
## "list"            0.04     0.25       0.04      0.25
## "numeric"         0.02     0.12       0.02      0.12
## 
## $by.total
##              total.time total.pct self.time self.pct
## "rw2s1"           16.20    100.00      5.28    32.59
## "sample"          10.52     64.94      2.90    17.90
## "sample.int"       7.62     47.04      7.62    47.04
## "-"                0.30      1.85      0.30     1.85
## "+"                0.04      0.25      0.04     0.25
## "list"             0.04      0.25      0.04     0.25
## "numeric"          0.02      0.12      0.02     0.12
## 
## $sample.interval
## [1] 0.02
## 
## $sampling.time
## [1] 16.2
The two tables list the functions called. The $by.self component lists the time spent by functions alone, while the $by.total table lists the time spent by functions and all the functions they call. So you see that rw2s1, being the parent function, takes the most time in $by.total, but sample.int is the bottom-level function that is taking the most time.
I don’t find this summaryRprof() output very intuitive. There are some moderately better tools out there in the packages profr and proftool, but I don’t like them either. Instead I’ve written a custom function that you can get here called proftable. Here it parses the same output as above:
proftable("out.out")
##  PctTime Stack                      
##  47.037  rw2s1 > sample > sample.int
##  32.593  rw2s1                      
##  17.901  rw2s1 > sample             
##   1.852  rw2s1 > -                  
##   0.247  rw2s1 > +                  
##   0.247  rw2s1 > list               
##   0.123  rw2s1 > numeric            
## 
## Total Time: 16.2 seconds
Here you can see that >50% of time is being taken up by sample.int, which is itself usually called by sample.
Misc profiling things
●	The latest version of R (3.0.0) allows code profiling by line number using Rprof(file, line.profiling=TRUE) and summaryRprof(file, lines="show"). I don’t yet have a good example of it and haven’t incorporated this function into proftable() yet. There’s an example of use at Stack Overflow
●	Note that, depending on the R interface you are using, it may wrap your code in some other functions, which will appear as the topmost functions in the stack. For instance, if use the “Source” button in Rtudio, all your stacks will start with something like:
●	source > withVisible > eval > eval >
●	[Update 5/8/2003: The latest verion of proftable can deal with line numbers. It also deals with wrapper stacks like above.]
●	trace() can also be useful to track down where a particular slow function is being called. Try usingtrace(function, tracer=cat(as.character(sys.calls())), for instance, to print the stack whenever function is called.
●	R also has the ability to do memory profiling - tracking how your code uses memory. These includeRprofmem(), tracemem(), and Rprof(memory.profiling=TRUE), but the documentation for these isn’t great and I haven’t found good resources on this topic. Let me know if you can point me to some resources.
●	Jeromy Anglim has a blog post of an Rprof() case study that is worth reading


Vectorization
See Chapters 3 amd 4 of the R Inferno
Pre-allocate memory
R is bad at continually re-sizing objects, because it makes an extra copy of these objects each time. So if you have a loop that creates a vector or list, don’t append to the vector or list with each pass of the loop. Instead, make an empty object of the correct size first, then fill in its elements.

Using simpler data structures

Simpler data structures that only store one type of data can be manipulated much faster. If you can represent data in a matrix instead of a data frame (e.g., if you can reduce it to all numbers or characters), you can speed things up considerably.
For instance, compare these operations for getting the standard deviation of 100,000 rows of 10 variables in both a matrix and a data frame:
myMatrix <- matrix(rnorm(1e+05), 10000, 100)
myDF <- as.data.frame(myMatrix)
microbenchmark(apply(myMatrix, 2, sd), apply(myDF, 2, sd))
Standard and internal functions
A few standard functions in R are pretty slow, often because they perform error checking and methods dispatch before actually doing arithmetic. mean is among the most notorious of these. mean(x)does a lot more than justsum(x)/length(x). It checks the form of the inputs, assigns the appropriate method, then dispatches.Internal(mean(x)). The latter is much faster, as it is written in C:
x <- sample(1:100, 100, replace = TRUE)
comp <- microbenchmark(mean(x), sum(x)/length(x), .Internal(mean(x)), times = 1e+05)
comp

Other Languages: More reward for more effort
I’ve mentioned a few times that functions written in other languages, like C or C++, are much faster than other R functions. Obviously, learning another language is an undertaking, but thanks to some recent developments, writing R functions in C has become much easier. The Rcpp package lets you write such C++ functions without learning much about compiling or other peripheral issues, and Hadley Wyckham has written an excellent beginners’s guide to Rcpp that teaches you just enough C++ to get started.
###################################################################################################################






