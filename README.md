# UKBB participation bias
Scripts to examine the associations between participation bias and rare variation, including damaging rare coding variants and copy number variants (CNVs) in ~500k UK Biobank participants. The majority of scripts were run on the UK Biobank Research Analysis Platform (RAP) in an R Studio workbench.

## Before these scripts: WES processing (stages 1–14)
Stages 1–14 process the whole exome sequencing (WES) data. They are shared with our earlier paper (PMID: 41963674) and are in a separate repository: [UKBB_RAP_WES](https://github.com/eilidhfenner/UKBB_RAP_WES). 

The scripts in this repo are run after stage 14 of WES processing, using the following outputs:
- **Transposed variant counts per gene per person** from stage 12 (`12_transpose_counts.Rmd`).
- **Gene sets** (`gene_sets_for_RAP_Feb25.csv`) from stage 13 (`13_Defining_gene_sets.Rmd`). Only a subset of these sets is used here.
- **Gene set counts per person** for exome-wide and pLI ≥ 0.9 genes from stage 14 (`14_gene_set_counts.Rmd`).
- **Ancestry PCs** (`ancestries_PCs.csv`) from stage 9 (`09_Ancestry_inference.Rmd`).
- **IDs with discordant sex** (`discordant_sex_ids.csv`) from stage 8 (`08_sex_inference.Rmd`).

## Other inputs
- **Phenotypes and covariates:** all `*_participant.csv` and `*cog.csv` files, `reweighting.csv` and `additional_weighting_phenotypes.csv` were selected in the UK Biobank cohort browser and exported with the Table Exporter tool on the RAP. Field IDs are listed in the paper.
- **DDG2P genes:** v5.1, downloaded on 8 November 2024 from [PanelApp](https://panelapp.genomicsengland.co.uk/panels/484/).
- **Pathogenic CNVs** (`Pathogenic_CNVs.csv`): CNV calls from Kendall et al. (2017) *Biol. Psychiatry* 82, 103–110 and Kendall et al. (2019) *Br. J. Psychiatry* 214, 297–304. These calls have been returned to the UKB. 

## Stages of processing
Scripts are numbered in the order they were run, carrying on from the WES repository. Each script starts with a short description of what it does.

- **Stage 15:** _'15_defining_gene_sets.Rmd'_
    - Defines the broad neuropsychiatric disorder (NPD) gene set used in this analysis from the WES repository gene sets and DDG2P. Run locally and then the output (`ppt_bias_sets.csv`) was uploaded to the RAP.
- **Stage 16:** _'16_gene_set_counts.Rmd'_
    - Reads the transposed counts from WES stage 12 and adds up counts across all genes in the NPD set, for high-confidence (LoFTEE) PTVs, deleterious (REVEL > 0.75) missense variants and synonymous variants. Writes one count table per set and variant class.
- **Stage 17:** _'17_participation_phenotypes_processing.Rmd'_
    - Cleans the cognitive test and participation phenotypes and adds the reweighting variables. Writes `phenotypes_all_cleaned_inc_reweighting.csv`.
- **Stage 18:** _'18_regressions.Rmd'_
    - Reads phenotypes from stage 17, gene set counts from stage 16 and WES stage 14, and pathogenic CNVs. Runs the burden regressions on participation outcomes, and the weighted and unweighted regressions on cognition.
- **Stage 19:** _'19_interaction_analysis.Rmd'_
    - Uses the same inputs as stage 18 to run the interaction regressions between variant burden and participation on cognition.
- **Stage 20:** _'20_plots.Rmd'_
    - Run locally on the results downloaded from stage 18. Plots the results.

## Data access
UK Biobank data are available to approved researchers through the [UK Biobank access process](https://www.ukbiobank.ac.uk/enable-your-research). This analysis was run under application 13310.

## Citation
If you use this code, please cite the associated preprint.