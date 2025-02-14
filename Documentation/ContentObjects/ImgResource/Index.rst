..  include:: /Includes.rst.txt
..  index:: Content objects; IMAGE_RESOURCE
..  _cobj-img-resource:

=============
IMG\_RESOURCE
=============

Objects of type IMG_RESOURCE returns a reference to an image, possibly
wrapped with :ref:`cobj-img-resource-stdWrap`. It can be used, for example,
for putting background images in tables or
table rows or to import an image in your own include scripts.
Depending on your use case you might prefer using the cObject
:ref:`IMAGE <cobj-image>`, which creates a complete :html:`img` tag.

..  contents::
    :local:

Properties
==========

..  _cobj-img-resource-file:

file
----

..  confval:: file

    :Data type: :ref:`->imgResource <imgresource>`


..  _cobj-img-resource-stdWrap:

stdWrap
-------

..  confval:: stdWrap

    :Data type: :ref:`->stdWrap <stdwrap>`
