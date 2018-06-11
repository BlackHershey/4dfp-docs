.. include:: ../global.rst

Dicom utilities
===============

.. _dcm_dump_file:

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
