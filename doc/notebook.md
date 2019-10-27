# Notebook for Algorithms in Bioinformatics course project  
  
## 2019-10-09, Wednesday  
  
**Morning - Project proposal**  
  
I formulated a project proposal for analysing a public data set containing RNA-seq data from treated and untreated fetal lung explant samples. 
The project would include initial data formatting, differential gene expression analysis, pathway analysis of the top genes, and finally the creation of a pathway network.  
   
I sent the project proposal to Lukas and later got it approved.  
  
  
**Afternoon – Initiation of a project directory and git repo**  
  
I set up the basic structure for the project, including initiation of a git repo and a conda environment. Basic python modules needed  were also installed:

```bash
$ conda create --name abi_project python=3.7

$ source activate abi_project

(abi_project) $ conda install numpy scipy jupyter matplotlib pandas seaborn statsmodels

(abi_project) $ python -V
Python 3.7.4

```
  
More modules will be needed to be installed later on.  
  
  
Data for the project was downloaded from ArrayExpress, E-GEOD-83888, and placed in the data/ directory.  
  
  
## 2019-10-10, Thursday  
  
**13:00 – Set up Jupyter Notebook for main analysis**  
  
Make the 'abi_project' conda environment available in Jupyter:  

```bash
$ python -m ipykernel install --user --name abi_project
```

I initiated a jupyter notebook within the 'abi_project' conda evironment called `main_analysis.ipynb` (placed in the `results/` dir), where the main analysis will be kept.  
  
I added the code for reading the needed data files and wrote some background information about the project. 
Did some basic visualisation of the data to get an idea of what it looks like. 
Also started to look into how to implement differential gene expression analysis, since the popular edgeR or DESeq2 mehtods are only available in R.  
  
  
## 2019-10-11, Friday  
  
**Afternoon – Look into DEA alternatives**
  
I couldn't find a good stand-alone python implementation for DEA, however there is the possibility to import the DESeq2 algorithm from R into python using the `rpy2` module in python. 
I found a python implementation of written by the GitHub user *wckdouglas*, and the script is available here:<a href="https://gist.github.com/wckdouglas/3f8fb27a3d7a1eb24c598aa04f70fb25#file-py_deseq-py">`py_deseq.py`</a>.

An example of how to use this script is available <a href="https://github.com/wckdouglas/diffexpr/blob/master/example/deseq_example.ipynb">here</a>.  
  
  
I started to work on my python analysis script by creating a `sample_table` dataframe which I can run together with the expression data in the DEA.  
   
  
Before proceeding, I also need to make sure that rpy2 is installed in my conda env:  
  
```bash
$ source activate abi_project
(abi_project) $ conda install rpy2
```

## 2019-10-14, Monday  

**15:30 – Continued to work with DEA**  
  
The python script for performing DESeq2 that I found last week requires some dependencied to be installed such as 'tzlocal' and the R installation of DESeq2. 
Thus, I needed to make sure that it was installed in the 'abi_project' conda env. 

I installed 'tzlocal' from within the conda env using `conda install -c conda-forge tzlocal`.  
  
To install DESeq2, I tried to do so by opening up an R session in the terminal from within the conda env, and then installed "BiocManager" followed by "DESeq2". 
It takes a while to install it, since it in turn requires several other packages to be installed...  
  
  
```bash
(abi_project) $ R --version
R version 3.6.1 (2019-07-05) ...
...

(abi_project) $ R
```

```R
# Install DESeq2
$ install.packages("BiocManager")
$ BiocManager::install("DESeq2")

# Exit R
$ q()
```

However, after having done so, it still doesn't seem to have installed. 
Instead, I therefore tried to install it via conda / bioconda / bioconductor by the following running `(abi_project) $ conda install -c bioconda bioconductor-deseq2`. 
This one also takes quite a while to finish, and ended with an error ("An error occurred while uninstalling package 'defaults::qt-5.9.7-h468cd18_1'", "PermissionError(1, 'Operation not permitted')", and "Operation not permitted: '/anaconda3/envs/abi_project/bin/Assistant.app/Contents/PkgInfo' -> '/anaconda3/envs/abi_project/bin/Assistant.app/Contents/PkgInfo.c~'").  
  
It seemed to be some permission error, especiallt with the \*.app files, so I tried to reinstall all those files by running `conda update --all`, and then rerunning installation of deseq2. 
However, it is still not working after this.  
  
I'll look into writing my own basic DEA script based on what we did at the course.  
   
  
## 2019-10-15, Tuesday  
  
**10:30 – Started writing my own version of a DEA**  

Had some issues with the q-value calculation function and had to spend quite some time to debug it. 
After restarting the jupyter notebook, it seemed to work. 
Don't know what went wrong... 
I realised now that it seems some lists that I used as input were formatted as characters and not as numbers. 
Now fixed. 
  
  
Added a simple log2 fold change calculation to the dea() function.  

Ran the DEA on the data (genes with only 0-counts removed). 
The lowest q-value was still pretty high: 0.098... 
May be due to the high number of samples, but it still feels a bit odd to have such high q-values. 
Some of the regular t-test p-values were pretty low though, so not sure what happened there.

I then proceeded to make a few plots to also have a closer look at the results. 
The p-value distribution did follow. 
  
One big difference between my crude DEA and DESeq2 is that I do not include shrinkage for fold-change estimation, meaning that fold changes for genes with low expression might appear very high even though the actual group difference in expression levels might only be a few counts. 
This is something that is dealt with very well in DESEq2. 
They also omit genes when calculating the FDR:  

> Multiple testing adjustment tends to be associated with a loss of power, in the sense that the FDR for a set of genes is often higher than the individual P values of these genes. However, the loss can be reduced if genes that have little or no chance of being detected as differentially expressed are omitted from the testing, provided that the criterion for omission is independent of the test statistic under the null hypothesis [22] (see Materials and methods). DESeq2 uses the average expression strength of each gene, across all samples, as its filter criterion, and it omits all genes with mean normalized counts below a filtering threshold from multiple testing adjustment. 
 
 
**17:00 – Filtering of genes**
  
Since the q-values I obtained were generally very high, I thought that maybe I'm including too many genes in the analysis. 
At first I only removed the genes which had a 0-count across all samples, but after looking in the article more closely I saw that they removed genes with a sample sum ≤ 5 counts. 
For what I'm doing, I could be even stricter with my filtering and I therefore applied a filter to remove genes qith a sum of ≤ 20 counts, resulting in a total of 12918 genes after filtering. 
  
The lowest q-value after filtering is now 0.067541.
  
  
## 2019-10-23, Wednesday  
  
**10:00 – DEGs and pathways**  
 
First I need to identify DEGs based on some cut-offs. 
The DEA results provided a table of statistics from the t-tests, q-values, and log2 fold-chage values. 
None of the q-values obtained was particularly low. 
Instead I will filter my genes to obtain DEGs based on the p-values and the log2 fold-change. 
  
For my initial tests, I will apply the following cutoffs:  
p_cutoff = 0.01  
fc_cutoff = log2(1.5) = 0.585...  
  
This gave me a list of 406 DEGs. 
The highest q-value among these genes is 0.1, so I'll need to keep into consideration that there is a high possibility of having some false positive genes among the DEGs.
  
  
Next, I need to identify which pathways that my DEGs belong to. 
I found a python module called `mygene` that seems to be able to provide me with what I want. 
  
To install the module I will call `conda install -c bioconda mygene` within my conda env.  
  
I started by extracting both KEGG and Reactome pathways for each genes. 
It seemed that the Reactome pathways covered more of the genes, while KEGG pathways were missing for some of them. 
To simplify the results, I therefore decided to only look at Reactome pathways.  
  
A few of the genes (7) were not at all found in the mygene database, and I decided to remove them from the analysis. 

From the 406 input DEGs, I ended up with 399 genes after filtering that were found in the mygene database.
And out of them, 232 genes had pathway annotations. 
These were the genes that I used to create an adjacency matrix out of.  
  
  
**Afternoon – Create adjacency matrix and initiate NetworkX**  
  
I identified all unique pathways present among my genes, and created a numpy array filled with zeros that have the same dimensions as the number of unique pathways. 
Thereafter I populated the matrix by going through each pathway and assigning +1 for each pathway-pathway coordinate in the matrix. 
The pathway-pathway comparisons are made within each gene entry, so therefore the +1 will imply the number of genes in which that particular pathway-pathway connection is made.  
  
Also, I annotated the number of genes for which each unique pathway is found within. 
I'm hoping that this number can be used to control the size of the node in the network.  
    
  
Next I needed to install networkx.  
Install NetworkX in python with conda: `conda install -c anaconda networkx`  
  
  
## 2019-10-24, Thursday  

**10:00 – Build the network**  
  
Started to have a look at networkx and if it'd be possible to build a network using the input matrix that I made. 
It seemed to be working! 
However, I'll need to trim the network a bit beforehand, because currently the network has a size of 17661 and is shaped like a strange eye with high node density as an ellipse and then a small subnetwork densly packed in the center.  
  
Will have to continue with this tomorrow since I have to work on some other things now.  
  
  
## 2019-10-25, Friday!  
  
**13:00 – Networking time**  
  
I realized that the adjacency matrix that I initially worked with was 2587x2587, which is for the total number of pathways identified from the gene list, and not the number of unique pathways. 
Therefore the input for the netowork was quite a bit off, since it included multiple entries for the same pathway and not one node per unique pathway.  
  
I fixed the creation of the adjacency matrix so that it is now using the list of unique pathways instead, resulting in a final matrix with a dim of 856x856. 

The plotted network looks a lot better now! 
Also adding the weight of the edges as edge widths and the number of genes per node as node size resulted in a nicer graph. 
Ideally I'd also like to color my nodes based on some pathway categorization, since that would make it easier to for instance spot a cluster of inflammation-related pathways.  
  
To make the graph interactive, my first-hand choice would be to see if the Plotly module could be of use here. 
I found a tutorial for it here: https://plot.ly/python/network-graphs/  
  
I did find another python library though that might be a quicker and easier way for me to visualize the data interactively. 
That module is called 'mpld3' and can be installed using: `conda install -c conda-forge mpld3`  

The way to implement what I want seems to be described here: https://stackoverflow.com/questions/33988130/interactive-labels-on-nodes-using-python-and-networkx  
  
  
## 2019-10-27, Sunday  
  
The mpld3 didn't want to work with my networkx object. The error I got said: "Object of type ndarray is not JSON serializable". I found a thread on StackOverflow (https://stackoverflow.com/questions/47380865/json-serialization-error-using-matplotlib-mpld3-with-linkedbrush) discussing the error and I therefore tested the following:

Add `import networkx` to `/anaconda3/envs/abi_project/lib/python3.7/site-packages/mpld3/_display.py`
  
and,  
  
Add  
`elif isinstance(obj,networkx.classes.reportviews.NodeView):
    return list(obj)`  
to the `default` function in class `NumpyEncoder` to make it work (also in `_display.py`)
  
  
However this didn't seem to work either, so I tried another solution from the same stackoverflow thread which involved installing a fix to the mpld3 module:

`python -m pip install --user "git+https://github.com/javadba/mpld3@display_fix"`

And this seemed to solve the issue, at least partially.

  
However I still couldn't get the interactive labels to work (using the tooltip plugin), and thethe error I got now showed that actually the mpld3 module being loaded is located in `~/.local/lib/python3.7/site-packages/mpld3/`, and not in the conda env. 
This was probably changed when I did the pip install of the fix. 
  
Therefore I decided to test the addition of the `elif` statement written out above in the `_diplay.py` file located in the local dir (`~/.local/lib/python3.7/site-packages/mpld3/_display.py`). ~/.local/lib/python3.7/site-packages/mpld3/`
Now it seems to work! :)  
  

**General improvements and bug fixes**  
  
I identified an error in the flattening of 'pw_ids' to 'pw_ids_flat' in the sense that the way I flattened the nested list ("[item for sublist in pw_ids for item in sublist]") couldn't handle single instances without instead breaking the string of the single instance apart. 
Instead, I now found another custom function to flatten the nested list that could handle mixing of different formats. 
So, instead of 2587 pathways, I now ended up with 2576, resulting in 846 unique pathways instead of 856.  
This fix caused a downstream issue in the creation of the adjacency matrix. 
  
  
In the identify_pathway() function, I had to add a line to replace "'on'" with 'on' (without quotes) since in one of the pathways this was a case and it was disrupting the conversion of the string to a dictionary (which relies on double-quotation marks). This will continue to work as long as there are no other such instances, which could appear if I choose to do the analysis with more genes.  
  
  
**Improve network and look at network metrics**  
I made some crude pathway classifications by simply grepping on certain keywords from the pathway descriptions. 

I got the interactive plots to work, so that they display the pathway ID and name when you hover over the node.





