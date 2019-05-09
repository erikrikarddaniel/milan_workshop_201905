# Brief instructions for the binning practical

## Installing MetaWRAP

MetaWRAP is a wrapper for several useful programs for assembly-based annotation of
metagenomes. Installing MetaWRAP will also install other programs (MEGAHIT, MetaBAT2
MaxBin, CheckM etc.) that are useful, so installing MetaWRAP is a convenient way
of getting access to the individual programs.

Moreover, you will have to the option of using the binning consolidation module in
MetaWRAP, which is provided in MetaWRAP itself.

Follow the instructions for "Better installation" at the MetaWRAP GitHub page:

https://github.com/bxlab/metaWRAP

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
checkm lineage_wf --tab_table -x fa --reduced_tree binning.d/metabat2_bins/ metabat2_bins.checkm 2>&1 | tee metabat2_bins.checkm.out
checkm lineage_wf --tab_table -x fa --reduced_tree binning.d/maxbin2_bins/ maxbin2_bins.checkm 2>&1 | tee maxbin2_bins.checkm.out
```

(Each set of bins took around 30 minutes on the VM.)

Compare the output from CheckM for the two binning programs!

# Refining the bins with MetaWRAP

In contrast to the binning, MetaWRAP can run CheckM in small memory mode in the bin_refinement module
by adding the --quick option.

```
metawrap bin_refinement -o metawrap_refinment -m 30 --quick -A metabat2_bins/ -B maxbin2_bins/ 2>&1 | tee metawrap_refinment.out
```

(The above took around two hours on the VM.)
