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

$ conda install numpy scipy jupyter matplotlib pandas seaborn statsmodels

$ python -V
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
  


