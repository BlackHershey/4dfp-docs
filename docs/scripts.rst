-------
Scripts
-------

Dicom utilities
===============

dcm_sort			sort dicom files by study series


fMRI oriented scripts
=====================

generic_cross_bold_pp_090115	
	generic EPI (BOLD) pre-processing
epi2mpr2atlv_4dfp		
	EPI :math:`\rightarrow` T1W :math:`\rightarrow` atlas
epi2t2w2mpr2atlv_4dfp	
	EPI :math:`\rightarrow` T2W :math:`\rightarrow` T1W :math:`\rightarrow` atlas	(8 parameter cross-modal; for “low”  res fMRI)
epi2t2w2mpr2atl1_4dfp	
	EPI :math:`\rightarrow` T2W :math:`\rightarrow` T1W :math:`\rightarrow` atlas	(9 parameter cross-modal; for “high” res fMRI)
epi2t2w2mpr2atl2_4dfp	
	EPI :math:`\rightarrow` T2W :math:`\rightarrow` T1W :math:`\rightarrow` atlas	(6 parameter cross-modal; best for distorted fMRI)
cross_day_imgreg_4dfp	
	link first session atlas transform to subsequent sessions via EPI “anat_ave” 
compute_run_sd1.csh		
	run var_4dfp on all bold directories
cross_day_imgreg_4dfp	
	compute cross-session BOLD atlas transform
chop_4dfp			
	extract range of frames (paste_4dfp wrapper)
extract_frame_4dfp		
	extract one frame (paste_4dfp wrapper)
run_dvar_4dfp			
	compute format (identity frames with too much motion)
conc_4dfp			
	create conc file
conc_mv			
	update conc file 4dfp image pointers
conc2format			
	compute conc-specific format using a fixed number of pre-steady state frames
RFX2.csh			
	random effects analysis (1 or 2 groups)


fcMRI oriented scripts
======================

fcMRI_preproc_090115.csh	
	fcMRI preprocessing including nuisance variable regression
seed_correl_090115.csh	
	compute multi-volume correlation maps


DTI
===

generic_DWI_script_090219	
	generic DTI processing
cross_DWI_imgreg_4dfp	
	atlas transform new data based on previous session results


Anatomical registration scripts
===============================

mpr2atl_4dfp			
	single T1W :math:`\rightarrow` atlas
avgmpr_4dfp			
	multiple T1W :math:`\rightarrow` atlas
t2w2mpr_4dfp\ :sup:`*`			
	T2W :math:`\rightarrow` T1W :math:`\rightarrow` atlas
epi2t1w_4dfp\ :sup:`*`	
	EPI :math:`\rightarrow` T1W
make_mprage_avg_4dfp\ :sup:`*`
	compute T1W anatomical average for group (list directed)
msktgen_4dfp\ :sup:`*`
	create tailored mask (by inversion of atlas transform)
cross_mpr_imgreg_4dfp\ :sup:`*`
	compute cross-session T1W atlas transform
new_atl_init_4dfp\ :sup:`*`
	initialize creation of a cohort-specific atlas-representative target image
newatl_refine_4dfp\ :sup:`*`
	refine cohort-specific atlas-representative target image


Miscellaneous scripts
=====================

t4img_4dfp			
	single image wrapper for t4imgs_4dfp
split_ROIs			
	split peak_4dfp ROI image into multiple mask images
brec				
	parse rec file by procedure depth


.. [*] assumes pre-existing atlas-transform t4 file
