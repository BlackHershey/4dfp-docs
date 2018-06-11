.. include:: ../global.rst

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
