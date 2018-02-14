--------
Appendix
--------

Input file formats (csh)
========================

.. _instructions:

Instructions file
-----------------

The instruction file specifies optional customizations for how the script should execute for a particular study.
Variables are defined using csh syntax.

.. note:: Variables will default to 0 or the empty string if not specified (excluding those with defaults explicitly mentioned above).

Example

.. code-block:: csh

    # instructions.txt

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

.. _params:

Params file
-----------

The params file specifies variables required for processing a specific participant (subject id, scan numbers, etc.).
It is specified per participant, using csh syntax for variable declaration.

.. code-block:: csh

    # TM201.params

    set patid = TM201
    set mprs = ( 8 )
    set t2ws = ( 10 )
    set irun = ( thumb1 browthumb blink1  brow1 blink2 brow2 thumb2 thumbbrow )
    set fstd = ( 14     16    18    20      22    24     26    28 )
    set sefm = ( 11 12 )
