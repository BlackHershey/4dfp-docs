File structure
--------------

The voxel data are stored in the form of a binary image as one UNIX file.
Consequently, 4dfp images may be directly loaded and viewed using IDL, matlab, fsleyes, etc. Information critical to interpreting the binary data (e.g., orientation, image dimensions, voxel dimensions) are stored in separate header file(s).
The 4dfp UNIX file name convention is demonstrated below, where filename is any valid filename string::

	<filename>.4dfp.img		# binary float voxel data
	<filename>.4dfp.ifh		# interfile header (ASCII text)
	<filename>.4dfp.hdr		# ANALYZE 7.5 header (binary)
	<filename>.4dfp.img.rec		# creation history

All 4dfp based image analysis programs used at the Washington University School of Medicine Neuroimaging Laboratory (NIL) read/write interfile headers. The minimal 4dfp format is comprised of the binary image data (.img) and the interfile header (.ifh). All NIL image analysis programs maintain an additional rec file (.img.rec), which records the image creation history.

The voxel data are stored in the form of a binary image as one UNIX file.
Consequently, 4dfp images may be directly loaded and viewed using IDL, matlab, fsleyes, etc. Information critical to interpreting the binary data (e.g., orientation, image dimensions, voxel dimensions) are stored in separate header file(s).
The 4dfp UNIX file name convention is demonstrated below, where filename is any valid filename string::

	<filename>.4dfp.img		# binary float voxel data
	<filename>.4dfp.ifh		# interfile header (ASCII text)
	<filename>.4dfp.hdr		# ANALYZE 7.5 header (binary)
	<filename>.4dfp.img.rec		# creation history

All 4dfp based image analysis programs used at the Washington University School of Medicine Neuroimaging Laboratory (NIL) read/write interfile headers. The minimal 4dfp format is comprised of the binary image data (.img) and the interfile header (.ifh). All NIL image analysis programs maintain an additional rec file (.img.rec), which records the image creation history.


Image data
==========

The 4dfp file format was developed to manage human head images acquired with a Siemens MRI scanner. The imato4dfp utility converts Siemens slice-based image (.ima) files into 4dfp format. This is accomplished by extracting (without reordering) the stored pixel values, converting short int to to float and writing the results to the 4dfp image file. Within each 3D volume the Siemens .ima files are read in order of decreasing image number.

Certain 4dfp conventions, especially regarding image orientation, reflect the Siemens Numaris operating system. For all acquisitions, including
oblique and double oblique, Numaris determines the principal orientation.
It is this orientation that appears in the interfile header and to which following table refers. The orientation-dependent axis flips required to display 4dfp image data in conventional radiologic orientation are tabulated below. It is assumed that the display is written left-to-right and bottom-to-top as in analyze_avw. It is also assumed that the image was acquired with the subject positioned in the scanner head-first and supine.

====================	=========	============================
Acquired orientation	Flip axes	First voxel (after flipping)
====================	=========	============================
2 (axial)		y		right occipital skull-base
3 (coronal)		y z		right skull-base occipital
4 (sagittal)		x y z		occipital skull-base right
====================	=========	============================

The above enumerated axis flips may be effected in analyze_avw at 4dfp image load time. Alternatively, 4dfp format may be converted to ANALYZE 7.5 format using the 4dfptoanalyze utility which automatically performs the indicated axis flips as it converts the voxel value number format from float to short int. 4dfp image data loaded into analyze_avw as prescribed above will be consistently resliced by analyze_avw. That is, all Siemens acquisition orientations will be consistently displayed in all analyze orientations (transverse, coronal, sagittal).

.. note:: x, y, and z denote voxel indices as ordered in memory. That is, if the x, y, and z indices run from 0 to nx-1, 0 to ny-1, and 0 to nz-1, then coordinate (x,y,z) is stored relative to the first voxel of each frame at offset x + nx*(y + ny*z).


Interfile header
================

The following is a listing of a 4dfp interfile header file (vm6c_b1.4dfp.ifh).
The image data (vm6c_b1.4dfp.img) were acquired in one 128 frame fMRI run.
Each frame has dimensions 64 x 64 x 18, The acquired voxels are 3 mm cubic. ::

	version of keys			:= 3.3
	number format			:= float
	conversion program		:= nifti_4dfp
	name of data file		:= T1w_acpc_dc.4dfp.ifh
	number of bytes per pixel	:= 4
	imagedata byte order 		:= littleendian
	orientation 			:= 2
	number of dimensions		:= 4
	matrix size [1]			:= 260
	matrix size [2] 		:= 311
	matrix size [3] 		:= 260
	matrix size [4] 		:= 1
	scaling factor (mm/pixel) [1]	:= 0.700000
	scaling factor (mm/pixel) [2]	:= 0.700000
	scaling factor (mm/pixel) [3]	:= 0.700000
	mmppix				:= 0.700000	-0.700000 	-0.700000
	center				:= 92.0000	-91.7000	-110.0000


Various image analysis programs may make use of additional interfile header fields. The minimal set of fields required to interpret the voxel data is listed below::

	number format			:= float
	number of bytes per pixel	:= 4
	orientation			:= 3
	number of dimensions		:= 4
	scaling factor (mm/pixel) [1]	:= 3.000000
	scaling factor (mm/pixel) [2]	:= 3.000000
	scaling factor (mm/pixel) [3]	:= 3.000000
	matrix size [1]			:= 64
	matrix size [2]			:= 64
	matrix size [3]			:= 18
	matrix size [4]			:= 128


rec file
========

The rec file format was designed to capture the creation history of each
particular 4dfp image. This is accomplished automatically provided that each UNIX executable which creates 4dfp output also produces a corresponding rec file. Rec files are ASCII text with the following format ::

	rec <filename>.4dfp.img `date` `user`
	UNIX command line which created <filename>.4dfp.img
	rcs $Id$ (program revision code)
	image/program specific processing information
	...
	rec file[s] corresponding to antecedent input 4dfp images
	endrec `date` `user`

The critical feature of the rec file convention is inclusion of antecedent rec files at all stages of processing. It follows that rec files corresponding to averaged images may grow large. The key words "rec" (first field of first line) and "endrec" (first field of last line) guarantee secure parsing of the accumulated processing history. The following is a listing of the rec file corresponding to the above illustrated interfile header after being passed through rmspike_4dfp and deband_4dfp ::

	rec vm6c_b1_rmsp_dbnd.4dfp.img  Thu May 18 17:16:23 2000  avi
	/data/petsun4/data1/solaris/deband_4dfp -n4 vm6c_b1_rmsp
	$Id: deband_4dfp.c,v 1.8 1999/11/20 00:55:49 avi Exp $
	Frame          1 slice multipliers: even=0.837060 odd=1.162940
	Frame          2 slice multipliers: even=0.997099 odd=1.002901
	Frame          3 slice multipliers: even=0.985484 odd=1.014516
	Frame          4 slice multipliers: even=0.986583 odd=1.013417
	Functional frame slice multipliers: even=0.986982 odd=1.013018
	rec vm6c_b1_rmsp.4dfp.img  Thu May 18 17:16:13 2000 avi
	/data/petsun4/data1/solaris/rmspike_4dfp -n4 -x33 vm6c_b1
	$Header: /data/petsun4/src_solaris/rmspike_4dfp/RCS/rmspike_4dfp.c,v 2.6 1997/05/23 00:49:24 yang Exp $
	No spike found in vm6c_b1.4dfp.img
	rec vm6c_b1.4dfp.img  Thu May 18 17:15:18 2000  avi
	/data/petsun4/data1/solaris/imato4dfp2 -fy /data/petsun23/vm6c/siem_im/bold1/5250 7 7 vm6c_b1
	$Id: imato4dfp2.c,v 1.12 2000/05/05 00:56:18 avi Exp $
	patient_id:		vm6c
	institution:		Washington University
	manufacturer_model:	MAGNETOM VISION
	parameter_file_name:	Initialized by sequence
	sequence_file_name:	/usr/users/tec/nbea_uc_tg2.ekc
	sequence_description:	ep_fid   90	TR    135.2	TE   37.0/1
	tilts:			Cor>Tra -12
	4dfp_dimensions:	64        64        18        128
	voxel_dimensions:	3.000000  3.000000  3.000000
	scan_date:		22-FEB-1999
	scan_time:		14:06:33-14:06:33
	endrec Thu May 18 17:15:18 2000  avi
	endrec
	endrec Thu May 18 17:16:26 2000  avi

The brec (beautify rec file) utility parses rec files and writes to stdout a more easily readable version of the text. Here is the above rec file filtered through brec ::

	1rec vm6c_b1_rmsp_dbnd.4dfp.img  Thu May 18 17:16:23 2000  avi
	1      /data/petsun4/data1/solaris/deband_4dfp -n4 vm6c_b1_rmsp
	1      $Id: deband_4dfp.c,v 1.8 1999/11/20 00:55:49 avi Exp $
	1      Frame          1 slice multipliers: even=0.837060 odd=1.162940
	1      Frame          2 slice multipliers: even=0.997099 odd=1.002901
	1      Frame          3 slice multipliers: even=0.985484 odd=1.014516
	1      Frame          4 slice multipliers: even=0.986583 odd=1.013417
	1      Functional frame slice multipliers: even=0.986982 odd=1.013018
	2      rec vm6c_b1_rmsp.4dfp.img  Thu May 18 17:16:13 2000 avi
	2            /data/petsun4/data1/solaris/rmspike_4dfp -n4 -x33 vm6c_b1
	2            $Header: /data/petsun4/src_solaris/rmspike_4dfp/RCS/rmspike_4dfp.c,v 2.6 1997/05/23 00:49:24 yan
	2            No spike found in vm6c_b1.4dfp.img
	3            rec vm6c_b1.4dfp.img  Thu May 18 17:15:18 2000  avi
	3                  /data/petsun4/data1/solaris/imato4dfp2 -fy /data/petsun23/vm6c/siem_im/bold1/5250 7 7 vm6c
	3                  $Id: imato4dfp2.c,v 1.12 2000/05/05 00:56:18 avi Exp $
	3                  patient_id:	vm6c
	3                  institution:		Washington University
	3                  manufacturer_model:	 MAGNETOM VISION
	3                  parameter_file_name:	 Initialized by sequence
	3                  sequence_file_name:	/usr/users/tec/nbea_uc_tg2.ekc
	3                  sequence_description:	ep_fid   90     TR    135.2     TE   37.0/1
	3                  tilts:		Cor>Tra -12
	3                  4dfp_dimensions:		64        64        18        128
	3                  voxel_dimensions:	3.000000  3.000000  3.000000
	3                  scan_date:	22-FEB-1999
	3                  scan_time:	14:06:33-14:06:33
	3            endrec Thu May 18 17:15:18 2000  avi
	2      endrec
	1endrec Thu May 18 17:16:26 2000  avi
