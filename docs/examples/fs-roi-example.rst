.. _fs-roi-example:

Creating a Freesurfer ROI Mask
------------------------------

This example will walk you through how to transform your Freesurfer segmentation into atlas space
and extract specific anatomical regions.

Transforming a Freesurfer segmentation to atlas space
+++++++++++++++++++++++++++++++++++++++++++++++++++++

If you ran recon-all using a native-space T1 image and did not specify a custom atlas,
you will need to transform your Freesurfer segmentation to your atlas space.
The command to do this is :ref:`freesurfer2mpr_4dfp`.

.. note:: If you have run :ref:`fcMRI_preproc_161012` on your subjects, you can skip this step as it has already been done.

As shown in the usage information for the script, we will need to supply the Freesurfer :code:`orig` and :code:`aparc+aseg`
files in 4dfp format. Since Freesurfer outputs images in mgz format, we will first need to convert the required images to
4dfp. There is no direct conversion from mgz to 4dfp, so we will use nifti as an intermediate format.

.. code-block:: csh

    $ cd /path/to/project
    $ cd NEWT002_s1/atlas

    $ mri_convert /path/to/fsdir/NEWT002_s1/mri/orig.mgz NEWT002_s1_orig.nii --out_orientation RAS
    $ nifti_4dfp -4 NEWT002_s1_orig.nii NEWT002_s1_orig.4dfp.img

    $ mri_convert /path/to/fsdir/NEWT002_s1/mri/aparc+aseg.mgz NEWT002_s1_aparc+aseg.nii --out_orientation RAS
    $ nifti_4dfp -4 NEWT002_s1_aparc+aseg.nii NEWT002_s1_aparc+aseg.4dfp.img

The script relies on an existing transform from the subject's MPRAGE to the target atlas. Because you don't
supply the script with a t4 file, it looks in the atlas directory for a t4 file of the form
<(4dfp) mpr>_to_<target>_t4, where <(4dfp) mpr> is the MPRAGE supplied as the first argument (sans the
4dfp extension) and <target> is the value supplied to the -T flag.

From the subject's atlas directory, we can now run the script - making sure to use the MPRAGE and
atlas that match the existing t4.

.. code-block:: csh

    $ ls *mpr*to*_t4
    NEWT002_s1_mpr1_to_TRIO_KY_NDC_t4

    $ freesurfer2mpr_4dfp NEWT002_s1_mpr1.4dfp.img NEWT002_s1_orig.4dfp.img -TTRIO_KY_NDC

When it's done, you should have the atlas-aligned aparc+aseg in 111, 222, and 333 space in your atlas folder.

.. code-block:: csh

    $ ls *aparc+aseg_on*.4dfp.img
    NEWT002_s1_aparc+aseg_on_TRIO_KY_NDC_111.4dfp.img
    NEWT002_s1_aparc+aseg_on_TRIO_KY_NDC_222.4dfp.img
    NEWT002_s1_aparc+aseg_on_TRIO_KY_NDC_333.4dfp.img


Creating ROI masks
++++++++++++++++++

Once you have the atlas-aligned segmentation image, you can create region masks by
masking the image using the Freesurfer-assigned region value (which can be found in the
`Freesurfer lookup table <https://surfer.nmr.mgh.harvard.edu/fswiki/FsTutorial/AnatomicalROI/FreeSurferColorLUT>`_).

To extract a particular region, we would use :ref:`zero_ltgt_4dfp` to zero out all the voxels that are not equal to
that region's value. If we wanted the left Amygdala, we would use the following:

.. code-block:: csh

    $ zero_ltgt_4dfp 18to18 NEWT002_s1_aparc+aseg_on_TRIO_KY_NDC_333 NEWT002_s1_lAmygdala

If we wanted a binary mask of both the left and right Amydala, then we would need to follow the steps above again,
this time using the value for the right Amygdala. Then, we would need to combine them together using the :code:`add`
operation of :ref:`imgopr_4dfp`. Finlly, we would use :ref:`maskimg_4dfp` with the :code:`-v1` flag to binarize the values.

.. code-block:: csh

    $ zero_ltgt_4dfp 54to54 NEWT002_s1_aparc+aseg_on_TRIO_KY_NDC_333 NEWT002_s1_rAmygdala
    $ imgopr_4dfp -aNEWT002_s1_amygdala NEWT002_s1_lAmygdala NEWT002_s1_rAmygdala
    $ maskimg_4dfp -v1 NEWT002_s1_amygdala NEWT002_s1_amygdala NEWT002_s1_amygdala_msk
