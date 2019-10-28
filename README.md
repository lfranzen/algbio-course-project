# Algorithms in Bioinformatics course project

*Lovisa Franzén, lovisa.franzen@scilifelab.se*  


## Description    

This is a project work for the Algorithms in Bioinformatics course organized by Lukas Käll.  
  
All the code for this project was written in Python 3, and I used Jupyter Notebook to store my code and results. 
The notebook is called `main_analysis.ipynb` and is located in the `results/` directory, along with an exported .html file generated of the notebook. It can also be viewed in nbviewer here: https://nbviewer.jupyter.org/github/lfranzen/algbio-course-project/blob/master/results/main_analysis.ipynb  
  
A report containing a brief summary of the project outcome can be read in the report.pdf document located in the `doc/` folder.  
  
  
## Environment  
  
The conda environment that was used for this project ("abi_project") was exported as a yml file.  
To install this environment, one can run:  
  
```bash
$ conda env create -f environment.yml
```

To enable the environment in Jupyter Notebook one also needs to add: 

```bash
$ python -m ipykernel install --user --name abi_project
```
  
  
## Data  
  
For this project I analysed a RNA-seq dataset that is publicly available at GEO with accession number GSE83888 (<a href="https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE83888">link</a>), published by Barrette et al. in 2016.  




