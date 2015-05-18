##liftover_UCSC
UCSC utility of liftover can be adopted to to convert OLD_BUILD version of genome to NEW_BUILD version genome.<br />
Main page is [Minimal_Steps_For_LiftOver](http://genomewiki.ucsc.edu/index.php/Minimal_Steps_For_LiftOver)<br />
Bioinformatic utilities are here to be downloaded:
[liftOver Utilities](http://hgdownload.cse.ucsc.edu/admin/exe/)<br />
The following steps have already been gone through once in the work of lab member Yang Lv's graduate paper, and succeed!<br />
Some steps may be further explored or understood, for this time, our time is limited, so we didn't spend too much time 	understanding the meaning of each step<br />

******
**Before the first step**
<br />Here, we strongly recommend you to convert .fa file to .2bit file, which stores the sequence as a binary file.
2 advantages:<br />
  1) sequence file would be small in size;<br />
  2) greatly improve the speed, probably over 1000 times.
		
	$faToTwoBit in.fa  out.2bit

**Step1** *using lastz to create lav file*
<br />See here to find details about lastz: [lastz Main Page](http://www.bx.psu.edu/miller_lab/dist/README.lastz-1.02.00/README.lastz-1.02.00a.html)
	
	$lastz OLD_BUILD_DIR/chr${i}_6.2bit NEW_BUILD_DIR/chr${i}_5.2bit --chain --notransition --step=100 > chr${i}.lav

####NOTICE
*the default parameter of lastz is --nochain --gapped... so specify --chain is necessary*
*also, last time we use lastz, --step=100, we convert genome of lower version to higher version, little in difference.*
*next time, maybe still have to adjust the parameter of the step*


**Step2** *use lavToAxt to convert .lav file to .axt file*

	$lavToAxt chr${i}.lav OLD_version.2bit NEW_version.2bit chr${i}.axt;

**Step3** *use axtChain to convert .axt to .chain*

	$axtChain chr${i}.axt -linearGap=medium chr${i}.axt OLD_version.2bit NEW_version.2bit chr${i}.chain;

**Step4** *merge and sort the .chain file*

	$chainMergeSort *chain | chainSplit over stdin;

**Step5** *concatenate all .chain file*

	$cat *chain > OLD2NEW.chain
******

**Final step** *liftover genomes*

	$liftOver oldFile map.chain newFile unMapped

******
####NOTICE
*1. default convertion file format would be .bed .ped, but .gff/.gtf/.gff2 could also be converted using parameter -gff*<br />
*2. the conversion rate > 90%*
******
