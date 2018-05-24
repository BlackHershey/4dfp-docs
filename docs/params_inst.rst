.. _params_inst:

-------------------------
Params/Instructions files
-------------------------

Many of the csh scripts take one or two text files as input (called “params” and “instructions”). The two files are for convenience.
You can specify settings all in one file (“params”), or specify some in one (“params”) and more in another (“instructions”). A typical,
convenient use case would be putting subject- or session-specific settings (e.g. patid, scan series numbers, etc.) in the “params” file
and study-specific settings that don’t change across subjects (e.g. target atlas, BOLD scan TR, etc.) in the “instructions” file.

Because the instructions file is sourced after the params file, you can reference settings from the params file in the instructions file:

.. code-block:: csh

    # in params
    set patid = TM201

    # in instructions
    set inpath =  /some/study/path/${patid}

.. TODO: add a blurb about default values for optional variables

Params file
===========

.. code-block:: csh

    # TM201.params

    set patid = TM201
    set mprs = ( 8 )
    set t2ws = ( 10 )
    set irun = ( thumb1 browthumb blink1  brow1 blink2 brow2 thumb2 thumbbrow )
    set fstd = ( 14     16        18      20    22     24    26     28        )
    set sefm = ( 11 12 )

Instructions file
=================

.. code-block:: csh

    # TM_instructions.txt

    @ sorted = 1
    @ economy = 5
    @ go = 1
    @ usescr = 0
    set target = $REFDIR/TRIO_Y_NDC
    @ nx = 72
    @ ny = 72
    set TR_vol = 0.6
    set TE_vol = 33
    set TR_slc = 0
    set delta = ""
    set ped = "y-"
    set dwell = "0.59"
    @ MBfac = 6
    @ epidir = 0
    @ skip = 5
    @ epi2atl = 1
    @ normode = 1
    set tse = ( ${t2ws[1]} ) # for bold pp

    set FDthresh = 0.2
    @ min_frames = 120

    set srcdir = $cwd

    #####################
    # FM unwarping
    #####################
    set datain = /data/nil-bluearc/black/scripts/TicModel_fieldmap_topup_datain.txt
    set FMmag = $srcdir/sefm/${patid}_sefm_mag.nii.gz
    set FMphase = $srcdir/sefm/${patid}_sefm_fieldmap.nii.gz
    set uwrp_cmnd   = /data/gizmo/data1/NEWT_phantom/kqa778_20_vs_64/fmri_unwarp_se.csh
    set rsam_cmnd   = /data/nil-bluearc/benzinger2/Tyler/scripts/one_step_resample.tcsh
