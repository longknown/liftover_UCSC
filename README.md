# liftover_UCSC
UCSC utility of liftover can be adopted to to convert OLD_BUILD version of genome to NEW_BUILD version genome.
# Main page is http://genomewiki.ucsc.edu/index.php/Minimal_Steps_For_LiftOver
# Bioinformatic utilities are here to be downloaded http://hgdownload.cse.ucsc.edu/admin/exe/
# The following steps have already been gone through once in the work of lab member Yang Lv's graduate paper, and succeed!
# Some steps may be further explored or understood
# for this time, our time is limited, so we didn't not spend too much time understanding the meaning of each step

################################################################################
### start of creating mapping files ###
Step1: using lastz to create lav file
# lastz Main pape: http://www.bx.psu.edu/miller_lab/dist/README.lastz-1.02.00/README.lastz-1.02.00a.html
lastz NEW_BUILD_DIR/chr${i}_6.2bit OLD_BUILD_DIR/chr${i}_5.2bit --chain --notransition --step=100 > chr${i}.lav

########## NOTICE ##########
# the default parameter of lastz is --nochain --gapped... so specify --chain is necessary
# also, last time we use lastz, --step=100, we convert genome of lower version to higher version, little in difference.
# next time, maybe still have to adjust the parameter of the step
############################

Step2: convert .lav file to .psl
lavToPsl chr${i}.lav chr${i}.psl;

Step3: convert .psl to .chain
pslToChain chr${i}.psl chr${i}.chain;

Step4: merge and sort the .chain file
chainMergeSort *chain | chainSplit over stdin;

### end of creating mapping files (map OLD_BUILD_GENOME to NEW_BUILD_GENOME) ###
################################################################################

Final step: liftover genomes
liftOver oldFile map.chain newFile unMapped

### NOTICE ###
# default convertion file format would be .bed .ped, but .gff/.gtf/.gff2 could also be converted using parameter -
# the conversion ratio ~ 
##############
