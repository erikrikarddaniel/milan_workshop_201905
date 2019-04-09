# Brief instructions for the binning practical

## Installing MetaWRAP

## Installing CheckM and dependencies

Full installation instructions for CheckM can be found here:

https://github.com/Ecogenomics/CheckM/wiki/Installation

The above does not suggest Conda, but pip, which is another Python specific
way of installing programs. I suggest you use Conda instead. There are 
instructions for that here:

https://hackmd.io/cMQgeGF6TOuuLJvCjQyo0Q#

In brief, create a Conda environment for CheckM and install the necessary programs:

```
conda create -n checkm python=2.7
conda install checkm
```

### Download, unpack CheckM data and `setRoot`

CheckM's database needs to be downloaded and unpacked. It can be found here:

https://data.ace.uq.edu.au/public/CheckM_databases/

Currently, the latest release is from 2015, which I have downloaded and unpacked
in `/home/franzetti/data/checkm`. To avoid more downloads, you can use that copy.

You need to tell CheckM where you've put the data, using the `data setRoot` command:

```
checkm data setRoot ~franzetti/data/checkm/
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

```
checkm lineage_wf --tab_table -x fa --reduced_tree metabat2_bins/ metabat2_bins.checkm 2>&1 | tee metabat2_bins.checkm.out
checkm lineage_wf --tab_table -x fa --reduced_tree maxbin2_bins/ maxbin2_bins.checkm 2>&1 | tee maxbin2_bins.checkm.out
```

(Each set of bins took around 30 minutes on the VM.)

# Refining the bins with MetaWRAP

```
metawrap bin_refinement -o metawrap_refinment -m 30 --quick -A metabat2_bins/ -B maxbin2_bins/ 2>&1 | tee metawrap_refinment.out
```
