.. include:: global.rst

-------
Scripts
-------

Dicom utilities
===============

.. _dcm_sort:

dcm_sort
--------
sort dicom files by study series (used for flat directory structures)

Usage:	dcm_sort <dicom_directory>

Examples::

	dcm_sort /data/petsun52/data1/JHILL/04271737
	dcm_sort /cdrom/botv/10251349 -p930589002 -c

Options

=======	==============================================================================
-d		verbose debug mode
-c		copy files (default symbolically link)
-t		toggle use of -t in call to dcm_dump_file (default ON)
-i		take files with integer filenames
-e<ext>	take files with specified extension
-r<str>	take files with filenames containing specified string (default is 7 digits)
-p<str>	take files only with dicom field 'PAT Patient Name' matching specified string
=======	==============================================================================

N.B.:	dcm_sort removes existing single study subdirectories

N.B.:	dcm_sort puts unclassifiable DICOMs into subdirectory study0

.. _pseudo_dcm_sort:

pseudo_dcm_sort.csh
-------------------
sort dicom files by study series (used for nested directory structures)

Usage:	pseudo_dcm_sort.csh <dicom directory>

Examples::

	pseudo_dcm_sort.csh RAW

Options

==	==========================================================================================================
-d	debug mode (set echo)
-s	DICOM files are within a subdirectory of numeric subdirectories (default directly in numeric subdirectory)
-e	identify DICOM files by specified extension (default extension = dcm)
-r	identify DICOM files by specified root (default root = 'MR\*')
-i	take DICOM files with integer filenames
-t	toggle off use of -t in call to dcm_dump_file (default ON)
==	==========================================================================================================

N.B.:	dicom subdirectories must be numeric |br|
N.B.:	default subdirectory of numeric subdirectory is 'DICOM'


fMRI oriented scripts
=====================

.. _cross_bold_pp:

cross_bold_pp
-------------
generic EPI (BOLD) pre-processing

Usage:	cross_bold_pp_<version>.csh <params file> [instructions_file]

Examples::

	cross_bold_pp_161012.csh VB16168.params
	generic_cross_bold_pp_090115.csh VB16168.params

.. _cross_bold_pp_161012:

cross_bold_pp_161012.csh
++++++++++++++++++++++++

Required variables

.. list-table::
	:header-rows: 1

	*	- Variable
		- Values
		- Description
	* 	- patid
		- |patid_vals|
		- |patid_desc|
	* 	- irun
		- |irun_vals|
		- |irun_desc|
	* 	- fstd
		- |fstd_vals|
		- |fstd_desc|
	*	- mprs
		- |mprs_vals|
		- |mprs_desc|
	*	- inpath
		- |inpath_vals|
		- |inpath_desc|
	* 	- target
		- |target_vals|
		- |target_desc|
	*	- go
		- |go_vals|
		- |go_desc|
	*	- nx
		- |nx_vals|
		- |nx_desc|
	* 	- ny
		- |ny_vals|
		- |ny_desc|
	*	- skip
		- |skip_vals|
		- |skip_desc|
	*	- TR_vol
		- |TR_vol_vals|
		- |TR_vol_desc|
	*	- TR_slc
		- |TR_slc_vals|
		- |TR_slc_desc|
	*	- epidir
		- |epidir_vals|
		- |epidir_desc|
	* 	- economy
		- |economy_vals|
		- |economy_desc|
	*	- epi2atl
		- |epi2atl_vals|
		- |epi2atl_desc|
	*	- FDthresh
		- |FDthresh_vals|
		- |FDthresh_desc|
	*	- normode
		- |normode_vals|
		- |normode_desc|
	*	- dwell
		- |dwell_vals|
		- |dwell_desc|
	*	- ped
		- |ped_vals|
		- |ped_desc|
	* 	- rsam_cmnd
		- |rsam_cmnd_vals|
		- |rsam_cmnd_desc| (recommended: :ref:`one_step_resample`)

=============	===========================================================================================
Economy value	Files to be removed
=============	===========================================================================================
> 2				original bold run images
> 3				frame-aligned images (_faln)
> 4				cross-realigned avg images (_r3d_avg or xr3d_BC_avg if $BiasField) (only if $epi2atl == 0)
=============	===========================================================================================

Field map correction variables (required)

========    ==============  =======================
Variable    Values          Description
========    ==============  =======================
**Option 1** (:ref:`measured_fm` - gradient echo)
---------------------------------------------------
gre         |gre_vals|      |gre_desc|
delta       |delta_vals|    |delta_desc|
TE_vol      |TE_vol_vals|   |TE_vol_desc|
**Option 2** (:ref:`measured_fm` - spin echo)
---------------------------------------------------
sefm        |sefm_vals|     |sefm_desc|
TE_vol      |TE_vol_vals|   |TE_vol_desc|
**Option 3** (:ref:`mean_fm`)
---------------------------------------------------
FMmean      |FMmean_vals|   |FMmean_desc|
**Option 4** (:ref:`basis_opt_fm`)
---------------------------------------------------
FMmean      |FMmean_vals|   |FMmean_desc|
FMBases     |FMBases_vals|  |FMBases_desc|
========    ==============  =======================

Optional parameters

.. tip:: Although :code:`tse` and :code:`pdt2` are optional, you should specify one or the other if you have them in order to get a better registration to atlas.

.. list-table::
    :widths: 15	5 5 65
    :class: wrap-row
    :header-rows: 1

    *	- Variable
        - Values
        - Default
        - Description
    *	- tse
    	- |tse_vals|
        -
    	- |tse_desc|
    *	- pdt2
    	- |pdt2_vals|
        -
    	- |pdt2_desc|
    * 	- day1_patid
    	- |day1_patid_vals|
        -
    	- |day1_patid_desc|
    *   - day1_path
        - |day1_path_vals|
        -
        - |day1_path_desc|
    *	- scrdir
    	- |scrdir_vals|
        -
    	- |scrdir_desc|
    * 	- sorted
        - |sorted_vals|
        - |sorted_default|
        - |sorted_desc|
    * 	- MB
    	- |MB_skip_vals|
        - |MB_skip_default|
    	- |MB_skip_desc|
    *	- sx
    	- |sx_vals|
        - |sx_default|
    	- |sx_desc|
    * 	- sy
    	- |sy_vals|
        - |sy_default|
    	- |sy_desc|
    * 	- E4dfp
    	- |E4dfp_vals|
        - |E4dfp_default|
    	- |E4dfp_desc|
    *	- use_anat_ave
    	- |use_anat_ave_vals|
        - |use_anat_ave_default|
    	- |use_anat_ave_desc|
    *	- min_frames
    	- |min_frames_vals|
        - |min_frames_crossbold_default|
    	- |min_frames_desc|
    *	- interleave
    	- |interleave_vals|
        -
    	- |interleave_desc|
    *	- seqstr
    	- |seqstr_vals|
        -
    	- |seqstr_desc|
    * 	- MBfac
    	- |MBfac_vals|
        - |MBfac_default|
    	- |MBfac_desc|
    * 	- lomotil
    	- |lomotil_vals|
        -
    	- |lomotil_desc|
    *	- BiasField
    	- |BiasField_vals|
        - |BiasField_default|
    	- |BiasField_desc|
    *	- FDtype
    	- |FDtype_vals|
        - |FDType_default|
    	- |FDtype_desc|
    *	- anat_aveb
    	- |anat_aveb_vals|
        -
    	- |anat_aveb_desc|
    *	- anat_avet
    	- |anat_avet_vals|
        - |anat_avet_default|
    	- |anat_avet_desc| (set excessively high to skip DVARS censoring)
    *	- cross_day_nostretch
    	- |cross_day_nostretch_vals|
        - |cross_day_nostretch_default|
    	- |cross_day_nostretch_desc|
    *	- Gad
    	- |Gad_vals|
        - |Gad_default|
    	- |Gad_desc|

Additional optional variables

.. warning:: Only specify the following variables if the action is desired. They will happen if you specify them at all (even if you set them to 0).

.. list-table::
	:widths: 15	65
	:class: wrap-row
	:header-rows: 1

	*	- Variable
		- Description
	*	- goto_UNWARP
		- |goto_UNWARP_desc|
	*	- epi_zflip
		- |epi_zflip_desc|
	*	- Siemens_interleave
		- |Siemens_interleave_desc|
	*	- nounpack
		- |nounpack_desc|


**Processing steps**

* Convert bold run dicoms to 4dfp format (:ref:`dcm_to_4dfp`)
* Covert mosiac format to volume -- if not $nounpack (:ref:`unpack_4dfp`)
* Correct slice timing and odd/even slice intensities -- if not $MB (:ref:`frame_align_4dfp`, :ref:`deband_4dfp`)
* Motion correction via rigid body transform of each volume to reference frame (:ref:`cross_realign3d_4dfp`)
* Bias field correction -- if $BiasField
* Compute and apply mode 1000 normalization (:ref:`normalize_4dfp`, :ref:`scale_4dfp`)
* Extract/format movement data from on cross_realign3d_4dfp output (:ref:`mat2dat`)
* Extract EPI first frame (anatomy) image (:ref:`paste_4dfp`)
* Make func_vols_ave image with high movement frames removed (FD if $FDthresh and DVARS) (:ref:`actmapf_4dfp`)
* Compute cross-session BOLD atlas transform -- if $day1_patid specified for current patid (:ref:`cross_day_imgreg_4dfp`)
* Convert MPRAGE dicoms to 4dfp format (:ref:`dcm_to_4dfp`)
* Compute MPRAGE atlas transforms (:ref:`mpr2atl1_4dfp` with first mpr if $Gad, otherwise :ref:`avgmpr_4dfp`)
* Compute EPI atlas transform
.. container:: toggle

    .. container:: header

        **(Show/Hide Details)**

    |cross_bold_v16_epi2atl|

* Make atlas transformed EPI anat_ave and t2w in 111, 222, and 333 atlas space (:ref:`t4img_4dfp`)
* Compute field mapping correction (:ref:`fmri_unwarp_170616`)
* Compute and apply unwarped epi to atlas transform (:ref:`imgreg_4dfp`, :ref:`t4_mul`, :ref:`t4img_4dfp`)
* Resample unwarped images ($rsam_cmnd)

cross_bold_pp_130702.csh
++++++++++++++++++++++++

|params_header|

.. list-table::
	:widths: 15	5 65
	:class: wrap-row
	:header-rows: 1

	*	- Variable
		- Values
		- Description
	* 	- patid
		- |patid_vals|
		- |patid_desc|
	* 	- irun
		- |irun_vals|
		- |irun_desc|
	* 	- fstd
		- |fstd_vals|
		- |fstd_desc|
	*	- mprs
		- |mprs_vals|
		- |mprs_desc|
	*	- tse
		- |tse_vals|
		- |tse_desc|
	*	- pdt2
		- |pdt2_vals|
		- |pdt2_desc|
	* 	- day1_patid
		- |day1_patid_vals|
		- |day1_patid_desc|
	*	- gre
		- |gre_vals|
		- |gre_desc|
	*	- FMmean
		- |FMmean_vals|
		- |FMmean_desc|
	*	- FMbases
		- |FMbases_vals|
		- |FMbases_desc|
	*	- FMmag
		- |FMmag_vals|
		- |FMmag_desc|
	* 	- FMphase
		- |FMphase_vals|
		- |FMphase_desc|

|inst_header|

.. list-table::
    :class: wrap-row
    :header-rows: 1

    *	- Variable
    	- Values
    	- Description
    * 	- target
    	- |target_vals|
    	- |target_desc|
    *	- scrdir
    	- |scrdir_vals|
    	- |scrdir_desc|
    * 	- sorted
    	- |sorted_vals|
    	- |sorted_desc|
    * 	- MB
    	- |MB_skip_vals|
    	- |MB_skip_desc|
    *	- sx
    	- |sx_vals|
    	- |sx_desc|
    * 	- sy
    	- |sy_vals|
    	- |sy_desc|
    * 	- E4dfp
    	- |E4dfp_vals|
    	- |E4dfp_desc|
    *	- use_anat_ave
    	- |use_anat_ave_vals|
    	- |use_anat_ave_desc|
    *	- goto_UNWARP
    	- |goto_UNWARP_vals|
    	- |goto_UNWARP_desc|
    *	- min_frames
    	- |min_frames_vals|
    	- |min_frames_desc|
    *	- epi_zflip
    	- |epi_zflip_vals|
    	- |epi_zflip_desc|
    *	- interleave
    	- |interleave_vals|
    	- |interleave_desc|
    *	- Siemens_interleave
    	- |Siemens_interleave_vals|
    	- |Siemens_interleave_desc|
    * 	- MBfac
    	- |MBfac_vals|
    	- |MBfac_desc|
    *	- go
    	- |go_vals|
    	- |go_desc|
    *	- nounpack
    	- |nounpack_vals|
    	- |nounpack_desc|
    *	- nx
    	- |nx_vals|
    	- |nx_desc|
    * 	- ny
    	- |ny_vals|
        - |ny_desc|
    *	- skip
    	- |skip_vals|
    	- |skip_desc|
    *	- TR_vol
    	- |TR_vol_vals|
    	- |TR_vol_desc|
    *	- TR_slc
    	- |TR_slc_vals|
    	- |TR_slc_desc|
    *	- epidir
    	- |epidir_vals|
    	- |epidir_desc|
    * 	- economy
    	- |economy_vals|
    	- |economy_desc|
    *	- epi2atl
    	- |epi2atl_vals|
    	- |epi2atl_desc|
    *	- FDthresh
    	- |FDthresh_vals|
    	- |FDthresh_desc|
    *	- anat_aveb
    	- |anat_aveb_vals|
    	- |anat_aveb_desc|
    *	- anat_avet
    	- |anat_avet_vals|
    	- |anat_avet_desc| (set excessively high to skip DVARS censoring)
    *	- normode
    	- |normode_vals|
    	- |normode_desc|
    *	- cross_day_nostretch
    	- |cross_day_nostretch_vals|
    	- |cross_day_nostretch_desc|
    *	- Gad
    	- |Gad_vals|
    	- |Gad_desc|
    *	- delta
    	- |delta_vals|
    	- |delta_desc|
    * 	- TE_vol
    	- |TE_vol_vals|
    	- |TE_vol_desc|
    *	- dwell
    	- |dwell_vals|
    	- |dwell_desc|
    *	- ped
    	- |ped_vals|
    	- |ped_desc|
    *	- uwrp_cmnd
    	- |uwrp_cmnd_vals|
    	- |uwrp_cmnd_desc|
    * 	- rsam_cmnd
    	- |rsam_cmnd_vals|
    	- |rsam_cmnd_desc|

=============	===========================================================================================
Economy value	Files to be removed
=============	===========================================================================================
> 2				original bold run images
> 3				frame-aligned images (_faln)
> 4				cross-realigned avg images (_r3d_avg or xr3d_BC_avg if $BiasField) (only if $epi2atl == 0)
=============	===========================================================================================

**Processing steps**

* Convert bold run dicoms to 4dfp format (:ref:`dcm_to_4dfp`)
* Covert mosiac format to volume -- if not $nounpack (:ref:`unpack_4dfp`)
* Correct slice timing and odd/even slice intensities -- if not $MB (:ref:`frame_align_4dfp`, :ref:`deband_4dfp`)
* Motion correction via rigid body transform of each volume to reference frame (:ref:`cross_realign3d_4dfp`)
* Compute and apply mode 1000 normalization (:ref:`normalize_4dfp`, :ref:`scale_4dfp`)
* Extract/format movement data from on cross_realign3d_4dfp output (:ref:`mat2dat`)
* Extract EPI first frame (anatomy) image (:ref:`paste_4dfp`)
* Make func_vols_ave image with high movement frames removed (DVARS) (:ref:`actmapf_4dfp`)
* Compute cross-session BOLD atlas transform -- if $day1_patid specified for current patid (:ref:`cross_day_imgreg_4dfp`)
* Convert MPRAGE dicoms to 4dfp format (:ref:`dcm_to_4dfp`)
* Compute MPRAGE atlas transforms (:ref:`mpr2atl1_4dfp` with first mpr if $Gad, otherwise :ref:`avgmpr_4dfp`)
* Compute EPI atlas transform
* Make atlas transformed EPI anat_ave and t2w in 111, 222, and 333 atlas space (:ref:`t4img_4dfp`)
* Compute field mapping correction ($uwrp_cmnd)
* Compute and apply unwarped epi to atlas transform (:ref:`imgreg_4dfp`, :ref:`t4_mul`, :ref:`t4img_4dfp`)
* Resample unwarped images ($rsam_cmnd)
* Make average atlas-aligned, unwarped image (:ref:`actmapf_4dfp`)

cross_bold_pp_121215.csh
++++++++++++++++++++++++

.. list-table::
	:widths: 15	5 65
	:class: wrap-row
	:header-rows: 1

	*	- Variable
		- Values
		- Description
	* 	- patid
		- |patid_vals|
		- |patid_desc|
	* 	- irun
		- |irun_vals|
		- |irun_desc|
	* 	- fstd
		- |fstd_vals|
		- |fstd_desc|
	*	- mprs
		- |mprs_vals|
		- |mprs_desc|
	*	- tse
		- |tse_vals|
		- |tse_desc|
	*	- pdt2
		- |pdt2_vals|
		- |pdt2_desc|
	* 	- day1_patid
		- |day1_patid_vals|
		- |day1_patid_desc|
	*	- day1_path
		- |day1_path_vals|
		- |day1_path_desc|

|inst_header|

.. list-table::
	:class: wrap-row
	:header-rows: 1

	*	- Variable
		- Values
		- Description
	* 	- target
		- |target_vals|
		- |target_desc|
	*	- scrdir
		- |scrdir_vals|
		- |scrdir_desc|
	* 	- sorted
		- |sorted_vals|
		- |sorted_desc|
	*	- nx
		- |nx_vals|
		- |nx_desc|
	* 	- ny
		- |ny_vals|
		- |ny_desc|
	*	- skip
		- |skip_vals|
		- |skip_desc|
	*	- TR_vol
		- |TR_vol_vals|
		- |TR_vol_desc|
	*	- TR_slc
		- |TR_slc_vals|
		- |TR_slc_desc|
	*	- Siemens_interleave
		- |Siemens_interleave_vals|
		- |Siemens_interleave_desc|
	*	- go
		- |go_vals|
		- |go_desc|
	*	- epidir
		- |epidir_vals|
		- |epidir_desc|
	* 	- economy
		- |economy_vals|
		- |economy_desc|
	*	- epi2atl
		- |epi2atl_vals|
		- |epi2atl_desc|
	*	- normode
		- |normode_vals|
		- |normode_desc|
	*	- to_MNI152
		- |to_MNI152_vals|
		- |to_MNI152_desc|

=============	=======================================================
Economy value	Files to be removed
=============	=======================================================
> 2				original bold images
> 3				frame-aligned images (_faln)
> 4				debanded images (_faln_dbnd) (only if $epi2atl == 0)
> 6				normalized images (_norm)
=============	=======================================================

**Processing steps**

* Convert bold run dicoms to 4dfp format (:ref:`dcm_to_4dfp`)
* Covert mosiac format to volume (:ref:`unpack_4dfp`)
* Correct slice timing and odd/even slice intensities (:ref:`frame_align_4dfp`, :ref:`deband_4dfp`)
* Motion correction via rigid body transform of each volume to reference frame (:ref:`cross_realign3d_4dfp`)
* Compute and apply mode 1000 normalization (:ref:`normalize_4dfp`, :ref:`scale_4dfp`)
* Extract/format movement data from on cross_realign3d_4dfp output (:ref:`mat2dat`)
* Extract EPI first frame (anatomy) image (:ref:`paste_4dfp`)
* Move anatomy image (anat_ave) to atlas directory
* Compute cross-session BOLD atlas transform if $day1_patid specified for current patid (:ref:`cross_day_imgreg_4dfp`)
* Convert MPRAGE dicoms to 4dfp format (:ref:`dcm_to_4dfp`)
* Compute MPRAGE atlas transforms (:ref:`avgmpr_4dfp`)
* Compute EPI atlas transform
.. container:: toggle

    .. container:: header

        **(Show/Hide Details)**

    |cross_bold_v12_epi2atl|
* Make atlas transformed EPI anat_ave in 111, 222, and 333 (711-2B or MNI152 if $to_MNI152) atlas space (:ref:`t4img_4dfp`)
* Make cross-realigned atlas-transformed resampled BOLD 4dfp stacks (:ref:`t4_xr3d_4dfp`)

generic_cross_bold_pp_090115
++++++++++++++++++++++++++++

|params_header|

.. list-table::
	:widths: 15	5 65
	:header-rows: 1

	*	- Variable
		- Values
		- Description
	* 	- patid
		- |patid_vals|
		- |patid_desc|
	* 	- irun
		- |irun_vals|
		- |irun_desc|
	* 	- fstd
		- |fstd_vals|
		- |fstd_desc|
	*	- mprs
		- |mprs_vals|
		- |mprs_desc|
	*	- tse
		- |tse_vals|
		- |tse_desc|
	*	- pdt2
		- |pdt2_vals|
		- |pdt2_desc|
	* 	- day1_patid
		- |day1_patid_vals|
		- |day1_patid_desc|
	*	- day1_path
		- |day1_path_vals|
		- |day1_path_desc|

|inst_header|

.. list-table::
	:class: wrap-row
	:header-rows: 1

	*	- Variable
		- Values
		- Description
	* 	- target
		- |target_vals|
		- |target_desc|
	*	- scrdir
		- |scrdir_vals|
		- |scrdir_desc|
	* 	- sorted
		- |sorted_vals|
		- |sorted_desc|
	* 	- MB
		- |MB_skip_vals|
		- |MB_skip_desc|
	*	- sx
		- |sx_vals|
		- |sx_desc|
	* 	- sy
		- |sy_vals|
		- |sy_desc|
	* 	- E4dfp
		- |E4dfp_vals|
		- |E4dfp_desc|
	*	- epi_zflip
		- |epi_zflip_vals|
		- |epi_zflip_desc|
	*	- interleave
		- |interleave_vals|
		- |interleave_desc|
	*	- Siemens_interleave
		- |Siemens_interleave_vals|
		- |Siemens_interleave_desc|
	* 	- MBfac
		- |MBfac_vals|
		- |MBfac_desc|
	*	- go
		- |go_vals|
		- |go_desc|
	*	- nounpack
		- |nounpack_vals|
		- |nounpack_desc|
	*	- nx
		- |nx_vals|
		- |nx_desc|
	* 	- ny
		- |ny_vals|
		- |ny_desc|
	*	- skip
		- |skip_vals|
		- |skip_desc|
	*	- TR_vol
		- |TR_vol_vals|
		- |TR_vol_desc|
	*	- TR_slc
		- |TR_slc_vals|
		- |TR_slc_desc|
	*	- epidir
		- |epidir_vals|
		- |epidir_desc|
	* 	- economy
		- |economy_vals|
		- |economy_desc|
	*	- epi2atl
		- |epi2atl_vals|
		- |epi2atl_desc|
	*	- normode
		- |normode_vals|
		- |normode_desc|
	*	- cross_day_nostretch
		- |cross_day_nostretch_vals|
		- |cross_day_nostretch_desc|
	*	- Gad
		- |Gad_vals|
		- |Gad_desc|

=============	=======================================================
Economy value	Files to be removed
=============	=======================================================
> 2				original bold images
> 3				frame-aligned images (_faln)
> 4				debanded images (_faln_dbnd) (only if $epi2atl == 0)
> 6				normalized images (_norm)
=============	=======================================================

**Processing steps**

* Convert bold run dicoms to 4dfp format (:ref:`dcm_to_4dfp`)
* Covert mosiac format to volume -- if not $nounpack (:ref:`unpack_4dfp`)
* Correct slice timing and odd/even slice intensities -- if not $MB (:ref:`frame_align_4dfp`, :ref:`deband_4dfp`)
* Motion correction via rigid body transform of each volume to reference frame (:ref:`cross_realign3d_4dfp`)
* Compute and apply mode 1000 normalization (:ref:`normalize_4dfp`, :ref:`scale_4dfp`)
* Extract/format movement data from on cross_realign3d_4dfp output (:ref:`mat2dat`)
* Extract EPI first frame (anatomy) image (:ref:`paste_4dfp`)
* Move anatomy image (anat_ave) to atlas directory
* Compute cross-session BOLD atlas transform if $day1_patid specified for current patid (:ref:`cross_day_imgreg_4dfp`)
* Convert MPRAGE dicoms to 4dfp format (:ref:`dcm_to_4dfp`)
* Compute MPRAGE atlas transforms (:ref:`mpr2atl1_4dfp` with first mpr if $Gad, otherwise :ref:`avgmpr_4dfp`)
* Compute EPI to atlas transform
* Make atlas transformed EPI anat_ave in 111, 222, and 333 atlas space (:ref:`t4img_4dfp`)
* Make cross-realigned atlas-transformed resampled BOLD 4dfp stacks (:ref:`t4_xr3d_4dfp`)

.. _fmri_unwarp_170616:

fmri_unwarp_170616.tcsh
-----------------------
distortion correction wrapper script for fMRI preprocessing

.. _measured_fm:

Measured field map
++++++++++++++++++

Usage: fmri_unwarp_170616.tcsh -map	<patid> <epi> <mag> <phase> <dwell> <te> <ped> <delta>

.. list-table::
	:class: wrap-row
	:header-rows: 1

	*	- Variable
		- Values
		- Description
	* 	- patid
		- |patid_vals|
		- |patid_desc|
	* 	- epi
		- |epi_vals|
		- |epi_desc|
	*	- mag
		- |mag_vals|
		- |mag_desc|
	*	- phase
		- |phase_vals|
		- |phase_desc|
	* 	- dwell
		- |dwell_vals|
		- |dwell_desc|
	*	- te
		- |TE_vol_vals|
		- |TE_vol_desc|
	*	- ped
		- |ped_vals|
		- |ped_desc|
	*	- delta
		- |delta_vals|
		- |delta_desc| (required only for gradient-echo field map)

.. _mean_fm:

Mean field map
++++++++++++++
Usage: fmri_unwarp_170616.tcsh -mean	<epi> <FMmean> <epi_to_atl_t4> <dwell> <ped>

.. list-table::
	:class: wrap-row
	:header-rows: 1

	*	- Variable
		- Values
		- Description
	* 	- epi
		- |epi_vals|
		- |epi_desc|
	* 	- FMmean
		- |FMmean_vals|
		- |FMmean_desc|
	*	- epi_to_atl_t4
		- |epi2atl_t4_vals|
		- |epi2atl_t4_desc|
	* 	- dwell
		- |dwell_vals|
		- |dwell_desc|
	*	- ped
		- |ped_vals|
		- |ped_desc|

.. _basis_opt_fm:

basis_opt field map
+++++++++++++++++++

Usage: fmri_unwarp_170616.tcsh -basis <epi> <t2w> <FMmean> <FMbases> <epi_to_t2w_t4> <epi_to_atl_t4> <dwell> <ped> <nbasis> [t2w brain mask]

.. list-table::
	:class: wrap-row
	:header-rows: 1

	*	- Variable
		- Values
		- Description
	* 	- epi
		- |epi_vals|
		- |epi_desc|
	* 	- t2w
		- |t2w_vals|
		- |t2w_desc|
	* 	- FMmean
		- |FMmean_vals|
		- |FMmean_desc|
	* 	- epi_to_t2w_t4
		- |epi2t2w_t4_vals|
		- |epi2t2w_t4_desc|
	*	- epi_to_atl_t4
		- |epi2atl_t4_vals|
		- |epi2atl_t4_desc|
	* 	- dwell
		- |dwell_vals|
		- |dwell_desc|
	*	- ped
		- |ped_vals|
		- |ped_desc|
	* 	- nbasis
		- |nbasis_vals|
		- |nbasis_desc|

N.B.:	with option -basis, basis_opt optimizes the <dwell> value (aka, echo spacing) by default |br|

.. _sefm_pp_AZS:

sefm_pp_AZS.csh
---------------
merge AP/PA into one image and run topup to derive field map

Usage:	sefm_pp_AZS.csh <params file> [instructions file]

Examples::

	sefm_pp_AZS.csh PSQ0001_s1.params ../PSQ_study.params

.. list-table::
	:class: wrap-row
	:header-rows: 1

	*	- Variable
		- Values
		- Description
	*	- patid
		- |patid_vals|
		- |patid_desc|
	* 	- sefm
		- |sefm_vals|
		- |sefm_desc|
	* 	- sorted
		- 1
		- |sorted_desc| (must be sorted)

.. _one_step_resample:

one_step_resample.csh
---------------------
one step resampling with support for bias field correction

Usage:	one_step_resample.csh <parameters file> [instructions]

Examples::

	one_step_resample.csh VB16168.params

Params variables

.. list-table::
	:class: wrap-row
	:header-rows: 1

	*	- Variable
		- Values
		- Description
	*	- patid
		- |patid_vals|
		- |patid_desc|
	* 	- day1_patid
		- |day1_patid_vals|
		- |day1_patid_desc|
	*	- day1_path
		- |day1_path_vals|
		- |day1_path_desc|
	* 	- irun
		- |irun_vals|
		- |irun_desc|

Instructions variables

.. list-table::
	:class: wrap-row
	:header-rows: 1

	*	- Variable
		- Values
		- Description
	* 	- use_anat_ave
		- |use_anat_ave_vals|
		- |use_anat_ave_desc|
	*	- outres
		- |outres_vals|
		- |outres_desc|
	*	- target
		- |target_vals|
		- |target_desc|
	*	- MB
		- |MB_skip_vals|
		- |MB_skip_desc|
	* 	- BiasField
		- |BiasField_vals|
		- |BiasField_desc|



epi2mpr2atlv_4dfp
-----------------
EPI :math:`\rightarrow` T1W :math:`\rightarrow` atlas

Usage:	epi2mpr2atlv_4dfp <epi_anat> <mpr_anat> [useold] [atlas target [711-2? OR -T<target including path>] [-S<atlas space>] [noinit]

Examples::

	epi2mpr2atlv_4dfp stem9_anat_ave stem9_654-3 useold 711-2C
	epi2mpr2atlv_4dfp stem9_anat_ave stem9_654-3 useold -T/data/cninds01/atlas/NP765 -S711-2B

Options

=======	============================================================================
useold	inhibits re-computation of all t4 files
noinit	inhibits reinitialization of epi->mpr t4 files
-T<str>	atlas target (specified string should include path) (default is 711-2B)
-S		specifies the atlas space (requires -T) (currently only 711-2B is supported)
=======	============================================================================

N.B.:	Any image argument may include a path, e.g., /data/petmr1/data7/stem/96_06_14_stem9/stem9_654-3

N.B.:	All named images must be in either ANALYZE or 4dfp format. ANALYZE will be converted to 4dfp


epi2t2w2mpr2atlv_4dfp
---------------------
EPI :math:`\rightarrow` T2W :math:`\rightarrow` T1W :math:`\rightarrow` atlas (8 parameter cross-modal; for “low”  res fMRI)

Usage:	epi2t2w2mpr2atlv_4dfp <epi_anat> <t2w_anat> <mpr_anat> [useold] [atlas_target]

Examples::

	epi2t2w2mpr2atlv_4dfp stem9_anat_ave stem9_643-2 stem9_654-3 useold 711-2Y

N.B.:	Any argument may include a path, e.g., /data/petmr1/data7/stem/96_06_14_stem9/stem9_654-3

N.B.:	All named images must be in either ANALYZE or 4dfp format. ANALYZE will be converted to 4dfp

N.B.:	'useold' instructs epi2t2w2mpr2atlv_4dfp to use existing t4 files

N.B.:	The default atlas_target is 711-2B

.. _epi2t2w2mpr2atl1_4dfp:

epi2t2w2mpr2atl1_4dfp
---------------------
EPI :math:`\rightarrow` T2W :math:`\rightarrow` T1W :math:`\rightarrow` atlas (9 parameter cross-modal; for “high” res fMRI)

Usage:	epi2t2w2mpr2atl1_4dfp <epi_anat> <t2w_anat> <mpr_anat> [useold] [711-2? OR -T<Target including path>] [-S<atlas space>]

Examples::

	epi2t2w2mpr2atl1_4dfp stem9_anat_ave stem9_643-2 stem9_654-3 711-2B
	epi2t2w2mpr2atl1_4dfp stem9_anat_ave stem9_654-3 useold -T/data/cninds01/atlas/NP765 -S711-2B

N.B.:	Any image argument may include a path, e.g., /data/petmr1/data7/stem/96_06_14_stem9/stem9_654-3

N.B.:	All named images must be in 4dfp format

N.B.:	-S specifies the atlas space. The only currently supported atlas space is 711-2B

.. _epi2t2w2mpr2atl2_4dfp:

epi2t2w2mpr2atl2_4dfp
---------------------
EPI :math:`\rightarrow` T2W :math:`\rightarrow` T1W :math:`\rightarrow` atlas (6 parameter cross-modal; best for distorted fMRI)

Usage:	epi2t2w2mpr2atl2_4dfp <epi_anat> <t2w_anat> <mpr_anat> [useold] [711-2? OR -T<Target including path>] [-S<atlas space>]

Examples::

	epi2t2w2mpr2atl2_4dfp stem9_anat_ave stem9_643-2 stem9_654-3 711-2B
	epi2t2w2mpr2atl2_4dfp stem9_anat_ave stem9_654-3 useold -T/data/cninds01/atlas/NP765 -S711-2B

N.B.:	Any image argument may include a path, e.g., /data/petmr1/data7/stem/96_06_14_stem9/stem9_654-3

N.B.:	All named images must be in 4dfp format

N.B.:	-S specifies the atlas space. The only currently supported atlas space is 711-2B

.. _cross_day_imgreg_4dfp:

cross_day_imgreg_4dfp
---------------------
link first session atlas transform to subsequent sessions via EPI “anat_ave”

Usage:	cross_day_imgreg_4dfp <curr_patid> <day1_atlas_path> <day1_patid> <atlas_representative_target> [options]

Examples::

	cross_day_imgreg_4dfp tpj0202 /data/petsun24/data1/tpj0201/atlas tpj0201 711-2Y
	cross_day_imgreg_4dfp tpj0202 /data/petsun24/data1/tpj0201/atlas tpj0201 -T/data/cninds01/data2/ATLAS/ALLEGRA_Y_111

Options

==========	=====================================================
-a<str>		specify image filename trailer (default = "anat_ave")
-nostretch	disable stretch in transform
-setecho	set echo
-S<str>		specify atlas space (default=711-2B)
==========	=====================================================

N.B.:	cross_day_imgreg_4dfp must be run in the current atlas directory

N.B.:	<atlas_representative_target> may be of form 711-2? OR -T/path/image

compute_run_sd1.csh
-------------------
run var_4dfp -sn4 on all bold directories (\*xr3d_norm and \*xr3d_atl) and makes movies

Usage:	compute_run_sd1.csh <patid>

Examples::

	compute_run_sd1.csh VB15792

cross_day_imgreg_4dfp
---------------------
compute cross-session BOLD atlas transform

Usage:	cross_day_imgreg_4dfp <curr_patid> <day1_atlas_path> <day1_patid> <atlas_representative_target> [options]

Examples::

	cross_day_imgreg_4dfp tpj0202 /data/petsun24/data1/tpj0201/atlas tpj0201 711-2Y
	cross_day_imgreg_4dfp tpj0202 /data/petsun24/data1/tpj0201/atlas tpj0201 -T/data/cninds01/data2/ATLAS/ALLEGRA_Y_111

Options

==========	=====================================================
-a<str>		specify image filename trailer (default = "anat_ave")
-nostretch	disable stretch in transform
-setecho	set echo
-S<str>		specify atlas space (default=711-2B)
==========	=====================================================

N.B.:	cross_day_imgreg_4dfp must be run in the current atlas directory

N.B.:	<atlas_representative_target> may be of form 711-2? OR -T/path/image

.. _run_dvar_4dfp:

run_dvar_4dfp
-------------
compute format (identify frames with too much motion) (:ref:`dvar_4dfp` wrapper)

Usage:	run_dvar_4dfp <(conc) concfile> [options]

Options

=======	=========================================================================================
-d 		debug mode
-v 		verbose mode
-p<str> specify printer on which to plot generated .dat.ps file
-P<str>	print previously generated results on specified printer (run on SunOS)
-x<flt>	set frame rejection threshold (default = mode + 2.5*(left s.d.) over non-skipped frames)
=======	=========================================================================================

N.B.:	run_dvar_4dfp is a wrapper for dvar_4dfp

N.B.:	options -b -m -n -t are passed to dvar_4dfp

N.B.:	option  -s is always passed to dvar_4dfp

.. _conc_4dfp:

conc_4dfp
---------
create conc file

Usage:	conc_4dfp <(conc) outroot> <(4dfp) 1> <(4dfp) 2> ...

Examples::

	conc_4dfp vb13157_faln_dbnd_xr3d_atl vb13157_b?_faln_dbnd_xr3d_atl.4dfp.img

Options

=======	==================================================================
-w		supress inclusion of current working directory in listed file path
-l<str>	read input 4dfp list
=======	==================================================================

N.B.:	output conc file always has extension "conc"

N.B.:	only files in or below the current working directory can be correctly addressed

conc_mv
-------
update conc file 4dfp image pointers

Usage:	conc_mv <conc file> <from> <to>

Examples::

	conc_mv TC26851_rmsp_faln_dbnd_xr3d_atl.conc /data/nil-bluearc/raichle/gusnard/np751 auto_evolve/AVI_TEST

Options

==	=======================================
-v 	verbose mode
-t	practice mode (<conc file> not changed)
==	=======================================

conc2format
-----------
compute conc-specific format using a fixed number of pre-steady state frames

Examples::

	conc2format vb13157_faln_dbnd_xr3d_atl.conc 4

Options

==	=================================
-v	verbose mode
-X	label first frame of each run 'X'
==	=================================

RFX2.csh
--------
random effects analysis (1 or 2 groups)

Usage:	RFX2.csh <list_group1> <Nimage_group1> [<list_group2> <Nimage_group2>]

Options

==	=====================================================
-d	debug mode
-R	suppress creation of large rec files (bootstrap mode)
==	=====================================================

N.B.:	<list_group[12]> name 4dfp images on which to run the t-test

N.B.:	<Nimage_group[12]> are 4dfp 'n' images (number of subjects for which each voxel is defined)

N.B.:	If one group is entered a t-test will be run on this group against the null hypothesis of 0

N.B.:	If two groups are entered a t-test will be run comparing the two groups and the computed statistic is Welch's approximate t' (Eqn. 8.11, p. 129 in Zar.)


fcMRI oriented scripts
======================

.. _fcMRI_preproc:

fcMRI_preproc
-------------
fcMRI preprocessing including nuisance variable regression.

.. attention:: :code:`fcMRI_preproc` assumes successful completion of BOLD preprocessing (:ref:`cross_bold_pp`).

Usage:	fcMRI_preproc_<version>.csh <params file> [instructions file]

Examples::

	fcMRI_preproc_161012.csh VB16168.params
	fcMRI_preproc_090115.csh VB16168.params


.. _fcMRI_preproc_161012:

fcMRI_preproc_161012.csh
++++++++++++++++++++++++

Revised version of :ref:`fcMRI_preproc_130715`

**Required variables**

.. list-table::
	:widths: 15	5 65
	:header-rows: 1

	*	- Variable
		- Values
		- Description
	* 	- patid
		- |patid_vals|
		- |patid_desc|
	*	- target
		- |target_vals|
		- |target_desc|
	*	- FSdir
		- |FSdir_vals|
		- |FSdir_desc|
	* 	- srcdir
		- |srcdir_vals|
		- |srcdir_desc|
	* 	- fcbolds
		- |fcbolds_vals|
		- |fcbolds_desc|
	* 	- TR_vol
		- |TR_vol_vals|
		- |TR_vol_desc|
	*	- CSF_lcube
		- |CSF_lcube_vals|
		- |CSF_lcube_desc|
	* 	- CSF_sd1t
		- |CSF_sd1t_vals|
		- |CSF_sd1t_desc|
	*	- CSF_svdt
		- |CSF_svdt_vals|
		- |CSF_svdt_desc|
	*	- WM_lcube
		- |WM_lcube_vals|
		- |WM_lcube_desc|
	*	- WM_svdt
		- |WM_svdt_vals|
		- |WM_svdt_desc|

**Optional variables**

.. list-table::
	:widths: 15	5 5 60
	:header-rows: 1

	*	- Variable
		- Values
		- Default
		- Description
	*	- FCdir
		- |FCdir_vals|
		- |FCdir_default|
		- |FCdir_desc|
	*	- day1_patid
		- |day1_patid_vals|
		-
		- |day1_patid_desc|
	*	- MB
		- |MB_skip_vals|
		- |MB_skip_default|
		- |MB_skip_desc|
	* 	- blur
		- |blur_vals|
		- |blur_default|
		- |blur_desc|
	*	- bpss_params
		- |bpss_params_vals|
		-
		- |bpss_params_desc|
	*	- conc
		- |conc_vals|
		-
		- |conc_desc|
	* 	- fmtfile
		- |fmtfile_vals|
		-
		- |fmtfile_desc|
	*	- FDthresh
		- |FDthresh_vals|
		-
		- |FDthresh_desc|
	* 	- FDtype
		- |FDtype_vals|
		- |FDType_default|
		- |FDtype_desc|
	* 	- anat_aveb
		- |anat_aveb_vals|
		- |anat_aveb_default|
		- |anat_aveb_desc|
	* 	- anat_avet
		- |anat_avet_vals|
		- |anat_avet_default|
		- |anat_avet_desc|
	* 	- min_frames
		- |min_frames_vals|
		- |min_frames_fcpreproc_default|
		- |min_frames_desc|
	*	- CSF_excl_lim
		- |CSF_excl_lim_vals|
		- |CSF_excl_lim_default|
		- |CSF_excl_lim_desc|
	* 	- task_regressor
	  	- |task_regressor_vals|
		-
	  	- |task_regressor_desc|
  	* 	- noGSR
		- |noGSR_vals|
		- |noGSR_default|
		- |noGSR_desc|

**Processing steps**

* Generate FS masks if they don't already exist (results in ../atlas) (:ref:`Generate_FS_Masks_AZS`)
* Create conc file (:ref:`conc_4dfp`) and move it to FCdir
* Compute frame censoring (FD and DVARS) (:ref:`run_dvar_4dfp`) and create avg censored image -- skipped if $fmtfile is specified, DVARS only if no $FDthresh is specified
* Compute defined mask and apply it (:ref:`compute_defined_4dfp`, :ref:`maskimg_4dfp`)
* Compute initial sd1 mean (:ref:`var_4dfp`, :ref:`qnt_4dfp`)
* Make timeseries zero mean (:ref:`var_4dfp`)
* Make movement regressors for each bold run (:ref:`mat2dat`)
* Temporal bandpass filter using $bpss_params (:ref:`bandpass_4dfp`)
* Make whole brain regressors including the 1st derivative (:ref:`qnt_4dfp`)
* Make extra-axial CSF regressors with mask threshold = $CSF_excl_lim (:ref:`maskimg_4dfp`)
* Make venticle movement_regressors (:ref:`qntv_4dfp`)
* Make white matter regressors (:ref:`qntv_4dfp`)
* Paste nuisance regressors together (including task_regressor if supplied)
* Pass final set of nuisance regressors (omitting WB and WB derivative) through :ref:`covariance` -D250
* Remove nuisance regressors out of volumetric time series (:ref:`glm_4dfp`)
* Spatial blur with f_half = $blur if specified (:ref:`gauss_4dfp`)


fcMRI_preproc_140413.csh
++++++++++++++++++++++++
|params_header|

.. list-table::
	:widths: 15	5 65
	:class: wrap-row
	:header-rows: 1

	*	- Variable
		- Values
		- Description
	* 	- patid
		- |patid_vals|
		- |patid_desc|
	* 	- srcdir
		- |srcdir_vals|
		- |srcdir_desc|
	* 	- fcbolds
		- |fcbolds_vals|
		- |fcbolds_desc|

|inst_header|

.. list-table::
	:widths: 15	5 65
	:class: wrap-row
	:header-rows: 1

	*	- Variable
		- Values
		- Description
	* 	- FCdir
	  	- |FCdir_vals|
	  	- |FCdir_desc|
  	* 	- MB
	  	- |MB_skip_vals|
	  	- |MB_skip_desc|
	* 	- conc
	  	- |conc_vals|
	  	- |conc_desc|
  	* 	- task_regressor
	  	- |task_regressor_vals|
	  	- |task_regressor_desc|
  	* 	- noGSR
		- |noGSR_vals|
		- |noGSR_desc|
	* 	- anat_aveb
		- |anat_aveb_vals|
		- |anat_aveb_desc|
	* 	- anat_avet
		- |anat_avet_vals|
		- |anat_avet_desc|
	* 	- FWHM
	  	- |FWHM_vals|
	  	- |FWHM_desc|

**Processing steps**

* Create conc file (:ref:`conc_4dfp`) and move it to FCdir
* Compute defined mask and apply it (:ref:`compute_defined_4dfp`, :ref:`maskimg_4dfp`)
* Compute frame censoring (DVARS) (:ref:`run_dvar_4dfp`)
* Compute initial sd1 mean (:ref:`var_4dfp`, :ref:`qnt_4dfp`)
* Make movement regressors for each bold run (:ref:`mat2dat`)
* Make whole brain regressors including the 1st derivative (:ref:`qnt_4dfp`)
* Make ventricle and bilateral white matter regressors and their derivatives (:ref:`qnt_4dfp`)
* Paste nuisance regressors together (including task_regressor if supplied)
* Remove nuisance regressors out of volumetric time series (:ref:`glm_4dfp`)
* Temporal bandpass filter with bh = .1 and oh = 2 (:ref:`bandpass_4dfp`)
* Spatial blur with f_half = 4.413/$FWHM (:ref:`gauss_4dfp`)

.. _fcMRI_preproc_130715:

fcMRI_preproc_130715.csh
++++++++++++++++++++++++

|params_header|

.. list-table::
	:widths: 15	5 65
	:class: wrap-row
	:header-rows: 1

	*	- Variable
		- Values
		- Description
	* 	- patid
		- |patid_vals|
		- |patid_desc|
	* 	- srcdir
		- |srcdir_vals|
		- |srcdir_desc|
	* 	- fcbolds
		- |fcbolds_vals|
		- |fcbolds_desc|

|inst_header|

.. list-table::
	:widths: 15	5 65
	:class: wrap-row
	:header-rows: 1

	*	- Variable
		- Values
		- Description
	* 	- FCdir
		- |FCdir_vals|
		- |FCdir_desc|
	*	- FSdir
		- |FSdir_vals|
		- |FSdir_desc|
	* 	- MB
		- |MB_skip_vals|
		- |MB_skip_desc|
	* 	- conc
		- |conc_vals|
		- |conc_desc|
	* 	- task_regressor
		- |task_regressor_vals|
		- |task_regressor_desc|
	* 	- noGSR
		- |noGSR_vals|
		- |noGSR_desc|
	* 	- anat_aveb
		- |anat_aveb_vals|
		- |anat_aveb_desc|
	* 	- anat_avet
		- |anat_avet_vals|
		- |anat_avet_desc|
	*	- CSF_lcube
		- |CSF_lcube_vals|
		- |CSF_lcube_desc|
	* 	- CSF_sd1t
		- |CSF_sd1t_vals|
		- |CSF_sd1t_desc|
	*	- CSF_svdt
		- |CSF_svdt_vals|
		- |CSF_svdt_desc|
	*	- WM_lcube
		- |WM_lcube_vals|
		- |WM_lcube_desc|
	*	- WM_svdt
		- |WM_svdt_vals|
		- |WM_svdt_desc|
	* 	- fmtfile
		- |fmtfile_vals|
		- |fmtfile_desc|
	* 	- bpss_params
		- |bpss_params_vals|
		- |bpss_params_desc|
	*	- blur
		- |blur_vals|
		- |blur_desc|

**Processing steps**

* Generate FS masks (results in ../atlas) (:ref:`Generate_FS_Masks_AZS`)
* Create conc file (:ref:`conc_4dfp`) and move it to FCdir
* Compute frame censoring (DVARS) (:ref:`run_dvar_4dfp`) and create avg censored image -- only if no $fmtfile specified
* Compute defined mask and apply it (:ref:`compute_defined_4dfp`, :ref:`maskimg_4dfp`)
* Compute initial sd1 mean (:ref:`var_4dfp`, :ref:`qnt_4dfp`)
* Make timeseries zero mean (:ref:`var_4dfp`)
* Make movement regressors for each bold run (:ref:`mat2dat`)
* Temporal bandpass filter using $bpss_params (:ref:`bandpass_4dfp`)
* Make whole brain regressors including the 1st derivative (:ref:`qnt_4dfp`)
* Make extra-axial CSF regressors (:ref:`maskimg_4dfp`)
* Make venticle movement_regressors (:ref:`qntv_4dfp`)
* Make white matter regressors (:ref:`qntv_4dfp`)
* Paste nuisance regressors together (including task_regressor if supplied)
* Pass final set of nuisance regressors (omitting WB and WB derivative) through :ref:`covariance` -D250
* Remove nuisance regressors out of volumetric time series (:ref:`glm_4dfp`)
* Spatial blur with f_half = $blur if specified (:ref:`gauss_4dfp`)

fcMRI_preproc_090115H.csh
+++++++++++++++++++++++++
Hallquist compliant version of :ref:`fcMRI_preproc_090115`

|params_header|

.. list-table::
	:widths: 15	5 65
	:class: wrap-row
	:header-rows: 1

	*	- Variable
		- Values
		- Description
	* 	- patid
		- |patid_vals|
		- |patid_desc|
	* 	- srcdir
		- |srcdir_vals|
		- |srcdir_desc|
	*	- workdir
		- |workdir_vals|
		- |workdir_desc|
	* 	- fcbolds
		- |fcbolds_vals|
		- |fcbolds_desc|

|inst_header|

.. list-table::
	:widths: 15	5 65
	:class: wrap-row
	:header-rows: 1

	*	- Variable
		- Values
		- Description
	* 	- FCdir
		- |FCdir_vals|
		- |FCdir_desc|
	* 	- MB
		- |MB_skip_vals|
		- |MB_skip_desc|
	* 	- conc
		- |conc_vals|
		- |conc_desc|
	* 	- task_regressor
		- |task_regressor_vals|
		- |task_regressor_desc|
	* 	- noGSR
		- |noGSR_vals|
		- |noGSR_desc|
	* 	- noWM
		- |noWM_vals|
		- |noWM_desc|
	*	- movement_regressors
		- |movement_regressors_vals|
		- |movement_regressors_desc|

**Processing steps**

* Create conc file (:ref:`conc_4dfp`) and move it to FCdir
* Compute defined mask and apply it (:ref:`compute_defined_4dfp`, :ref:`maskimg_4dfp`)
* Compute initial sd1 mean (:ref:`var_4dfp`, :ref:`qnt_4dfp`)
* Spatial blur with f_half = .735452 (:ref:`gauss_4dfp`)
* Temporal bandpass filter with bh = .1 and oh = 2 (:ref:`bandpass_4dfp`)
* Make movement regressors for each bold run (:ref:`mat2dat`) (if $movement_regressors is not "none")
* Make whole brain, ventricle, and bilateral white matter regressors (:ref:`qnt_4dfp`)
* Paste nuisance regressors together (including task_regressor if supplied)
* Pass final set of nuisance regressors (omitting WB and WB derivative) through :ref:`covariance` -D500
* Remove nuisance regressors out of volumetric time series (:ref:`glm_4dfp`)

.. _fcMRI_preproc_090115:

fcMRI_preproc_090115.csh
++++++++++++++++++++++++
|params_header|

.. list-table::
	:widths: 15	5 65
	:class: wrap-row
	:header-rows: 1

	*	- Variable
		- Values
		- Description
	* 	- patid
		- |patid_vals|
		- |patid_desc|
	* 	- srcdir
		- |srcdir_vals|
		- |srcdir_desc|
	*	- workdir
		- |workdir_vals|
		- |workdir_desc|
	* 	- fcbolds
		- |fcbolds_vals|
		- |fcbolds_desc|

|inst_header|

.. list-table::
	:widths: 15	5 65
	:class: wrap-row
	:header-rows: 1

	*	- Variable
		- Values
		- Description
	* 	- FCdir
		- |FCdir_vals|
		- |FCdir_desc|
	* 	- MB
		- |MB_skip_vals|
		- |MB_skip_desc|
	* 	- conc
		- |conc_vals|
		- |conc_desc|
	* 	- task_regressor
		- |task_regressor_vals|
		- |task_regressor_desc|
	* 	- noGSR
		- |noGSR_vals|
		- |noGSR_desc|

**Processing steps**

* Create conc file (:ref:`conc_4dfp`) and move it to FCdir
* Compute defined mask and apply it (:ref:`compute_defined_4dfp`, :ref:`maskimg_4dfp`)
* Compute initial sd1 mean (:ref:`var_4dfp`, :ref:`qnt_4dfp`)
* Spatial blur with f_half = .735452 (:ref:`gauss_4dfp`)
* Temporal bandpass filter with bh = .1 and oh = 2 (:ref:`bandpass_4dfp`)
* Make movement regressors for each bold run (:ref:`mat2dat`)
* Make whole brain, ventricle, and bilateral white matter regressors (:ref:`qnt_4dfp`)
* Paste nuisance regressors together (including task_regressor if supplied)
* Remove nuisance regressors out of volumetric time series (:ref:`glm_4dfp`)

.. _seed_correl:

seed_correl
-----------
compute multi-volume correlation maps

.. attention:: :code:`seed_correl` assumes successful completion of BOLD preprocessing (:ref:`cross_bold_pp`) and fcMRI preprocessing (:ref:`fcMRI_preproc`).

Usage:	seed_correl_<version>.csh <parameters file> [instructions] [options]

Examples::

	seed_correl_161012.csh VB16168.params

.. _seed_correl_161012:

seed_correl_161012.csh
++++++++++++++++++++++

Options

=======	===========================================================
-noblur	analyze unblurred version of preprocessed data
-A		use format in atlas subdirectory (default FCmaps directory)
=======	===========================================================

**Required variables**

.. list-table::
	:widths: 15	5 65
	:header-rows: 1

	*	- Variable
		- Values
		- Description
	* 	- patid
		- |patid_vals|
		- |patid_desc|
	*	- ROIdir
		- |ROIdir_vals|
		- |ROIdir_desc|
	* 	- skip
		- |skip_vals|
		- |skip_desc|

**ROI specification variables (required)**

===========    ==================   =======================
Variable       Values               Description
===========    ==================   =======================
**Option 1**
-----------------------------------------------------------
ROIimg         |ROIimg_vals|        |ROIimg_desc|
**Option 2**
-----------------------------------------------------------
ROIlist        |ROIlist_vals|       |ROIlist_desc|
**Option 3**
-----------------------------------------------------------
ROIlistfile    |ROIlistfile_vals|   |ROIlistfile_desc|
===========    ==================   =======================

**Optional variables**

.. list-table::
	:widths: 15	5 5 60
	:header-rows: 1

	*	- Variable
		- Values
		- Default
		- Description
	*	- FCdir
		- |FCdir_vals|
		- |FCdir_default|
		- |FCdir_desc|
	* 	- MB
		- |MB_skip_vals|
		- |MB_skip_default|
		- |MB_skip_desc|
	*	- blur
		- |blur_vals|
		- |blur_default|
		- |blur_desc|
	*	- bpss_params
		- |bpss_params_vals|
		-
		- |bpss_params_desc|
	* 	- conc
		- |conc_vals|
		-
		- |conc_desc|
	* 	- fmtfile
		- |fmtfile_vals|
		-
		- |fmtfile_desc|

**Processing Steps**

* Create (multi-volume) ROI image if $ROIlistfile or $ROIlist specified (:ref:`paste_4dfp`)
* Calculate seed (ROI) regressors (:ref:`qntm_4dfp`)
* Compute total correlations (:ref:`glm_4dfp`, options: -t)
* Mask total correlation image (_tcorr) by defined voxels (:ref:`maskimg_4dfp`, options: -1)
* Fisher z transform total correlation (:ref:`rho2z_4dfp`)
* Compute zero-lag ROI-ROI correlation matrix (if number of ROIs <= 256) (:ref:`covariance`, options: -u, -o, -m0)

seed_correl_140413.csh
++++++++++++++++++++++

seed_correl_130715.csh
++++++++++++++++++++++

seed_correl_090115.csh
++++++++++++++++++++++

.. _Generate_FS_Masks_AZS:

Generate_FS_Masks_AZS.csh
--------------------------
generate Freesurfer masks for whole brain, WM, GM, CSF

Usage:	Generate_FS_Masks_AZS.csh <parameters file> [instructions]

Examples::

	Generate_FS_Masks_AZS.csh FCS_039_A_1.params ../uwrp_process_Stroke_SMG_Subjects.params

.. list-table::
	:widths: 15	5 65
	:class: wrap-row
	:header-rows: 1

	*	- Variable
		- Values
		- Description
	* 	- patid
		- |patid_vals|
		- |patid_desc|
	*	- FSdir
		- |FSdir_vals|
		- |FSdir_desc|
	*	- target
		- |target_vals|
		- |target_desc|

DTI
===

generic_DWI_script_090219
-------------------------
generic DTI processing

Usage:	generic_DWI_script_090219 params_file [instructions_file]

cross_DWI_imgreg_4dfp
---------------------
atlas transform new data based on previous session results

Usage:	cross_DWI_imgreg_4dfp <curr_dwi> <day1_dwi_path> <day1_dwi> <atlas_representative_target> [options]

Examples::

	cross_DWI_imgreg_4dfp 6770_dwi /data/petsun24/data1/5575 5575_dwi [abspath/]711-2Y

N.B.:	cross_DWI_imgreg_4dfp must be run in the current DWI directory


Anatomical registration scripts
===============================

mpr2atl_4dfp
------------
single T1W :math:`\rightarrow` atlas

Usage:	mpr2atl_4dfp <mpr_anat> [options]

Examples::

	mpr2atl_4dfp vc1234_654-3[.4dfp.img]
	mpr2atl_4dfp vc1234_654-3[.4dfp.img] -T/data/petsun23/data1/atlas/NP345_111[.4dfp.img] -S711-2B

Options

===========================		=======================================================
711-2<C|O|Y|K|L|G|H|V|F> 		specify 711-2? series atlas representative target image
-T<target including path>		specify arbitrary atlas representative target image
-S<atlas space>*				specify atlas space (default=711-2B space)
crossmodal						use cross-modal mpr->target registration
useold							suppress recomputation  of existing t4 file
redo							suppress initialization of existing t4 file
setecho							set echo
===========================		=======================================================

N.B.:	<mpr_anat> may include a path, e.g., /data/petmr1/data7/stem9/scout/654-3 |br|
N.B.:	<mpr_anat> must be in either ANALYZE short int or 4dfp format; ANALYZE will be converted to 4dfp |br|

.. _mpr2atl1_4dfp:

mpr2atl1_4dfp
-------------
T1W :math:`\rightarrow` atlas

Usage:	mpr2atl1_4dfp <mpr_anat> [options]

Examples::

	mpr2atl1_4dfp vc1234_654-3[.4dfp.img]
	mpr2atl1_4dfp vc1234_654-3[.4dfp.img] -T/data/petsun23/data1/atlas/NP345[.4dfp.img]

Options

=========================	===================================================
-T<target including path>	specify arbitrary atlas representative target image
crossmodal					use cross-modal mpr->target registration
useold						suppress recomputation of existing t4 file
redo						suppress t4 file initialization
setecho						set echo
=========================	===================================================

.. _avgmpr_4dfp:

avgmpr_4dfp
-----------
multiple T1W :math:`\rightarrow` atlas

Usage:	avgmpr_4dfp <img1> <img2> ... <avgout> [useold] [711-2<B-Z> OR -T<Target including path>]

Examples::

	avgmpr_4dfp va2345_mpr1 va2345_mpr2 va2345_mpr3 va2345_mpr4 va2345_mpr_n4
	avgmpr_4dfp va2345_mpr1 va2345_mpr2 va2345_mpr3 va2345_mpr4 none

Options

======	=======================================================================================
useold	suppresses unnecessary recomputation of atlas transformation, e.g., <img1>_to_711-2B_t4
======	=======================================================================================

N.B.:	Each named image must be in 4dfp format and acquired in the same subject. Mixed orientations are allowed. Any component image filename may include a unix path.

N.B.:	If <avgout> = "none", t4 and lst files will be generated but averaged images will not.

t2w2mpr_4dfp\ :sup:`*`
----------------------
T2W :math:`\rightarrow` T1W :math:`\rightarrow` atlas

Usage:	t2w2mpr_4dfp <4dfp mprage> <4dfp t2w> [options]

Examples::

	t2w2mpr_4dfp vc6383_130-4 vc6383_130-5

Options

==========	=========================================================
-T<target>	specify atlas target (<target> may include absolute path)
nostretch	disable stretch
setecho		set echo
debug		debug mode
==========	=========================================================

N.B.:	t2w2mpr_4dfp assumes that <4dfp mprage> is in the current working directory
and that its atlas transform, e.g., vc6383_130-4_to_711-2V_t4 exists and is in the current working directory

epi2t1w_4dfp\ :sup:`*`
----------------------
EPI :math:`\rightarrow` T1W

Usage:	epi2t1w_4dfp <4dfp epi> <4dfp t1w> <tarstr>

Examples::

	epi2t1w_4dfp 070630_4TT00280_t1w 070630_4TT00280_anat_ave -T/data/cninds01/data2/atlas/TRIO_Y_NDC

N.B.:	epi2t1w_4dfp assumes that the <4dfp t1w> atlas transform, e.g.,070630_4TT00280_t1w_to_TRIO_Y_NDC_t4
exists and is in the current working directory

N.B.:	<tarstr> is either '711-2?' or '-T/targetpath/target'

make_mprage_avg_4dfp\ :sup:`*`
------------------------------
compute T1W anatomical average for group (list directed)

Usage:	make_mprage_avg_4dfp <study_id> <t4file_list>

Examples::

	make_mprage_avg_4dfp NP659_all NP659_mpr_t4.lst

N.B.:	the output average will be named <study_id>_mpr_avg

N.B.:	make_mprage_avg_4dfp assumes that the MP-RAGE 4dfp image files are in the same directories together their atlas transform t4files

N.B.:	<t4file_list> should list the t4files including path (e.g.:
vc12605c/PROCESSED/vc12605c_949-3_to_711-2Y_t4)

Inputs

t4file_list
	ls vc?????c/PROCESSED/\*t4 | awk '$1 !~/anat/' >! <t4file_list>


msktgen_4dfp\ :sup:`*`
----------------------
create tailored mask (by inversion of atlas transform)

Usage:	msktgen_4dfp <(4dfp) image> [threshold] -T<target including path>  -S<atlas space>

Examples::

	msktgen_4dfp 4859-5_mpr
	msktgen_4dfp 4859-5_mpr -T/data/petsun29/data1/atlas/NP345_111[.4dfp.img] -S711-2B

Options

===============	======================================================================================================
threshold		specify threshold for mask (default is 200) - use a higher threshold for a tighter mask and vice versa
-S<atlas space>	specify atlas space (default=711-2B space)
-T<target>		specify atlas target
===============	======================================================================================================

N.B.:	msktgen_4dfp uses the first legitimate atlas transform t4 file it sees in
	the current working directory, i.e., one of <image>_to_711-2*_t4 or  one of <image>_to_<target>_t4

cross_mpr_imgreg_4dfp\ :sup:`*`
-------------------------------
compute cross-session T1W atlas transform

Usage:	cross_mpr_imgreg_4dfp <session1_abspath> <session2_abspath> <target>

Examples::
	cross_mpr_imgreg_4dfp /data/disk1/P44W_16800_L1 /data/disk2/P44W_16800_L2 711-2L
	cross_mpr_imgreg_4dfp /data/disk1/P44W_16800_L1 /data/disk2/P44W_16800_L2 /bmr01/01/nmrgrp/avi/P44W_C_111

N.B.:	<target> may be of the form '711-2[B-Z]' OR '-T[mypath/]mytarget'

N.B.:	cross_mpr_imgreg_4dfp assumes that each session patid is <sessionpath>:t

newatl_init_4dfp\ :sup:`*`
--------------------------
initialize creation of a cohort-specific atlas-representative target image

.. Attention:: After successful execution, execute newatl_refine_4dfp

Usage:	newatl_init_4dfp <t4list> <newatl>

Examples::

	newatl_init_4dfp symph-mpr_to_711-2B_t4.lst 711-2S

Options

==	=================================================================================================
-m	mask all input images (each input image must be paired with a 4dfp mask named <input_image>_mask)
==	=================================================================================================

N.B.:	<t4list> is a text file listing the absolute addresses of extant atlas transform
	t4files corresponding to a set of structural images

N.B.:	<newatl> specifies the name of the new atlas representative target image

N.B.:	<t4list> itself and the t4files named in it may exist in other directories

N.B.:	all images (\*.4dfp.img and \*.4dfp.ifh) referred to in <t4list> must exist
	either in their original directory or in the current working directory -
	newatl_init_4dfp will copy these images into the cwd as necessary

newatl_refine_4dfp\ :sup:`*`
----------------------------
refine cohort-specific atlas-representative target image

.. Attention:: Execute after successful completion of newatl_init_4dfp

Usage:	newatl_refine_4dfp	<newatl>

Examples::

	newatl_refine_4dfp 711-2S

Options

=======	==================================================================================================
-b		suppress gauss 1.1 pre-blur of component images
-m		mask all input images ((each input image must be paired with a 4dfp mask named <input_image>_mask)
-T<str>	set reference target to specified image (default = /data/petsun43/data1/atlas/711-2B)
=======	==================================================================================================

N.B.:	<newatl> specifies the name of the new atlas representative target image

N.B.:	all images (\*.4dfp.img and \*.4dfp.ifh) referred to in <newatl>.lst must exist in the working directory


Miscellaneous scripts
=====================

freesurfer2mpr_4dfp
-------------------
transform freesurfer generated images back to atlas space

Usage:	freesurfer2mpr_4dfp <(4dfp) mpr> <(4dfp) orig> [options]

Examples::

	freesurfer2mpr_4dfp vc1234_654-3[.4dfp.img] vc1234_orig
	freesurfer2mpr_4dfp vc1234_654-3 vc1234_orig -T711-2V -alh.ribbon.mgz -arh.ribbon apply

Options

==========	=============================================================================
-skew		general affine orig->mpr registeration (default 6 parameter rigid body)
-T<target>	specify atlas representative target
-a<segimg>	add named (4dfp format) freesurfer segemntation result to "apply" list
apply		proceed directly to transform (4dfp format) segmentations
force		force atlas transformation of segmentation results even if it already exists
setecho		set echo
==========	=============================================================================

N.B.:	<(4dfp) orig> is the freesurfer-resampled 256x256x256 coronal mpr |br|
N.B.:	the default "apply" list includes (4dfp format) images named \*parc\* and \*aseg\* |br|

.. tip:: You must convert the freesurfer-created mgz (i.e. orig, aparc+aseg) images to 4dfp before running this script. To convert mgz to 4dfp::

	$ mri_convert orig.mgz orig.nii --out-orientation RAS
	$ nifti_4dfp orig.nii orig.4dfp.img

split_ROIs
----------
split peak_4dfp ROI image into multiple mask images

Usage:	split_ROIs <4dfp ROI file> [start_ROI_number] [end_ROI_number] [options]

Examples::

	split_ROIs sum_condition_time_anova_ROI[.4dfp[.img]] 0 82

Options

========================	===================================================================
[<start|end>_ROI_number]	ROI numbers count from 0 (fidl convention) (defaults are 0, nROI-1)
-x							flip ROI to contralateral hemisphere
-0							name output mask file by ROI number (default name as in ifh)
-d<int>						specify difference between ROI number and voxel value (default 2)
========================	===================================================================

N.B.:	split_ROIs output files are put into the subdirectory ./single_ROIs

brec
----
parse rec file by procedure depth

Usage:	brec <my_file[.rec]> [-depth_limit]


.. [*] assumes pre-existing atlas-transform t4 file
