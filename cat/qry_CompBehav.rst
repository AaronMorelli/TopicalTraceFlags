======================
Compilation Behavioral
======================

All flags in this category modify the compilation process in such a way as to produce alternative 
query plans. Note that many flags in the :doc:`Statistics and Estimation <qry_StatsAndEst>` category
also modify compilation behavior; those flags all do so by modifying the cardinality estimation process,
whereas all of these flags change non-estimation behaviors.

The information returned falls into the below categories: 

#. `Disk IO Optimization`_ --> Flags whose goal is to improve Disk IO performance (often by promoting sequential IO)
#. `Hashing and Batching`_ --> Flags that modify hashing operations and flags that adjust batch mode processing
#. `Phases and Timeouts`_ --> Flags that alter the phases the optimizer enters or its timeout values for short-circuiting phase entry.
#. `Miscellaneous`_ --> Other, miscellaneous flags.
#. `Fixes and Past Relevance`_ --> Fix flags and flags which are no longer relevant.

Note that the line between these flags and those in the :doc:`Query Execution <qry_QueryExec>` category
can be a bit grey. Flags in this category affect behaviors that persist in the cached plan between
executions, while flags in the :doc:`Query Execution <qry_QueryExec>` category affect behavior that
is normally decided at runtime.

|

Short Descriptions
------------------

.. This comment line is as long as we would ever want the short desc to be in the table below.

.. list-table::
	:widths: 10 60
	:header-rows: 1

	* - Flag
	  - Short Description
	* - See Also
	  - 
	* - :doc:`2301 <qry_StatsAndEst>`
	  - Modifies estimation mathematical assumptions; may affect non-estimation behavior.
	* - \* :doc:`2312 <qry_StatsAndEst>`
	  - Forces the use of the new CE during optimization, inevitably changing plan shapes.
	* - ...
	  - 
	* - :ref:`Disk Opts <DiskOpts>`
	  - 
	* - 2332_
	  - Forces a sort (for sequential IO) in Insert/Update/Delete plans
	* - 2340_
	  - Disables nested loops "implicit batch sort" (in post-opt rewrite phase)
	* - 8633_
	  - Enable prefetch; useful for IO perf in Insert/Update/Delete plans.
	* - 8738_
	  - (Apparently) disables sorting before a key lookup operator.
	* - 8744_
	  - Disables pre-fetching for the Nested Loops operator.
	* - 8795_
	  - Disables sorting for sequential IO in Insert/Update/Delete plans.
	* - 9115_
	  - Disables both implicit batch sort (TF 2340) and NL prefetching (TF 8744)
	* - ...
	  - 
	* - :ref:`Hash/Batch <HashBatch>`
	  - 
	* - 2441_
	  - Enables the use of hash joins to column store indexes in certain cases.
	* - 7359_
	  - Disables the internal bitmap used for hash matches and joins.
	* - 7497_
	  - (Full purpose unknown) Can be used w/7498 to disable "optimized bitmaps"
	* - 7498_
	  - (Full purpose unknown) Can be used w/7497 to disable "optimized bitmaps"
	* - 9347_
	  - (Purpose unknown) Appears related to batch mode sorting.
	* - 9349_
	  - Disables batch mode top sort operator.
	* - 9358_
	  - Disables batch-mode sort operations.
	* - 9453_
	  - Disables batch mode, forcing row mode.
	* - ...
	  - 
	* - :ref:`Phases/TOs <PhasesTO>`
	  - 
	* - 8671_
	  - Disables the optimizer short-circuit due to "Good Enough Plan Found"
	* - 8677_
	  - Skips the "Search 1" optimization phase (if applicable)
	* - 8757_
	  - Skip the Trivial Plan optimization phase
	* - 8780_
	  - Sets the optimizer timeout value to a very high constant. **Do Not Use!**
	* - 8788_
	  - Appears to have a similar effect as 8780. **Do Not Use!**
	* - ...
	  - 
	* - :ref:`Misc <Misc>`
	  - 
	* - 2329_
	  - Disables the "Few outer rows" dimension table optimization
	* - 2335_
	  - Causes optimizer to create plans that are more conservative w/memory.
	* - 7470_
	  - Adjusts calculation for memory requirements for sorts.
	* - 8602_
	  - Tells optimizer to ignore index hints.
	* - \* 8649_
	  - Strongly encourages optimizer to generate a parallel plan.
	* - 8690_
	  - Prevents the optimizer use of "performance spools"
	* - 8692_
	  - Forces optimizer to use eager spools when it needs Halloween Protection.
	* - 8722_
	  - Disables all "other" (besides index and join) query hints.
	* - 8746_
	  - Among other effects, disables "rowset sharing" optimization.
	* - 8755_
	  - Disables all join hints.
	* - 8758_
	  - Among other effects, disables plan rewrites to a single operator plan.
	* - 8790_
	  - Forces a wide-update plan for any data-changing query.
	* - 9130_
	  - Disables the pushing of non-sargable filter predicates into seeks or scans.
	* - 9348_
	  - Applies a row limit to whether bulk insert is attempted or not.
	* - ...
	  - 
	* - :ref:`Fix/PastRel <FixPastRel3>`
	  - 
	* - 8687_
	  - (Perhaps) disables query parallelism.
	* - 8720_
	  - In SQL 2000, apparently had the same effect as OPTION(KEEPFIXED PLAN)
	* - 9059_
	  - Allows optimizer to choose an index seek for numerics of varying precisions.

	 
.. This comment line is as long as we would ever want the short desc to be in the table above.

|

.. _DiskOpts: 


Disk IO Optimization
--------------------

.. _2332:

2332
	PWhite: "Force DML Request Sort (CUpdUtil::FDemandRowsSortedForPerformance)"
	Appears to be the counterpoint of 8795_.
	
	PWhite_1_ 

.. _2340:

2340 ``Doc2014``
	`BOL 2014`_: "Causes SQL Server not to use a sort operation (batch sort) for optimized nested loop joins when generating 
	a plan. Beginning with SQL Server 2016 SP1, to accomplish this at the query level, add the *DISABLE_OPTIMIZED_NESTED_LOOP* 
	USE HINT query hint instead of using this trace flag."
	
	Dima: "Disable Nested Loops Implicit Batch Sort on the Post Optimization Rewrite Phase."
	Dima's article is useful to contrast NL batch sorting and 2340 from NL prefetching and 8744, 
	and discusses 9115.
	
	2009160_ | CSS_1_ | Dima_1_ 
	
.. _8633:

8633
	PWhite: "Enable prefetch (CUpdUtil::FPrefetchAllowedForDML and CPhyOp_StreamUpdate::FDoNotPrefetch)"
	
	PWhite_1_ 
	
.. _8738:

8738
	(Apparently) disables an optimization where rows are sorted before a Key Lookup operator. The optimization is meant to 
	promote Sequential IO rather than the random nature of IO from Key Lookups. Note that the context in which this flag 
	is described means that the above description may not be very precise, or even the only use of this flag.
	
	PWhite_18_ 
	
.. _8744:

8744 ``Doc2014``
	`BOL 2014`_: "Disable pre-fetching for the Nested Loop operator."
	PWhite: "Disable prefetch (CUpdUtil::FPrefetchAllowedForDML)." 
	Dima's article is useful to contrast NL prefetching and 8744 from NL batch sorting and 2340/9115.

	920093_ | Dima_1_ | PWhite_1_ | PWhite_2_ | PWhite_19_ | Connect_3_ 

	
.. _8795:
	
8795
	PWhite: "Disable DML Request Sort (CUpdUtil::FDemandRowsSortedForPerformance)"
	Appears to be the counterpoint of 2332_.
	
	PWhite_1_ | PWhite_3_ 

.. _9115:

9115
	Dima: "Disables both [NLoop Implicit Batch Sort {TF 2340} and NL Prefetching {TF 8744}], and not only on the 
	Post Optimization, but the explicit Sort also." PWhite: "Disable prefetch (CUpdUtil::FPrefetchAllowedForDML)"
	
	Dima_1_ | PWhite_1_ | Halincrest_1_ 

|

.. _HashBatch:

Hashing and Batching
--------------------

.. _2441:

2441
	Enables the use of a hash join for joins to column store indexes even when the join clause 
	would normally be removed "during query normalization".
	
	3146123_ 
	
.. _7359:

7359
	Disables the bitmap associated with hash matching. This bitmap is used for "bit-vector filtering" 
	and can reduce the amount of data written to TempDB during hash spills.
	
	Dima_2_ 
	
.. _7497:

7497
	Full behavior and intended purpose unknown, but the PWhite post uses it in concert with 7498 
	to disable "optimized bitmaps".
	
	PWhite_4_
	
.. _7498:

7498
	Full behavior and intended purpose unknown, but the PWhite post uses it in concert with 
	7497 to disable "optimized bitmaps".
	
	PWhite_4_ 

.. _9347:
	
9347 ``Doc2014``
	`BOL 2014`_: "Disables batch mode for sort operator. SQL Server 2016 introduces a new batch mode sort operator 
	that boosts performance for many analytical queries."
	
	3172787_ 

.. _9349:
	
9349 ``Doc2014``
	`BOL 2014`_: "Disables batch mode for top N sort operator. SQL Server 2016 introduces a new batch mode top 
	sort operator that boosts performance for many analytical queries."
	
.. _9358:
	
9358
	Disables batch-mode sort operations.
	
	3171555_ 

.. _9453:
	
9453
	Disables Batch Mode in Parallel Columnstore query plans. (Note that a plan using batch 
	mode appears to require a recompile before the TF takes effect).
	
	Niko_1_ 

|

.. _PhasesTO:

Phases and Timeouts
-------------------

.. _8671:

8671
	Dima: disables the logic that prunes the memo and prevents the optimization process from stopping 
	due to "Good Enough Plan found". Can significantly increase the amount of time, CPU, and memory 
	used in the compilation process.
	
	Dima_4_ 

.. _8677: 

8677
	Skips "Search 1" phase of query optimization (if applicable), and only Search 0 and Search 2 execute.
	
	DBally_1_ 
	
.. _8757: 

8757
	Skip Trivial Plan optimization, essentially forcing entry into Full optimization for a query.
	
	PWhite_5_ | PWhite_6_ 

.. _8780: 
	
8780
	Dima: increases the "timeout" value that the optimizer sets to 3072000 transformations. Normally, 
	the optimizer sets its internal timeout value to something based on the complexity of the query. 
	
	.. warning::
	
		Paul White once tweeted: "There's never a good reason to use or promote that dangerous flag"
	
	Dima_3_ 
	
.. _8788:
	
8788
	Dima: notes that 8788 appears to have a similar effect on the timeout as 8780, but he hasn’t yet 
	been able to determine the difference in effect between 8788 and 8780.
	
	.. warning::
	
		Presumably Paul White's tweet about 8780 (above) applies here as well.
	
	Dima_3_ 


|

.. _Misc:

Miscellaneous
-------------

.. _2329: 

2329
	Disables "Few Outer Rows" optimization that helps maximize parallelization of dimensional queries.
	
	Dima_5_ 

.. _2335: 

2335 ``Doc2014``
	`BOL 2014`_: "Causes SQL Server to assume a fixed amount of memory is available during query optimization. 
	It does not limit the memory SQL Server grants to execute the query. The memory configured for SQL Server 
	will still be used by data cache, query execution and other consumers."
	
	KB: Causes the optimizer to generate plans that are "more conservative in terms of memory consumption 
	when executing the query." KB describes scenario where large values of "max server memory" may 
	lead to inefficient plans.
	
	2413549_ | PWhite_7_ 

.. _7470: 
	
7470
	KB: "Makes SQL Server consider internal data management memory overhead when calculating required 
	memory for sort." Can help avoid sort spills to tempdb when the estimate is otherwise accurate.
	
	3088480_

.. _8602:

8602
	Ignore index hints that are specified in query/procedure.
	
	Kalen_1_ | SiebelPDF_ 

.. _8649:
	
8649
	Strongly encourages the optimizer to generate parallel plans. (Perhaps by setting the costing 
	for parallel exchange operators to 0.)
	
	PWhite_5_ | PWhite_8_ | Machanic_1_ 


.. _8690:
	
8690
	Prevents the optimizer from using "performance spools' (either table or index) in a query plan.
	
	2962767_ | CSS_2_ | PWhite_9_ | Connect_1_ | Machanic_2_ (near the end)
	
	

.. _8692:
	
8692
	Force optimizer to use an Eager Spool when it needs Halloween Protection
	
	PWhite_10_ | PWhite_11_ 

	
.. _8722:
	
8722
	Disables all "other" (besides index and join) hints. This includes the OPTION clause. From Database-Wiki 
	(but I suspect originally from a Khen book): "By running all three (8602, 8755, and 8722) flags, you can 
	disable all hints in a query."
	
	SQLMag_1_ | Database-Wiki_ 

.. _8746:
	
8746
	Whatever else it does, one effect is to disable the "rowset sharing" optimization described in the 
	PWhite miniseries.
	
	PWhite_11_ | PWhite_12_ 

.. _8755:

8755
	Disables all join hints.
	
	Database-Wiki_ 
	
.. _8758:
	
8758
	PWhite desc 1: "A [workaround to the MERGE bug described] is to apply 8758 – unfortunately this disables 
	a number of optimisations, not just the one above, so it’s not really recommended for long term use." 
	PWhite 2: "Disable rewrite to a single operator plan (CPhyOp_StreamUpdate::PqteConvert)"
	
	PWhite_13_ | PWhite_1_ 

.. _8790:
	
8790
	PWhite: "Undocumented trace flag 8790 forces a wide update plan for any data-changing query (remember that 
	a wide update plan is always possible)."
	
	956718_ | PWhite_14_ | PWhite_15_ 


.. _9130:
	
9130
	Ballantyne SQLBits: "Disable non-sargable pushed predicates." Prohibits the optimizer from pushing 
	residual predicates down into "access method" iterators (i.e. seeks and scans).
	
	PWhite_16_ | PWhite_17_ | DBally_2_ | Connect_2_ 

.. _9348:
	
9348
	Sets a row limit (based on cardinality estimates) that controls whether a bulk insert is attempted or 
	not (assuming conditions are met for a bulk insert). Introduced as a workaround for memory errors 
	encountered with bulk insert.
	
	2998301_

| 

.. _FixPastRel3:

Fixes and Past Relevance
------------------------
These flags either are old and irrelevant for modern builds, appear only in CTPs, or enable a fix 
in a CU but are baselined in a later service pack or release.

.. _8687:
	
8687
	Found a reference to it (via books.google.com) in Ken Henderson’s Guru’s Guide to Transact-SQL, 
	page 503: "Disables query parallelism".

.. _8720:

8720
	In SQL 2000, apparently would have the same effect as OPTION(KEEPFIXED PLAN).
	
	Halincrest_2_ 
	
.. _9059:
	
9059
	(Needs investigation) Turns back behavior to SQL 2000 SP3 after a SP4 installation, this allows the 
	optimizer to choose an index seek when comparing numeric columns or numeric constants that are of 
	different precision or scale; else would have to change schema/code.
	
	899976_ 


.. Official Links 

.. _BOL 2014: https://technet.microsoft.com/en-us/library/ms188396.aspx

.. _899976: http://support.microsoft.com/kb/899976/en-us

.. _920093: http://support.microsoft.com/kb/920093

.. _956718: http://support.microsoft.com/kb/956718/en-us

.. _2009160: http://support.microsoft.com/kb/2009160

.. _2413549: http://support.microsoft.com/kb/2413549

.. _2962767: http://support2.microsoft.com/kb/2962767

.. _2998301: http://support2.microsoft.com/kb/2998301

.. _3088480: https://support.microsoft.com/en-us/kb/3088480

.. _3146123: https://support.microsoft.com/en-us/kb/3146123

.. _3172787: https://support.microsoft.com/en-us/kb/3172787

.. _3171555: https://support.microsoft.com/en-us/kb/3171555


.. MSFT Blog links

.. _CSS_1: http://blogs.msdn.com/b/psssql/archive/2010/01/11/high-cpu-after-upgrading-to-sql-server-2005-from-2000-due.aspx

.. _CSS_2: https://blogs.msdn.microsoft.com/psssql/2015/12/15/spool-operator-and-trace-flag-8690/


.. Non-MSFT bloggers

.. _DBally_1: https://sqlbits.com/Sessions/Event12/Query_Optimizer_Internals_Traceflag_fun

.. _DBally_2: http://sqlblogcasts.com/blogs/sqlandthelike/archive/2012/12/06/my-new-favourite-traceflag.aspx

.. _Dima_1: http://www.queryprocessor.com/batch-sort-and-nested-loops/

.. _Dima_2: http://www.queryprocessor.com/hash-join-execution-internals/

.. _Dima_3: http://www.somewheresomehow.ru/optimizer_unleashed_1/

.. _Dima_4: http://www.somewheresomehow.ru/optimizer_unleashed_2/

.. _Dima_5: http://www.queryprocessor.com/few-outer-rows-optimization/

.. _Halincrest_1: http://www.hanlincrest.com/SQLServerLockEscalation.htm

.. _Halincrest_2: http://www.hanlincrest.com/SQLserverStoredProcRecompiles.htm

.. _Kalen_1: http://sqlblog.com/blogs/kalen_delaney/archive/2008/02/26/lost-without-a-trace.aspx

.. _Machanic_1: http://sqlblog.com/blogs/adam_machanic/archive/2013/07/11/next-level-parallel-plan-porcing.aspx

.. _Machanic_2: http://www.youtube.com/watch?v=_IRWvlSQxS8

.. _Niko_1: http://www.nikoport.com/2014/07/24/clustered-columnstore-indexes-part-35-trace-flags-query-optimiser-rules/

.. _PWhite_1: http://sqlblog.com/blogs/paul_white/archive/2013/01/26/optimizing-t-sql-queries-that-change-data.aspx

.. _PWhite_2: http://sqlblog.com/blogs/paul_white/archive/2013/03/08/execution-plan-analysis-the-mystery-work-table.aspx

.. _PWhite_3: http://sqlperformance.com/2014/10/t-sql-queries/performance-tuning-whole-plan

.. _PWhite_4: http://sqlperformance.com/2015/11/sql-plan/hash-joins-on-nullable-columns

.. _PWhite_5: http://sqlblog.com/blogs/paul_white/archive/2011/12/23/forcing-a-parallel-query-execution-plan.aspx

.. _PWhite_6: http://sqlblog.com/blogs/paul_white/archive/2012/04/28/query-optimizer-deep-dive-part-1.aspx

.. _PWhite_7: http://dba.stackexchange.com/questions/53726/difference-in-execution-plans-on-uat-and-prod-server

.. _PWhite_8: http://sqlblog.com/blogs/paul_white/archive/2013/06/17/improving-partitioned-table-join-performance.aspx

.. _PWhite_9: http://dba.stackexchange.com/questions/52552/index-not-making-execution-faster-and-in-some-cases-is-slowing-down-the-query

.. _PWhite_10: http://www.sqlperformance.com/2013/02/sql-plan/halloween-problem-part-4

.. _PWhite_11: http://sqlperformance.com/2016/03/sql-plan/changes-to-a-writable-partition-may-fail

.. _PWhite_12: http://sqlperformance.com/2015/12/sql-plan/optimizing-update-queries

.. _PWhite_13: http://sqlblog.com/blogs/paul_white/archive/2010/08/04/another-interesting-merge-bug.aspx

.. _PWhite_14: http://sqlblog.com/blogs/paul_white/archive/2012/12/10/merge-bug-with-filtered-indexes.aspx

.. _PWhite_15: http://sqlperformance.com/2014/06/sql-plan/filtered-index-side-effect

.. _PWhite_16: http://sqlblog.com/blogs/paul_white/archive/2012/10/15/cardinality-estimation-bug-with-lookups-in-sql-server-2008-onward.aspx

.. _PWhite_17: http://sqlblog.com/blogs/paul_white/archive/2013/06/11/hello-operator-my-switch-is-bored.aspx

.. _PWhite_18: https://answers.sqlperformance.com/questions/603/why-is-the-sort-operator-needed-in-this-plan.html

.. _PWhite_19: https://answers.sqlperformance.com/questions/392/there-are-2-identical-worksets-in-question-this-is.html




.. Connect links

.. _Connect_1: http://connect.microsoft.com/SQL/feedback/ViewFeedback.aspx?FeedbackID=453982

.. _Connect_2: http://connect.microsoft.com/SQLServer/feedback/details/767395/cardinality-estimation-error-with-pushed-predicate-on-a-lookup

.. _Connect_3: http://connect.microsoft.com/SQLServer/feedback/details/780194/make-dbcc-trace-flags-available-as-option-querytraceon


.. Forums 


.. Other Links 

.. _Database-Wiki: http://database-wiki.com/2012/10/20/documented-sql-server-trace-flags-use-them-cautiously/

.. _SiebelPDF: http://download.microsoft.com/download/6/e/5/6e52bf39-0519-42b7-b806-c32905f4a066/eim_perf_flowchart_final.pdf

.. _SQLMag_1: http://sqlmag.com/sql-server/investigating-trace-flags
