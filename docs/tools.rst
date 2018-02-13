-----
Tools
-----

.. |br| raw:: html

Filter in space
===============

.. _gauss_4dfp:

gauss_4dfp
----------
spatial frequency domain

Usage:	gauss_4dfp <4dfp|conc input> f_half [outroot]

Examples::

	gauss_4dfp pt349_study9to9 0.1
	gauss_4dfp p1234ho5 0.7 p1234ho5_g7

Options

=======	========================================================================
outroot	output file name (default = <inroot>_g<10*f_half>) (only for 4dfp input)
-@<b|l>	output big or little endian (default input endian)
-w		(wrap) suppress x and y padding
-d		differentiate
=======	========================================================================

N.B.:	f_half is half frequency in 1/cm

N.B.:	FWHM*f_half = (2ln2/pi) = 0.4412712

N.B.:	conc files must have extension "conc"

butterworth_4dfp†
-----------------
spatial frequency domain

imgblur_4dfp
------------
spatial domain

Usage:	imgblur_4dfp [options] <image_file> <FWHM_in_mm>

Examples::

	imgblur_4dfp -yz vc345 5.5

Options

=======	==================================================
-x		selective x blur
-y		selective y blur
-z		selective z blur
-@<b|l>	output big or little endian (default input endian)
=======	==================================================

N.B.:	default blur is 3D isotropic

parzen_4dfp†
------------
spatial domain

hsphere_4dfp
------------
convolve image with hard sphere kernel

Usage:	hsphere_4dfp <file_4dfp> <flt>

Examples::

	hsphere_4dfp np1234_zmap_222 5

N.B.:	second argument specifies hard sphere radius in mm


Filter in time
==============

.. _bandpass_4dfp:

bandpass_4dfp
-------------
Independent specification of low and high ends; remove linear trends; remove DC

Usage:	bandpass_4dfp <(4dfp|conc) input> <TR_vol>

Examples::

	bandpass_4dfp qst1_b1_rmsp_dbnd_xr3d[.4dfp.img] 2.36 -bl0.01 -ol1 -bh0.15 -oh2

Options

============	=================================================================================
-b[l|h]<flt>	specify low end or high end half frequency in hz
-o[l|h]<int>	specify low end or high end Butterworth filter order
-n<int>			specify number of pre-functional frames (default = 0)
-I<int>			specify interpolation mode (0 = none; 1 = linear; 2 = cubic spline) (default = 1)
-f<string>		specify frames-to-count format (overrides option -n)
-F<string>		read frames-to-count format from specified file (overrides options -n and -f)
-t<str>			change output filename trailer (default="_bpss")
-a				retain DC (constant) component
-r				retain linear trend
-E				code undefined voxels as 1.e-37
-M				disable undefined mask computation
-B				compute gain using correct Butterworth formula (default squared Butterworth gain)
-@<b|l>			output big or little endian (default input endian)
============	=================================================================================

N.B.:	undefined values are zero, NaN, or 1.e-37

N.B.:	input conc files must have extension "conc"

N.B.:	omitting low  end order specification disables high pass component

N.B.:	omitting high end order specification disables low  pass component


Register in space (and other t4 oriented programs)
==================================================

imgreg_4dfp
-----------
compute transform (various modes)

Usage: 	imgreg_4dfp target_imag target_mask source_imag source_mask t4file mode |br|
or:	 	imgreg_4dfp target_imag        none source_imag source_mask t4file mode |br|
or:		imgreg_4dfp target_imag        none source_imag        none t4file mode |br|

Mode options

====	====================================================================================================================
1		enable coordinate transform
2		enable 3D alignment
4		enable affine warp (12 parameters in 3D 6 parameters in 2D)
8		enable voxel size adjust
16		disable x voxel size adjust
32		disable y voxel size adjust
64		disable z voxel size adjust
128		unassigned
256		when set use difference image minimization (for similar contrast mechanisms)
512		superfine mode (2 mm cubic grid metric sampling)
1024	fast mode (12 mm cubic grid metric sampling)
2048	fine mode (5 mm cubic grid metric sampling)
4096	[T] restricted to translation explored at 7.5 mm intervals
8192	enable parameter optimization by computation of the metric gradient in parameter space and inversion of the Hessian
====	====================================================================================================================

.. _t4imgs_4dfp:

t4imgs_4dfp
-----------
apply transforms, resample and average (list directed)

Usage:	t4imgs_4dfp [options] <inlist> <outfile>

Options

==========	===================================================
-z			normalize by sqrt(n) rather than n (for z images)
-s			interpolate by 3D cubic spline (default is 3D linear)
-N			output NaN (default 0.0) for undefined values
-B			internally convert to_711-2A_t4->to_711-2B_t4
-n			use nearest neighbor interpolation
-R			suppress creation of rec file
-O111		output in 111 space instead of default 333.0 space
-O222		output in 222 space instead of default 333.0 space
-O333.n		output in 333.n space (y shifted up by n pixels)
-Omy_image	duplicate dimensions of my_image.4dfp.ifh
-@<b|l>		output big or little endian (default CPU endian)
==========	===================================================

N.B.: t4file intensity scale ingnored with option -n

t4img_4dfp
----------
single image wrapper for :ref:`t4imgs_4dfp`

Usage:	t4img_4dfp <t4file> <imgfile> [outfile]

Examples::

	t4img_4dfp  vce1_mprS_to_711-2B_t4	vce1_mprS.4dfp.img -O222
	t4img_4dfp  vce1_mprS_to_711-2B_t4 	vce1_mprS vce_mprS_711-2B -O222
	t4img_4dfp  none			vce1_mprS vce1_mprS_222 -O222

Options (for more options, see :ref:`t4imgs_4dfp`)

=======	====================================================
outfile	specify name for output file (default is <imgfile>t)
=======	====================================================

N.B.:	4dfp filename extensions are optional

N.B.:	option -n causes fidl ROI names to be copied to the output ifh

wrpsmg_4dfp
-----------
apply transforms, resample and average difference images (list directed)

Usage:	wrpsmg_4dfp [options] <inlist> <outfile>

Options

==========	================================================
-N			output NaN (default 0.0) for undefined values
-w			create sum of weights image
-s			create square root variance (sd) image
-O111		output in 111 space
-O222		output in 222 space (default)
-O333.n		output in 333.n space (y shifted up by n pixels)
-Omy_image	duplicate dimensions of my_image.4dfp.ifh
-@<b|l>		output big or little endian (default CPU endian)
==========	================================================

stretch_out
-----------
remove transform stretch

Usage:	stretch_out <t4file> [t4file_new]

N.B.:	default output filename is <t4file>"r"

t4_mul
------
compose transforms

Usage:	t4_mul <left_t4file> <right_t4file> [product_t4file]

Examples::

	t4_mul vm11b_anat_ave_to_vm11b_234-3_t4 vm11b_234-3_to_711-2B_t4 [vm11b_anat_ave_to_711-2B_t4]

t4_inv
------
invert transform

Usage: 	t4_inv <t4file> [inv_t4file]

Examples::

	t4_inv vm11b_anat_ave_to_vm11b_234-3_t4 [vm11b_234-3_to_vm11b_anat_ave_t4]

Options

==	==========================================
-u	suppress (intensity) scale field in output
==	==========================================

t4_factor
---------
decompose affine transform into components (translation, rotation, stretch)

Usage: 	t4_factor <t4file>

Examples::

	t4_factor vm11b_anat_ave_to_vm11b_234-3_t4

t4_null
-------
create an identity transform t4 file

Usage:	t4_null <t4file>

Examples::

	t4_null vm11b_mpr1_to_711-2B_t4

t4_resolve
----------
compute optimal rigid body transforms connecting a set of images

Usage:	t4_resolve <image1> <image2> ...

Options

=======	=============================================================================
-v		verbose mode
-m		generate mat file output
-s		include intensity scale factor in t4 file output
-w		weight inversely in proportion to scale in sub file output (sum counts mode)
-o<str>	write resolved output with specified fileroot
-r<flt>	set VOI rms radius in mm (default=50)
=======	=============================================================================

N.B.:	t4_resolve looks for t4 files <image1>_to_<image2>_t4, <image1>_to_<image3>_t4, ...
N.B.:	t4_resolve automatically strips filename extensions when constructing t4 filenames

t4_pts
------
inter-convert coordinates, e.g., 711-2B :math:`\leftrightarrow` MNI152

Usage:	t4_pts <t4file> <pts.lst> [new pts.lst]

Examples::

	t4_pts 711-2B_to_MNI152lin_T1_t4 711-2B_coords MNI152_coords


Threshold and mask
==================

zero_slice_4dfp
---------------
zero specified range of slices in selected direction

Usage:	zero_slice_4dfp <4dfp image>

Examples::

		zero_slice_4dfp vce20_mpr -z1to3
		zero_slice_4dfp vce20_mpr <x|y|z> istart iend [outroot]

Options

====================	====================================================
-<x|y|z><int>to<int>	specify x y z limits (single required argument mode)
-f						interpret slice numbers using 4dfp<->analyze flips
-o						specify output fileroot (default = <image>z)
-@<b|l>					output big or little endian (default input endian)
====================	====================================================

N.B.:	slices count from 1

N.B.:	two usages are supported: 1 or 4 required arguments

zero_lt_4dfp
------------
threshold by voxel value

Usage:	zero_lt_4dfp <flt> <file_4dfp> [outroot]

Examples::

	zero_lt_4dfp 90 pt349_study9to9
	zero_lt_4dfp 90 pt349_study9to9 pt349_study9to9z

Options

=======	==================================================
-@<b|l>	output big or little endian (default input endian)
=======	==================================================

N.B.:	default output 4dfp root is <file_4dfp>"z"

zero_gt_4dfp
------------
threshold by voxel value

Usage:	zero_gt_4dfp <flt> <(4dfp) image> [outroot] [options]

Examples::

 	zero_gt_4dfp 90 pt349_study9to9
 	zero_gt_4dfp 90 pt349_study9to9 pt349_study9to9z

Options

=======	==================================================
-@<b|l>	output big or little endian (default input endian)
=======	==================================================

N.B.:	default output 4dfp root is <(4dfp) image>"z"

N.B.:	first field can't be used for options because threshold might be negative

zero_ltgt_4dfp
--------------
zero voxels **outside** specified range

Usage:	zero_ltgt_4dfp <flt[to<flt>]> <(4dfp) image> [outroot] [options]

Examples::

	zero_ltgt_4dfp -30to90 pt349_study9to9

Options

=======	==================================================
-@<b|l>	output big or little endian (default input endian)
=======	==================================================

N.B.:	default output 4dfp root is <(4dfp) image>"z"

N.B.:	first field can't be used for options because lower range might be negative

zero_gtlt_4dfp
--------------
zero voxels **within** specified range

Usage:	zero_gtlt_4dfp <flt[to<flt>]> <(4dfp) image> [outroot] [options]

Examples::

	zero_gtlt_4dfp -30to90 pt349_study9to9

Options

=======	==================================================
-@<b|l>	output big or little endian (default input endian)
======= ==================================================

N.B.:	default output 4dfp root is <(4dfp) image>"z"

N.B.:	first field can't be used for options because lower range might be negative

.. _maskimg_4dfp:

maskimg_4dfp
------------
apply 4dfp mask to 4dfp image

Usage:	maskimg_4dfp <(4dfp) imgfile> <(4dfp) mskfile> <(4dfp) outfile>

Examples::

	maskimg_4dfp -t23.2 va1234_mpr mask va1234_mpr_msk

Options

=======	===========================================================
-N		replace NaN in <imgfile> with corresponding <mskfile> value
-e		report to stdout mean <imgfile> within-mask value
-1		apply first frame of <mskfile> to all frames of <imgfile>
-R		suppress creation of rec file
-v<flt>	specify <outfile> uniform within-mask value
-p<flt>	specify <mskfile> threshold as percent of <mskfile> max
-t<flt>	specify <mskfile> threshold directly (default = 0.0)
-A		threshold mask by absolute value of <mskfile>
-@<b|l>	output big or little endian (default <imgfile> endian)
=======	===========================================================

N.B.:	<imgfile> and <mskfile> may be the same

cluster_4dfp
------------
sort/count/zero (above threshold) contiguous voxels into clusters

Usage:	cluster_4dfp <(4dfp) root>

Examples::

	cluster_4dfp my_timage -At3.5

Options

=======	============================================================================================
-n<int>	zero out clusters with voxel count below specified criterion (output image trailer = 'clus')
-f<int>	address specified volume (counting from 1) of multi-volume stack (default is first volume)
-t<flt>	specify image value threshold (default = 0)
-a<str>	append specified string (preceded by "_") to all output filenames
-@<b|l>	output big or little endian (default input endian)
-A		apply threshold test to image absolute value
-R		convert clusters to (fidl compliant) ROI image (output image trailer = 'ROI')
-l		create list file of region center of mass indices
-v		verbose mode
=======	============================================================================================

N.B.:	-l center of mass indices can be converted to atlas coordinates using index2atl -af


Dicom utilities
===============

dcm_dump_file
-------------
dump dicom header info to stdout

Usage: dcm_dump_file [-b] [-g] [-l] [-m mult] [-t] [-v] [-w flag] [-z] file [file ...]

Options

=======	=========================================================
-b		Input files are stored in big-endian byte order
-e		Exit on file open error.  Do not process other files
-g		Remove group length elements
-l		Use (retired) length-to-end attribute for object length
-m mult	Change VM limit from 0 to mult
-t		Part 10 file
-v		Place DCM facility in verbose mode
-w		Set open options; flag can be REPEAT
-z		Perform format conversion (verification) on data in files
=======	=========================================================


Image algebra
=============

sqrt_4dfp
---------
:math:`\sqrt{A}`

Examples::

	sqrt_4dfp vce20_mpr

Options

=======	==================================================
-@<b|l>	output big or little endian (default input endian)
-E		output undefined voxels as 1.0e-37 (default 0.0)
=======	==================================================

N.B.:	default output filename = <image>_sqrt

scale_4dfp
----------
m*A + b

Usage:	scale_4dfp <image_4dfp> <scale_factor> [options]

Examples::

	scale_4dfp b2_xfrm_avg 12
	scale_4dfp b2_xfrm_avg 12 -b5 -ax12+5

Options

=======	==================================================
-E		preserve 1.0e-37 values (fidl NaN convention)
-a<str>	append trailer to output file name
-b<flt>	add specified constant to each voxel
-@<b|l>	output big or little endian (default input endian)
=======	==================================================

N.B.:	<image_4dfp> is overwritten unless the trailer option is used
N.B.:	<scale_factor> must be specified for proper operation

ratio_4dfp†
-----------
A/B

imgopr_4dfp
-----------
A+B, A-B, A*B, A/B, various special operations

Usage:	imgopr_4dfp -<operation><(4dfp) outroot> <(4dfp) image1> <(4dfp) image2> ...

Operations

=	====================================================================
a	add
s	subtract (image1 - image2)
p	product
r	ratio (image1 / image2)
e	mean (expectation)
v	variance
g	geometric mean
n	count defined (see -u option) voxels
x	voxelwize maximum
y	voxelwize minimum
G	report serial number (counting from 1) of image with greatest value
P	unsplit multiple ROIs into fidl compatible ROI file
=	====================================================================

Options

=======	========================================================
-u		count only defined (not NaN or 1.e-37 or 0.0) voxels
-R		suppress creation of rec file
-N		output undefined voxels as NaN
-Z		output undefined voxels as 0
-E		output undefined voxels as 1.E-37 (default)
-c<flt>	multiply output by specified scaling factor
-l<lst>	read input file names from specified list file
-@<b|l>	output big or little endian (default first input endian)
=======	========================================================

N.B.:	image dimensions must match except for binary operations {aspr} in which a 1 volume second image may be paired with a multi-volume first image


Interconvert image formats
==========================

ima_info†
---------
dump selected Siemens (pre-DICOM) header info to stdout

imato4dfp1†
-----------
Siemens (pre-DICOM) :math:`\rightarrow` 4dfp for structural images

imato4dfpC†
-----------
Siemens (pre-DICOM) :math:`\rightarrow` 4dfp for functional data

dcm_to_4dfp
-----------
DICOM :math:`\rightarrow` 4dfp

Usage:	dcm_to_4dfp [-b base] [-d gggg eeee] [-f] [-g] [-u] file(s)

Slice Spacing Options: [-c] [-t <flt> or S or T]

Slice Position Options: [-X] [-Y] [-Z]

Examples::

 	dcm_to_4dfp *
   	dcm_to_4dfp -b ID101 -f -g -u *IMA
   	dcm_to_4dfp -d 0008 0030 -t 4.98 -g *.dcm
   	dcm_to_4dfp -b P0089 -t T -g mydir/*

Options

==============	===============================================================================
[-b base] 		Output base filename follows the -b
[-c]	    	Slice Spacing: By Image Position (0020 0032)
[-d gggg eeee] 	Divide series by group and element number (Default: ID series time (0008 0031))
[-f]	    	Directories will be created, and dicom files will be moved
[-g]	    	Add image name, XYZ relative position, and number to rec file
[-q]      		 Slice Spacing: Do not compute by Image Position
[-r]       		Rescale: Use the rescale slope and intercept fields
[-t <flt>] 		Slice Spacing: Use input value.[-t <flt>]
[-t T]     		Slice Spacing: Use Slice Thickness 0018 0050.[-t T]
[-t S]     		Slice Spacing: Use Slice Spacing 0018 0088 [-t S]
[-u]			Output files named using sequence tag 0018 0024 plus number
==============	===============================================================================

4dfp Coordinant System is determined by Image Position (0020 0032).
Multivolume and BOLD images are ordered by REL Image Number (0020 0013).
[-X]	Sagittal:	image positions will be ordered low to high
[-Y]	Coronal:	image positions will be high to low
[-Z]	Transverse:	image positions will be high to low
[-@ <b|l>]	output big or little endian (default CPU endian)

N.B.: -t S is the default slice spacing

N.B.: Default slice position is transverse ordered by REL Image Number (0020 0013)

endian_4dfp
-----------
report status and interconvert big :math:`\leftrightarrow` little endian

Usage:	endian_4dfp <(4dfp) image>

Options

=========	=============================================
-@<b|l|c> 	make <(4dfp) image> big, little or CPU endian
-t			perform var(log(fabs(.))) test
=========	=============================================

N.B.:	<(4dfp) image> may be overwritten

N.B.:	absent option -@ endian_4dfp only reports state of <(4dfp) image>

4dfptoanalyze
-------------
4dfp :math:`\rightarrow` analyze 7.5

Usage:	4dfptoanalyze <(4dfp) filename>

Options

=======	===================================================================================
-c<flt>	scale output values by specified factor
-8		output 8 bit unsigned char
-SPM99	include origin and scale in hdr (http:/wideman-one.com/gw/brain/analyze/format.doc)
-@<b|l>	output big or little endian (default CPU endian)
=======	===================================================================================

analyzeto4dfp
-------------
analyze 7.5 (int or char) :math:`\rightarrow` 4dfp

Usage: analyzeto4dfp <analyze_image>

Options

=======	=================================================
-s		apply SPM2 ROIScaleFactor
-x		flip first  axis
-y		flip second axis
-z		flip third  axis
-@<b|l>	toutput big or little endian (default CPU endian)
-O<int>	supply orientation code (in range [0-5])
=======	=================================================

N.B.:	to convert SPM2 use options -x and -s

ifh2hdr
-------
create analyze 7.5 header

Usage:	ifh2hdr <(4dfp) file>

examples::

	ifh2hdr vc654_mpr_atl -r-500to1500

Options

================	=========
-r<flt>[to<flt>]	set range
================	=========

hdr2txt
-------
dump analyze 7.5 header info

Usage: hdr2txt <analyze_image>

Examples::

	hdr2txt brain_asig[.hdr]

index2atl
---------
convert atlas indices (ASCII text) to mm (e.g. atlas coordinates)

Usage: index2atl <(4dfp) ifhroot> <index_list_file>

Examples::

	index2atl -af time_BOXzstat_333_t88.4dfp.ifh time_BOXzstat_333_t88_index.lst

Options

=======	============================================================================
-f		input indices use FORTRAN convention (first index=1) (default first index=0)
-a		indices were read under orientation-specific 4dfp<->analyze flips
-o<str>	output coordinates to specified file
=======	============================================================================

N.B.:	<(4dfp) ifhroot> corresponds to the 4dfp image from which the indices were read

asciito4dfp
-----------
convert text columns to 4dfp format timeseries

Usage:	asciito4dfp <text file> <(4dfp) out>

Options

=======	================================================
-@<b|l> output big or little endian (default CPU endian)
=======	================================================

N.B.:	columns in <text file> map to voxels in <(4dfp) out>

N.B.:	'#' in <text file> introduce comments

N.B.:	<text file> lines beginning with '#' are included in <(4dfp) out>.img.rec

mpetto4dfp
----------
convert microPET images  4dfp

Usage:	mpetto4dfp <microPET_data>

Examples::

	mpetto4dfp m1042-cft1_v1

Options

=======	===============================================================
-x		flip x
-y		flip y
-z		flip z
-w		create frame duration listing for use with actmapf_4dfp -w
-c<flt>	scale all voxel values by specified factor
-o<str>	name 4dfp output using specified string (default same as input)
-@<b|l> output big or little endian (default input endian)
=======	===============================================================

Amirato4dfp†
------------
convert Amira :math:`\rightarrow` 4dfp

vto4dfp
-------
Varian fid/procpar :math:`\rightarrow` 4dfp

Usage:	vto4dfp <varian file path>

Examples::

	vto4dfp /home/usr/shimonyj/vto4dfp/hard_010703 -odwi_010703

Options

=======	====================================================================
-v		verbose mode
-D		suppress subtraction of k-space DC offset
-I		perform Fourier interpolation; output voxel count will be quadrupled
-F		phase reverse odd echos in multi-echo data
-o<str>	specify 4dfp outroot (default="fid")
-c<flt>	intensity scale output (mag) image by specified constant
-m<flt>	scale voxel dimensions by specified constant
-@<b|l>	output big or little endian (default CPU endian)
=======	====================================================================

N.B.:	vto4dfp expects <varian file path> to contain files "fid" and "procpar"

nifti_4dfp
----------
interconvert nifti :math:`\leftrightarrow` 4dfp

Usage: nifti_4dfp -<4|n> <infile> <outfile> [options]

Examples::

	nifti_4dfp -n time_BOXzstat_333_t88.4dfp.ifh time_BOXzstat_333_t88.nii

Options

============	================================================================
-T <t4 file>	specify a t4 file to use converting TO NIfTI from 4dfp
-n				convert TO NIfTI from 4dfp
-4				convert TO 4dfp from NIfTI
-N				suppress saving of mmppix and center fields in output ifh
-@<val>			specify endianness for output, b or B for big, l or L for little
============	================================================================

N.B.:	exactly one of -4 or -n must be specified

N.B.:	".4dfp.ifh" or ".nii" are appended to filenames specified without extension

N.B.:	option -N has effect only on converting nii->4dfp

N.B.:	option -T has effect only on converting 4dfp->nii


Rearrange voxels in space or time
=================================

collate_slice_4dfp
------------------
collate interleaved datasets

Usage:	collate_slice_4dfp <4dfp img1> <4dfp img2> ... <4dfp imgn> <4dfp imgout>

Options

=======	================================================
-v		verbose mode
-@<b|l>	output big or little endian (default CPU endian)
=======	================================================

.. _paste_4dfp:

paste_4dfp
----------
append or average selected frames from multiple files (list directed)

Usage:	paste_4dfp <inlist> <outfile>

Options

=======	==========================================================
-a		append successive epochs (default average)
-p<int>	specify period in frames (default=1)
-@<b|l>	output big or little endian (default initial input endian)
=======	==========================================================

extract_frame_4dfp
------------------
extract single frame from stack (:ref:`paste_4dfp` wrapper)

Usage:	extract_frame_4dfp <(4dfp) stack> <(int) frame>

Examples::

	extract_frame_4dfp CDR.5to1+ 3

Options

=======	==============================================================
-o<str>	specifiy output 4dfp fileroot (default = <stack>_frame<frame>)
=======	==============================================================

chop_4dfp
---------
extract contiguous frames from stack (:ref:`paste_4dfp` wrapper)

usage:	chop_4dfp <(4dfp) stack> <(int) frame0> <(int) frame1>

Examples::

	chop_4dfp vb12345_b5_dbnd_xr3d[.4dfp[.img]] 4 68

Options

=======	=========================================================================
-o<str>	specify output 4dfp fileroot (default = <stack>_frames<frame0>to<frame1>)
=======	=========================================================================

crop_4dfp
---------
crop or roll (correct image wrap)

Usage:	crop_4dfp <(4dfp) inroot> [(4dfp) outroot]

Options

=======================	==============================================================================
-<x|y|z><int>[to[<int>]	specify x y z crop/expand limits (1-indexed)
-s<x|y|z><int>			scroll specified axis by specified number of pixels (after cropping/expanding)
-f						interpret specifications under 4dfp<->analyze flips
-Z						zero voxels instead of physically cropping
-@<b|l>					output big or little endian (default input endian)
=======================	==============================================================================

N.B.:	if upper crop limit exceeds input dimension undefined voxels will be set to 1.e-37

N.B.:	default (4dfp) output root is <(4dfp) inroot>"_crop"

reindex_4dfp
------------
.. FIXME: figure out what these symbols are supposed to be

xy, slicevolume

Usage:	reindex_4dfp <(4dfp> input> <index1> <index2> [options]

Examples::

	reindex_4dfp my4Dstack 3 4

Options

=======	==============================================================
-v		verbose mode
-o<str>	specify 4dfp output root (default = <input>_r<index1><index2>)
-@<b|l>	output big or little endian (default input endian)
=======	==============================================================

N.B.:	reindex_4dfp swaps specified indices

N.B.:	<index1> and <index2> must be unequal integers in the range 1-4 except as follows
	- <index1> == 4 and <index2> == 0: right rotate indices (first index <-  last index)
	- <index1> == 0 and <index2> == 4:  left rotate indices ( last index <- first index)

unpack_4dfp
-----------
mosaic :math:`\rightarrow` volume

Usage:	unpack_4dfp <(4dfp) input> <(4dfp) output>

Examples::

	unpack_4dfp 030211_EL_b_1 030211_EL_b1

Options

========	==================================================
-V			read frame count from input ifh slice count
-R			multiply output x and y voxsiz by pack factor
-z			flipz (unpack slices in reverse order)
-y			flipy
-nx<int>	specify unpacked nx (default=64)
-ny<int>	specify unpacked ny (default=64)
-sx<int>	squeeze unpacked x dimension by specified factor
-sy<int>	squeeze unpacked y dimension by specified factor
-@<b|l>		output big or little endian (default input endian)
========	==================================================

multipack_4dfp
--------------
volume :math:`\rightarrow` mosaic

flip_4dfp
---------
flip x, y, z

Usage:	flip_4dfp <(4dfp) image> [(4dfp) output]

Examples::

	flip_4dfp -yz vc345 vc345_flipyz

Options

=======	==================================================
-x		flip x
-y		flip y
-z		flip z
-@<b|l>	output big or little endian (default input endian)
=======	==================================================

N.B.:	default output fileroot = <image>_flip[xyz]

split_4dfp
----------
split assembled volumes

T2S_4dfp
--------
transverse :math:`\rightarrow` sagittal

Usage:	T2S_4dfp <(4dfp) imgroot> [(4dfp) outroot]

Examples::
	T2S_4dfp vm6c_mpr
	T2S_4dfp vm6c_mpr vm6c_mprS

Options

=======	==================================================
-@<b|l>	output big or little endian (default input endian)
=======	==================================================

N.B.:	default output root = <imgroot>"S"

S2T_4dfp
--------
sagittal :math:`\rightarrow` transverse

Usage:	S2T_4dfp <(4dfp) imgroot> [(4dfp) outroot]

Examples::

	S2T_4dfp vm6c_mpr
	S2T_4dfp vm6c_mpr vm6c_mprT

Options

=======	==================================================
-@<b|l>	output big or little endian (default input endian)
=======	==================================================

N.B.:	default output root = <imgroot>"T"

C2T_4dfp
--------
coronal :math:`\rightarrow` transverse

Usage:	C2T_4dfp <(4dfp) image> [(4dfp) outroot]

Examples::

	C2T_4dfp vm6c_b1
 	C2T_4dfp vm6c_b1 vm6c_b1T

Options

=======	==================================================
-@<b|l>	output big or little endian (default input endian)
=======	==================================================

N.B.:	default output root = <imgroot>"T"

T2C_4dfp
--------
transverse :math:`\rightarrow` coronal

Usage:	T2C_4dfp <(4dfp) imgroot> [(4dfp) outroot]

Examples::

	T2C_4dfp vc12345_b1
	T2C_4dfp vc12345_b1 vc12345_b1C

Options

=======	==================================================
-@<b|l>	output big or little endian (default input endian)
=======	==================================================

N.B.:	default output root = <imgroot>"C"


Image segmentation and gain field correction
============================================

partitiond_gfc_4dfp
-------------------
intensity inhomogeneity  correction assuming 3D parabolic gain field

Usage:	partitiond_gfc_4dfp <imgroot>

Examples::

	partitiond_gfc_4dfp vc1440_mpr_n4_111_t88.4dfp

Options

================	=====================================================
-g					freeze initial gain field
-n					force negative definite quadratic gain field
-v					verbose mode
-p<flt> 			pre-blur by specified FWHM in mm
-b<flt>				specify bandwidth in intensity units (default=200.0)
-e<flt>				specify drms convergence criterion (default=0.000200)
-i<flt>				specify sigma (default=1.000000)
-l<int>				specify iteration limit (default=8)
-m<flt>				specify gfc computation region count (default=24)
-s<flt>				specify space constant in mm (default=4.000000)
-z<flt>				specify background threshold (default=180.0)
-M<flt>				specify maximum correction factor
-r<flt>[to<flt>]	specify gfc range (default=0.0to10000.0)
================	=====================================================


"Format" manipulation
=====================

condense
--------
generate maximally compact format string

Usage:	condense <format_str>

Examples::

	condense "4x86+4x86+4x86+4x86+4x86+4x86+4x86+4x86+4x86+"
	# output: 9(4x86+)

Options

=======	===================================================================
-v		verbose mode
-f<str>	read input format string from specified file (default command line)
=======	===================================================================

format2lst
----------
expand format string

Usage:	format2lst <format|fmtfile>

Examples::

	format2lst "2x3-2+1-2+2-2+1-1+2-1+1-1+1-1+2-1+1-1+1-2+2-1+1-1+2-2+1-2+2-" -e
	# output: xx---++-++--++-+--+-+-+--+-+-++--+-+--++--

Options

==	=======================================
-w	convert {'x' '+' '-'} to {0.0 1.0 -1.0}
-e	expand on single line
==	=======================================


fMRI oriented programs
======================

.. _compute_defined_4dfp:

compute_defined_4dfp
--------------------
generate mask of voxels defined over all frames

Usage:	compute_defined_4dfp <4dfp|conc input>

Options

=======	==================================================
-z		count zero voxels as undefined (default defined)
-f<str>	specify frames-to-count format (default count all)
-F<str>	read frames-to-count format from specified file
=======	==================================================

cs2ap_4dfp
----------
convert cosine and sine amplitude images to amplitude and phase

Usage:	cs2ap_4dfp <(4dfp) cos_img> <(4dfp) sin_img> <(4dfp) outroot>

Options

=======	============================================================
-t<flt>	specify amplitude threshold for phase map (default = 0.0000)
-w<flt>	specify pre-blur FWHM in mm (default = 0.0000)
-@<b|l>	output big or little endian (default input endian)
=======	============================================================

normalize_4dfp
--------------
scale to achieve mode 1000

Usage:	normalize_4dfp <(4dfp) image>

Examples::

	normalize_4dfp -n3 my_run_4dfp
	normalize_4dfp -n3 -v2 my_run_4dfp

Options

=======	===============================================================
-n<int>	specify number of pre-functional frames
-v0		no frame to frame intensity stabilization
-v1		volume based frame to frame intensity stabilization (default)
-v2		slice  based frame to frame intensity stabilization
-s		disable mode=1000 normalization
-z		subtract mean volume from functional frames
-h		create <image>.hist file suitable for plotting, e.g., with xmgr
-a<str>	specify trailer (default="norm")
-m<str>	read specified 4dfp mask (default blur & threshold input image)
-@<b|l>	output big or little endian (default input endian)
=======	===============================================================

deband_4dfp
-----------
correct systematic odd vs. even slice  intensity banding

Usage:	deband_4dfp <(4dfp) image>

Examples::

	deband_4dfp -n3 mybold
 	deband_4dfp -F"3x125+" mybold

Options

=======	=========================================================
-e		deband by exponential gradient model (default flat model)
-g		deband by linear gradient model (default flat model)
-n<int>	specify number of pre-functional frames
-F<str>	specify complete functional/non-functional format
-@<b|l>	output big or little endian (default input endian)
=======	=========================================================

rmspike_4dfp
------------
remove artifact due to k-space DC offset

Usage:	rmspike_4dfp <file_4dfp>

Examples::

	rmspike_4dfp -n3 -x33 test_b1.4dfp.img
	rmspike_4dfp -x33 -F"45(1x6+)" test_b1

Options

=======	==================================================
-n<int>	specify number of anatomy frames
-x<int>	restrict search to specified column
-y<int>	restrict search to specified row
-F<str>	specify whole run functional/non-functional format
-@<b|l>	output big or little endian (default input endian)
=======	==================================================

cross_realign3d_4dfp
--------------------
motion correct fMRI timeseries within and across runs

Usage:	cross_realign3d_4dfp -l4dfp_list_file
		cross_realign3d_4dfp <run1_4dfp> <run2_4dfp>

Examples::

	cross_realign3d_4dfp run1_4dfp run2_4dfp run3_4dfp
	cross_realign3d_4dfp -sqwv -lruns_4dfp.lst
	cross_realign3d_4dfp -pwqsf -n3 -lruns_4dfp.lst

Options

=======	===================================================================
-d		debug mode
-@<b|l>	output big or little endian (default CPU endian)
-f		force recomputing even if output files exist
-g		enable linear intensity gradient compensation
-c		use cross-modal registration always
-l<str>	specify list file of 4dfp filenames
-m<str>	specify 4dfp mask to be applied to all runs (default compute)
-n<int>	specify number of pre-functional frames
-b<flt>	specify pre-blur in reciprocal mm (default=0.06)
-p		2D (planar) realignment (default 3D)
-q		minimize status reporting
-r<int>	specify non-default reference frame
-s		enable stretch
-v[0|1]	disable/enable per frame intensity normalization (default disabled)
-w		enable wrap addressing
-Z		output undefined voxels as 0.0 (default 1.0e-37)
-R		disable resampling
=======	===================================================================

t4_xr3d_4dfp
------------
motion correct and resample in atlas space in one step

Usage:	t4_xr3d_4dfp [options] <t4file> <input_4dfp_stack>

Examples::

	t4_xr3d_4dfp -aatl anat_ave_to_711-2B_t4 b1_rmsp_dbnd

Options

=======	===============================================================
-a<str>	specify outfile name trailer (default = "xr3d")
-c<flt>	scale output by specified factor
-N		output undefined voxels as NaN
-Z		output undefined voxels as 0
-E		output undefined voxels as 1.e-37 (default)
-v[0|1]	set per frame intensity equalization mode (default = OFF)
-@<b|l>	output big or little endian (default input endian)
-f		fast (linear interpolation resample instead of 3D cubic spline)
-e		echo mat file to stdout frame by frame (verbose mode)
-O111	output in 111 space
-O222	output in 222 space
-O333.n	output in 333.n space (y shifted up by n pixels)
-O<str>	output image dimensions according to <str>.4dfp.ifh
=======	===============================================================

N.B.:	default output format = 333.0

.. _mat2dat:

mat2dat
-------
convert cross_realign3d_4dfp mat files to spread sheet format
Usage:	mat2dat <mat_file>

Examples::

	mat2dat atten5_b1_rms4_dbnd_xr3d[.mat]

Options

=============	======================================================================
-I				save trajectory as 4dfp
-R				save trajectory relative to run mean (remove accumulated movememnt)
-D				save differentiated trajectory
-L				write local (in $cwd) (default write parallel to <mat_file>)
-n<int>			specify number of pre steady state frames (default=0)
-l<int>			lowpass filter (< 0.1 Hz) specified motion parameter (counting from 1)
 TR_vol=<flt>	specify TR_vol in sec (required only with option -l)
-r<flt>			specify head radius in mm for total motion computation (default=50mm)
-f<str>			specify frames to count format, e.g., "4x120+4x76+"
=============	======================================================================

N.B.:	-f option overrides -n

frame_align_4dfp
----------------
correct asynchronous slice acquisition

Usage: frame_align_4dfp <(4dfp) input> <frames_to_skip> [options]

Examples::

	frame_align_4dfp bold_run.4dfp.img 4
	frame_align_4dfp bold_run.4dfp.img 4 -TR_vol 2.5
	frame_align_4dfp bold_run.4dfp.img 4 -TR_vol 2.5 -TR_slc .136

Options

=============	========================================================================
-N				enable interleaved order 2,4,6,...,1,3,5,... for even total slice counts
-S				specify sequential slice acquisition (default interleaved)
-d <0|1>		specify slice acquisition direction (0:Inf->Sup; 1:Sup->Inf) (default=0)
-m <int>		specify multi-band factor) (default=1)
-TR_vol <flt>	specify frame TR in sec (default=2.36)
-TR_src <flt>	specify slice TR in sec (default=TR_vol/nslice)
=============	========================================================================

N.B.:	space between option and value

interp_4dfp
-----------
correct asynchronous slice acquisition and resample in time

Usage:	interp_4dfp <(4dfp) image> <TR_vol_in> <TR_slice_in> <TR_vol_out>

Examples::

	interp_4dfp bold_run[.4dfp[.img]] 2.25 .136 2.5

Options

=======	========================================================================
-d<0|1> specify slice acquisition direction (0:Inf->Sup; 1:Sup->Inf) (default=1)
-@<b|l>	output big or little endian (default input endian)
=======	========================================================================

N.B.: if <TR_slice_in> is input as 0 slices are spaced evenly on TR_vol

jitter
------
optimally distribute n events on m frames

Usage:	jitter <(int) nevent> <(int) nframe> <(flt) tr_vol>

Examples::

	jitter 20 100 2.0 -s4

Options

=======	=============================================================================
-r<int>	specify randomization seed (default=0)
-s<int>	add specified number of skip frames to output event series (default=0)
-g<flt>	specify max interval in sec (t_max; default=30.00) (ignored when -F specfied)
-m<flt>	specify min interval in sec (t_min; default=tr_vol)
-o<str>	output named fidl-type event file
-v		verbose mode
-F		use flat distribution of delay intervals (default Poisson process)
=======	=============================================================================

N.B:	nevent must be at least 3

N.B:	first event is ALWAYS on frame skip; last  event is ALWAYS on frame skip + nframe, duration = Inf; fMRI run should include additional frames at end


GLM and related operations
==========================

.. _glm_4dfp:

glm_4dfp
--------
multivariate voxelwise regression/correlation

Usage:	glm_4dfp <format|fmtfile> <profile> <4dfp|conc input>

Examples::

	glm_4dfp "4x124+" doubletask.txt b1_rmsp_dbnd_xr3d_norm

Options

=======	===========================================================================
-Z		supress automatic removal of mean from input regressors
-C<str>	read  partial beta coefficients from specified 4dfp image (default compute)
-o[str]	save  partial beta images with specified trailer (default = "coeff")
-R   	compute  partial beta images as percent modulation
-b[str]	save  total   beta images with specified trailer (default = "tbeta")
-p[str]	save  partial corr images with specified trailer (default = "pcorr")
-t[str]	save  total   corr images with specified trailer (default = "tcorr")
-r[str]	save  residual timeseries with specified trailer (default = "resid")
-@<b|l>	output big or little endian (default input endian)
=======	===========================================================================

N.B.:	conc files must have extension "conc"

N.B.:	<profile> lists temporal profiles (ASCII npts x ncol; '#' introduces comments)

N.B.:	<profile> line limits are 81920 chars and 8192 fields

N.B.:	absent -C, options -o and -r require design matrix inversion; dimension limit 256

actmapf_4dfp
------------
voxelwise evaluate timeseries inner product against reference waveform

Usage:	actmapf_4dfp <format|fmtfile> <4dfp|conc input>

Examples::

	actmapf_4dfp -zu "3x3(11+4x15-)" b1_rmsp_dbnd_xr3d_norm
	actmapf_4dfp -aanatomy -c10 -u "+" ball_dbnd_xr3d.conc
	actmapf_4dfp -zu "4x124+" b1_rmsp_dbnd_xr3d -wweights.txt

Options

===============	=====================================================
-a<str>			specify 4dfp output root trailer (default = "actmap")
-c<flt>			scale output by specified factor
-u				scale weights to unit variance
-z				adjust weights to zero sum
-R				compute relative modulation (default absolute)
-w<weight file>	read (text) weights from specified filename
-@<b|l>			output big or little endian (default input endian)
===============	=====================================================

N.B.:	conc files must have extension "conc"

N.B.:	when using weight files 'x' frames in format are not counted

N.B.:	relative modulation images are zeroed where mean intensity < 0.5*whole_image_mode

t4_actmapf_4dfp
---------------
same functionality as actmapf_4dfp but with simultaneous resampling

GC_4dfp
-------
Granger causality mapping
Usage:	GC_4dfp <format> <4dfp|conc input> <order>

Examples::

	GC_4dfp "4(4x190+)" VB20579_rmsp_faln_dbnd_xr3d_atl.conc 2

Options

=======	=================================================================
-w<str>	specify timecourse profile file (one or more columns)
-i<int>	use only specified column (counting from 1) of timecourse profile
-a<str>	append specifed string to map output
-g		write lagged covariance (gamma) 4dfp stack
-D		write difference of directed influences (Fx->y - Fy->x) map
-Z		write Geweke "N(0,2)" measure (difference of square roots) map
-F		write Fx,y, Fx->y, Fy->x, Fx.y map stack
-@<b|l>	output big or little endian (default input endian)
=======	=================================================================

N.B.:	conc files must have extension "conc"

N.B.:	effective frame count is determined by <format>

N.B.:	'x' frames in format are not counted

GC_dat
------
Granger causality on ASCII column data

Usage:	GC_dat <format> <input_datafile> <order>

Examples::

	GC_dat "4x106+" ROI_timeseries.dat 2

Options

=======	==================================================
-d		debug mode
-v		verbose mode
-u		normalize all input timeseries to unit variance
-x<int>	specify dimensionality of x process (default = 1)
-m		create text listing of AR model
-w		write residual after full AR modeling
-P		format residual output suitable for plotting (xyy)
=======	==================================================

.. _covariance:

covariance
----------
covariance, correlation, coherence, etc. on ASCII column data

Usage:	covariance <format|fmtfile> <profile>

Examples::

	covariance "4x124+" doubletask.txt

Options

=======	===============================================================================
-q		quiet mode
-t		optionally remove trend (ramp) from input timeseries
-u		optionally normalize all input timeseries to unit variance
-o		output lagged CCV dat files (CCR with -u)
-a		output lagged ACV dat file  (ACR with -u)
-r		output Bartlett smoothed cross spectra (spectral density with -u)
-p		output Bartlett smoothed auto  spectra (spectral density with -u)
-e		compute eigenvectors of lag 0 CCV
-L		read ROI labels from <profile> (default ignore '#' commented lines)
-T<int>	additionally smooth spectra with Tukey window of specified width (in frames)
-d<flt>	specify frame TR in sec for Fourier analysis (default = 1.0000)
-m<int>	specify CCV function maxumum lag in frames (default = 32)
-D<flt>	SVD lag 0 CCV and output new profile with cndnum < specified value (implies -e)
-g<str>	regress timeseries in named file out of <profile>
=======	===============================================================================

N.B.:	all input timeseries are made zero mean as a first step

N.B.:	region names can be specified on the first line of <profile> with '#' in the first column

covariance_analysis
-------------------
compute Bartlett correction for autocorrelation fMRI timeseries

Usage:	covariance_analysis <lstfile>


Evaluate and ROI-oriented programs
==================================

peak_4dfp
---------
locate and consolidate maxima to generate ROI

Usage: peak_4dfp <file_4dfp>

Examples::

	peak_4dfp grand_average_222[.4dfp.img] -s10

Options

================	===========================================================================================
-s<flt>				preblur with hard sphere kernel of specified radius (invokes hsphere_4dfp)
-n<int>				limit initial pos and neg peak list lengths (default=1000)
-c<flt>[to<flt>]	specify sign inverted curvature thresholds (default none)
-v<flt>[to<flt>]	specify peak value thresholds (default none)
-d<flt>				consolidate extremum pairs closer than specified distance
-o<flt>				output a fidl compatible 4dfp format ROI file with regions of specified radius
-m<str>				apply named mask file to output ROIs
-N<int>				specify output ROI minimum voxel count (default = 1)
-a<str>				append specified string to ROI output filename
-q					quiet mode (suppress rec file listing)
-F					force preblur image creation even if hsphere_4dfp result exists (no effect without -s<flt>)
-@<b|l>				output big or little endian (default input endian)
================	===========================================================================================

N.B.:	operations controlled by options -s, -n, -c, -v, -d, -o, -m, -N are applied serially in listed order

N.B.:	all distances are in mm

read_4dfp
---------
report value of image at specified real coordinate

Usage:	read_4dfp <flt x0> <flt y0> <flt z0> <4dfp imgroot> [options]

Examples::

	read_4dfp 33.1 -56.2 18. grand_average_222[.4dfp.img]

Options

==	============
-v	verbose mode
==	============

imgmax_4dfp
-----------
report maximum and minimum values

Usage:	imgmax_4dfp <my_image[.4dfp.img]>

Options

==	============================================
-m	report min as well as max
-e	report max/min values in scientific notation
-r	report root sum of squares
-v	verbose (time series) mode
==	============================================

img_hist_4dfp
-------------
construct voxel value histogram; evaluate moments

Usage:	img_hist_4dfp <(4dfp) image>

Options

================	============================================================================
-b<int>				specify number of bins (default = 100)
-f<int>				select volume (counting from 1) of 4dfp stack (default analyze all volumes)
-t<flt>				specify image intensity threshold
-r<flt>[to<flt>]	specify histogram range
-m<(4dfp) mask>		mask input using (non-zero voxels of) specified mask (only first frame used)
-h					create <image>.hist file suitable for plotting, e.g., with xmgr
-p					create <image>.dat  file suitable for input to numerical procedures
-x					create <image>.xtile percentile listing
-C					create output files in $cwd (default parallel to <(4dfp) image>)
-u					normalize output .hist and .dat distributions to unit area
-s					report moments
================	============================================================================

N.B.:	option -f causes selected volume to be reported in filename of -{hpx} created files

.. _qnt_4dfp:

qnt_4dfp
--------
report mean value within 3D ROI

Usage:	qnt_4dfp <(4dfp)|(conc) image> <(4dfp) mask>

Examples::

	qnt_4dfp -t23.2 va1234_mpr mask

Options

================	===============================================================================================
-s					time series mode
-d					include backwards differences (differentiated signal) in output (requires -f or -F, implies -s)
-D					count only defined (finite, non 0.0, non-NaN, non 1.e-37) <image> voxels
-A					apply threshold test to absolute value of <mask>
-W					interpret <mask> as spatial weights (negative values allowed) (disables mask threshold testing)
-v<flt>[to<flt>]	count only <image> voxels within specified range
-f<str>				specify frames to count format, e.g., "4x120+4x76+"
-F<str>				read frames-to-count format from specified file
-p<flt>				specify mask threshold as percent of <mask> max
-t<flt>				specify absolute <mask> threshold (default = 0.0)
-c<flt>				scale output mean values by specified constant (default = 1.0)
================	===============================================================================================

N.B.:	only the first frame of <mask> is used

N.B.:	<image> and <mask> may be the same

N.B.:	conc files must have extension "conc"

qntm_4dfp
---------
evaluate multiple volumes in multiple ROIs

Usage:	qntm_4dfp <(4dfp)|(conc) image> <(4dfp) ROI>

Examples::

	qntm_4dfp TC30274_rmsp_faln_dbnd_xr3d_atl.conc iter10_roi_-02_-37_+27m_ROI

Options

=======	=======================================================
-Z		count zero voxels in <image> as defined
-V		force code_by_volume even if the number of volumes is 1
-N		create ROIs/voxel image
-o<str>	write output to specified text file (default stdout)
-h		suppress printing output header
=======	=======================================================

N.B.:	conc files must have extension "conc"

N.B.:	only defined voxels (not 0.0 and not NaN and not 1.e-37 and finite) are counted

N.B.:	<(4dfp) ROI> may either a value-coded single volume ROI image or a multi-volume mask

N.B.:	<(4dfp) ROI> coded values are integerized

N.B.:	qntm_4dfp ignores <(4dfp) ROI> ifh center and mmppix fields

.. _qntv_4dfp:

qntv_4dfp
---------
evaluate multiple volumes in ROI subdivided into cubes

Usage:	qntv_4dfp <(4dfp)|(conc) image> <(4dfp) ROI>

Examples::

	qntv_4dfp TC30274_rmsp_faln_dbnd_xr3d_atl.conc iter10_roi_-02_-37_+27m_ROI

Options

=======	====================================================================================
-H		include header info in output
-V		print defined voxel counts per die
-D		create die image (voxels >= ncrit)
-K		create die (voxel) coordinate listing
-Z		count zero voxels in <image> as defined
-O<int>	select output type (see below)
-f<str>	specify frames-to-count format (default count all frames)
-F<str>	read frames-to-count format from specified file (supersedes option -f)
-l<int>	specify length of die in voxels (default 1)
-n<int>	specify minimum die voxel count (default 1)
-t<flt>	specify svd output tolerance - ratio of least to greatest eigenvalue (default 1e-06)
-o<str>	write output to specified text file (default stdout)
=======	====================================================================================

-O<int> options

=	==========================================================
1	timeseries directly extracted from dice
2	timeseries extracted from dice with mean removed
3	die timeseries passed through svd multiplied by eigenvalue
4	die timeseries passed through svd (unit variance)
=	==========================================================

N.B.:	conc files must have extension "conc"

N.B.:	only defined voxels (not 0.0 and not NaN and not 1.e-37 and finite) are counted

N.B.:	qntv_4dfp ignores <(4dfp) ROI> ifh center and mmppix fields

N.B.:	to obtain a GLM condition number = X specificy sqrt(1/X) as tol with option -t

qntw_4dfp
---------
evaluate multiple volumes using weighted ROI

Usage:	qntw_4dfp <(4dfp)|(conc) image> <(4dfp) ROI>

Examples::

	qntw_4dfp TC30274_rmsp_faln_dbnd_xr3d_atl.conc iter10_roi_-02_-37_+27m_ROI

Options

=======	====================================================
-L<int>	specify ROI weight L-norm (default = 0)
-o<str>	write output to specified text file (default stdout)
-Z		count zero voxels in <image> as defined
-H		include heaer info in output
=======	====================================================

N.B.:	conc files must have extension "conc"

N.B.:	<(4dfp) ROI> is interpreted as a multi-volume voxel-wise set of weights

N.B.:	only defined voxels (not 0.0 and not NaN and not 1.e-37 and finite) are counted

N.B.:	qntw_4dfp ignores <(4dfp) ROI> ifh center and mmppix fields

.. _var_4dfp:

var_4dfp
--------
evaluate variance or s.d. about mean over timeseries

Usage:	var_4dfp <(4dfp|conc) input>

Examples::

	var_4dfp -sn3 -c10 test_b1_rmsp_dbnd

Options

=======	==================================================================
-d		debug mode
-m		remove mean volume from stack
-s		compute s.d. about mean
-G		compute mean ignoring run boundaries (default within runs)
-v		compute variance about mean (default operation)
-z		output logical and of all defined voxels
-n<int>	specify number of pre-functional frames per run (default = 0)
-f<str>	specify frames to count format, e.g., "4x120+4x76+" (overrides -n)
-F<str>	read format from specified file
-c<flt>	scale output image values by specified factor
-N		output undefined voxels as NaN
-Z		output undefined voxels as 0
-E		output undefined voxels as 1.e-37 (default)
-@<b|l>	output big or little endian (default input endian)
=======	==================================================================

N.B.:	input conc files must have extension "conc"

N.B.:	identically zero input voxels are counted as defined

N.B.:	options {-v -s -m -z} are mutually exclusive

N.B.:	absent -G voxelwise mean is individually computed over each run in conc

N.B.:	-f option overrides -n

.. _dvar_4dfp:

dvar_4dfp
---------
evaluate variance or s.d. about mean over differentiated timeseries

Usage:	dvar_4dfp [options] <stack_4dfp>

Examples::

	dvar_4dfp -n3 test_b1_rmsp_dbnd -mtest_anat_ave_mskt

Options

===============	==================================================
-m<(4dfp) mask>	use specified 4dfp mask
-n<int>			specify number of pre-functional (anatomy) frames
-t<flt>			specify maskfile threshold (default = 0.0)
-b<flt>			specify preblur FWHM in mm (default none)
-s				output sqrt(dvar) (default dvar)
-@<b|l>			output big or little endian (default input endian)
===============	==================================================

burn_sphere_4dfp
----------------
“burn in” sphere at specified real coordinates

Usage:	burn_sphere_4dfp <flt x0> <flt y0> <flt z0> <4dfp imgroot> <4dfp outroot> [options]

Examples::

	burn_sphere_4dfp 33.1 -56.2 18. grand_average_222[.4dfp.img] -v2 -o7.5
	burn_sphere_4dfp 33.1 -56.2 18. 222 -v2 -o7.5

Options

=======	====================================================================================
-a		superimpose sphere on image (default duplicate input format with zero background)
-s		sum overlapping spheres (default overwrite)
-v<flt>	specify burn in value (default=1.0000)
-o<flt>	specify sphere radius in mm (default=6.0000) (radius of 0 creates single pixel burn)
-l<lst>	read sphere coordinates from specified list (command line coords ignored)
-@<b|l>	output big or little endian (default input endian)
=======	====================================================================================

N.B.:	without -a only the input ifh (or standard atlas string) is required

N.B.:	specifying <4dfp imgroot> as "333[.n]" "222" or "111" generates standard atlas space output

N.B.:	if the 4dfp image does not exist the default output endianness is CPU endian

ROI_resolve_4dfp
----------------
resolve a set of possibly overlapping ROIs into a disjoint set

Usage:	ROI_resolve_4dfp <(4dfp) ROI1> <(4dfp) ROI2> <(4dfp) ROI3> ...

Options

=======	================================================
-l<lst>	read input file names from specified list file
-@<b|l>	output big or little endian (default CPU endian)
=======	================================================

N.B.:	output 4dfp fileroots are same as inputs with appended "z"

imgsurf_4dfp
------------
move ROI coordinates to nearest surface

Usage:	imgsurf_4dfp <(4dfp) image> <point_list>

N.B.:	<point_list> lists loci in atlas coordinates (X Y Z) in mm

spatial_corr_4dfp
-----------------
compute image similarity as correlation over space

Usage:	spatial_corr_4dfp <image_x> <mask_x> <image_y> <mask_y> [output_text_file]

Options

=======	=========================================================
-C		compute covariance (default correlation)
-M		suppress removal of patial means
-c<flt>	scale output covariance matrix values by specified factor
=======	=========================================================

N.B.:	image dimensions must match

N.B.:	spatial_corr_4dfp counts only defined (not NaN or 1.e-37 or 0.0) voxels

spatial_cov_multivol_4dfp
-------------------------
compute volume-pair covariance over space

Usage:	spatial_cov_multivol_4dfp <(4dfp) image> <(4dfp) mask>

Options

=======	================================================================
-Z		compute covariance with respect to zero (default wrt image mean)
-p<int>	generate specified number of PCs (default none)
-c<flt>	scale text output covariance matrix values by specified factor
=======	================================================================

N.B.:	spatial_cov_multivol_4dfp counts only defined (not NaN or 1.e-37) voxels

N.B.:	zero voxels are counted as defined in <(4dfp) image> in cov computation

N.B.:	zero voxels are counted as undefined in <(4dfp) image> in <(4dfp) mask>

N.B.:	all zero or all undefined <(4dfp) image> volumes are ignored


SPM-like voxelwise statistical operations
=========================================

t2z_4dfp
--------
t-map :math:`\rightarrow` Z-map

Usage:	t2z_4dfp <(4dfp) t-image>

Examples::

	t2z_4dfp NP705_cond1_zfrm_RFX -nNP705_cond1_N

Options

=======	==================================================
-l		output -log10(prob(t))
-N<flt>	specify global n
-n<str>	specify 4dfp n-image (up to two allowed)
-@<b|l>	output big or little endian (default input endian)
=======	==================================================

N.B.:	undefined (1.e-37, NaN) voxels in input are output as 1.e-37

N.B.:	output values are assigned the same sign as the input t value

N.B.:	the same n values apply to all volumes the input <t-image>

z2logp_4dfp
-----------
Z-map :math:`\rightarrow` log\ :sub:`10`\ p-map

Usage:	z2logp_4dfp <(4dfp) Z-image>

Examples::

	z2logp_4dfp vce20_z[.4dfp[.img]]

Options

=======	==================================================
-2		two sided test (default one sided)
-p		output p-values (default output -log10(p))
-@<b|l>	output big or little endian (default input endian)
=======	==================================================

N.B.:	probability computed on assumption that voxel values are N(0,1)

N.B.:	undefined (1.e-37, NaN, Inf) voxels in input are output as 1.e-37

rho2z_4dfp
----------
r-map :math:`\leftrightarrow` Fisher z-map

Usage:	rho2z_4dfp <(4dfp) image> [outroot]

Examples::

	rho2z_4dfp vce20_rho[.4dfp[.img]]

Options

=======	==================================================
-r		reverse (convert z to r) (output trailer = _corr)
-E		output undefined voxels as 1.0e-37 (default 0.0)
-@<b|l>	output big or little endian (default input-endian)
=======	==================================================

N.B.:	default r to z output filename = <image>_zfrm


DTI
===

dwi_xalign3d_4dfp
-----------------
motion compensation for dwi data (single run)

Usage:	dwi_xalign3d_4dfp <(4dfp) dwi> <(4dfp) mask>

Examples::

	dwi_xalign3d_4dfp hbo08a_dwi1 hbo08a_dwi1_mskt -s -g2-4 -g5,13,18,23

Options

=====================================	==============================================================
-p										planar (2D; disable cross-slice) alignment
-w										enable wrap addressing
-s										enable cross DWI voxel size adjust (principal axis stretch)
-a										compute group arithmeric mean volume (default geometric mean)
-n										zero negative values in output image
-I<int>									specify volume number of I0 counting from 1 (default 1)
-f<flt>									specify pre-blur filter half freq (1/mm) (default none)
-d<flt>									specify sampling interval in mm (default=5.0000)
-i<flt>									specify displacment search radius in mm (default=3.0000)
-j<flt>									specify parameter search object radius in mm (default=40.0000)
-c<int>									specify number of within-group cycles (default=3)
-g<int>[-<int>][,<int>[-<int>]][,...]	program alignment group
=====================================	==============================================================

N.B.:	<(4dfp) mask> may be "none"

N.B.:	I0 should not be named in any programmed alignent group

dwi_cross_xalign3d_4dfp
-----------------------
cross-run motion compensation and averaging of dwi data

Usage:	dwi_cross_xalign3d_4dfp <(4dfp) dwi1> <(4dfp) dwi2> <(4dfp) dwin> ... <(4dfp) dwi_out>

Examples::

	dwi_cross_xalign3d_4dfp -sgmjo_sub2-dwi1_mskt jo_sub2-dwi1 jo_sub2-dwi2 jo_sub2-dwi_all
	dwi_cross_xalign3d_4dfp -sgmjo_sub2-dwi1_mskt -ljo_sub2_dwi.lst jo_sub2-dwi_all

Options

===============	====================================================================
-p				planar (2D; disable cross-slice) alignment
-w				enable wrap addressing
-s				enable cross DWI voxel size adjust (principal axis stretch)
-n				zero negative values in output image
-z<x|y|z><flt>	zoom output x y or z dimension by specified factor
-g				use group geometric mean (\*_geom) volumes for cross-run registration
-a				append successive runs in output (default average)
-m<(4dfp) mask>	specify first volume mask
-I<int>			specify volume number of I0 counting from 1 (default 1)
-f<flt>			specify pre-blur filter half freq (1/mm) (default none)
-d<flt>			specify sampling interval in mm (default=5.0000)
-i<flt>			specify displacment search radius in mm (default=3.0000)
-j<flt>			specify parameter search object radius in mm (default=40.0000)
-l<lst>			read input file names from specified file (use before naming output)
-@<b|l>			output big or little endian (default input endian)
===============	====================================================================

N.B.:	option -I (non-default I0 volume) must be matched according to use of option -g

diff_4dfp
---------
diffusion tensor computation given dwi

Usage:	diff_4dfp <prm_file> <file_4dfp> <opt mask_file> <opt CO_file>

Examples::

	diff_4dfp tp7_params.dat /data/emotion/data3/track_sub3/track_sub3_DTI_avg

Options

=======	==================================================================================
**Computational**
------------------------------------------------------------------------------------------
-N		compute D using nonlinear Levenberg-Marquardt (default log linear LS)
-Z		use nonlinear approach to repair bad voxels from log linear LS
-c		estimate non-mobile diffusion term (applies only to Levenberg-Marquardt algorithm)
-s<int>	compute only selected slice for debugging
-v<int>	compute only selected voxel for debugging
-B<flt>	ignore bad encoding at specified threshold (units = s.d.) (default=3.0)
-b<flt>	ignore encodings with noisy background at specified threshold (default=3.0)
-S<flt>	subtract a fraction of S0 image from data (def=0.1), not compatible with B,b
-C		subtract a CO fraction from data using an imported file, not compatible with B,b
-G<int>	Correct tbi data, =1 CCIR remove encode 1, =2 SLCH remove encode 10,22
------------------------------------------------------------------------------------------
**Masking**
------------------------------------------------------------------------------------------
-m		Use external mask included as third input file
-M		compute threshold mask without holes
-t<flt>	specify mask threshold as fraction of I0 mode (default=0.1000)
-h<flt>	specify minimum I0 mode (default=100.00)
-n<int>	specify number of I0 histogram smoothings (default=4)
------------------------------------------------------------------------------------------
**Output**
------------------------------------------------------------------------------------------
-a<str>	append specified trailer to output fileroots
-p		print out pixel numbers for debugging
-D		output D tensor
-F		output FA (fractional anisotropy)
-E		output eigenvalues
-V[int]	output [specified number of (default=1)] eigenvectors (principal first)
-P		output prolaticity
-R		output single residue volume for model
-r		output squared residue values for all encodings in a separate file
-o		output extra full LM output files (applies only to LM algorithm) (implies -N)
-d		debug mode, provide extra volume of output as needed
-@<b|l>	output big or little endian (default input endian)
=======	==================================================================================

N.B.:	the first data volume must have high SNR from b=0 or low b value

N.B.:	optional output volumes are appended to MD and RA

N.B.:	output order: MD,RA,(Dxx,Dyy,Dzz,Dxy,Dxz,Dyz),(FA),(E123,RD),(CO),(Res),(Evecs),(Prol)

N.B.:	-b and -B are independent but can both be applied

N.B.:	-b requires -m and mask dimensions must match image dimensions

N.B.:	-B, -b parameter useful range is 1.5 to 3

N.B.:	eigenvalue ordering is = Eval1 < Eval2 < Eval3

N.B.:	-c produces an franctional constant output CO = C/(C+S0)

diffRGB_4dfp
------------
dwi :math:`\rightarrow` RGB map

Usage:	diffRGB_4dfp <prm_file> <file_4dfp>

Examples::

	diffRGB_4dfp -t0.5 -qc1.7 tp7_params.dat /data/DTI_avg

Options

=======	==========================================================================
-q		scale intesity by sqrt(Asig) instead of Asig
-G		change color coding to bgr (default rgb)
-c<flt>	specify the intensity scale value (default=1.0000)
-t<flt>	specify mask threshold as fraction of I0 mode (default=0.1000)
-T<str>	specify t4 file used to transform DWI data
-h<flt>	specify minimum I0 mode (default=100.00)
-n<int>	specify number of I0 histogram smoothings (default=4)
-D		input <file_4dfp> is 8 volume diff_4dfp -D output (Dbar, Asigma, D tensor)
-@<b|l>	output big or little endian (default input endian)
=======	==========================================================================

N.B.:	<prm_file> is ignored with -D option

whisker_4dfp
------------
dwi :math:`\rightarrow` whiskers (visualized in Matlab)

Usage:	whisker_4dfp <prm_file> <file_4dfp>

Examples::

	whisker_4dfp tp7_params.dat -dz3 /data/emotion/data3/track_sub3/track_sub3_DTI_avg

Options

==============	==============================================================
-h<flt>			specify minimum I0 mode (default=100.00)
-n<int>			specify number of I0 histogram smoothings (default=4)
-t<flt>			specify mask threshold as fraction of I0 mode (default=0.1000)
-T<str>			specify t4 file used to transform DWI data
-E				additionally output eigenvalues
-3				output 3 eigenvectors scaled by eigenvalue
-d<x|y|z><int>	specify quiver spacing in pixels (default=1)
==============	==============================================================

N.B.:	default output is first two eigenvectors scaled by Asigma


Operations on short int (“Analyze 7.5”) format images
=====================================================

addgrid†
--------
“burn in” grid lines

hard_ellipse†
-------------
“burn in” ellipsoid

2Dhist
------
construct 2D histogram voxel value histogram

usage:	2Dhist <img1> <img2> <2Dhist_out>

Examples::

	2Dhist va1701_tir_ecatt_ANALYZE va1701_mpr_S va1701_tir_mpr_2Dhist
	2Dhist va1701_tir_ecatt_ANALYZE va1701_mpr_S va1701_tir_mpr_2Dhist -r1:0to2500
	2Dhist va1701_tir_ecatt_ANALYZE va1701_mpr_S va1701_tir_mpr_2Dhist -r1:2500

Options

======================	=====================================================
-g						suppress grid burn-in
-r<1|2>:[<int>to]<int>	specify usable voxel value range for <img1> or <img2>
-@<b|l>					output big or little endian (default CPU endian)
======================	=====================================================

N.B.:	2Dhist operates on short int (ANALYZE) format images

fcm_fitgain3d†
--------------
multi-spectral image segmentation using fuzzy class means


† Solaris only
