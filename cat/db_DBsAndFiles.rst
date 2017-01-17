===================
Databases and Files
===================

Flags in this category fall into the below groupings:

#. `Allocation`_ --> Flags that modify how pages in MDF/NDF files are allocated to individual objects.
#. `Other`_ --> Flags not related to page allocation.
#. `Fixes and Past Relevance`_ --> Fix flags and flags which are no longer relevant.


See also: 3449 and 8903 ([Disk and Network IO](https://github.com/AaronMorelli/SQLServerTraceFlags/blob/master/Categories/DiskNetworkIO.md))

(TODO: ensure 1802 is present in the See Also for Security)
(TODO: find other entry for 9394 and place an in-line link to its other home. Ditto for the other section)
(TODO: 10207 should be moved to the corrupt Checkdb section and placed here in the see also section)
TODO: is 9929 in multiple locations? e.g. the special features one?
TODO: should 1482 move to a t-log focused page?
TODO: 2330 should have a reference in one of the optimization pages

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
	* - ...
	  - 
	* - :ref:`Allocation <FAlloc>`
	  - 
	* - 670_
	  - (or 671) Disables deferred deallocation.
	* - 1106_
	  - Creates a new ring buffer to track TempDB allocations.
	* - \* 1117_
	  - When one files grows, all files in filegroup grow with it.
	* - \* 1118_
	  - Eliminates most single page allocations.
	* - 1165_
	  - Prints calculations used for proportional fill algorithm.
	* - 1180_
	  - Changes the way text/image data is allocated
	* - 1197_
	  - Prevents use of worktable cache for certain TempDB allocations.
	* - 1806_
	  - Disables instant file initialization
	* - ...
	  - 
	* - :ref:`Other <DBFOther>`
	  - 
	* - 272_
	  - Reverts identity column generation behavior to pre-SQL 2012 behavior.
	* - 647_
	  - Skips a new-in-2012 data check that lengthens ALTER TABLE durations.
	* - 1140_
	  - Allows reversion to an older algorithm for mixed page allocs.
	* - 1482_
	  - Prints info on a variety of t-log management operations.
	* - 1808_
	  - Ignores auto-closing DBs even if auto-close property is set.
	* - 1816_
	  - Provides more details around IO errors for stretch database.
	* - 2330_
	  - Stops data collection for sys.db_index_usage_stats.
	* - 2548_
	  - Causes DBCC SHRINK and other options to skip LOB_COMPACTION.
	* - 3861_
	  - Allows DB startup code to move system tables to primary FG.
	* - 9851_
	  - Disables Hekaton auto-merge process for checkpoint files.
	* - 9929_
	  - Reduces Hekaton checkpoint files to 1 MB each.
	* - 10204_
	  - Disables merge/recompress of smaller columnstore delta rowgroups.
	* - 10207_
	  - Disables skipping of corrupt segments for columnstore scans.
	* - ...
	  - 
	* - :ref:`Fix/PastRel <FixPastRel6>`
	  - 
	* - 1802_
	  - Workaround for permissions change upon DB detach
	* - 1807_
	  - Allows creation of DB file on UNC paths
	* - 9394_
	  - Enables fix for AV re: Japanese characters
	* - 10202_
	  - Enabled new column store DMV in pre-release SQL version.

	 
.. This comment line is as long as we would ever want the short desc to be in the table above.

|

.. _FAlloc: 
	 
Allocation
----------

.. _670: 

670 (or 671)
	CSS: Disables deferred deallocation. But note Paul White’s comment on the post! The flag 
	number may actually be 671.
	
	CSS_1_


.. _1106: 

1106 (Info)
	Creates a new RB in sys.dm_os_ring_buffers that tracks allocations made in TempDB.
	
	947204_ | BobWard_Pass2011_ | Arvind_1_
	
	
.. _1117:

1117 ``Doc2014``
	`BOL 2014`_: "When a file in the filegroup meets the autogrow threshold, all files in the filegroup grow.

	Note: Beginning with SQL Server 2016 this behavior is controlled by the AUTOGROW_SINGLE_FILE and 
	AUTOGROW_ALL_FILES option of ALTER DATABASE, and trace flag 1117 has no affect."
	
	This flag is commonly associated with tempdb but applies to all databases when on. The flag is typically
	used to ensure that all files grow evenly to maintain a well balanced proportional-fill allocation algorithm.
	Nacho gives a very special/rare edge case for sysfiles1. Chris Adkin has some interesting screenshots on its 
	effect under certain workloads. (PRand_1_ doesn't reference this flag but its info is highly relevant to
	this topic.)
	
	`ALTER DATABASE file and filegroup options`_ | BobWard_Pass2011_ | PRand_4_ | 
	Nacho_2_ | CAdkin_2_ | SQLArticlesDotCom_

.. _1118: 

1118 ``Doc2014``
	`BOL 2014`_: "Removes most single page allocations on the server, reducing contention on the SGAM page. When a 
	new object is created, by default, the first eight pages are allocated from different extents (mixed extents). 
	Afterwards, when more pages are needed, those are allocated from that same extent (uniform extent). The SGAM 
	page is used to track these mixed extents, so can quickly become a bottleneck when numerous mixed page allocations 
	are occurring. This trace flag allocates all eight pages from the same extent when creating new objects, 
	minimizing the need to scan the SGAM page. For more information, see this Microsoft Support article.

	Note: Beginning with SQL Server 2016 this behavior is controlled by the SET MIXED_PAGE_ALLOCATION option of 
	ALTER DATABASE, and trace flag 1118 has no affect."
	
	`ALTER DATABASE SET Options`_ | 328551_ | 837938_ | 936185_ | 2154845_ | 
	CSS_3_ | CSS_4_ | PRand_5_ | CAdkin_2_

	
.. _1140:

1140
	Allows reversion to an older, more aggressive form of the mixed-page-allocation algorithm. 
	The flag was introduced as a workaround for a bug in 2005SP2/SP3 and SQL 2008 where mixed page 
	allocations climb continually in tempdb for workloads that use tempdb extremely heavily. That 
	behavior was due to a change in the way that mixed-page allocations are done. KB has a great 
	description of both the "old" and "new" way that free pages are found for a mixed-page 
	allocation to be performed.

	2000471_

	
.. _1165:

1165 (Info)
	Outputs the recalculated #’s (every 8192 allocations) for the proportional fill algorithm 
	when multiple files are present. Requires TF 3605, output goes to SQL error log.
	
	BobWard_Pass2011_ | PRand_1_
	

.. _1180:

1180
	(Very old, may not be functional) KB notes that after a SQL 7.0 fix is installed, this 
	flag will cause text/image data to be placed in free pages in partially-allocated extents; 
	w/o the flag, text/image data is placed in newly-allocated extents until the file size 
	limit is reached; only then will partially-allocated extents be used for new data.
	
	272220_

	
.. _1197:
	
1197
	Bob Ward uses to prevent allocation of TempDB pages (by Work Tables) from being pulled from 
	a worktable cache (see around 1:25:00). The (very old) KB references for use w/1180 in 
	reclaiming space from inefficiently-stored text/image data.
	
	BobWard_Pass2011_ | 324432_


.. _1806:
	
1806
	Disables instant file initialization.
	
	2574695_ | PFE_1_ | PRand_2_ 

|

.. _DBFOther: 
	 
Other
-----

.. _272:

272
	Connect: "In SQL Server 2012 the implementation of the identity property has been changed to accommodate 
	investments into other features. In previous versions of SQL Server the tracking of identity generation 
	relied on transaction log records for each identity value generated. In SQL Server 2012 we generate identity 
	values in batches and log only the max value of the batch. This reduces the amount and frequency of information 
	written to the transaction log improving insert scalability. If you require the same identity generation 
	semantics as previous versions of SQL Server there are two options available:
		- Use trace flag 272. This will cause a log record to be generated for each generated identity value. The performance of identity generation may be impacted by turning on this trace flag.
		- Use a sequence generator with the NO CACHE setting. This will cause a log record to be generated for each generated sequence value. Note that the performance of sequence value generation may be impacted by using NO CACHE."

	Later in the Connect discussion, one commenter notes that when adding the TF as a startup flag, the flag only 
	appears to work when using the "lowercase t" syntax rather than the more common "uppercase T" syntax.
		
	Connect_1_ 


.. _647: 
	
647
	Avoids a new-in-SQL 2012 data check (done when adding a column to a table) that can cause 
	ALTER TABLE… ADD <column> operations to take a very long time. The KB has a useful query 
	for determining the row size for a table. 
	
	2986423_ 


.. _1482: 

1482 (Info)
	Prints info to the Error Log (3605 not necess.) for a variety of transaction log operations, 
	including when MinLSN value is reset, when a VLF is formatted, etc. (First discovered in 
	Bob Ward’s PASS 2014 talk on SQL Server IO, and then tested for myself.)
	
	<links needed>


.. _1808: 

1808
	Directs SQL Server to ignore auto-closing databases even if the Auto-close property is set 
	to ON. Must be set globally. Present in 2005+.
	
	Nacho_1_


.. _1816: 

1816 (Info)
	Bob Ward briefly references this flag in his PASS2014 IO talk, saying it "could provide 
	more details around errors" that occur with IO done to SQL data files in Azure Storage 
	(stretch/http IO, I think he means).

	<links needed>
	

.. _2330:

2330
	Stops the collection of statistics for sys.db_index_usage_stats. CAdkin: also disables
	sys.dm_db_missing_index_group_stats, and thus is useful when seeing high waits on the 
	OPT_IDX_STATS spinlock.
	
	2003031_ | PRand_3_ | BrentOzar_1_ | CAdkin_1_

	
.. _2548: 
	
2548
	"SQL 2005 has a –T2548 dbcc tracon(-1, 2548) that allows shrink* and other LOB_COMPACTION actions 
	to be skipped. Enabling this returns shrink* behavior to that similar to SQL 2000."
	
	CSS_2_


.. _3861: 

3861
	Allows the DB startup code to move system tables to the primary filegroup. Introduced for a 
	bug in SQL 2014 upgrade process, where system tables could be created in a secondary filegroup 
	(if that FG was the default).
	
	3003760_
	
	
.. _9851: 
	
9851
	Disables Hekaton’s auto-merge process; if this flag is enabled, the various merge-related 
	procedures will need to be called manually. First seen in a Sunil Agarwal session at 
	PASS 2014, also present in Kalen Delaney’s book on Hekaton.
	
	<links needed>
	
.. _9929:
	
9929
	Enables an update that reduces the "disk footprint [of In-Memory OLTP] by reducing the 
	In-Memory checkpoint files to 1 MB (megabytes) each."
	
	3147012_


.. _10204: 

10204 ``Doc2016``
	`BOL 2016`_: "Disables merge/recompress during columnstore index reorganization. In SQL 
	Server 2016, when a columnstore index is reorganized, there is new functionality to 
	automatically merge any small compressed rowgroups into larger compressed rowgroups, as 
	well as recompressing any rowgroups that have a large number of deleted rows."

	
.. _10207:

10207
	When a CCI is corrupt, allows a scan to skip corrupt segments and suppress errors 5288 and 
	5289, thus enabling the copy-out of data in a corrupt CCI.
	
	3067257_ | RelSvcs_1_ 


|

.. _FixPastRel6:

Fixes and Past Relevance
------------------------
These flags either are old and irrelevant for modern builds, appear only in CTPs, or enable a 
fix in a CU but are baselined in a later service pack or release.

.. _1802:
	
1802
	Workaround for: "after you detach a Microsoft SQL Server 2005 database that resides on network-
	attached storage, you cannot reattach the SQL Server database... This problem occurs because SQL 
	Server 2005 resets the file permissions when the database is detached. When you try to reattach 
	the database, it cannot be attached because of limited share permissions." 
	
	It sounds like this flag disables functionality in changing permissions on database files after 
	the DB is detached, thus has security implications.
	
	922804_ | StorEng_1_ (comments, though Kevin likely means 1807)

.. _1807:
	
1807
	Allows the creation of a database file on UNC paths, and is a workaround for errors 5105 and 
	5110. The KB describes MSFT policy towards DBs on network locations.
	
	304261_

.. _9394:
	
9394
	(9394 is either doing double-duty or there’s a typo. See other entry for 9394) Apparently 
	enables a fix for an access violation when a table with Japanese characters has an 
	indexed changed.
	
	3142595_


.. _10202:
	
10202
	Sunil Agarwal PASS 2014 demo script: enables new DMV named sys.dm_db_column_store_row_group_physical_stats. 
	DMV was not in 2014 at the time of the demo, thus appears to be in a future (or internal) 
	version of SQL Server.



.. Official Links 

.. _BOL 2014: https://technet.microsoft.com/en-us/library/ms188396.aspx

.. _BOL 2016: https://technet.microsoft.com/en-us/library/ms188396.aspx

.. _ALTER DATABASE file and filegroup options: https://msdn.microsoft.com/en-us/library/bb522469.aspx

.. _ALTER DATABASE SET Options: https://msdn.microsoft.com/en-us/library/bb522682.aspx

.. _USE HINT: https://technet.microsoft.com/en-us/library/ms181714.aspx

.. _DB SCOPED CONFIG: https://technet.microsoft.com/en-us/library/mt629158.aspx

.. _2012SP2: http://support.microsoft.com/kb/2958429

.. _272220: https://support.microsoft.com/en-us/kb/272220

.. _304261: https://support.microsoft.com/en-us/kb/304261

.. _324432: https://support.microsoft.com/en-us/kb/324432

.. _328551: https://support.microsoft.com/en-us/kb/328551

.. _837938: https://support.microsoft.com/en-us/kb/837938

.. _922804: https://support.microsoft.com/en-us/kb/922804

.. _936185: https://support.microsoft.com/en-us/kb/936185

.. _947204: https://support.microsoft.com/en-us/kb/947204

.. _2000471: https://support.microsoft.com/en-us/kb/2000471

.. _2003031: https://support.microsoft.com/en-us/kb/2003031

.. _2154845: https://support.microsoft.com/en-us/kb/2154845

.. _2574695: https://support.microsoft.com/en-us/kb/2574695

.. _2986423: https://support.microsoft.com/en-us/kb/2986423

.. _3003760: https://support.microsoft.com/en-us/kb/3003760

.. _3067257: https://support.microsoft.com/en-us/kb/3067257

.. _3142595: https://support.microsoft.com/en-us/kb/3003760

.. _3147012: https://support.microsoft.com/en-us/kb/3147012



.. MSFT Blog links

.. _Arvind_1: https://blogs.msdn.microsoft.com/arvindsh/2014/02/24/tracking-tempdb-internal-object-space-usage-in-sql-2012/

.. _BobWard_Pass2011: https://www.youtube.com/watch?v=SvseGMobe2w&feature=youtu.be

.. _CSS_1: https://blogs.msdn.microsoft.com/psssql/2009/11/17/how-it-works-controlling-sql-server-memory-dumps/

.. _CSS_2: http://blogs.msdn.com/b/psssql/archive/2008/03/28/how-it-works-sql-server-2005-dbcc-shrink-may-take-longer-than-sql-server-2000.aspx

.. _CSS_3: https://blogs.msdn.microsoft.com/psssql/2008/12/17/sql-server-2005-and-2008-trace-flag-1118-t1118-usage/

.. _CSS_4: https://blogs.msdn.microsoft.com/psssql/2009/06/04/sql-server-tempdb-number-of-files-the-raw-truth/

.. _Nacho_1: https://blogs.msdn.microsoft.com/ialonso/2012/04/11/want-your-sql-server-to-simply-ignore-the-auto_close-setting-for-all-open-databases-for-which-it-has-been-enabled/

.. _Nacho_2: https://blogs.msdn.microsoft.com/ialonso/2011/12/01/attempt-to-grow-all-files-in-one-filegroup-and-not-just-the-one-next-in-the-autogrowth-chain-using-trace-flag-1117/

.. _PFE_1: https://blogs.msdn.microsoft.com/sql_pfe_blog/2009/12/22/how-and-why-to-enable-instant-file-initialization/

.. _RelSvcs_1: https://blogs.msdn.microsoft.com/sqlreleaseservices/partial-results-in-a-query-of-a-clustered-columnstore-index-in-sql-server-2014/

.. _StorEng_1: https://blogs.msdn.microsoft.com/sqlserverstorageengine/2010/02/21/backup-compression-and-virtual-device-interface-vdi/


.. Non-MSFT bloggers

.. _BrentOzar_1: https://www.brentozar.com/archive/2015/11/trace-flag-2330-who-needs-missing-index-requests/

.. _CAdkin_1: https://exadat.co.uk/2015/04/14/well-known-and-not-so-well-known-sql-server-tuning-knobs-and-switches/

.. _CAdkin_2: https://exadat.co.uk/2015/04/14/well-known-and-not-so-well-known-sql-server-tuning-knobs-and-switches/

.. _PRand_1: http://www.sqlskills.com/blogs/paul/investigating-the-proportional-fill-algorithm/

.. _PRand_2: http://www.sqlskills.com/blogs/paul/a-sql-server-dba-myth-a-day-330-instant-file-initialization-can-be-controlled-from-within-sql-server/

.. _PRand_3: http://www.sqlskills.com/blogs/paul/the-pros-and-cons-of-trace-flags/

.. _PRand_4: http://www.sqlskills.com/blogs/paul/tempdb-configuration-survey-results-and-advice/

.. _PRand_5: http://www.sqlskills.com/blogs/paul/misconceptions-around-tf-1118/



.. Connect links

.. _Connect_1: http://connect.microsoft.com/SQLServer/feedback/details/739013/alwayson-failover-results-in-reseed-of-identity


.. Forums 



.. Other Links 

.. _SQLArticlesDotCom: http://sql-articles.com/articles/general/day-6trace-flag-1117-auto-grow-equally-in-all-data-file/

