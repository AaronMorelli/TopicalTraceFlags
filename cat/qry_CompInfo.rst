=========================
Compilation Informational
=========================

Flags in this category fall into the below groupings:

#. `Trees and Memos`_ --> Flag prints one or more of the "trees" constructed during the query optimization process, or the state of the memo at a given point in the optimization process.
#. `Phases and Rules`_ --> The phases entered by the optimizer and the rules it uses in those phases.
#. `Miscellaneous`_ --> Resource usage, operator-specific info, and other "miscellaneous" info.
#. `Fixes and Past Relevance`_ --> Fix flags and flags which are no longer relevant.

All flags in this category are informational (the redundant "Info" tag has been retained
for the sake of screenshots and copy-pasting), as far as we know.

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
	* - \* :doc:`2363 <qry_StatsAndEst>`
	  - New CE: Outputs info about stats used and resulting cardinality estimates.
	* - :doc:`8721 <qry_StatsAndEst>`
	  - Prints info to error log when AutoStat occurs.
	* - :doc:`9204 <qry_StatsAndEst>`
	  - Prints which stats objects the QO found interesting and then fully loaded.
	* - :doc:`9292 <qry_StatsAndEst>`
	  - Prints which stats objects the QO found interesting and loaded the header.
	* - ...
	  - 
	* - :ref:`Trees/Memos <TreeMemo>`
	  - 
	* - 7352_
	  - Final query tree after the post-optimization rewrite phase.
	* - 8605_
	  - Initial logical tree (input into actual optimization). AKA "converted tree".
	* - 8606_
	  - Multiple trees: Input, Simplified, Join-collapsed, and before/after Proj Norm
	* - 8607_
	  - Optimization output tree, before post-optimization rewrite phase.
	* - 8608_
	  - Initial Memo structure
	* - 8612_
	  - Adds cardinality info to trees produced by 8605, 8606, 8607.
	* - 8615_
	  - Final Memo structure.
	* - 8620_
	  - Adds Memo arguments to 8619 (which outputs the transformation rules applied)
	* - 8621_
	  - Adds trees to the transformation rule printout from 8619.
	* - ...
	  - 
	* - :ref:`Phases/Rules <PhasesRules>`
	  - 
	* - 2372_
	  - Prints memory utilization for each entered optimization phase.
	* - 2373_
	  - Prints memory utilization before/after each applied optimizer rule.
	* - 8609_
	  - Prints task and operation type counts
	* - 8619_
	  - Prints transformation rules applied during optimization.
	* - 8628_
	  - Places transformation rules used into the query plan. (8666 exposes in XML)
	* - 8675_
	  - Prints the query optimization phases entered.
	* - 8739_
	  - Dima: "Group Optimization Info"
	* - ...
	  - 
	* - :ref:`Misc <Misc>`
	  - 
	* - 205_
	  - Reports to error log when recompile occurs due to auto-update of stats.
	* - 445_
	  - Full func unknown; prints sql text from compilations to error log.
	* - 2315_
	  - Aaron discovered: appears to output mem allocations during compilation
	* - 2318_
	  - Aaron discovered: maybe info about join reordering?
	* - 2336_
	  - Aaron discovered: maybe connects cached page likelihoods, mem status, and costing?
	* - 2398_
	  - Aaron discovered: outputs info about "Smart Seek costing"
	* - 7357_
	  - Various info about hash join and match operators.
	* - 8666_
	  - Causes useful info present in the binary query plan to be exposed in the XML.
	* - 8719_
	  - (Maybe past relevance) Apparently outputs IO prefetch for NL joins/bookmarks.
	* - ...
	  - 
	* - :ref:`Fix/PastRel <FixPastRel2>`
	  - 

	 
.. This comment line is as long as we would ever want the short desc to be in the table above.

|

.. _TreeMemo: 

Trees and Memos
---------------

.. _7352:

7352 (Info)
	The final query tree after Post-optimization re-write. 
	
	PWhite_4_ | PWhite_5_ | Dima_5_ | Dima_6_ 

.. _8605:

8605 (Info)
	The initial logical tree (the input into query optimization). (Paul also calls this the “converted tree” in Part 4 of his series) 
	
	PWhite_1_ | Nevarez_1_

.. _8606:

8606 (Info)
	Displays additional logical trees, including the Input Tree, the Simplified Tree, the Join-collapsed Tree, the 
	"Tree before Project Normalization", and the "Tree after Project Normalization" 
	
	PWhite_2_ | Dima_1_ | Nevarez_1_ 


.. _8607:

8607 (Info)
	Displays the optimization output tree, before Post-optimization rewrite. 
	Has a "Query marked as cachable" note if the plan can be cached.

	PWhite_3_ | PWhite_6_ | PWhite_7_ | PWhite_8_ | PWhite_9_ | Dima_3_ | Dima_6_ | Nevarez_1_

.. _8608:

8608 (Info)
	Shows the initial Memo structure
 
	PWhite_3_ | Dima_3_ | Dima_4_ | Nevarez_2_

.. _8612: 

8612 (Info)
	Dima: adds cardinality info to the various trees produced by flags 8605, 8606, and 8607.
	
	PWhite_8_ | Dima_3_ | Dima_7_ | Dima_8_ 

.. _8615:

8615 (Info)
	Show the final Memo structure
	
	PWhite_3_ | Dima_3_ | Dima_4_ | Dima_9_ 

.. _8620: 

8620 (Info)
	PWhite: "Add memo arguments to 8619"
	
	PWhite_4_
	
.. _8621: 
	
8621 (Info)
	PWhite: "Rule with resulting tree" (use with 8619)
	
	PWhite_4_


|

.. _PhasesRules: 

Phases and Rules
----------------

.. _2372:

2372 (Info)
	Nevarez: "shows memory utilization during the different optimization stages."
	
	Nevarez_1_ | Dima_8_ 

.. _2373:

2373 (Info)
	Shows memory utilization before and after various optimizer rules are applied (e.g. IJtoIJsel). 
	Provides a way to "trace" what rules are used when optimizing a query.
	
	PWhite_4_ | Dima_2_ | Dima_8_ | Dima_9_ 

.. _8609:
	
8609 (Info)
	PWhite: "Task and operation type counts"
	
	PWhite_4_ | Dima_10_ 

.. _8619:
	
8619 (Info)
	PWhite: "Apply rule with description"; Dima: "Show Applied Transformation Rules"
	
	PWhite_4_ | PWhite_10_ | Dima_8_ 

.. _8628:

8628 (Info)
	When used with 8666 (see below), causes extra information about the transformation rules applied to be 
	put into the XML showplan.
	
	Dima_11_ | PWhite_11_ 
	
.. _8675:

8675 (Info)
	Display query optimization phases, along with info (timing, costs, etc) about each phase.
	
	PWhite_3_ | PWhite_12_ | PWhite_13_ | PWhite_9_ 

.. _8739:
	
8739 (Info)
	Dima: "Group Optimization Info"
	
	Dima_10_ 


|

.. _Misc:

Miscellaneous
----------------

.. _205:

205 ``Doc2014`` (Info)
	`BOL 2014`_: "Reports to the error log when a statistics-dependent stored procedure is being recompiled 
	as a result of auto-update statistics."
	Its appearance in `Randal-SQL-SDB407`_ indicates that this flag is fairly old.
	
	195565_ 

.. _445:

445 (Info)
	Full functionality unknown. Prints “Compile issued:” and then the text of the sql statement 
	being compiled to the SQL error log. Personally confirmed that this still works in SQL 2014 
	even though it appears to be a very old trace flag. Discovered via `SQLService.se`_

.. _2315:
	
2315 (Info)
	Aaron: personally discovered. Seems to output memory allocations taken during the compilation 
	process (and maybe the plan as well? "PROCHDR"), as well as memory broker states & values at 
	the beginning and end of compilation.

.. _2318:

2318 (Info)
	Aaron: personally discovered. I’ve only seen one type of output so far: 
	"Optimization Stage:  HEURISTICJOINREORDER". Maybe useful in combo with other compilation 
	trace flags to see the timing of join reordering?

.. _2336:

2336 (Info)
	Aaron: personally discovered. Appears to tie memory info and cached page likelihoods with costing.

.. _2398:

2398 (Info)
	Aaron: personally discovered. Outputs info about "Smart Seek costing": 
	e.g.: "Smart seek costing (75.2) :: 1.34078e+154 , 1".

.. _7357:

7357 (Info)
	Info re: hash operators, including role reversal, recursion levels, Unique Hash optimization 
	usage, hash-related bitmap, etc. For parallel query plans, 7357 does NOT send output to the 
	console window. However, output to the SQL Server error log can be enabled by enabling 3605. 
	
	Dima_12_ | PWhite_4_

.. _8666: 

8666 (Info)
	Causes some useful info (including stat object thresholds) already present in the internal 
	representation of a plan to be included in the XML plan output.
	
	Fabiano_1_ | DBally_1_ | PWhite_14_ | PWhite_15_ 

.. _8719: 

8719 (Info)
	In SQL 2000, apparently would show IO prefetch on loop joins and bookmarks. I (Aaron) was 
	unable to replicate the query plan behavior on SQL 2012 using the same test, so this flag 
	may be obsolete. (Would be really nice if it wasn't!)
	
	Hanlincrest_ 

| 

.. _FixPastRel2:

Fixes and Past Relevance
------------------------
These flags either are old and irrelevant for modern builds, appear only in CTPs, or enable a fix 
in a CU but are baselined in a later service pack or release.




.. Official Links 

.. _BOL 2014: https://technet.microsoft.com/en-us/library/ms188396.aspx



.. MSFT Blog links


.. Non-MSFT bloggers

.. _DBally_1: http://dataidol.com/davebally/2014/04/12/reasons-why-your-plans-suck-no-56536/

.. _Dima_1: http://www.somewheresomehow.ru/optimizer-part-1-simplification/

.. _Dima_2: http://www.somewheresomehow.ru/optimizer-part-2-trivial-plan-optimization/

.. _Dima_3: http://www.somewheresomehow.ru/optimizer-part-3-full-optimiztion-optimization-search0/

.. _Dima_4: http://www.somewheresomehow.ru/optimizer-part-4-optimization-full-optimization-search1/

.. _Dima_5: http://www.queryprocessor.com/batch-sort-and-nested-loops/

.. _Dima_6: http://www.queryprocessor.com/few-outer-rows-optimization/

.. _Dima_7: http://www.somewheresomehow.ru/rowgoal-on-non-uniform-distribution/

.. _Dima_8: http://www.somewheresomehow.ru/cardinality-estimation-framework-2014-first-look/

.. _Dima_9: http://www.somewheresomehow.ru/isnumeric_ce_bug_eng/

.. _Dima_10: http://www.somewheresomehow.ru/good-enough-plan/

.. _Dima_11: http://www.queryprocessor.com/tf_8628/

.. _Dima_12: http://www.queryprocessor.com/hash-join-execution-internals/

.. _Fabiano_1: http://blogfabiano.com/2012/07/03/statistics-used-in-a-cached-query-plan/

.. _Halincrest: http://www.hanlincrest.com/SQLServerLockEscalation.htm

.. _Nevarez_1: http://www.benjaminnevarez.com/2012/04/more-undocumented-query-optimizer-trace-flags/

.. _Nevarez_2: http://www.benjaminnevarez.com/2012/04/inside-the-query-optimizer-memo-structure/

.. _PWhite_1: http://sqlblog.com/blogs/paul_white/archive/2012/04/28/query-optimizer-deep-dive-part-1.aspx

.. _PWhite_2: http://sqlblog.com/blogs/paul_white/archive/2012/04/28/query-optimizer-deep-dive-part-2.aspx

.. _PWhite_3: http://sqlblog.com/blogs/paul_white/archive/2012/04/29/query-optimizer-deep-dive-part-3.aspx

.. _PWhite_4: http://sqlblog.com/blogs/paul_white/archive/2012/05/01/query-optimizer-deep-dive-part-4.aspx

.. _PWhite_5: http://sqlblog.com/blogs/paul_white/archive/2013/08/31/sql-server-internals-nested-loops-prefetching.aspx

.. _PWhite_6: http://sqlblog.com/blogs/paul_white/archive/2013/01/26/optimizing-t-sql-queries-that-change-data.aspx

.. _PWhite_7: http://sqlblog.com/blogs/paul_white/archive/2013/02/01/a-creative-use-of-ignore-dup-key.aspx

.. _PWhite_8: http://sqlblog.com/blogs/paul_white/archive/2013/06/11/hello-operator-my-switch-is-bored.aspx

.. _PWhite_9: http://www.sqlperformance.com/2013/07/sql-plan/working-around-missed-optimizations

.. _PWhite_10: http://sqlblog.com/blogs/paul_white/archive/2013/02/06/incorrect-results-with-indexed-views.aspx

.. _PWhite_11: http://sqlperformance.com/2015/04/sql-plan/internals-of-the-seven-sql-server-sorts-part-1

.. _PWhite_12: http://sqlblog.com/blogs/paul_white/archive/2013/06/17/improving-partitioned-table-join-performance.aspx

.. _PWhite_13: http://www.sqlperformance.com/2013/06/sql-indexes/recognizing-missed-optimizations

.. _PWhite_14: http://sqlperformance.com/2015/12/sql-plan/optimizing-update-queries

.. _PWhite_15: http://sqlperformance.com/2016/03/sql-plan/changes-to-a-writable-partition-may-fail


.. Connect links


.. Forums 


.. Other Links 

.. _Randal-SQL-SDB407: http://www.scribd.com/doc/109431789/Randal-SQL-SDB407-Undocumented

.. _SQLService.se: http://sqlservice.se/sv/start/blogg/updated-microsoft-sql-server-trace-flag-list.aspx
