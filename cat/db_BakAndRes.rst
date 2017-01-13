==================
Backup and Restore
==================

Flags in this category fall into the below groupings:

#. `Allocation`_ --> Flags that modify how pages in MDF/NDF files are allocated to individual objects.
#. `Other`_ --> Functionality toggles not related to page allocation.
#. `Fixes and Past Relevance`_ --> Fix flags and flags which are no longer relevant.


See also: 

TODO: 3028 may need to be moved to a fix flag section, ditto for 3057, 3111, 3117 
TODO: 3607 should at least be referenced in the TranLog Recovery section, and in the CheckDB Corruption section.
TODO: 9958 should at least be referenced in the TranLog Recovery section


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
	* - 1140_
	  - Workaround for mixed page allocation bug.
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


|

.. _FixPastRel1:

Fixes and Past Relevance
------------------------
These flags either are old and irrelevant for modern builds, appear only in CTPs, or enable a 
fix in a CU but are baselined in a later service pack or release.

.. _1140:

1140
	Workaround for a bug in 2005SP2/SP3 and SQL 2008 where mixed page allocations climb continually, 
	due to a change in the way that mixed-page allocations are done. KB has a great description 
	of both the "old" and "new" way that free pages are found for a mixed-page allocation to be performed.

	2000471_

	


.. Official Links 

.. _BOL 2014: https://technet.microsoft.com/en-us/library/ms188396.aspx

.. _BOL 2016: https://technet.microsoft.com/en-us/library/ms188396.aspx

.. _ALTER DATABASE file and filegroup options: https://msdn.microsoft.com/en-us/library/bb522469.aspx

.. _ALTER DATABASE SET Options: https://msdn.microsoft.com/en-us/library/bb522682.aspx


.. MSFT Blog links



.. Non-MSFT bloggers



.. Connect links


.. Forums 



.. Other Links 
