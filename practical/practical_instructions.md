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
metawrap binning -a assembly.fna -o binning.d --metabat2 --maxbin2 *.fastq 2>&1 | tee binning.out
```

(The above took a little bit more than an hour for me on the VM.)

Running CheckM -- an important part of binning because it evaluates completeness and contamination --
is possible to do automatically with metawrap binning, but requires more memory than you have on the
VMs (>=40 GiB). There's a smaller memory option, but it's not exposed by MetaWRAP, so let's run CheckM
manually.
