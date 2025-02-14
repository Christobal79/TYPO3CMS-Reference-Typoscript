..  include:: /Includes.rst.txt
..  _DatabaseQueryProcessor:

======================
DatabaseQueryProcessor
======================

The :php:`DatabaseQueryProcessor` fetches records from the database, using
standard TypoScript :ref:`select` semantics. The result is then passed to the
:ref:`cobj-fluidtemplate` as an array.

This way a :ref:`cobj-fluidtemplate` cObject can iterate over the
array of records.


Options:
========

..  _DatabaseQueryProcessor-if:

..  confval:: if

    :Required: false
    :Data type: :ref:`if` condition
    :default: ''

    Only if the condition is met the data processor is executed.


..  _DatabaseQueryProcessor-table:

..  confval:: table

    :Required: true
    :Data type: :ref:`data-type-string` / :ref:`stdWrap`
    :default: ''

    Name of the table from which the records should be fetched.

..  _DatabaseQueryProcessor-as:

..  confval:: as

    :Required: false
    :Data type: :ref:`data-type-string` / :ref:`stdWrap`
    :default: 'records'

    The variable's name to be used in the Fluid template.

..  _DatabaseQueryProcessor-dataProcessing:

..  confval:: dataProcessing

    :Required: false
    :Data type: array of :ref:`dataProcessing`
    :default: []

    Array of data processors to be applied to all fetched records.

..  note::
    All other options will be interpreted as in the TypoScript function
    :typoscript:`select`, including :typoscript:`pidInList`,
    :typoscript:`orderBy`, :typoscript:`where`, etc. See the reference of
    :ref:`select`.

..  warning::
    When using the DatabaseQueryProcessor, you may encounter issues with
    language and/or versioning overlays, that currently can not be resolved.
    See `here <https://forge.typo3.org/issues/85284#note-5>`__ for more
    information.


Example: Display haiku records
==============================

Please see also :ref:`dataProcessing-about-examples`.

TypoScript
----------

We define the :typoscript:`dataProcessing` property to use the
:php:`DatabaseQueryProcessor`:

..  include:: /CodeSnippets/DataProcessing/TypoScript/DatabaseQueryProcessor.rst.txt

..  versionadded:: 12.1
    One can use the alias :typoscript:`database-query` instead
    of the fully-qualified class name
    :php:`\TYPO3\CMS\Frontend\DataProcessing\DatabaseQueryProcessor`.


The Fluid template
------------------

In the Fluid template then iterate over the records. As we used the recursive
data processor :ref:`FilesProcessor` on the image records, we can also output
the images.

..  include:: /CodeSnippets/DataProcessing/Template/DataProcDb.rst.txt


Output
------

Each entry of the records array contains the data of the table in :php:`data`
and the data of the images in :php:`files`.

..  figure:: /Images/ManualScreenshots/FrontendOutput/DataProcessing/DatabaseProcessor.png
    :class: with-shadow
    :alt: Haiku record data dump and output

    Haiku record data dump and output
