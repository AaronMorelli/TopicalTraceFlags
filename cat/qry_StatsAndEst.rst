=====================================
Statistics and Cardinality Estimation
=====================================

Flags in this category fall into the below groupings:

#. `Cardinality Estimation Behavior`_ --> Flags that affect the way that cardinality estimation is done by the query optimizer
#. `Informational, Stats Update, and Other`_ --> Return info about the cardinality estimation process during optimization, or manipulate or provide information about statistics objects
#. `Fixes and Past Relevance`_ --> Fix flags and flags which are no longer relevant.

TODO: add a SeeAlso in the security section to here for 9485

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
	* - :doc:`205 <qry_CompInfo>`
	  - Reports to error log when recompile occurs due to auto-update of stats.
	* - :doc:`8612 <qry_CompInfo>`
	  - Adds cardinality info to query opt trees that are output by by 8605/8606/8607.
	* - :doc:`8666 <qry_CompInfo>`
	  - Returns hidden nodes in query plan XML including stats usage and thresholds.
	* - :doc:`9348 <qry_CompBehav>`
	  - Applies a row limit (based on card est) to whether bulk insert is attempted or not.
	* - ...
	  - 
	* - :ref:`CE Behav <CEBehav>`
	  - 
	* - 2301_
	  - Old CE: use a different set of mathematical assumptions during estimation.
	* - \* 2312_
	  - Forces the use of the new cardinality estimator during optimization.
	* - 2324_
	  - Disables the creation of "implied predicates".
	* - 2328_
	  - Reverts const/const comparison (incl. parms/vars) estimations to SQL2K guesses.
	* - \* 2389_
	  - Enables optimizer tracking of ascending keys and subsequent recompilation behavior.
	* - \* 2390_
	  - Similar to 2390 and the ascending key problem. See description below for more.
	* - 2453_
	  - Applies recompile thresholds for temp tables to table variables
	* - 4137_
	  - Adjusts calculation used for multiple AND predicates; effective w/correlation.
	* - 4138_
	  - Disables rowgoal logic for queries that contain TOP, OPTION(FAST N), IN, or EXISTS.
	* - \* 4139_
	  - Enables "quick stats" histogram amendments even if key col isn't branded ascending.
	* - 9471_
	  - New CE: Use min selectivity for both conjunctive (AND) and disjunctive (OR) preds.
	* - 9472_
	  - New CE: Assume independence for multiple WHERE predicates.
	* - 9476_
	  - New CE: Use simple containment (old CE default) instead of base containment.
	* - 9479_
	  - New CE: Force use of "simple join" estimation algorithm
	* - \* 9481_
	  - New CE: Force use of the old cardinality estimation model.
	* - 9482_
	  - New CE: Turn off "overpopulated primary key" CE adjustment
	* - 9483_
	  - New CE: Forces QO to create non-persisted filtered stat objects.
	* - 9488_
	  - New CE: Reverts the estimation for multi statement TVFs back to 1 row.
	* - 9489_
	  - New CE: turn off new CE ascending key logic.
	* - ...
	  - 
	* - :ref:`Other <InfoStatsMisc>`
	  - 
	* - 2309_
	  - Adjusts DBCC SHOW_STATISTICS output, showing info for partial statistics.
	* - \* 2363_
	  - New CE: Outputs info about stats used and resulting cardinality estimates.
	* - \* 2371_
	  - Lowers relative threshold for stats-based recompilation as table gets larger.
	* - 2388_
	  - Adjusts DBCC SHOW_STATISTICS output to show (not-)ascending status of stats obj.
	* - 7471_
	  - Allows multiple statistics objects on the same table to be updated concurrently.
	* - 8721_
	  - Prints info to error log when AutoStat occurs.
	* - 9204_
	  - Prints which stats objects the QO found interesting and then fully loaded.
	* - 9292_
	  - Prints which stats objects the QO found interesting and loaded the header.
	* - 9485_
	  - Disables SELECT permission for DBCC SHOW_STATISTICS.
	* - ...
	  - 
	* - :ref:`Fix/PastRel <FixPastRel1>`
	  - 

	 
.. This comment line is as long as we would ever want the short desc to be in the table above.

|

.. _CEBehav: 
	 
Cardinality Estimation Behavior
-------------------------------

.. _2301: 

2301 ``Doc2014``
	`BOL 2014`_: "Enable advanced decision support optimizations". 920093_ adds: "Makes your optimizer work harder by enabling 
	advanced optimizations that are specific to decision support queries, applies to processing of large data sets." 
	IJose_1_ lists 5 fundamental mathematical assumptions the optimizer makes when using this flag, and contrasts these assumptions
	with behavior without this flag.
		- Integer Modelling for values falling between histogram steps. Helps with inequality filters.
		- Comprehensive Histogram Usage even when the cardinality estimate for a relation drops below the number of steps in the histogram.
		- Base Containment Assumption (see blog post)
		- Comprehensive Density Remapping changes how estimation is done when a CONVERT function call is present
		- Comprehensive Density Matching allows equi-joined columns to be considered equivalent for certain estimation operations.
	`Database-Wiki`_ adds that the flag "discourages  order-preserving parallelism", especially parallel merge join. It also claims that
	the density remapping logic is also relevant to UPPER and LOWER (and CAST, of course) function calls.
	
	.. warning::
	
		IJose adds: "...compile times will increase, and in some cases memory consumption can increase dramatically."
	
	920093_ | IJose_1_ | Dima_1_ | Connect_1_


.. _2312: 

2312 ``Doc2014``
	`BOL 2014`_: "Enables you to set the query optimizer cardinality estimation model to the SQL Server 2014 through SQL Server 2016 
	versions, dependent of the compatibility level of the database." AKA force use of the new CE.
	
	`New CE Whitepaper`_ | 2801413_ | Nevarez_1_


.. _2324:

2324
	Disables the creation of "implied predicates". Implied predicates can be safely, mathematically inferred 
	by other criteria in the query and added to the internal representation of the query to assist in 
	cardinality estimation and various other optimizer transforms.
	
	SQLPerf_1_


.. _2328: 

2328
	Reverts back to SQL 2000 behavior when comparing 2 constants ("constants" here includes parameters or variables whose
	values are known at compile time). The SQL 2000 behavior uses a guess formula, whereas the SQL 2005+ behavior uses
	the compile-time values and the statistics object(s) to generate a cardinality estimate. Dima demonstrates this  
	selectivity guess (a call to CScaop_Comp::GuessSelect)
	
	IJose_2_ | Dima_2_ 

.. _2389: 

2389 ``Doc2014``
	`BOL 2014`_: "Enable automatically generated quick statistics for ascending keys (histogram 
	amendment). If trace flag 2389 is set, and a leading statistics column is marked as ascending, then the histogram used to estimate 
	cardinality will be adjusted at query compile time." Itzik has very insightful comments about this flag and the ascending key 
	problem under both the old and new CE. Kejser notes that (at the time of his posts) this TF doesn’t work with partitioned tables, 
	and has his own solution. See also 9489_ (Dima). *(See also* `2388`_, `2390`_, and `4139`_ *)*
	
	Hotfixes: 922063_,  929278_,  2952101_ | Itzik_1_ | IJose_3_ | Stellato_2_ | 
	`Kejser Part 1`_ | `Kejser Part 2`_ | Nevarez_2_

.. _2390: 

2390 ``Doc2014``
	`BOL 2014`_: "Enable automatically generated quick statistics for ascending or unknown keys (histogram amendment). If trace flag 
	2390 is set, and a leading statistics column is marked as ascending or unknown, then the histogram used to estimate cardinality 
	will be adjusted at query compile time." Closely tied to 2389_, 4139_, and the ascending key problem.
	
	.. warning::
	
		Read Ian Jose's blog entry carefully before using 2390.
	
	IJose_3_

.. _2453:
	
2453 ``Doc2014``
	`BOL 2014`_: "Allows a table variable to trigger recompile when enough number of rows are changed."
	Applies familiar temp table rowcount recompilation thresholds to table variables. Bertrand points out important caveats. 
	
	2952444_ | 2012SP2_ | CSS_1_ | Bertrand_ 

.. _4136: 

4136 ``Doc2014``
	`BOL 2014`_: "Disables parameter sniffing unless OPTION(RECOMPILE), WITH RECOMPILE or OPTIMIZE FOR value is used."
	There are now a number of ways for disabling parameter sniffing:
		- This flag
		- The USE HINT option **DISABLE_PARAMETER_SNIFFING**
		- The database-scoped option **PARAMETER_SNIFFING**
		- The OPTIMIZE FOR UNKNOWN query hint
		
	Dima notes that 4136 has no effect on parameter sniffing's "cousin", runtime constant sniffing.
	
	980653_ | `USE HINT`_ | `DB SCOPED CONFIG`_ | Dima_10_ 
	
.. _4137: 
	
4137 ``Doc2014``
	`BOL 2014`_: "Causes SQL Server to generate a plan using minimum selectivity when estimating AND predicates for filters 
	to account for correlation, under the query optimizer cardinality estimation model of SQL Server 2012 and earlier versions.
	Beginning with SQL Server 2016 SP1, to accomplish this at the query level, add the **ASSUME_MIN_SELECTIVITY_FOR_FILTER_ESTIMATES**. 
	USE HINT" The BOL USE HINT link notes that this hint is *equivalent* to 4137 (old CE) and *similar* to 9471_ (new CE).
	
	Paul White covers "Minimum Selectivity" well, and also points out that 4137 only applies min selectivity to 
	conjunctive (AND) predicates, while 9471_ applies it to both conjunctive and disjunctive (OR) predicates.
	
	2658214_ | `USE HINT`_ | PWhite_1_ | Dima_3_ | Connect_2_

.. _4138: 	

4138 ``Doc2014``
	`BOL 2014`_: "Causes SQL Server to generate a plan that does not use row goal adjustments with queries that contain 
	TOP, OPTION (FAST N), IN, or EXISTS keywords. Beginning with SQL Server 2016 SP1, to accomplish this at the query level, 
	add the **DISABLE_OPTIMIZER_ROWGOAL** USE HINT."
	
	The 2 interesting StackExch links show Martin Smith at work. 
	
	2667211_ | `USE HINT`_ | PWhite_2_ | PWhite_3_ | StackExch_1_ | StackExch_2_
	
.. _4139:

4139 ``Doc2014``
	`BOL 2014`_: "Enable automatically generated quick statistics (histogram amendment) regardless of key column status. If trace flag 
	4139 is set, regardless of the leading statistics column status (ascending, descending, or stationary), the histogram used to estimate 
	cardinality will be adjusted at query compile time. Beginning with SQL Server 2016 SP1, to accomplish this at the query level, add 
	the **ENABLE_HIST_AMENDMENT_FOR_ASC_KEYS** USE HINT."
	
	The KB introduces the flag as a fix flag for a case where 90% of newly-inserted values were NOT higher than the highest key value.
	However, the BOL description indicates an intent for the flag to be used in any situation where out-of-bounds values are too costly.

	.. warning::
	
		3192117 notes this flag can cause access violations on certain SQL 2016 builds.
	
	2952101_ | 3192117_ | `USE HINT`_	
	

.. _9471:
	
9471 ``Doc2014``
	`BOL 2014`_: "Causes SQL Server to generate a plan using minimum selectivity for single-table filters, under the query optimizer 
	cardinality estimation model of SQL Server 2014 through SQL Server 2016 versions. Beginning with SQL Server 2016 SP1, to accomplish 
	this at the query level, add the **ASSUME_MIN_SELECTIVITY_FOR_FILTER_ESTIMATES** USE HINT."
	The BOL USE HINT link notes that this hint is *equivalent* to 4137_ (old CE) and *similar* to 9471 (new CE).
	
	Paul White covers "Minimum Selectivity" well, and also points out that 4137 only applies min selectivity to 
	conjunctive (AND) predicates, while 9471 applies it to both conjunctive and disjunctive (OR) predicates.
	
	`USE HINT`_ | PWhite_1_ | Milo_1_ | Connect_2_

.. _9472:
	
9472
	Assumes independence for multiple WHERE predicates in the SQL 2014 CE. Predicate independence was the default before SQL 2014, 
	and thus this flag can be used to emulate pre-SQL 2014 CE behavior in a more specific fashion than TF 9481.
	
	PWhite_1_ | Connect_2_
	
	
.. _9476:

9476 ``Doc2014``
	`BOL 2014`_: "Causes SQL Server to generate a plan using the Simple Containment assumption instead of the default Base Containment 
	assumption, under the query optimizer cardinality estimation model of SQL Server 2014 through SQL Server 2016 versions.
	Beginning with SQL Server 2016 SP1, to accomplish this at the query level, add the USE HINT **ASSUME_JOIN_PREDICATE_DEPENDS_ON_FILTERS**."
	
	Simple containment is the old CE default; base containment is the new CE default.
	Paul White email: "Ignores the histograms (avoiding coarse alignment) and simply assumes containment at the join."

	`USE HINT`_ | 3189675_ | Dima_1_

.. _9479: 
	
9479
	Dima: "Forces the optimizer to use Simple Join [estimation] even if a histogram is available...will force optimizer to use a simple join 
	estimation algorithm, it may be CSelCalcSimpleJoinWithDistinctCounts, CSelCalcSimpleJoin or CSelCalcSimpleJoinWithUpperBound, 
	depending on the compatibility level and predicate comparison type." 
	
	Paul White email: "uses simple containment instead of base containment for simple joins"
	
	Dima_4_

.. _9481:
	
9481 ``Doc2014``
	`BOL 2014`_: "Enables you to set the query optimizer cardinality estimation model to the SQL Server 2012 and earlier versions, 
	irrespective of the compatibility level of the database. Beginning with SQL Server 2016 SP1, to accomplish this at the query 
	level, add the USE HINT **FORCE_LEGACY_CARDINALITY_ESTIMATION**."
	
	Note also the **LEGACY_CARDINALITY_ESTIMATION** database-scoped setting.
	
	`USE HINT`_ | `DB SCOPED CONFIG`_ | 2801413_

.. _9482:
	
9482
	Turns off the "overpopulated primary key" adjustment that the optimizer might use when determining that a "dimension" table 
	(the schema could be OLTP as well) has many more distinct values than the "fact" table. (The seminal example is where a Date 
	dimension is populated out into the future, but the fact table only has rows up to the current date). Since join cardinality 
	estimation occurs based on the contents of the histograms of the joined columns, an "overpopulated primary key" can result 
	in higher selectivity estimates, causing rowcount estimates to be too low.
	
	Dima_5_

.. _9483: 

9483
	Forces the optimizer to create (if possible) a filtered statistics object based on a predicate in the query. 
	This filtered stat object is not persisted. In Dima’s example, the filtered stat object is actually created on the join 
	column…i.e. "CREATE STATISTICS [filtered stat obj] ON [table] (Join column) WHERE (predicate column = 'literal')" 
	
	Dima_6_

.. _9488:
	
9488
	Reverts the estimation behavior for multi-statement TVFs back to 1 row instead of the 100-row estimate behavior that was 
	adopted in SQL 2014. 
	
	Dima_7_

.. _9489:
	
9489
	Turns off the new CE logic that handles ascending keys.
	
	Dima_8_

	
	
	
|

.. _InfoStatsMisc: 

Informational, Stats Update, and Other
--------------------------------------

.. _2309:

2309 (Info)
	In SQL 2014, enables output from a 3rd parameter for DBCC SHOW_STATISTICS such that the partial statistics histogram 
	(for just one partition) is shown.
	
	Stellato_1_ | DBIServices_1_

	
.. _2363:

2363 (Info)
	Outputs information regarding statistics information used and derived during the (new CE) optimization process.
	Dima shows it giving insight into the new "calculator" framework in 2014 for deriving statistics at intermediate nodes of the tree. 
	It also shows which histograms (statistics objects) were loaded, and acts as a replacement for 9204_ and 9292_.
	
	Dima_9_ | PWhite_1_ 

	
.. _2371:	

2371 ``Doc2014``
	`BOL 2014`_: "Changes the fixed auto update statistics threshold to dynamic auto update statistics threshold. **Note:** Beginning 
	with SQL Server 2016 this behavior is controlled by the engine and trace flag 2371 has no effect."
	Switches the threshold for a performance-based recompile (for a given stats object) from the default of 20%+500 rows to 
	SQRT(1000*<num rows in table>).
	
	2754171_ | SRGolla_ | SAPonSQLServer_ 

.. _2388: 

2388 (Info)
	Changes the output of DBCC SHOW_STATISTICS. Instead of the normal Header/Vector/Histogram output, instead we get a single row that 
	gives information related to whether the lead column of the stat object is considered to be ascending or not. This TF is primarily 
	helpful in watching the state of a stat object change from  "Unknown", to "Ascending" (and potentially to "Stationary").
	
	Nevarez_2_ | SQLSasquatch_2_ | Stellato_2_ 

	
.. _7471:
	
7471
	Allows multiple statistics objects on the same table to be updated concurrently. 
	
	3156157_ | TigerTeam_1_

.. _8721:
	
8721 ``Doc2014`` (Info)
	`BOL 2014`_: "Reports to the error log when auto-update statistics executes." The KB also describes locks taken by autostats. 
	
	195565_ | SQLSasquatch_1_

.. _9204:
	
9204 (Info)
	PWhite: for the old CE, gives "the interesting statistics which end up being fully loaded and used to produce cardinality 
	and distribution estimates for some plan alternative or other." 
	
	PWhite_4_ 

.. _9292:

9292 (Info)
	PWhite: for the old CE, gives "a report of statistics objects which are considered 'interesting' by the query optimizer when 
	compiling, or recompiling the query in question. For [these] potentially useful statistics, just the header is loaded."
	
	PWhite_4_ 

.. _9485: 

9485 ``Doc2012``
	`BOL 2014`_: "Disables SELECT permission for DBCC SHOW_STATISTICS."
	
	
|

.. _FixPastRel1:

Fixes and Past Relevance
------------------------
These flags either are old and irrelevant for modern builds, appear only in CTPs, or enable a fix 
in a CU but are baselined in a later service pack or release.




.. Official Links 

.. _BOL 2014: https://technet.microsoft.com/en-us/library/ms188396.aspx

.. _USE HINT: https://technet.microsoft.com/en-us/library/ms181714.aspx

.. _DB SCOPED CONFIG: https://technet.microsoft.com/en-us/library/mt629158.aspx

.. _2012SP2: http://support.microsoft.com/kb/2958429

.. _New CE Whitepaper: https://msdn.microsoft.com/en-us/library/dn673537.aspx

.. _195565: http://support.microsoft.com/kb/195565/en-us

.. _920093: http://support.microsoft.com/kb/920093

.. _922063: http://support.microsoft.com/?kbid=922063

.. _929278: http://support.microsoft.com/?kbid=929278

.. _980653: https://support.microsoft.com/en-us/kb/980653

.. _2658214: http://support.microsoft.com/kb/2658214

.. _2667211: https://support.microsoft.com/en-us/kb/2667211

.. _2754171: http://support.microsoft.com/kb/2754171

.. _2801413: http://support.microsoft.com/kb/2801413

.. _2952101: http://support.microsoft.com/kb/2952101

.. _2952444: http://support.microsoft.com/kb/2952444

.. _2998301: http://support2.microsoft.com/kb/2998301

.. _3156157: https://support.microsoft.com/en-us/kb/3156157

.. _3189675: https://support.microsoft.com/en-us/kb/3189675

.. _3192117: https://support.microsoft.com/en-us/kb/3192117

.. _CSS_1: http://blogs.msdn.com/b/psssql/archive/2014/08/11/if-you-have-queries-that-use-table-variables-sql-server-2012-sp2-can-help.aspx



.. MSFT Blog links

.. _IJose_1: http://blogs.msdn.com/b/ianjo/archive/2006/04/24/582219.aspx

.. _IJose_2: http://blogs.msdn.com/b/ianjo/archive/2006/03/28/563419.aspx

.. _IJose_3: http://blogs.msdn.com/b/ianjo/archive/2006/04/24/582227.aspx

.. _SAPonSQLServer: http://blogs.msdn.com/b/saponsqlserver/archive/2011/09/07/changes-to-automatic-update-statistics-in-sql-server-traceflag-2371.aspx

.. _SRGolla: http://blogs.msdn.com/b/srgolla/archive/2012/09/04/sql-server-statistics-explained.aspx

.. _TigerTeam_1: https://blogs.msdn.microsoft.com/sql_server_team/boosting-update-statistics-performance-with-sql-2014-sp1cu6/




.. Non-MSFT bloggers

.. _Bertrand: http://sqlperformance.com/2014/06/t-sql-queries/table-variable-perf-fix

.. _Dima_1: http://www.queryprocessor.com/ce_join_base_containment_assumption/

.. _Dima_2: http://www.somewheresomehow.ru/isnumeric_ce_bug_eng/

.. _Dima_3: http://www.somewheresomehow.ru/cardinality-estimation-framework-2014-first-look/

.. _Dima_4: http://www.queryprocessor.com/join-estimation-internals/

.. _Dima_5: http://www.queryprocessor.com/ce_opk/

.. _Dima_6: http://www.queryprocessor.com/ce_filteredstats/

.. _Dima_7: http://www.queryprocessor.com/ce_mtvf/

.. _Dima_8: http://www.queryprocessor.com/ce_asckey/

.. _Dima_9: http://www.somewheresomehow.ru/cardinality-estimation-framework-2014-first-look/

.. _Dima_10: http://www.queryprocessor.com/runtime-constants-sniffing/

.. _Itzik_1: http://sqlmag.com/sql-server/seek-and-you-shall-scan-part-ii-ascending-keys

.. _`Kejser Part 1`: http://blog.kejser.org/2011/07/01/the-ascending-key-problem-in-fact-tables-part-one-pain/

.. _`Kejser Part 2`: http://blog.kejser.org/2011/07/07/the-ascending-column-problem-in-fact-tables-part-two-stat-job/

.. _Milo_1: http://milossql.wordpress.com/2014/01/02/new-cardinality-estimator-part-4-single-table-and-multiple-predicates/

.. _Nevarez_1: http://www.sqlperformance.com/2013/12/t-sql-queries/a-first-look-at-the-new-sql-server-cardinality-estimator

.. _Nevarez_2: http://www.benjaminnevarez.com/2013/02/statistics-on-ascending-keys/

.. _PWhite_1: http://www.sqlperformance.com/2014/01/sql-plan/cardinality-estimation-for-multiple-predicates

.. _PWhite_2: https://answers.sqlperformance.com/questions/1609/trying-to-figure-out-how-to-resolve-the-data-skew.html

.. _PWhite_3: http://sqlperformance.com/2015/07/sql-plan/locking-and-performance

.. _PWhite_4: http://sqlblog.com/blogs/paul_white/archive/2011/09/21/how-to-find-the-statistics-used-to-compile-an-execution-plan.aspx

.. _Stellato_1: http://sqlperformance.com/2015/05/sql-statistics/incremental-statistics-are-not-used-by-the-query-optimizer

.. _Stellato_2: http://sqlperformance.com/2016/07/sql-statistics/trace-flag-2389-new-cardinality-estimator

.. _SQLSasquatch_1: http://sql-sasquatch.blogspot.com/2016/06/sql-server-trace-flag-8721-auto-stats.html

.. _SQLSasquatch_2: http://sql-sasquatch.blogspot.ca/2014/03/in-search-of-sqlserver-stats.html






.. Connect links

.. _Connect_1: https://connect.microsoft.com/SQLServer/feedback/details/772232/make-optimizer-estimations-more-accurate-by-using-metadata

.. _Connect_2: http://connect.microsoft.com/SQLServer/feedback/details/801908/sql-server-2014-cardinality-estimation-regression



.. Forums 

.. _SQLPerf_1: https://answers.sqlperformance.com/questions/2299/why-not-seek-predicate.html

.. _StackExch_1: http://dba.stackexchange.com/questions/55198/huge-slowdown-to-sql-server-query-on-adding-wildcard-or-top

.. _StackExch_2: http://dba.stackexchange.com/questions/57018/why-is-sql-server-using-a-clustered-index-scan-for-a-self-referencing-fk-cascade


.. Other Links 

.. _Database-Wiki: http://database-wiki.com/2012/10/20/documented-sql-server-trace-flags-use-them-cautiously/
	
.. _DBIServices_1: http://www.dbi-services.com/index.php/blog/entry/sql-server-2014-new-incremental-statistics


