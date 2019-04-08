# Brief instructions for the binning practical

## Installing MetaWRAP

### Set CheckM data root

Set CheckM's data root to `/home/franzetti/data/checkm`, with this command:

```
checkm data setRoot
```

## Running the binning

### The actual binning step with metaBAT2 and maxBin2

```
metawrap binning -a assembly.fna -o binning.d --metabat2 --maxbin2 --run-checkm *.fastq 2>&1 | tee binning.out
```
