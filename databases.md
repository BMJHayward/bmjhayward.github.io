### The SQL Server Query Optimizer
#### https://www.red-gate.com/simple-talk/databases/sql-server/performance-sql-server/the-sql-server-query-optimizer/

+ It is a cost-based optimiser
+ analyses several execution plans for query, chooses the cheapest
+ join-order of a query has big effect
  - efficient order improves query time
  - can increase number of plans needed to analyse

2 main components in core:
  1. storage engine
  2. query processor (aka relational engine)

+ storage engine manages disk<->RAM IO, concurrency and data integrity
+ query processors takes queries, makes plans, executes, delivers query results

sql statement
     |
     V
 parsing -> parse tree/algebrised tree
     |
     V
 binding
     |
     V
 query optimiser -> execution plan
     |
     V
 execution engine -> plan caching in RAM
     |
     V
 results


optimiser translates logical ops from query into physical ops to get their cost
could be millions of possible plans, not all are calculated

search space of a query: all possible execution plans
  + all possible plans in this space return the same results
  + candidate plans are kept in RAM during opt step, in the "Memo" object

cost of query = Σ(physical operators)
cost of a physical op is some pre-determined formula of CPU, RAM, IO
using cached plan not always optimal, as parameters and metadata may change
use hints as a last resort because they override the optimiser

search space is often combinatorial explosion - i.e. can't ever efficiently look
for the best query
- accurate cost and cardinality estimation are optimisers main goals
sql server query optimiser is based on the cascades framework 1999
  + is extensible so new rules and operators etc can be added easily

db can give you actual or estimated plans in graphic, text or xml
if asking for estimated plan, engine may execute something else as query
may be recompiled

physical operators implement minimum 3 methods:
  open()
  close()
  getrow()

To get different plan formats:

--------------------------------------------------------------
P        Estimated Execution Plan 	Actual Exection Plan      |
--------------------------------------------------------------
Text     SET SHOWPLAN_TEXT ON       SET STATISTICS PROFILE ON |
--------------------------------------------------------------
Text     SET SHOWPLAN_ALL ON 	    SET STATISTICS PROFILE ON |
--------------------------------------------------------------
Graphic  in IDE 	                in IDE                    |
--------------------------------------------------------------
XML      SET SHOWPLAN_XML ON 	    SET STATISTICS XML ON     |
--------------------------------------------------------------

e.g.

```sql
SET SHOWPLAN_XML ON
GO
SELECT DISTINCT(City) FROM Person.Address
GO
SET SHOWPLAN_XML OFF
```

To see plans for currently running queries:
```sql
SELECT query_plan FROM sys.dm_exec_requests
CROSS APPLY sys.dm_exec_query_plan(plan_handle)
WHERE session_id = 135
```

join ordering is complicated and a current research topic
the columns/conditions of the join are called the "join predicate"
if multiple joins, they don't have to be done sequentially, they can start at
different times
2 main decisions:
  1. select a join ordering
  2. select a join algorithm
joins are associative and commutative
note the execution plan might show a different join order to the one in the query
queries are tree objects in processor, shape is affected by join ordering e.g:
  + left-deep trees
  + right-deep trees
  + bushy trees

e.g. left deep:
`JOIN( JOIN( JOIN(A, B), C), D)`
e.g. bushy:
`JOIN(JOIN(A, B), JOIN(C, D))`

Tables 	Left-Deep Trees 	Bushy Trees
1 	1 	1
2 	2 	2
3 	6 	12
4 	24 	120
5 	120 	1,680
6 	720 	30,240
7 	5040 	665,280
8 	40,320 	17,297,280
9 	362,880 	518,918,400
10 	3,628,800 	17,643,225,600
11 	39,916,800 	670,442,572,800
12 	479,001,600 	28,158,588,057,600

left-deep trees grow by n!
bushy grows by  (2n-2)! / (n-1)1

table scan:
index scan:
index seek:

logical ops physical ops

join        nested loops join
            merge join
            hash join

## Designing Data Intensive Appllications ##

#### Martin Kleppmann ####

##### Part 1 chapter 1 #####

reliable scalable maintainable
data intesive vs compute intensive applications

reliable: performs as expected
  doesn't fall apart when used in unexpected ways
  suitable perf for load and use cases

faults vs failures, not the same thing
tolerate vs prevent faults, depending on available solutions and resources
hardware vs software vs human errors

latency vs response time
latency is waiting to be handled
response time is users view of time taken

use percentiles instead of summary stats mean and median. especially mean as it's skewable
keep list of response times and plot the distribution
  - if too much data, there are agg algorithms to help e.g. forward decay, t-digest

can the system still evolve and maintain when it gets old? design for that

##### chapter 2 #####
data models and query languages

for each layer of data representation, ask how it is represented on the layer below i.e. closer to the hardware, and how is it used in the layer above
document model vs relational model vs hierarchical vs network
relational is dominant
in relational, processing is done in transactions or batches
nosql models exist as well, highly varied, motivated by scaling and flexibility demands
polyglot persistence -> use or relational + other data storage models in a platform/application
object-relational mismatch: a clunky transition layer is needed between db relations and OOP structures
json model of data storage is convenient for querying
normalisation - if data is repeated instead of referenced, it's not normalised
use many-to-one relationships with foreign keys to achieve normalisation
doument model, json model has more flexible, or no schema
  makes one-to-many relationships easy, but many-to-many is difficult
  joins are difficult
hierarchy model each record has one and only one parent node
network model: records can have more than one parent
  developer has to write the access path for each data access, complex
relational model: records are rows of data, can be added whenever
access path is simple - query, index and query plan is written by the optimiser, not the developer
  - i.e. select * from tableName; gets everything

nosql models have better data locality - i.e. a whole record or useful unit of data is stored together
    e.g. in a single json record in a json array
migrations between schemas can be more complex in sql/relational, but less app code changes
locality is achievable in relational db as well
  - e.g. nesting/grouping related data eg spanner db, hbase, column-family in bigtable model
sql is declarative language, most programming languages are imperative
declarative is easier to parallelise



