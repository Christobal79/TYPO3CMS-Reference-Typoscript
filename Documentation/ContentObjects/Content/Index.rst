..  include:: /Includes.rst.txt
..  index::
    Content objects; CONTENT
    CONTENT
    Content objects; SQL
    Content objects; Database
..  _cobj-content:

=======
CONTENT
=======

An object with the content type CONTENT is designed to generate content by allowing to
finely select records and have them rendered.

What records are visible is controlled by `start` and `end` fields and
more standard fields automatically. The internal value `SYS_LASTCHANGED`
is raised to the maximum timestamp value of the respective records.

.. seealso::

   The cObject :ref:`RECORDS <cobj-records>` in contrast is for displaying
   lists of records from a variety of tables without fine graining.


.. contents::
   :local:

Properties
==========

..  _cobj-content-select:

select
------

..  confval:: select

    :Data type: :ref:`select`

    The SQL-statement, a :sql:`SELECT` query, is set here,
    including automatic visibility control.


..  _cobj-content-table:

table
-----

..  confval:: table

    :Data type:  *table name* / :ref:`stdwrap`

    The table, the content should come from. Any table can be used;
    a check for a table prefix is not done.

    In standard configuration this will be :sql:`tt_content`.


.. _cobj-content-renderObj:

renderObj
---------

..  confval:: renderObj

    :Data type: :ref:`data-type-cObject`
    :Default: :typoscript:`< [table name]`

    The cObject used for rendering the records resulting from the query in
    :ref:`cobj-content-select`.

    If :typoscript:`renderObj` is not set explicitly, then
    :typoscript:`< [table name]` is used. So
    in this case the configuration of the according :ref:`cobj-content-table`
    is being copied.

    See the notes on the example below.


..  _cobj-content-slide:

slide
-----

..  confval:: slide

    :Data type: :ref:`data-type-integer` / :ref:`stdWrap`

    If set and no content element is found by the select command, the
    rootLine will be traversed back until some content is found.

    Possible values are:

    :typoscript:`-1`
        Slide back up to the site root.

    :typoscript:`1`
        Only the current level.

    :typoscript:`2`
        Up from one level back.

    Use :typoscript:`-1` in combination with :ref:`collect <cobj-content-slide-collect>`.


..  _cobj-content-slide-collect:

..  confval:: slide.collect

    :Data type: :ref:`data-type-integer` / :ref:`stdWrap`

    If set, all content elements found on the current and parent pages will be
    collected. Otherwise, the sliding would stop after the first hit. Set this
    value to the amount of levels to collect on, or use :typoscript:`-1`
    to collect up to the site root.


..  _cobj-content-slide-collectFuzzy:

..  confval:: slide.collectFuzzy

    :Data type: :ref:`data-type-boolean` / :ref:`stdWrap`

    Only useful with :ref:`slide.collect <cobj-content-slide-collect>`. If no content
    elements have been found for the specified depth in collect mode, traverse
    further until at least one match has occurred.


..  _cobj-content-slide-collectReverse:

..  confval:: slide.collectReverse

    :Data type: :ref:`data-type-boolean` / :ref:`stdWrap`

    Reverse order of elements in collect mode. If set, elements of the current
    page will be at the bottom.


..  _cobj-content-wrap:

wrap
----

..  confval:: wrap

    :Data type: :ref:`wrap <data-type-wrap>` / :ref:`stdWrap`

    Wrap the whole content.


..  _cobj-content-stdWrap:

stdWrap
-------

..  confval:: stdWrap

    :Data type: :ref:`stdWrap`

    Apply `stdWrap` functionality.


..  _cobj-content-cache:

cache
-----

..  confval:: cache

    :Data type: :ref:`cache <cache>`

    See :ref:`cache function description <cache>` for details.


Examples
========

CONTENT explained in detail
---------------------------

See PHP class :php:`\TYPO3\CMS\Frontend\ContentObject\ContentContentObject`
for details on code level.

..  code-block:: typoscript
    :caption: EXT:site_package/Configuration/TypoScript/setup.typoscript

    1 = CONTENT
    1 {
       if {
       }
       table = tt_content
       select {
          pidInList = this
          where = colPos = 1
          orderBy = sorting
       }
       renderObj = < tt_content
       slide = 0
       slide {
          collect = 0
          collectReverse = 0
          collectFuzzy = 0
       }
       wrap =
       stdWrap =
    }

Expanded form:

..  code-block:: typoscript
    :caption: EXT:site_package/Configuration/TypoScript/setup.typoscript

    1 = CONTENT

    // STEP 1: do nothing if 'if' evaluates to false

    1.if {
       # ifclause =
    }


..  code-block:: typoscript
    :caption: EXT:site_package/Configuration/TypoScript/setup.typoscript

    // STEP 2: define parameters

    1.table = tt_content           # default='' #stdWrap

    1.select {
       pidInList = this
       where = colPos = 1
       orderBy = sorting
    }

    # renderObj = <TABLEVALUE      # default!
    1.renderObj =

    # slide = 0                    # default! #stdWrap
    1.slide = -1

    # slideCollect = 0             # default! #stdWrap
    1.slide.collect =

    # slideCollectReverse = false  # default! #stdWrap
    1.slide.collectReverse =

    # slideCollectFuzzy = false    # default! #stdWrap
    1.slide.collectFuzzy =


..  code-block:: typoscript
    :caption: EXT:site_package/Configuration/TypoScript/setup.typoscript

    // STEP 3: find all records

    // STEP 4: apply the renderObj to each record and collect
    //         the results as string 'totalResult'

    // STEP 5: Apply wrap to the 'totalResult'
    1.wrap = |                     # default!

    // STEP 6: Apply stdWrap to the 'totalResult'
    1.stdWrap =                    # default! #stdWrap

    // STEP 6: Return 'totalResult'


See also: :ref:`if`, :ref:`select`, :ref:`data-type-wrap`, :ref:`stdWrap`,
:ref:`data-type-cobject`


..  _cobj-content-examples:

Display all tt_content records from this page
----------------------------------------------

Here is an example of the CONTENT object:

..  code-block:: typoscript
    :caption: EXT:site_package/Configuration/TypoScript/setup.typoscript

    1 = CONTENT
    1.table = tt_content
    1.select {
       pidInList = this
       orderBy = sorting
       where = {#colPos}=0
    }

Since in the above example `.renderObj` is not set explicitly, TYPO3
will automatically set :typoscript:`1.renderObj < tt_content`, so that `renderObj`
will reference the TypoScript configuration of `tt_content`. The
according TypoScript configuration will be copied to `renderObj`.


Apply special rendering
========================

Here is an example of record-rendering objects:

..  code-block:: typoscript
    :caption: EXT:site_package/Configuration/TypoScript/setup.typoscript

    page = PAGE
    page.typeNum = 0

    # The CONTENT object executes a database query and loads the content.
    page.10 = CONTENT
    page.10.table = tt_content
    page.10.select {

       # "sorting" is a column from the tt_content table and
       # keeps track of the sorting order, which was specified in
       # the backend.
       orderBy = sorting

       # Only select content from column "0" (the column called
       # "normal") and quote the database identifier (column name)
       # "colPos" (indicated by wrapping with {#})
       where = {#colPos}=0
    }

    # For every result line from the database query (that means for every content
    # element) the renderObj is executed and the internal data array is filled
    # with the content. This ensures that we can call the .field property and we
    # get the according value.
    page.10.renderObj = COA
    page.10.renderObj {

       10 = TEXT

       # The field tt_content.header normally holds the headline.
       10.stdWrap.field = header

       10.stdWrap.wrap = <h1>|</h1>

       20 = TEXT

       # The field tt_content.bodytext holds the content text.
       20.stdWrap.field = bodytext

       20.stdWrap.wrap = <p>|</p>
    }
