-----
Tools
-----

Filter in space
===============

gauss_4dfp
	spatial frequency domain
butterworth_4dfp†
	spatial frequency domain
imgblur_4dfp
	spatial domain
parzen_4dfp†
	spatial domain
hsphere_4dfp
	convolve image with hard sphere kernel


Filter in time
==============

bandpass_4dfp
	independent specification of low and high ends
	remove linear trends
	remove DC


Register in space (and other t4 oriented programs)
==================================================

imgreg_4dfp
	compute transform (various modes)
t4imgs_4dfp
	apply transforms, resample and average (list directed)
wrpsmg_4dfp			
	apply transforms, resample and average difference images (list directed)
stretch_out			
	remove transform stretch
t4_mul
	compose transforms
t4_inv
	invert transform
t4_factor
	decompose affine transform into components (translation, rotation, stretch)
t4_null
	create an identity transform t4 file
t4_resolve
	compute optimal rigid body transforms connecting a set of images
t4_pts
	inter-convert coordinates, e.g., 711-2B :math:`\leftrightarrow` MNI152


Threshold and mask
==================

zero_slice_4dfp		
	zero specified range of slices in selected direction
zero_lt_4dfp
	threshold by voxel value
zero_gt_4dfp			
	threshold by voxel value
zero_ltgt_4dfp
	zero voxels outside specified range
zero_gtlt_4dfp
	zero voxels within specified range
maskimg_4dfp
	apply 4dfp mask to 4dfp image
cluster_4dfp
	sort/count/zero (above threshold) contiguous voxels into clusters


Dicom utilities
===============

dcm_dump_file
	dump dicom header info to stdout


Image algebra
=============

sqrt_4dfp
	:math:`\sqrt{A}`		
scale_4dfp			
	m*A + b
ratio_4dfp†
	A/B
imgopr_4dfp
	A+B, A-B, A*B, A/B, various special operations
maskimg_4dfp
	A :math:`\land` B


Interconvert image formats
==========================

ima_info			
	dump selected Siemens (pre-DICOM) header info to stdout†
imato4dfp1			
	Siemens (pre-DICOM) :math:`\rightarrow` 4dfp for structural images†
imato4dfpC			
	Siemens (pre-DICOM) :math:`\rightarrow` 4dfp for functional data†
dcm_to_4dfp
	DICOM :math:`\rightarrow` 4dfp
endian_4dfp			
	report status and interconvert big :math:`\leftrightarrow` little endian
4dfptoanalyze			
	4dfp :math:`\rightarrow` analyze 7.5
analyzeto4dfp			
	analyze 7.5 (int or char) :math:`\rightarrow` 4dfp
ifh2hdr				
	create analyze 7.5 header
hdr2txt				
	dump analyze 7.5 header info
index2atl			
	convert atlas indices (ASCII text) to mm (e.g. atlas coordinates)
asciito4dfp			
	convert text columns to 4dfp format timeseries
mpetto4dfp			
	convert microPET images  4dfp
Amirato4dfp†			
	convert Amira :math:`\rightarrow` 4dfp
vto4dfp			
	Varian fid/procpar :math:`\rightarrow` 4dfp
nifti_4dfp			
	interconvert nifti :math:`\leftrightarrow` 4dfp


Rearrange voxels in space or time
=================================

collate_slice_4dfp		
	collate interleaved datasets
paste_4dfp			
	append or average selected frames from multiple files (list directed)
extract_frame_4dfp		
	extract single frame from stack (paste_4dfp wrapper)
chop_4dfp
	extract contiguous frames from stack (paste_4dfp wrapper)
crop_4dfp
	crop or roll (correct image wrap)
reindex_4dfp
	xy, slicevolume
unpack_4dfp			
	mosaic :math:`\rightarrow` volume
multipack_4dfp		
	volume :math:`\rightarrow` mosaic
flip_4dfp			
	flip x, y, z		
collate_slice_4dfp		
	assemble interleaved volumes
split_4dfp			
	split assembled volumes
T2S_4dfp			
	transverse :math:`\rightarrow` sagittal
S2T_4dfp			
	sagittal :math:`\rightarrow` transverse
C2T_4dfp			
	coronal :math:`\rightarrow` transverse
T2C_4dfp			
	transverse :math:`\rightarrow` coronal


Image segmentation and gain field correction
============================================

partitiond_gfc_4dfp		
	intensity inhomogeneity  correction assuming 3D parabolic gain field


"Format" manipulation
=====================

condense			
	generate maximally compact format string
format2lst
	expand format string


fMRI oriented programs
======================

compute_defined_4dfp	
	generate mask of voxels defined over all frames
cs2ap_4dfp			
	convert cosine and sine amplitude images to amplitude and phase
normalize_4dfp		
	scale to achieve mode 1000 
deband_4dfp			
	correct systematic odd vs. even slice  intensity banding
rmspike_4dfp		
	remove artifact due to k-space DC offset
cross_realign3d_4dfp	
	motion correct fMRI timeseries within and across runs
t4_xr3d_4dfp			
	motion correct and resample in atlas space in one step
mat2dat			
	convert cross_realign3d_4dfp mat files to spread sheet format
frame_align_4dfp		
	correct asynchronous slice acquisition
interp_4dfp			
	correct asynchronous slice acquisition and resample in time
jitter				
	optimally distribute n events on m frames


GLM and related operations
==========================

glm_4dfp			
	multivariate voxelwise regression/correlation
actmapf_4dfp			
	voxelwise evaluate timeseries inner product against reference waveform
t4_actmapf_4dfp		
	same functionality as actmapf_4dfp but with simultaneous resampling
GC_4dfp			
	Granger causality mapping
GC_dat			
	Granger causality on ASCII column data 
covariance			
	covariance, correlation, coherence, etc. on ASCII column data
covariance_analysis		
	compute Bartlett correction for autocorrelation fMRI timeseries
		

Evaluate and ROI-oriented programs
==================================

peak_4dfp			
	locate and consolidate maxima to generate ROI
read_4dfp			
	report value of image at specified real coordinate
imgmax_4dfp			
	report maximum and minimum values
img_hist_4dfp			
	construct voxel value histogram; evaluate moments
qnt_4dfp			
	report mean value within 3D ROI
qntm_4dfp			
	evaluate multiple volumes in multiple ROIs
qntv_4dfp			
	evaluate multiple volumes in ROI subdivided into cubes
qntw_4dfp			
	evaluate multiple volumes using weighted ROI
var_4dfp			
	evaluate variance or s.d. about mean over timeseries
dvar_4dfp			
	evaluate variance or s.d. about mean over differentiated timeseries
burn_sphere_4dfp		
	“burn in” sphere at specified real coordinates
ROI_resolve			
	resolve a set of possibly overlapping ROIs into a disjoint set
imgsurf_4dfp			
	move ROI coordinates to nearest surface
spatial_corr_4dfp		
	compute image similarity as correlation over space
spatial_cov_multivol_4dfp	
	compute volume-pair covariance over space


SPM-like voxelwise statistical operations
=========================================

t2z_4dfp			
	t-map :math:`\rightarrow` Z-map
z2logp_4dfp			
	Z-map :math:`\rightarrow` log\ :sub:`10`\ p-map
rho2z_4dfp			
	r-map :math:`\leftrightarrow` Fisher z-map


DTI
===

dwi_xalign3d_4dfp		
	motion compensation for dwi data (single run)
dwi_cross_xalign3d_4dfp	
	cross-run motion compensation and averaging of dwi data
diff_4dfp			
	diffusion tensor computation given dwi
diffRGB_4dfp			
	dwi :math:`\rightarrow` RGB map
whisker_4dfp			
	dwi :math:`\rightarrow` whiskers (visualized in Matlab)


Operations on short int (“Analyze 7.5”) format images
=====================================================

addgrid†			
	“burn in” grid lines
hard_ellipse†			
	“burn in” ellipsoid
2Dhist				
	construct 2D histogram voxel value histogram
fcm_fitgain3d†		
	multi-spectral image segmentation using fuzzy class means


† Solaris only