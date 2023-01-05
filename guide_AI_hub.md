# Using the ML-computers in MIEVU 4020

In this course we will use the computers from the [AI-hub project](https://www.uio.no/tjenester/it/forskning/kompetansehuber/uio-ai-hub-node-project/it-resources/ml-nodes/index.html) at UiO. This is a computing resource at UiO for both students and researchers. These computers allow for fast computations and they have the required software installed. 

These notes complements the [documentation](https://www.uio.no/tjenester/it/forskning/kompetansehuber/uio-ai-hub-node-project/it-resources/ml-nodes/index.html) of the AI hub. In addition this [gudie](https://github.com/aasmunkv/nam-shub-01) might be useful. It covers details on how to save some typing when using `ssh`, but it connects to a computer (nam-shub-01) at the Department of Mathematics, which you don't have access to, so all steps need to be adjusted if you want to follow this guide. 

## Logging in to the ML-computers
The AI hub has several 
To log in to this computer type
```
ssh [username]@ml2.hpc.uio.no
```
in the terminal (if you are a Windows user use a PowerShell). You will be asked to type your password at UiO to access the ml-computers. 

## Using the module system

The ML-computers have numerous users which all need different scientific software and different versions of the software. To facilitate this, the computers have installed a [module system](https://www.uio.no/tjenester/it/forskning/kompetansehuber/uio-ai-hub-node-project/it-resources/ml-nodes/module-system.html). This system allows users to load the specific software that they need. The four commands that we will need in this course are
```
module avail                 # List all the different modules
module avail [search phrase] # List all modules containing the search phrase
module load [module name]    # Load the module with the given name
module list                  # List all the modules you have loaded
module purge                 # Unload all modules
```
### Example 
For example, you can search for all modules starting with "pytorch"
by typing
```
[ml2 ~]$ module avail pytorch

-------------------- /software/easybuild/modules/all ---------------------
   PyTorch-bundle/1.7.0
   PyTorch-bundle/1.10.0-MKL-bundle-pre-optimised (D)
   PyTorch/1.6.0-fosscuda-2019b-Python-3.7.4
   PyTorch/1.7.0
   PyTorch/1.7.1-fosscuda-2020b
   PyTorch/1.8.1-foss-2020b
   PyTorch/1.8.1-fosscuda-2020b-imkl
   PyTorch/1.9.0-fosscuda-2020b                   (D)

  Where:
   D:  Default Module

Use "module spider" to find all possible modules and extensions.
Use "module keyword key1 key2 ..." to search for all possible modules
matching any of the "keys".


[ml2 ~]$ 
```

### Loading the software needed in MIEVU 4020
In MIEVU 4020 we will use the following modules (note that we start by unloading all modules)
```
module purge
module load JupyterLab/3.2.8-GCCcore-10.3.0 
module load PyTorch/1.12.0-foss-2022a-CUDA-11.7.0
module load torchvision/0.11.3-foss-2021a-CUDA-11.3.1
```
One can also install this software locally using, e.g., `pip` or `anaconda`. This is not recommended since packages installed this way will not necessarily be optimized for the underlying hardware. However, we do not have access to all the software we need via the module system. We, therefore, install some software locally using `pip`. 
Please install the following packages
```
pip install --user matplotlib, mlxtend, scikit-learn
```  
All software installed this way is located in `~/.local`. A crude way to uninstall these packages (if needed) is by deleting the directory `rm -R ~/.local`. 

## Monitoring resources

To check the CPU and memory usage type
```
htop
```
To exit, press `q`.

To check GPU usage, type
```
[ml2 ~]$ nvidia-smi
Wed Feb 16 11:54:48 2022       
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 465.19.01    Driver Version: 465.19.01    CUDA Version: 11.3     |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|                               |                      |               MIG M. |
|===============================+======================+======================|
|   0  NVIDIA GeForce ...  On   | 00000000:18:00.0 Off |                  N/A |
| 28%   31C    P8    20W / 250W |   1286MiB / 11019MiB |      0%      Default |
|                               |                      |                  N/A |
+-------------------------------+----------------------+----------------------+
|   1  NVIDIA GeForce ...  On   | 00000000:3B:00.0 Off |                  N/A |
| 28%   31C    P8    25W / 250W |   9592MiB / 11019MiB |      0%      Default |
|                               |                      |                  N/A |
+-------------------------------+----------------------+----------------------+
|   2  NVIDIA GeForce ...  On   | 00000000:86:00.0 Off |                  N/A |
| 41%   71C    P2   148W / 250W |   2793MiB / 11019MiB |     60%      Default |
|                               |                      |                  N/A |
+-------------------------------+----------------------+----------------------+
|   3  NVIDIA GeForce ...  On   | 00000000:AF:00.0 Off |                  N/A |
| 28%   31C    P8    18W / 250W |      1MiB / 11019MiB |      0%      Default |
|                               |                      |                  N/A |
+-------------------------------+----------------------+----------------------+
                                                                               
+-----------------------------------------------------------------------------+
| Processes:                                                                  |
|  GPU   GI   CI        PID   Type   Process name                  GPU Memory |
|        ID   ID                                                   Usage      |
|=============================================================================|
|    0   N/A  N/A   3055611      C   ...-GCCcore-8.3.0/bin/python     1283MiB |
|    1   N/A  N/A   4096438      C   ...GCCcore-10.2.0/bin/python     9589MiB |
|    2   N/A  N/A    675589      C   python                           1395MiB |
|    2   N/A  N/A    675667      C   python                           1395MiB |
+-----------------------------------------------------------------------------+
[ml2 ~]$ 

```
This will give you an overview of the current usage. To monitor the usage over time, use the `watch`. That is 
```
watch -n 2 nvidia-smi
```
This command will execute the `nvidia-smi` command every two seconds. 

## Selecting which GPU to use
The ml2 computer has four GPUs enumerated with the numbers 0,1,2,3. By default, you have access to all of them. In this couse, we will not consider models which need to be stored on more than one GPU. It can (sometimes) be challenging to select which GPU to use from within the code itself. The easiest way to choose which GPU to use is by setting the environment variable `CUDA_VISIBLE_DEVICES`. This can be done in a given session by typing
```
export CUDA_VISIBLE_DEVICES=3
```
Above we selected GPU number 3. When you start python after running this command, it will only see GPU number 3 (and it will be visible as GPU number 0).

An alternative way of selecting this GPU is by setting environment variable right before starting a given process (for example, python)
```
CUDA_VISIBLE_DEVICES=3 python
``` 
**Select a GPU where not all the memory is occupied, and do not occupy the GPU when it is not in use**.
 
## Start a Jupyter-lab notebook

We want to execute all our code on the ML-computers, but we want to edit the code (locally) in our browser. To do this, we use jupyter-lab notebooks.

To set this up we need two terminals. In the first terminal, we start a jupyter-lab notebook, which forward its content to a given port number. In the second terminal we will open a ssh connection listing to this port number. This is done as follows.
##### Terminal 1
```
[vegant ~]$ # In terminal 1
[vegant ~]$ ssh vegarant@ml2.hpc.uio.no
[ml2 ~]$ module purge
[ml2 ~]$ module load JupyterLab/2.2.8-GCCcore-10.2.0
[ml2 ~]$ module load PyTorch/1.9.0-fosscuda-2020b
[ml2 ~]$ CUDA_VISIBLE_DEVICES=3 jupyter-lab --no-browser --port=8089
[I 12:41:03.761 LabApp] Writing notebook server cookie secret to /itf-fi-ml/home/vegarant/.local/share/jupyter/runtime/notebook_cookie_secret
[W 12:41:03.964 LabApp] JupyterLab server extension not enabled, manually loading...
[I 12:41:03.967 LabApp] JupyterLab extension loaded from /storage/software/JupyterLab/2.2.8-GCCcore-10.2.0/lib/python3.8/site-packages/jupyterlab
[I 12:41:03.967 LabApp] JupyterLab application directory is /storage/software/JupyterLab/2.2.8-GCCcore-10.2.0/share/jupyter/lab
[I 12:41:03.969 LabApp] Serving notebooks from local directory: /itf-fi-ml/home/vegarant
[I 12:41:03.969 LabApp] Jupyter Notebook 6.1.4 is running at:
[I 12:41:03.970 LabApp] http://localhost:8089/?token=3dff2472f0b721c24c1d3901cd00529bf47bcd2fcf97b26b
[I 12:41:03.970 LabApp]  or http://127.0.0.1:8089/?token=3dff2472f0b721c24c1d3901cd00529bf47bcd2fcf97b26b
[I 12:41:03.970 LabApp] Use Control-C to stop this server and shut down all kernels (twice to skip confirmation).
[C 12:41:03.976 LabApp] 
    
    To access the notebook, open this file in a browser:
        file:///itf-fi-ml/home/vegarant/.local/share/jupyter/runtime/nbserver-951234-open.html
    Or copy and paste one of these URLs:
        http://localhost:8089/?token=3dff2472f0b721c24c1d3901cd00529bf47bcd2fcf97b26b
     or http://127.0.0.1:8089/?token=3dff2472f0b721c24c1d3901cd00529bf47bcd2fcf97b26b

```
##### Terminal 2
```
ssh -N -L 8089:localhost:8089 vegarant@ml2.hpc.uio.no
```
Running this command will open an ssh connection to the ml2 computer on port 8089. The command will hang there as long as the connection is open. To abort it, type `Ctrl+C`.

##### Open the notebook

To open the notebook, copy the link starting with `http://localhost` or `http://127.0.0.1` from terminal 1.

##### Important warning
Each user needs to **choose a port number that is not occupied**. In the guide above, we have used port number 8089. Hence at most one of you can use this port number.

Above, all commands are executed with the username `vegarant`. You need to replace this username with your own.

## Data
Data used in the course is located in the directory
```
/itf-fi-ml/shared/MIEVU4020
```

### Moving files to the ML-computers
To move files from your computer to one of the ML-computers, use the `scp` command.
That is 
```
scp /path/to/file.txt [username]@ml2.hpc.uio.no:
```
It is important to remember the `:` at the end of `[username]@ml2.hpc.uio.no:`.

To move files from the ML-computers to your personal computer, simply reverse the command.
```
scp [username]@ml2.hpc.uio.no:path/to/file.txt .
```
