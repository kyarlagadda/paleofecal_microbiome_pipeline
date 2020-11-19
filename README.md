# paleofecal_microbiome_pipeline
## Shotgun metagenomic analytical pipeline

This pipeline was made to analyze shotgun microbial read data, including OTU assignment, functional profiling, diversity assessments, discriminant analyses, and pathogen identification. It assumes that reads have already been pre-processed as per [this pipeline](https://github.com/kelsey-witt/diet-taxonomy-pipeline). 

## Required software

* [SourceTracker](https://github.com/danknights/sourcetracker)
* [biobakery](https://github.com/biobakery/biobakery)
  * [KneadData](https://github.com/biobakery/biobakery/wiki/kneaddata)
  * [MetaPhlAn](https://github.com/biobakery/biobakery/wiki/metaphlan2)
  * [HUMAnN](https://github.com/biobakery/biobakery/wiki/humann2)
  * [LEfSe](https://github.com/biobakery/biobakery/wiki/lefse)
