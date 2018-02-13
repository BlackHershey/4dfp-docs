.. csh script instruction file data dictionary
.. |var_header| replace:: Script variables (to be included in :ref:`instructions`)

.. |anat_aveb_vals| replace:: <flt>
.. |anat_aveb_desc| replace:: run_dvar_4dfp preblur in mm

.. |anat_avet_vals| replace:: <flt>
.. |anat_avet_desc| replace:: run_dvar_4dfp_criterion

.. |blur_vals| replace:: <flt>
.. |blur_desc| replace:: f_half for spatial blur (no blurring if unspecified)

.. |conc_vals| replace:: <str>
.. |conc_desc| replace:: pre-existing conc file to use

.. |CSF_excl_lim_vals| replace:: <flt>
.. |CSF_excl_lim_desc| replace:: (default = .126)

.. |delta_vals| replace:: <flt>
.. |delta_desc| replace:: difference between field map echo times (ms)

.. |dwell_vals| replace:: <flt>
.. |dwell_desc| replace:: EPI dwell time/echo spacing (ms)

.. |economy_vals| replace:: 1-6
.. |economy_desc| replace:: level of removal for intermediate files created during execution (higher economy will remove more files)

.. |epi2atl_vals| replace:: 0,1,2
.. |epi2atl_desc| replace:: if EPI to atlas transform is required (0 = no transform, 1 = transform to 333 space, 2 = proceed to t4_xr3d_4dfp)

.. |epidir_vals| replace:: 0,1
.. |epidir_desc| replace:: direction of EPI slices (0 = inferior to superior, 1 = superior to inferior)

.. |FCdir_vals| replace:: <str>
.. |FCdir_desc| replace:: (default=FCmaps, will be made if doesn't already exist)

.. |FDthresh_vals| replace:: <flt>
.. |FDthresh_desc| replace:: frame displacement thresholds

.. |FDtype_vals| replace:: 1,2
.. |FDtype_desc| replace:: ??

.. |fmtfile_vals| replace:: <str>
.. |fmtfile_desc| replace:: format file (if unspecified, frame censoring will be calculated)

.. |FWHM_vals| replace:: <int>
.. |FWHM_desc| replace:: full-width half maximum for spatial blur (default = 6)

.. |go_vals| replace:: 0,1
.. |go_desc| replace:: if calls should be executed (if 0, statements will only be printed, not executed)

.. |MB_vals| replace:: 0,1
.. |MB_desc| replace:: enable slicing timing correction and debanding (choices=0,1)

.. |MBfac_vals| replace:: <int>
.. |MBfac_desc| replace:: multiband factor (default = 1)

.. |min_frames_vals| replace:: <int>
.. |min_frames_desc| replace:: minimum number of remaining frames after scrubbing for participant to be included

.. |movement_regressors_vals| replace:: raw,bpss,none
.. |movement_regressors_desc| replace:: (default="bpss")

.. |noGSR_vals| replace:: 0,1
.. |noGSR_desc| replace:: suppress global signal (WB) regression

.. |noWM_vals| replace:: 0,1
.. |noWM_desc| replace:: supress WM regression

.. |normode_vals| replace:: 0,1
.. |normode_desc| replace:: if per-frame volume intensity should be modified

.. |nx_vals| replace:: <int>
.. |nx_desc| replace:: number of voxels on the x-axis

.. |ny_vals| replace:: <int>
.. |ny_desc| replace:: number of voxels on the y-axis

.. |onestep_vals| replace:: 0,1
.. |onestep_desc| replace:: exit program at end of each step

.. |ped_vals| replace:: x,x-,y,y-,z,z-
.. |ped_desc| replace:: EPI phase encoding direction (default = y-)

.. |skip_vals| replace:: <int>
.. |skip_desc| replace:: number of pre-steady state frames

.. |sorted_vals| replace:: 0,1
.. |sorted_desc| replace:: if dcm sort already been run (if 0, dcm_sort will be run)

.. |target_vals| replace:: <str>
.. |target_desc| replace:: atlas to be used for alignment

.. |task_regressor_vals| replace:: <str>
.. |task_regressor_desc| replace:: ??

.. |TE_vol_vals| replace:: <int>
.. |TE_vol_desc| replace:: echo time (ms)

.. |TR_slc_vals| replace:: <flt>
.. |TR_slc_desc| replace:: time per slice (s)

.. |TR_vol_vals| replace:: <flt>
.. |TR_vol_desc| replace:: time per frame (s)

.. |usescr_vals| replace:: <str>
.. |usescr_desc| replace:: if a scratch directory should be used
