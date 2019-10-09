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
  

