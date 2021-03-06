# TissueMiner FAQ


### How do I run snakemake with a project specific configuration file instead of `default_config.R`?

* For running [TissueMiner API](https://mpicbg-scicomp.github.io/tissue_miner/user_manual/TM_R-UserManual.html#tissueminer-api-configuration)
Export a shell variable named `TM_CONFIG` pointing to the config file you would like to use. E.g. to use `config/flywing_tm_config.R`, before running snakemake, just do
```  
export TM_CONFIG=$TM_HOME/config/flywing_tm_config.R
```


* For running [TissueMiner snakemake workflow](https://mpicbg-scicomp.github.io/tissue_miner/user_manual/TM_R-UserManual.html#tissueminer-snakemake-automated-workflow)
    + Ubuntu: `export TM_CONFIG=$TM_HOME/config/flywing_tm_config.R`
    + Docker: defined my_config.R in the movie repository and run the command below
    ```
    alias tm='docker run --rm -ti -v $(dirname $PWD):/movies -w /movies/$(basename $PWD) -u rstudio etournay/tissue_miner'
    tm "export TM_CONFIG=/movies/my_config.R; sm all"
    ```

### What kind of data are required to run TissueMiner ?

TissueMiner uses the cell tracking information from TissueAnalyzer, which implies that cell tracking must have been done in TissueAnalyzer.
Two masks - tracked_cell_resized.tif and cell_division.tif - from TissueAnalyzer are read by TissueMiner. These 2 masks are required.

In addition, the user must provide the cumulative time (in seconds) of the movie, in a file called cumultimesec.txt

### Can the cell segmentation procedure be done with another tool ?

Yes, it can. Any segmentation tool that would produce a binary segmentation mask can be used in TissueAnalyzer for further cell tracking.
Only the tracking step in TissueAnalyzer is currently required for TissueMiner to work.

### Is TissueAnalyzer part of TissueMiner ?

TissueMiner works **downstream of** TissueAnalyzer. Therefore TissueAnalyzer is included in the TissueMiner framework.


### How to change the number of digits used for padding ?
Edit the tm.snkmk file located in the *workflow* folder, and find and replace %03d by the desired padding (ex: %04d for 4 digits).

### How to update my TissueMiner installation?


On the docker plateform (Linux, MacOs, Windows) simply pull latest image again with
```
docker pull etournay/tissue_miner
```

On ubuntu simply fetch the changes by
```
cd  ${TM_HOME} && git pull origin && cd parser && make clean all
```


### How to force `git pull origin` in case of conflict ?

Caution: if you do this, you may lose all of your local changes in tissue_miner

```
git fetch --all
git reset --hard origin/master
```

