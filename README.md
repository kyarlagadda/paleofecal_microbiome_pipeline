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

Profiling is done in [MetaPhlAn](https://github.com/biobakery/biobakery/wiki/metaphlan2) with "-t rel_ab_w_read_stats" specified. After this, an OTU table is made using the attached R script, which makes this data easy to import/use in other contexts. Note that 
