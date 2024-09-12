## NSCC Cheatsheet
A workflow cheatsheet for NSCC AI cluster.
From submitting job using [tmux](https://github.com/tmux/tmux) , running experiments, to downloading results.
#### Submit Job
Note that you have to submit job inside tmux, otherwise the job will be terminated when you lose nscc connection(very likely).
Tmux:
```bash
# 1 GPU 24 hrs
qsub -I -l walltime=24:00:00 -P 12003885 -q ai -l select=1:ncpus=128:ngpus=1:mem=512G -M {your email address} -m bea
# 2 GPUs 72 hrs
qsub -I -l walltime=72:00:00 -P 12003885 -q ai -l select=1:ncpus=128:ngpus=2:mem=512G -M {your email address} -m bea
```

#### Job ready
Then update below `PBS_JOBID` and GPU Number for `ssh`.
Tmux:
```bash
PBS_JOBID={jobid} # e.g. 7951875.pbs101
echo $PBS_JOBID 
cd /raid/pbs.$PBS_JOBID/
pwd

source /home/project/12003885/miniconda3/bin/activate
conda activate cvcl
module load git
module load cuda
module load gcc

git clone {your repo url}

```

#### Modify Your Experiment Settings
Detach Tmux, ssh GPU to modify your settings.

```bash
PBS_JOBID={jobid} ssh {gpu server} # e.g. asp2a-gpu006
cd /raid/pbs.$PBS_JOBID
```

#### Run Experiments
Then, back to server where have tmux(e.g. `asp2a-login-ntu01`)
```bash
exit # if you directly ssh to the GPU from Tmux
ssh {your tmux query server}
tmux ls
tmux a
```

#### Download results
Tmux:
```bash
# Move Exp Results
mv /raid/pbs.$PBS_JOBID/ {your project | personal folder}

# Copy
scp -r ...

# Sychronizing 
rsync -avz --progress ...
```
