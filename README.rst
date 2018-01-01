----
4dfp
----

The 4dfp (4-dimensional floating point) format was designed for functional neuroimaging. The four dimensions typically correspond to x, y, z, and time. Three dimensional structural images can be represented in 4dfp format by setting the depth of the fourth dimension to 1.


.. note:: NIFTII and 4dfp images are ALWAYS y-flipped to each other. Be sure to use 4dfp tools to convert back and forth, so that this is accounted for.