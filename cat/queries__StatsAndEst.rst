=========================
Statistics and Estimation
=========================

Flags in this category generally do one of four things:

#. Affect the way that cardinality estimation is done by the query optimizer
#. Return info about the cardinality estimation choices during optimization
#. Manipulate or provide information about statistics objects
#. Enable fixes (in CUs) to the cardinality estimation process

|

Short Descriptions
------------------


.. list-table::
	:widths: 10 60
	:header-rows: 1

	* - Flag
	  - Short Description
	* - See Also
	  - 
	* - :doc:`8666 <queries__CompilationInfo>`
	  - TODO
	* - ...
	  - 
	* - :ref:`CE Behav <CEBehav>`
	  - 
	* - 2301_
	  - Old CE uses a different set of mathematical assumptions during estimation.
	* - 2312_
	  - Forces the use of the new CE during optimization.
	* - 2324_
	  - Disables the creation of "implied predicates".
	* - ...
	  - 
	* - :ref:`Misc <Miscellaneous>`
	  - 
	* - 2309_
	  - Adjusts DBCC SHOW_STATISTICS output, showing info for partial statistics.
	* - ...
	  - 
	* - :ref:`Lim Life <LimLife>`
	  - 
	* - 4139_
	  - CU Fix for gap in TF 2389 behavior when 90% of new rows are NOT higher than max val.
	 
.. This comment line is as long as we would ever want the short desc to be in the table above.

|

.. _CEBehav: 
	 
Cardinality Estimation Behavior
-------------------------------

.. _2301: 

2301 ``Doc2005`` (Info)
	(Note that the online descriptions of 2301 describe behavioral changes in the optimization process that are primarily tied to 
	cardinality estimation. However, it is possible that this TF enables other, non-cardinality-related behavioral changes). 
	KB: "Makes your optimizer work harder by enabling advanced optimizations that are specific to decision support queries, applies 
	to processing of large data sets." `Database-Wiki`_: "Trace flag 2301 affects costing, which affects plan choice. Trace flag 2301 
	discourages order-preserving parallelism. The most significant effect is that trace flag 2301 discourages parallel merge join. 
	Trace flag 2301 enables the following advanced logic in the cardinality estimation: 
		- Base containment assumption
		- Integer-based interpolation
		- Use of the histogram even when the cardinality estimate is low
		- Unlimited density remapping  
	When a potential cardinality estimation issue exists, enable trace flag 2301 to determine whether the issue is addressed by the logic that is enabled by this trace flag. 
	When you should not use trace flag 2301:
	The potential disadvantage of this trace flag is that it uses more time and more memory during optimization. Do not use this trace flag for 
	OLTP queries and for frequently compiled queries. One known issue is that if applications perform many column remapping functions 
	(such as CONVERT, CAST, UPPER, or LOWER) and have many densities, enabling trace flag 2301 consumes lots of memory..."
	
	920093_ | IJose_1_ | Dima_1_ | Connect_1_


.. _2312: 

2312 ``Doc2014``
	Forces the use of the new cardinality estimation framework (aka "new CE"). 
	
	2801413_ | Nevarez_1_


.. _2324:

2324
	Disables the creation of "implied predicates". Implied predicates can be safely, mathematically inferred 
	by other criteria in the query and added to the internal representation of the query to assist in 
	cardinality estimation and various other optimizer transforms.
	
	SQLPerf_1_





|

.. _Miscellaneous: 

Miscellaneous
-------------

.. _2309:

2309
	In SQL 2014, enables output from a 3rd parameter for DBCC SHOW_STATISTICS such that the partial statistics histogram 
	(for just one partition) is shown.
	
	EStellato_1_ | DBIServices_1_


|

.. _LimLife:

Limited Lifespan
----------------

.. _4139: 

4139
	(related to 2389) Enables a CU fix where if 90% of newly-inserted values were NOT higher than the highest key value, 
	the column would not be marked as ascending. 3192117 notes this flag can cause access violations on certain SQL 2016 builds. 
	
	2952101_ | 3192117_





.. KB Links 

.. _920093: http://support.microsoft.com/kb/920093

.. _2801413: http://support.microsoft.com/kb/2801413

.. _2952101: http://support.microsoft.com/kb/2952101

.. _3192117: https://support.microsoft.com/en-us/kb/3192117



.. Connect links

.. _Connect_1: https://connect.microsoft.com/SQLServer/feedback/details/772232/make-optimizer-estimations-more-accurate-by-using-metadata








.. Guru Links (CSS, Paul Randal, Paul White, etc.)

.. _IJose_1: http://blogs.msdn.com/b/ianjo/archive/2006/04/24/582219.aspx

.. _Dima_1: http://www.queryprocessor.com/ce_join_base_containment_assumption/

.. _Nevarez_1: http://www.sqlperformance.com/2013/12/t-sql-queries/a-first-look-at-the-new-sql-server-cardinality-estimator

.. _EStellato_1: http://sqlperformance.com/2015/05/sql-statistics/incremental-statistics-are-not-used-by-the-query-optimizer



.. Forums 

.. _SQLPerf_1: https://answers.sqlperformance.com/questions/2299/why-not-seek-predicate.html





.. Other Links 

.. _Database-Wiki: http://database-wiki.com/2012/10/20/documented-sql-server-trace-flags-use-them-cautiously/
	
.. _DBIServices_1: http://www.dbi-services.com/index.php/blog/entry/sql-server-2014-new-incremental-statistics
