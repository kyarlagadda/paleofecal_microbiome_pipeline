# paleofecal_microbiome_pipeline
## Shotgun metagenomic analytical pipeline

This pipeline was made to analyze shotgun microbial read data, including OTU assignment, functional profiling, diversity assessments, discriminant analyses, and pathogen identification. It assumes that reads have already been pre-processed as per [this pipeline](https://github.com/kelsey-witt/diet-taxonomy-pipeline). 

## Required software

* [biobakery](https://github.com/biobakery/biobakery)
  * [KneadData](https://github.com/biobakery/biobakery/wiki/kneaddata)
  * [MetaPhlAn](https://github.com/biobakery/biobakery/wiki/metaphlan2)
  * [HUMAnN](https://github.com/biobakery/biobakery/wiki/humann2)
  * [LEfSe](https://github.com/biobakery/biobakery/wiki/lefse)
* [R](https://www.r-project.org/)
 * [SourceTracker](https://github.com/danknights/sourcetracker)
 * [phyloseq](https://joey711.github.io/phyloseq/)

Note that MetaPhlAn and HUMAnN have more updated versions than what is used here (version 3 as opposed to 2) that are generally functional in the described pipeline below, but may require a few additional tweaks to work exactly (i.e. MetaPhlAn 3 has additional lines in the profile output header that need to be trimmed for the downstream programs).

## Pipeline

### Filtering

Reads are first filtered against a human microbiome database ([KneadData](https://github.com/biobakery/biobakery/wiki/kneaddata) links to one option) as well as laboratory or environmental contaminants as appropriate. This can be done by creating a custom index as specified in [KneadData](https://github.com/biobakery/biobakery/wiki/kneaddata), and then filtering all samples against that index.

### Profiling

Profiling is done in [MetaPhlAn](https://github.com/biobakery/biobakery/wiki/metaphlan2) with "-t rel_ab_w_read_stats" specified. Check that this file has two header lines (#SampleID and #clade_name) or edit to ensure that this is the case. If using MetaPhlAn 3, for example, a script like the following may help convert the header:

```for f in *txt 
 do 
 sed -i '1d' $f
 sed -i '1d' $f 
 sed -i '2d' $f 
 sed -i '3d' $f
 done
```

After this, an OTU table is made using the attached R script, which makes this data easy to import/use in other contexts. Note that this R script can merge additional microbiome data into the total OTU table, so long as it fits 1 of 2 formats:

1. It is a profile with taxa names similar to the MetaPhlAn outputs
2. It is a profile with OTU's numbered instead of named, and has an associated table linking the OTU numbers to the taxonomic identifier

These varying files can be placed in the PerSample (1) or Paired (2) folders that the script references. If no samples are being used in the Paired folder, leave a set of paired empty files there. Further, using the $estimated_number_of_reads_from_the_clade statement will generate an OTU table with read counts (better for alpha diversity calculations) while using $relative_abundance will generate an OTU table with relative abundance. 

### Analyses

SourceTracker can take the generated tables as an input. Some trimming may be necessary (the last column on merged_otu_lineage.txt is extraneous information, and if no paired samples are used, the empty sample column can be removed from both generated files).
