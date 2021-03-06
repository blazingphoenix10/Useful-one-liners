# Recording useful one liners

## Contents
- diff
- grep and grepl
- apply, lapply, unlist
- sapply
- ls
- stopifnot
- match
- names
- split and cat
- do.call
- scale
- sweep
- model.matrix
- browser

## diff

Following is a linux command to look at differences between 2 files, 
helps a lot when you want to compare two different versions of a file

```
diff file1.txt file2.txt
```
It shows the changes that need to be made in file 1 to become file 2.
Or put another way, it shows the differences between file 1 and file 2, with respect to file 1. 

The format in which it shows the difference is as follows: >File1LineNumber{add (a),change (c), delete(d)}File2LineNumber
Eg: 1a2, means add line number two from file 2 after file 1 

## grep and grepl

grep returns indices of occurences of PATTERN in TEXT:
  ```r
    grep("virus", c("coronavirus", "rhinovirus", "cyanobacteria")
  ```

grepl indicates whether PATTERN occurs in TEXT as a logical vector
```r
  grepl("virus", c("coronavirus", "rhinovirus", "cyanobacteria")
```
## apply, lapply, unlist

Checking if the gene expression values conform to a normal distribution. If it does, then we can apply statisical tests such as t-test which assumes the sample is normally distributed. The function unlist is useful in extracting data from the list of lists.
```r
  s <- apply(gExpMatrix, 2, shapiro.test)
  s_pVal <- unlist(lapply(s, function(s) s$p.value))
  s_W <- unlist(lapply(s, function(s) s$statistic))

  s_pVal_adj <- p.adjust(s_pVal, "BH")
  (length(which(s_pVal_adj <= FDR))/length(s_pVal))*100
```
## sapply

 ```r
    sample_ids <- c("S108B355.BM_10_739","S109B355.BM_10_791","S112B394.BM_10_621")
    barcodes <- sapply(strsplit(sample_ids,"_"), function(x){x[3]})
  ```
  The above returns: 739, 791, 621
  
## ls(packageName)

ls(BRETIGEA)
Lists all the functions in a package.

## stopifnot(condition)

The program stops if the condition inside stopifnot is not evaluated to TRUE.

## match

 ```r
    x <- c("Chennai" ,"Bangalore", "Mumbai")
    y <- c("Bangalore", "Mumbai", "Chennai")
    match(x, y)
    match(y,x)
  ```
The above returns:
3, 1, 2
2, 3, 1

## names

 ```r
   t <- t.test(c(1,2), c(3,4))
   names(t)
   t$p.value
  ```
The above returns:
[1] "statistic"   "parameter"   "p.value"     "conf.int"    "estimate"    "null.value" 
[7] "stderr"      "alternative" "method"      "data.name"
[1] 0.1055728

## str

```{r}
a <- svd(matrix(rnorm(100), nrow = 25, ncol = 4))
str(a)
```
The above return a compact representation of the structure of the U, sigma, V matrices.

## split and cat

Uploading files of size > 1 GB to the server using remote access is difficult. So, instead large files can be split into smaller files, uploaded to server and then merged.

```
split -b 200m _CellRangerOutput_matrix.mtx cellRanger_segment.mtx
cat cellRanger_segment.mtx* > _CellRangerOutput_matrix.mtx
rm cellRanger_segment.mtx*
```

## do.call

```r
readBMfile <- function(fileName){
  file <- read.csv(fileName)
  row.names(file) <- file[,1]
  return(as.data.frame(t(file[,-1])))
}
bm <- do.call(rbind, lapply(list.files(pattern = "geneExp_corrected.csv"), readBMfile))
```
## scale
```r
x <- c(1, 2, 3, 4)
scaled_x <- scale(x)

scaled_x[1] == (x[1] - mean(x))/sqrt(var(x))
```

## sweep
```r
# The following is a function in the package CellCODE
# sweep is used when we want to perform row/column specific operations, as opposed to apply functions where all the rows and columsn are subject to the same operation.
svds <- function(data){
  mm = apply(data,1,mean) # per gene measure
  tmp = sweep(data,1,mm, "-") # mean subtraction
  svd(tmp)
}
```

## model.matrix
```r
sample_gp <- c("Dx", "Ctl", "Ctl", "Ctl", "Ctl", "Dx", "Dx", "Dx")
model.matrix(~1 + sample_gp)
```

## browser

browser() function is used for debugging. It can be called within a function, to explore the different local variables created within a function.
