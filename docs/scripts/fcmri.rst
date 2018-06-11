.. include:: ../global.rst

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

**Required parameters**

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

**Optional parameters**

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

**Required parameters**

.. list-table::
	:widths: 15	5 60
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
	* 	- TR_vol
		- |TR_vol_vals|
		- |TR_vol_desc|
	*	- skip
		- |skip_vals|
		- |skip_desc|
	* 	- fcbolds
		- |fcbolds_vals|
		- |fcbolds_desc|


**Optional parameters**

.. list-table::
	:widths: 15	5 5 65
	:header-rows: 1

	*	- Variable
		- Values
		- Default
		- Description
	* 	- FCdir
		- |FCdir_vals|
		- |FCdir_default|
		- |FCdir_desc|
	* 	- anat_aveb
		- |anat_aveb_vals|
		- 10
		- |anat_aveb_desc|
	* 	- anat_avet
		- |anat_avet_vals|
		- 7
		- |anat_avet_desc|
	* 	- FWHM
	  	- |FWHM_vals|
		- |FWHM_default|
	  	- |FWHM_desc|
	* 	- MB
	  	- |MB_skip_vals|
		- |MB_skip_default|
	  	- |MB_skip_desc|
	* 	- conc
		- |conc_vals|
		-
		- |conc_desc|
	* 	- task_regressor
	  	- |task_regressor_vals|
		-
	  	- |task_regressor_desc|
	* 	- noGSR
		- |noGSR_vals|
		- |noGSR_default|
		- |noGSR_desc|

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
