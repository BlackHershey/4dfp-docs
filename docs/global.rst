.. role:: required
.. |req| replace:: :required:`*`

.. |br| raw:: html

    </br>

.. csh script params file data dictionary
.. |params_header| replace:: :ref:`params` variables

.. |day1_path_vals| replace:: <str>
.. |day1_path_desc| replace:: path to day1 atlas directory (if patid is not patient's first session)

.. |day1_patid_vals| replace:: <str>
.. |day1_patid_desc| replace:: patient directory for first session (if patid is not patient's first session)

.. |fcbolds_vals| replace:: <array>
.. |fcbolds_desc| replace:: list of bold run folders

.. |FMbases_vals| replace:: <img>
.. |FMbases_desc| replace:: ??

.. |FMmag_vals| replace:: <img>
.. |FMmag_desc| replace:: field map magnitude image

.. |FMmean_vals| replace:: <img>
.. |FMmean_desc| replace:: mean field map image

.. |FMphase_vals| replace:: <img>
.. |FMphase_desc| replace:: field map phase image

.. |fstd_vals| replace:: <int array>
.. |fstd_desc| replace:: list of scan numbers that map to run folders

.. |gre_vals| replace:: <int array>
.. |gre_desc| replace:: gradient echo measured field map scan numbers (magnitude image should be first, followed by phase image)

.. |irun_vals| replace:: <str array>
.. |irun_desc| replace:: list of run folders

.. |mprs_vals| replace:: <int array>
.. |mprs_desc| replace:: list of mprage scan numbers

.. |patid_vals| replace:: <str>
.. |patid_desc| replace:: unique identifier for current subject/session

.. |pdt2_vals| replace:: <int array>
.. |pdt2_desc| replace:: list containing one study number for ptd

.. |ROIdir_vals| replace:: <str>
.. |ROIdir_desc| replace:: directory containing ROI image(s)

.. |ROIimg_vals| replace:: <str>
.. |ROIimg_desc| replace:: base filename for single ROI image

.. |ROIlist_vals| replace:: <str array>
.. |ROIlist_desc| replace:: list of base filenames for ROI images

.. |ROIlistfile_vals| replace:: <str>
.. |ROIlistfile_desc| replace:: list file (.lst) of base filenames for ROI image

.. |sefm_vals| replace:: <int array>
.. |sefm_desc| replace:: spin echo measured field maps

.. |srcdir_vals| replace:: <str>
.. |srcdir_desc| replace:: source directory path (contains run directories)

.. |tse_vals| replace:: <int array>
.. |tse_desc| replace:: list of tse scan numbers

.. |workdir_vals| replace:: <str>
.. |workdir_desc| replace:: working directory path

.. csh script instruction file data dictionary
.. |inst_header| replace:: :ref:`instructions` variables

.. |anat_aveb_vals| replace:: <flt>
.. |anat_aveb_desc| replace:: run_dvar_4dfp preblur in mm

.. |anat_avet_vals| replace:: <flt>
.. |anat_avet_desc| replace:: run_dvar_4dfp criterion

.. |BiasField_vals| replace:: 0,1
.. |BiasField_desc| replace:: perform bias field correction

.. |blur_vals| replace:: <flt>
.. |blur_desc| replace:: f_half for spatial blur (no blurring if unspecified)

.. |bpss_params_vals| replace:: <str>
.. |bpss_params_desc| replace:: additional options to use for bandpass filtering (-E,M,F already specified)

.. |conc_vals| replace:: <str>
.. |conc_desc| replace:: pre-existing conc file to use

.. |cross_day_nostretch_vals| replace:: 0,1
.. |cross_day_nostretch_desc| replace:: disable stretch for cross-day transform

.. |CSF_excl_lim_vals| replace:: <flt>
.. |CSF_excl_lim_desc| replace:: mask threshold for CSF (default = .126)

.. |delta_vals| replace:: <flt>
.. |delta_desc| replace:: difference between field map echo times (ms)

.. |dwell_vals| replace:: <flt>
.. |dwell_desc| replace:: EPI dwell time/echo spacing (ms)

.. |E4dfp_vals| replace:: 0,1
.. |E4dfp_desc| replace:: if 4dfp files already exist (skips :ref:`dcm_to_4dfp`)

.. |economy_vals| replace:: <int>
.. |economy_desc| replace:: level of removal for intermediate files created during execution (higher economy will remove more files)

.. |epi2atl_vals| replace:: 0,1,2
.. |epi2atl_desc| replace:: if EPI to atlas transform is required (0 = no transform, 1 = transform to 333 space, 2 = skip to resampling step)

.. |epi_zflip_vals| replace:: 0,1
.. |epi_zflip_desc| replace:: flip z when unpacking (:ref:`unpack_4dfp`)

.. |epidir_vals| replace:: 0,1
.. |epidir_desc| replace:: direction of EPI slices (0 = inferior to superior, 1 = superior to inferior)

.. |FCdir_vals| replace:: <str>
.. |FCdir_desc| replace:: output directory name (default = FCmaps)

.. |FDthresh_vals| replace:: <flt>
.. |FDthresh_desc| replace:: frame displacement thresholds

.. |FDtype_vals| replace:: 1,2
.. |FDtype_desc| replace:: frame displacement calculation (1 = absolute value, 2 = squares)

.. |fmtfile_vals| replace:: <str>
.. |fmtfile_desc| replace:: format file

.. |FWHM_vals| replace:: <int>
.. |FWHM_desc| replace:: full-width half maximum for spatial blur (default = 6)

.. |Gad_vals| replace:: 0,1
.. |Gad_desc| replace:: if gadolinium contrast was used

.. |go_vals| replace:: 0,1
.. |go_desc| replace:: if calls should be executed (if 0, statements will only be printed, not executed)

.. |goto_UNWARP_vals| replace:: 1
.. |goto_UNWARP_desc| replace:: immediately go to unwarp step (will happen if variable is defined)

.. |inpath_vals| replace:: <str>
.. |inpath_desc| replace:: starting directory (usually subject directory)

.. |interleave_vals| replace:: -S
.. |interleave_desc| replace:: sequential slice acquisition (:ref:`frame_align_4dfp`)

.. |lomotil_vals| replace:: <int>
.. |lomotil_desc| replace:: lowpass filter specified motion parameter (:ref:`mat2dat`)

.. |MB_skip_vals| replace:: 0,1
.. |MB_skip_desc| replace:: skip slice timing correction and debanding

.. |MBfac_vals| replace:: <int>
.. |MBfac_desc| replace:: multiband factor (default = 1)

.. |min_frames_vals| replace:: <int>
.. |min_frames_desc| replace:: minimum number of remaining frames after scrubbing for participant to be included (default = 240)

.. |movement_regressors_vals| replace:: raw,bpss,none
.. |movement_regressors_desc| replace:: (default="bpss")

.. |noGSR_vals| replace:: 0,1
.. |noGSR_desc| replace:: suppress global signal (WB) regression

.. |noWM_vals| replace:: 0,1
.. |noWM_desc| replace:: supress WM regression

.. |normode_vals| replace:: 0,1
.. |normode_desc| replace:: if per-frame volume intensity should be modified

.. |nounpack_vals| replace:: 0,1
.. |nounpack_desc| replace:: skips unpacking step

.. |nx_vals| replace:: <int>
.. |nx_desc| replace:: number of voxels on the x-axis

.. |ny_vals| replace:: <int>
.. |ny_desc| replace:: number of voxels on the y-axis

.. |onestep_vals| replace:: 0,1
.. |onestep_desc| replace:: exit program at end of each step

.. |ped_vals| replace:: x,x-,y,y-,z,z-
.. |ped_desc| replace:: EPI phase encoding direction (default = y-)

.. |rsam_cmnd_vals| replace:: <str>
.. |rsam_cmnd_desc| replace:: script to use for resampling

.. |scrdir_vals| replace:: <str>
.. |scrdir_desc| replace:: scratch directory to be used if desired

.. |Siemens_interleave_vals| replace:: 0,1
.. |Siemens_interleave_desc| replace:: enables Siemens interleave order (:ref:`frame_align_4dfp`)

.. |skip_vals| replace:: <int>
.. |skip_desc| replace:: number of pre-steady state frames

.. |sorted_vals| replace:: 0,1
.. |sorted_desc| replace:: if dcm sort already been run (if 0, dcm_sort will be run)

.. |sx_vals| replace:: <int>
.. |sx_desc| replace:: unpacked x-dimension squeeze factor (:ref:`unpack_4dfp`)

.. |sy_vals| replace:: <int>
.. |sy_desc| replace:: unpacked y-dimension squeeze factor (:ref:`unpack_4dfp`)

.. |target_vals| replace:: <img>
.. |target_desc| replace:: atlas to be used for alignment

.. |task_regressor_vals| replace:: <str>
.. |task_regressor_desc| replace:: optional externally supplied task regressor

.. |TE_vol_vals| replace:: <int>
.. |TE_vol_desc| replace:: echo time (ms)

.. |to_MNI152_vals| replace:: 0,1
.. |to_MNI152_desc| replace:: transform to MNI152 atlas space

.. |TR_slc_vals| replace:: <flt>
.. |TR_slc_desc| replace:: time per slice (s)

.. |TR_vol_vals| replace:: <flt>
.. |TR_vol_desc| replace:: time per frame (s)

.. |use_anat_ave_vals| replace:: 0,1
.. |use_anat_ave_desc| replace:: use _anat_ave epi image (default is _func_vols_ave)

.. |uwrp_cmnd_vals| replace:: <str>
.. |uwrp_cmnd_desc| replace:: script to use for unwarping
