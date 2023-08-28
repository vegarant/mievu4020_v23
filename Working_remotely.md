# Working remotely

In this course, we have access to four computers:

* `baal`
* `lie`
* `metis`
* `wabun`

These can be accessed remotely via `ssh`. To do this, type the following in the terminal.

```
ssh -J UIO-USER-NAME@login.math.uio.no  UIO-USER-NAME@COMPUTER-NAME.uio.no
```
Replace `UIO-USER-NAME` with your username and `COMPUTER-NAME` with one of the computers above.

## Start a Jupyter-lab notebook

To work remotely with Jupyter-lab notebooks, we need to use two terminals. 

In the first terminal, we start a jupyter-lab notebook, which forwards its content to a given port number. In the second terminal we will open an ssh connection listening to this port number. This is done as follows.
##### Terminal 1
```
[vegant ~]$ ssh -J vegarant@login.math.uio.no vegarant@baal.uio.no
Last login: Fri Jan  6 11:11:50 2023 from login-math-rhel9-prod01.uio.no
[baal ~]$ 
[baal ~]$ module purge 
[baal ~]$ module load JupyterLab/3.0.16-GCCcore-10.3.0
[baal ~]$ module load PyTorch/1.11.0-foss-2021a-CUDA-11.3.1 
[baal ~]$ module load torchvision/0.11.3-foss-2021a-CUDA-11.3.1
[baal ~]$ 
[baal ~]$ jupyter-lab --no-browser --port=8089
[I 2023-01-06 11:12:28.119 ServerApp] jupyterlab | extension was successfully linked.
[I 2023-01-06 11:12:28.192 LabApp] JupyterLab extension loaded from /opt/uio/modules/rhel8/easybuild/software/JupyterLab/3.0.16-GCCcore-10.3.0/lib/python3.9/site-packages/jupyterlab
[I 2023-01-06 11:12:28.192 LabApp] JupyterLab application directory is /mn/sarpanitu/modules/rhel8/easybuild/software/JupyterLab/3.0.16-GCCcore-10.3.0/share/jupyter/lab
[I 2023-01-06 11:12:28.196 ServerApp] jupyterlab | extension was successfully loaded.
[I 2023-01-06 11:12:28.196 ServerApp] Serving notebooks from local directory: /mn/sarpanitu/ansatte-u4/vegarant
[I 2023-01-06 11:12:28.196 ServerApp] Jupyter Server 1.9.0 is running at:
[I 2023-01-06 11:12:28.196 ServerApp] http://localhost:8089/lab?token=29b210cea7ed9d9849b103ff441fdb75e4e864df4bac7a70
[I 2023-01-06 11:12:28.197 ServerApp]  or http://127.0.0.1:8089/lab?token=29b210cea7ed9d9849b103ff441fdb75e4e864df4bac7a70
[I 2023-01-06 11:12:28.197 ServerApp] Use Control-C to stop this server and shut down all kernels (twice to skip confirmation).
[C 2023-01-06 11:12:28.204 ServerApp] 
    
    To access the server, open this file in a browser:
        file:///mn/sarpanitu/ansatte-u4/vegarant/.local/share/jupyter/runtime/jpserver-672017-open.html
    Or copy and paste one of these URLs:
        http://localhost:8089/lab?token=29b210cea7ed9d9849b103ff441fdb75e4e864df4bac7a70
     or http://127.0.0.1:8089/lab?token=29b210cea7ed9d9849b103ff441fdb75e4e864df4bac7a70
```

```
##### Terminal 2

ssh -L 8089:localhost:8089 -J vegarant@login.math.uio.no vegarant@baal.uio.no
```
Running this command will open an ssh connection to the ml2 computer on port 8089. 

##### Open the notebook

To open the notebook, copy the link starting with `http://localhost` or `http://127.0.0.1` from terminal 1.

##### Important warning
Each user needs to **choose a port number that is not occupied**. In the guide above, we have used port number 8089. Only one user can occupy this port at the same time. 

Above, all commands are executed with the username `vegarant`. You need to replace this username with your own.

### Moving files to the computers
To move files from your computer to one of the computers, use the `scp` command.
That is 
```
scp /path/to/file.txt UIO-USER-NAME@login.math.uio.no:
```
It is important to remember the `:` at the end of `UIO-USER-NAME@login.math.uio.no:`.

To move files from the computers to your personal computer, simply reverse the command.
```
scp UIO-USER-NAME@login.math.uio.no:path/to/file.txt .
```


