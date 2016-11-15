===========
Disclaimers
===========

The extensive set of trace flags in SQL Server, both documented and undocumented, provide a powerful way to modify
the behavior of the SQL Server engine and to learn more about its inner workings. A working familiarity with the
the more commonly-used trace flags can be helpful in designing and supporting non-trivial SQL Server installations,
and it is difficult to go very far in the blogosphere w/o encountering undocumented trace flags. A true master 
SQL Server professional understands trace flags well enough to use them properly and avoid abuse.

However, the power of trace flags and the number of undocumented flags (or even the number of documented but not 
well-understaood flags) makes it imperative to always tread with caution. The vast majority of flags in this
repository should never be used in a production environment w/o the direct guidance of Microsoft support. 

Limitations of this repository:

- The descriptions chosen for each flag in this repository are summaries created by the contributors and cannot be guaranteed to completely describe every behavioral impact of the flag being described.
	
- The summary descriptions usually derive from existing content on the internet (reachable via the links) and only very rarely reflect the individual experiences of the contributors to this repository. The accuracy of these descriptions can only ever be as good as its sources, and no guarantee is every made that the sources are accurate. 
	
- This repository makes no claim, nor is able to make the claim, that it comprehensively lists every trace flag in the SQL Server product. Many flags may exist only in internal builds inside Microsoft. Additionally, the numeric gaps between flags may or may not indicate the presence of undiscovered flags. There is simply no way for anyone outside of direct employees of Microsoft with source code access to create a truly "comprehensive repository".
	
- No attempt has been made to define the lifespan (earliest and latest builds) for a given trace flag and its effect. The number of flags and the number of SQL Server versions in "recent/modern" history make this effort completely unfeasible. The lifespan of a given flag may sometimes be determined from the nature of the flag, or from various online resources. 
	
