---
title: "R on HPC Systems"
author: Thomas Girke
date: April 22, 2021
output: 
  ioslides_presentation:
    keep_md: yes
    logo: ./images/ucr_logo.png
    widescreen: yes
    df_print: paged
    smaller: true
subtitle: "Tutorial for R Users" 
bibliography: bibtex.bib
---

<!---
- ioslides manual: 
   https://bookdown.org/yihui/rmarkdown/ioslides-presentation.html

- Compile from command-line
Rscript -e "rmarkdown::render('R_for_HPC.Rmd'); knitr::knit('R_for_HPC.Rmd', tangle=TRUE)"
-->

<!---
  Note: following css chunks are required for scrolling support beyond slide boundaries
-->

<style>
slides > slide {
  overflow-x: auto !important;
  overflow-y: auto !important;
}
</style>

<style type="text/css">
pre {
  max-height: 300px;
  overflow-y: auto;
}

pre[class] {
  max-height: 300px;
}
</style>

<style type="text/css">
.scroll-300 {
  max-height: 300px;
  overflow-y: auto;
  background-color: inherit;
}
</style>

## How to Navigate this Slide Show?

<br/>

- This __ioslides__ presentation contains scrollable slides. 
- Which slides are scrollable, is indicated by a tag at the bottom of the corresponding slides stating: 

<p style='text-align: center;'> __[ Scroll down to continue ]__ </p>

- The following single character keyboard shortcuts enable alternate display modes of __ioslides__:
    - `f`: enable fullscreen mode
    - `w`: toggle widescreen mode
    - `o`: enable overview mode
    - `h`: enable code highlight mode
- Pressing Esc exits all of these modes. Additional details can be found [here](https://bookdown.org/yihui/rmarkdown/ioslides-presentation.html).

# Outline

- <div class="white">__Background__</div>
- Neovim-based IDE for R
- Parallel R with _batchtools_
- References

## R Language

<div class="columns-2">

### About

- Statistical environment and programming language ([CRAN](https://cran.r-project.org/)) widely used in academia and data science.
- Free and runs on all common operating systems
- Large ecosystem of extension packages, _e.g._ [Bioconductor](http://bioconductor.org) and [CRAN](https://cran.r-project.org/web/packages/index.html) 

### Working environments

- R console and basic Rguis
- [RStudio](https://rstudio.com/products/rstudio/features/<Paste>)
- Jupyter Notebooks and JupyterHub
- [Nvim-R-tmux](http://hpcc.ucr.edu/manuals_linux-cluster_terminalIDE.html): Neovim-based IDE for R
- [Emacs Speaks Statistics (ESS)](http://ess.r-project.org/)
- Many additional options: Rgedit, RKWard, Eclipse, Tinn-R, Notepad++, NppToR

<img title="r environments" src="images/rinterface.png" style="width:500px;"> 
</div>

## RStudio Server for Web-based HPCC Access

- Integrated development environment (IDE) for R. RStudio local GUI and RStudio Server is web-based.
- User access to RStudio Server on HPCC
    - The URL is: [https://rstudio.hpcc.ucr.edu](https://rstudio.hpcc.ucr.edu)
    - Username and password: same as for ssh access to HPCC

<div class="columns-2">

Some useful RStudio shortcuts:

- `Ctrl+Enter`: send code to R console
- `Ctrl+Alt+Enter`: send code to terminal
- `Ctrl+Shift+C`: comment/uncomment
- `Ctrl+1/2`: switch window focus

![](./images/rstudio.png)
</div>

## Nvim-R-Tmux: Terminal-based R Environment {.flexbox .vcenter}

URLs: [1. main page](https://github.com/jalvesaq/Nvim-R), [2. HPCC manual](http://hpcc.ucr.edu/manuals_linux-cluster_terminalIDE.html), [3. custom installs](https://gist.github.com/tgirke/7a7c197b443243937f68c422e5471899) 

<center><img title="Nvim-R-Tmux" src="https://raw.githubusercontent.com/jalvesaq/Nvim-R/master/Nvim-R.gif"></center>

Animated screenshot of Nvim-R (from [here](https://github.com/jalvesaq/Nvim-R))

## Advantages of Command-line UI {.flexbox .vcenter} 

- Knowledge of command-line interface is essential for working on a computer cluster efficiently 
- Main advantage: language agnostic approach that works with most computer languages
- Users of Emacs may want to consider using ESS instead 

# Outline

- Background
- <div class="white">__Neovim-based IDE for R__</div>
- Parallel R with _batchtools_
- References

## Introduction to Nvim-R-Tmux

- <b><font color="red">Note:</font></b> the content on the following slides is also available in this tutorial section [here](https://girke.bioinformatics.ucr.edu/GEN242/tutorials/linux/linux/#textcode-editors).
- The examples are assuming that users are logged into their HPCC cluster account, or are working on a system where vim/nvim is accessible via a terminal.
- The following introduces Nvim-R combined with Tmux. 
- Similar instructions are available in HPCC's Nvim-R-Tmux tutorial [here](http://hpcc.ucr.edu/manuals_linux-cluster_terminalIDE.html).
- Note: Nvim-R and Tmux are two separate tools that can be used independently or in combination, and both are useful for remote terminal work. 
- For simplicity, some of the following examples use Nvim-R without Tmux. Once users know the basics of both then it is trivial to combine them as needed. 


## Vim/Nvim Basics

The following opens a file (here `myfile.txt`) with nvim (or vim). This can be a new file or an existing one. 


```bash
nvim myfile.txt # for neovim (or 'vim myfile.txt' for vim)
```

### Modes

In Vim/Nvim there are three main modes: __normal__, __insert__ and __command__ mode. The most important commands for switching between the three modes are:

* `i`: switches from the normal to the insert mode. The latter is used for typing. 
* `Esc`: switches from the insert mode back to the normal mode.
* `:`: starts the command mode (from normal mode) at the bottom of the terminal window.

The cursor is moved with the arrow keys. In Nvim one can also enable mouse-based movements of the cursor. `Fn Up/Down` allows to page. In the following, all 
commands starting with `:` need to be typed in the command mode. All other commands are typed in the normal mode after pushing the `Esc` key. 

<p style='text-align: right;'> __[ Scroll down to continue ]__ </p>
<br/><br/>

### Important modifier keys to control Vim/Nvim

* `:w`: saves changes to file. If in editing mode, `Esc` needs to be pressed first.
* `:q`: quits file that has not been changed; use `q!` to quit file without saving changes.
* `:wq`: saves and quits file

### Useful resources for learning Vim/Nvim

* [Interactive Vim Tutorial](http://www.openvim.com)
* [Official Vim Documentation](http://vimdoc.sourceforge.net/)
* [HPCC Linux Manual](http://hpcc.ucr.edu/manuals_linux-basics_vim.html)

## Tmux for Managing Terminal Sessions

### What is Tmux?

- Tmux is a virtual terminal multiplexer providing re-attachable terminal sessions
- Advantage: work in a terminal session cannot get lost due to internet disruptions or even when switching computers
- Combined with the `Nvim-r` plugin it provides a flexible working environment for R 
- Users can send code from a script to the R console or command-line.
- On HPCC both Nvim-R and Tmux are pre-configured and easy to install 

<center><img title="Nvim-R" src="https://raw.githubusercontent.com/jalvesaq/Nvim-R/master/Nvim-R.gif" style="width:400px;"></center>

</br></br></br></br>

## Nvim-R-Tmux Configuration in HPCC User Accounts

Skip these steps if Nvim-R-Tmux is already configured in your account. Or follow the [detailed
instructions](https://gist.github.com/tgirke/7a7c197b443243937f68c422e5471899) to install Nvim-R-Tmux from scratch on your own system (_e.g._ laptop or computer).

__1.__ Log in to your user account on HPCC and execute on the command-line: 


```bash
install_nvimRtmux
```
__2.__ To enable the nvim-R-tmux environment, log out and in again.

__3.__ Follow usage instructions of next section.

## Typical Usage Workflow for Nvim-R-Tmux


__1. Start tmux session from login node (not compute node!)__

Running Nvim from tmux provides reattachment functionality. Skip this step if this is not required.


```bash
tmux # starts a new tmux session 
tmux a # attaches to an existing or preconfigured session 
```

__2. Open nvim-connected R session__ 

Open a `*.R` or `*.Rmd` file with `nvim` and initialize a connected R session
with `\rf`. Note, the resulting split window among Nvim and R behaves like a split
viewport in `nvim` or `vim` meaning the usage of `Ctrl-w w` followed by `i` and
`Esc` is important for session navigation.


```bash
nvim myscript.R # or *.Rmd file
```

__3. Send R code from nvim to the R pane__

Single lines of code can be sent from nvim to the R console by pressing the space bar. To send 
several lines at once, one can select them in nvim's visual mode and then hit the space bar. 

<p style='text-align: right;'> __[ Scroll down to continue ]__ </p>
<br/><br/>

- Please note, the default command for sending code lines in the nvim-r-plugin is `\l`. This key 
  binding has been remapped in the provided `.config/nvim/init.vim` file to the
  space bar. Most other key bindings (shortcuts) still start with the `\` as
  LocalLeader, _e.g._ `\rh` opens the help for a function/object where the cursor
  is located in nvim. More details on this are given on the next slide(s).
- The most comprehensive manual on this is the official `Nvim-R` documentation [here](https://github.com/jalvesaq/Nvim-R/blob/master/doc/Nvim-R.txt).

## Keybindings to Control Environment

### Important keybindings for nvim

* `\rf`: opens vim-connected R session. If you do this the first time in your user account, you might be asked to create an `R` directory under `~/`. If so approve this action by pressing `y`. 
* `spacebar`: sends code from vim to R; here remapped in `init.vim` from default `\l`
* `:split` or `:vsplit`: splits viewport (similar to pane split in tmux)
* `gz`: maximizes size of viewport in normal mode (similar to Tmux's `Ctrl-a z` zoom utility) 
* `Ctrl-w w`: jumps cursor to R viewport and back; toggle between insert (`i`) and command (`Esc`) mode is required for navigation and controlling the environment.
* `Ctrl-w r`: swaps viewports
* `Ctrl-w =`: resizes splits to equal size
* `:resize <+5 or -5>`: resizes height by specified value

<br/><br/>
<p style='text-align: right;'> __[ Scroll down to continue ]__ </p>
<br/><br/><br/><br/>

* `:vertical resize <+5 or -5>`: resizes width by specified value
* `Ctrl-w H` or `Ctrl-w K`: toggles between horizontal/vertical splits
* `Ctrl-spacebar`: omni completion for R objects/functions when nvim is in insert mode. Note, this has been remapped in `init.vim` from difficult to type default `Ctrl-x Ctrl-o`. 
* `:h nvim-R`: opens nvim-R's user manual; navigation works the same as for any Vim/Nvim help document
* `:Rhelp fct_name`: opens help for a function from nvim's command mode with text completion support
* `Ctrl-s and Ctrl-x`: freezes/unfreezes vim (some systems)


### Important keybindings for tmux

__Pane-level commands__

* `Ctrl-a %`: splits pane vertically
* `Ctrl-a "`: splits pane horizontally
* `Ctrl-a o`: jumps cursor to next pane
* `Ctrl-a Ctrl-o`: swaps panes
* `Ctrl-a <space bar>`: rotates pane arrangement
* `Ctrl-a Alt <left or right>`: resizes to left or right
* `Ctrl-a Esc <up or down>`: resizes to left or right

__Window-level comands__

* `Ctrl-a n`: switches to next tmux window 
* `Ctrl-a Ctrl-a`: switches to previous tmux window
* `Ctrl-a c`: creates a new tmux window 
* `Ctrl-a 1`: switches to specific tmux window selected by number

__Session-level comands__

* `Ctrl-a d`: detaches from current session
* `Ctrl-a s`: switch between available tmux sessions
* `$ tmux new -s <name>`: starts new session with a specific name
* `$ tmux ls`: lists available tmux session(s)
* `$ tmux attach -t <id>`: attaches to specific tmux session  
* `$ tmux attach`: reattaches to session 
* `$ tmux kill-session -t <id>`: kills a specific tmux session
* `Ctrl-a : kill-session`: kills a session from tmux command mode 

## Use Same Environment for Other Languages

### Basics

For languages other than R one can use the
[vimcmdline](https://github.com/jalvesaq/vimcmdline) plugin for nvim (or vim).
Supported languages include Bash, Python, Golang, Haskell, JavaScript, Julia,
Jupyter, Lisp, Macaulay2, Matlab, Prolog, Ruby, and Sage. The nvim terminal
also colorizes the output, as in the screenshot below, where different colors
are used for general output, positive and negative numbers, and the prompt
line.

<center><img title="vimcmdline" src="https://cloud.githubusercontent.com/assets/891655/7090493/5fba2426-df71-11e4-8eb8-f17668d9361a.png" ></center>
<center>vimcmdline</center>

### Install

To install it, one needs to copy from the `vimcmdline` repository the directories
`ftplugin`, `plugin` and `syntax` and their files to `~/.config/nvim/`. For
user accounts of UCR’s HPCC, the above install script `install_nvimRtmux` includes the 
install of `vimcmdline` (since 09-Jun-18).

### Usage

The usage of `vimcmdline` is very similar to `nvim-R`. To start a connected terminal session, one
opens with nvim a code file with the extension of a given language (_e.g._ `*.sh` for Bash or `*.py` for Python), 
while the corresponding interactive interpreter session is initiated
by pressing the key sequence `\s` (corresponds to `\rf` under `nvim-R`). Subsequently, code lines can be sent 
with the space bar. More details are available [here](https://github.com/jalvesaq/vimcmdline). 

## Nvim-R Demo 

To try out the following instructions, users want to log into their HPCC
account via `ssh`, and then preferentially connect to a node by initializing an
interactive `srun` session. The latter mimics the best practices for a real workflow 
but is not necessary for this basic exercise.


```bash
srun --x11 --partition=short --mem=2gb --cpus-per-task 4 --ntasks 1 --time 1:00:00 --pty bash -l
```

- Under `--partition` it is important to assign the name of a partition a user has access to 
    - Most users have access to: `short`, `batch`, `intel` and `highmem` 
    - Users of labs owning computer nodes also can access: `<pi_name>lab` 
- For more details on argument settings for `srun`, see [here](http://hpcc.ucr.edu/manuals_linux-cluster_jobs.html)

Download `R_for_HPC_demo.R` file to you HPCC account as follows.  


```bash
wget https://raw.githubusercontent.com/tgirke/GEN242/main/static/custom/slides/R_for_HPC/demo_files/R_for_HPC_demo.R
```

<p style='text-align: right;'> __[ Scroll down to continue ]__ </p>
<br/><br/>

Open `nvim_demo.R` with nvim. The content of this file is shown in the following code 
block. Next, initialize a Nvim-connected R session with `\rf`, and then execute the 
code by pressing the space bar on your keyboard. 


```r
library(tidyverse)                                                                                                                                                            
write_tsv(iris, "iris.txt") # Creates sample file                                                                                                                             
read_tsv("iris.txt") %>% # Import with read_tbv from readr package                                                                                                            
    as_tibble() %>%                                                                                                                                                           
    group_by(Species) %>%                                                                                                                                                     
    summarize_all(mean) %>%                                                                                                                                                   
    reshape2::melt(id.vars=c("Species"), variable.name = "Samples", value.name="Values") %>%                                                                                  
    ggplot(aes(Samples, Values, fill = Species)) +                                                                                                                            
    geom_bar(position="dodge", stat="identity")
```

<p style='text-align: right;'> __[ Scroll down to continue ]__ </p>
<br/><br/><br/><br/>

If X11 is enabled in a user session then the above code will generate the following bar plot in a separate graphics window.

![](R_for_HPC_files/figure-html/nvim-r-tmux-demo_run-1.png)<!-- -->

## Selecting R Versions on HPCC

- Like many other software tools, R versions are managed under HPCC's module system. 
- To use a specific R version in Nvim-R, one simply loads it prior to starting Nvim. Instructions
  for enabling additional R version toggle options are provided [here](https://gist.github.com/tgirke/7a7c197b443243937f68c422e5471899#rversion_toggle).

Which R versions are available can be listed with the following command.


```bash
module avail R
```

The version labeled `default` is used by default. A specific R version can be loaded as follows.


```bash
module load R/4.0.1
```

Check which modules (including R) are loaded in a user's environment.


```bash
module list
```


# Outline

- Background
- Neovim-based IDE for R
- <div class="white">__Parallel R with _batchtools_ __</div>
- References

## Parallel Evaluations in R 

- <b><font color="red">Note:</font></b> the content on the following slides is also available in this tutorial section [here](https://girke.bioinformatics.ucr.edu/GEN242/tutorials/rparallel/rparallel/).
- R provides a large number of packages for parallel evaluations on multi-core, multi-socket and multi-node systems. The latter are usually referred to as computer clusters.
- MPI is also supported
- For an overview of parallelization packages available for R see [here](https://cran.r-project.org/web/views/HighPerformanceComputing.html)
- One of the most comprehensive parallel computing environments for R is
  [`batchtools`](https://mllg.github.io/batchtools/articles/batchtools.html#migration). Older versions of this package were released under the name `BatchJobs` [@Bischl2015-rf]. 
- `batchtools` supports both multi-core and multi-node computations with and without schedulers. By making use of
  cluster template files, most schedulers and queueing systems are supported (_e.g._ Torque, Sun Grid Engine, Slurm). 
- The `BiocParallel` package (see [here](https://bioconductor.org/packages/release/bioc/html/BiocParallel.html)) 
  provides similar functionalities as `batchtools`, but is tailored to use Bioconductor objects.

## Reminder: Traditional Job Submission for R 

This topic is covered in more detail in the basic Linux/HPC tutorial [here](https://girke.bioinformatics.ucr.edu/GEN242/tutorials/linux/linux/#queuing-system-slurm). 
Briefly, the following shows how to submit a script for precessing to the computing nodes. 

__1.__ Create Slurm submission script, here called [script_name.sh](https://raw.githubusercontent.com/tgirke/GEN242/main/static/custom/slides/R_for_HPC/demo_files/script_name.sh) with:


```bash
#!/bin/bash -l
#SBATCH --nodes=1
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=1
#SBATCH --mem-per-cpu=1G
#SBATCH --time=1-00:15:00 # 1 day and 15 minutes
#SBATCH --mail-user=useremail@address.com
#SBATCH --mail-type=ALL
#SBATCH --job-name="some_test"
#SBATCH -p short # Choose queue/partition from: intel, batch, highmem, gpu, short

Rscript my_script.R
```

__2.__ Submit R script called [my_script.R](https://raw.githubusercontent.com/tgirke/GEN242/main/static/custom/slides/R_for_HPC/demo_files/my_script.R) by above Slurm script with:


```bash
sbatch script_name.sh
```

## Parallel Evaluations on Clusters with `batchtools` 

- The following introduces the usage of `batchtools` for a computer cluster
  using SLURM as scheduler (workload manager). SLURM is the scheduler used by
  the HPCC at UCR.
- Similar instructions are provided this tutorial section covering
  `batchtools`
  [here](https://girke.bioinformatics.ucr.edu/GEN242/tutorials/rparallel/rparallel/)
- To simplify the evaluation of the R code on the following slides, the
  corresponding text version is available for download from
  [here](https://raw.githubusercontent.com/tgirke/GEN242/main/static/custom/slides/R_for_HPC/demo_files/R_for_HPC_demo.R).

## Hands-on Demo of `batchtools`

### Set up working directory for SLURM
First login to your cluster account, open R and execute the following lines. This will
create a test directory (here `mytestdir`), redirect R into this directory and then download
the required files: 

+ [`slurm.tmpl`](https://github.com/ucr-hpcc/ucr-hpcc.github.io/blob/master/presentations/2020-12-18_Workshop/R_for_HPC/demo_files/slurm.tmpl)
+ [`.batchtools.conf.R`](https://github.com/ucr-hpcc/ucr-hpcc.github.io/blob/master/presentations/2020-12-18_Workshop/R_for_HPC/demo_files/.batchtools.conf.R)


```r
dir.create("mytestdir")
setwd("mytestdir")
download.file("https://bit.ly/3gZJBsy", "slurm.tmpl")
download.file("https://bit.ly/3nvSNHA", ".batchtools.conf.R")
```

### Load package and define some custom function

The following code defines a test function (here `myFct`) that will be run on the cluster for demonstration
purposes. 

<p style='text-align: right;'> __[ Scroll down to continue ]__ </p>
<br/><br/>

The test function (`myFct`) subsets the `iris` data frame by rows, and appends the host name and R version of each
node where the function was executed. The R version to be used on each node can be
specified in the `slurm.tmpl` file (under `module load`).


```r
library('RenvModule')
module('load','slurm') # Loads slurm among other modules

library(batchtools)
myFct <- function(x) {
    Sys.sleep(10) # to see job in queue, pause for 10 sec
	result <- cbind(iris[x, 1:4,],
    Node=system("hostname", intern=TRUE),
	Rversion=paste(R.Version()[6:7], collapse="."))
}
```

### Submit jobs from R to cluster

The following creates a `batchtools` registry, defines the number of jobs and resource requests, and then submits the jobs to the cluster
via SLURM.


```r
reg <- makeRegistry(file.dir="myregdir", conf.file=".batchtools.conf.R")
Njobs <- 1:4 # Define number of jobs (here 4)
ids <- batchMap(fun=myFct, x=Njobs) 
done <- submitJobs(ids, reg=reg, resources=list(partition="short", walltime=120, ntasks=1, ncpus=1, memory=1024))
waitForJobs() # Wait until jobs are completed
```

### Summarize job status 
After the jobs are completed one can inspect their status as follows.


```r
getStatus() # Summarize job status
showLog(Njobs[1])
# killJobs(Njobs) # # Possible from within R or outside with scancel
```

### Access/assemble results

The results are stored as `.rds` files in the registry directory (here `myregdir`). One
can access them manually via `readRDS` or use various convenience utilities provided
by the `batchtools` package.


```r
readRDS("myregdir/results/1.rds") # reads from rds file first result chunk
loadResult(1) 
lapply(Njobs, loadResult)
reduceResults(rbind) # Assemble result chunks in single data.frame
do.call("rbind", lapply(Njobs, loadResult))
```

### Remove registry directory from file system

By default existing registries will not be overwritten. If required one can explicitly
clean and delete them with the following functions. 


```r
clearRegistry() # Clear registry in R session
removeRegistry(wait=0, reg=reg) # Delete registry directory
# unlink("myregdir", recursive=TRUE) # Same as previous line
```

### Load registry into R 

Loading a registry can be useful when accessing the results at a later state or 
after moving them to a local system. 


```r
from_file <- loadRegistry("myregdir", conf.file=".batchtools.conf.R")
reduceResults(rbind)
```

## Conclusions

### Nvim-R-Tmux

- Steeper learning curve than GUI-based IDEs, including RStudio or Jupyter Notebooks
- However, it is much more
    - powerful, flexible, robust and language agnostic solution for working on remote systems
    - time learning it is well invested, especially for students and researchers with complex data analysis and programming needs

### Advantages of `batchtools`

- many parallelization methods multiple cores, and across both multiple CPU sockets and nodes
- most schedulers supported
- takes full advantage of a cluster
- robust job management by organizing results in registry file-based database
- simplifies submission, monitoring and restart of jobs 
- well supported and maintained package

# Outline

- Background
- Neovim-based IDE for R
- Parallel R with _batchtools
- <div class="white">__References__</div>


## References 

