# Getting started

In this guide we cover how to set up and install the software needed for the course MIEVU 4020 on the university's computers. We will be working on the computers at the Department of Mathematics, so all instructions are tailor-made for these computers. 

## Using the module system

Our workstations have numerous users who all need different scientific software and different versions of the same software. To facilitate this the computers have installed a [module system](https://www.mn.uio.no/math/english/services/it/help/user-environment.html). This system allows users to load the specific software that they need. The four commands that we will need in this course are
```
module avail                 # List all the different modules
module avail [search phrase] # List all modules containing the search phrase
module load [module name]    # Load the module with the given name
module list                  # List all the modules you have loaded
module purge                 # Unload all modules
```

### Example -- Searching for a module

You can search for all modules starting with "torchvision"
by typing

```
[baal ~]$ module avail torchvision

------------------------ /opt/uio/modules/rhel8/easybuild/modules/vis ------------------------
   torchvision/0.3.0-foss-2019a-Python-3.7.2
   torchvision/0.7.0-fosscuda-2019b-Python-3.7.4-PyTorch-1.6.0-imkl
   torchvision/0.7.0-fosscuda-2019b-Python-3.7.4-PyTorch-1.6.0
   torchvision/0.8.1-fosscuda-2019b-Python-3.7.4-PyTorch-1.7.0-imkl
   torchvision/0.8.1-fosscuda-2019b-Python-3.7.4-PyTorch-1.7.0
   torchvision/0.8.2-fosscuda-2020b-PyTorch-1.7.1
   torchvision/0.11.3-foss-2021a-CUDA-11.3.1                        (D)

  Where:
   D:  Default Module

Use "module spider" to find all possible modules.
Use "module keyword key1 key2 ..." to search for all possible modules matching any of the
"keys".


[baal ~]$ 
```

### Loading the software needed in MIEVU 4020
In MIEVU 4020 we will use the following modules (note that we start by unloading all modules)
```
module purge 
module load JupyterLab/3.0.16-GCCcore-10.3.0
module load PyTorch/1.11.0-foss-2021a-CUDA-11.3.1 
module load torchvision/0.11.3-foss-2021a-CUDA-11.3.1
```
One can also install this software locally using, e.g., `pip` or `anaconda`. This is not recommended since packages installed this way will not necessarily be optimized for the underlying hardware. That being said, we still need to install some packages using `pip` to get all the versions to work together.
 
Install the following packages.
```
pip install --user matplotlib mlxtend scikit-learn
```  
All software installed this way is located in `~/.local`. A crude way to uninstall these packages (if needed) is by deleting the directory `rm -R ~/.local`. 

You need to run the `module load ...` commands above every time you open a new terminal, but you do not need to install the packages with `pip` every time. 

## Start a Jupyter-lab notebook

After loading all the modules, you can start a jupyter-lab notebook.
```
[baal ~]$ jupyter-lab 
```

## Monitoring resources

To check the CPU and memory usage type
```
htop
```
To exit, press `q`.
 

