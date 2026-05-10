# SQLynx AS

SQLynx is a specialized consultancy focused on SQL Server performance diagnostics, troubleshooting, and query tuning for Microsoft SQL Server and Azure SQL.

The focus is production-grade diagnostics for SQL Server environments affected by plan cache bloat and unstable observability.

- Company: SQLynx AS (Norway)
- Website: https://sqlynx.no
- Toolkit: https://sqlynx.no/sqlynx-performance-kit/

# SQLynx Performance Kit

Specialized SQL Server performance diagnostics for **Microsoft SQL Server** and **Azure SQL**.

This repository currently contains a single focused stored procedure and is intended to grow into a broader toolkit over time. The emphasis is on **production-grade diagnostics** for scenarios where traditional performance analysis becomes unreliable.

About https://sqlynx.no/sqlynx-performance-kit/

## Overview

SQLynxTool is designed for troubleshooting SQL Server environments affected by **plan cache bloat** and unstable workload observability.

The toolkit focuses on scenarios where traditional DMV-based performance analysis becomes unreliable because execution statistics are short-lived, incomplete, or continuously evicted from cache.

Instead of relying on stable long-term aggregation, SQLynxTool focuses on extracting actionable diagnostic signals from volatile and degraded plan cache conditions.



## Current Component

### `sp_LynxTopQueryMemoryLoad`

`sp_LynxTopQueryMemoryLoad` is a specialized diagnostic stored procedure intended for use in scenarios involving **plan cache bloat**, where performance statistics stored in DMVs become unreliable because plans are evicted too frequently.

Under these conditions:

- Plan cache churn causes frequent eviction
- Aggregated DMV statistics disappear before they become useful
- Standard “top query” analysis may miss important queries
- Historical CPU, I/O, and duration metrics become incomplete or misleading
- High-impact queries may never stay in cache long enough to stand out in traditional reports

This procedure is designed to help recover useful diagnostic signals when standard workload analysis breaks down.

---

## Problem Context

In healthy environments, SQL Server DMVs often provide enough information to identify expensive or high-impact queries through cumulative metrics.

That approach becomes much less effective when plan cache bloat is present.

Typical symptoms include:

- Large volumes of single-use or low-reuse plans
- Frequent recompilation or plan eviction
- Poor visibility into workload history
- Inconsistent or fragmented query-level statistics
- Performance instability without obvious “top offenders”

---

## Purpose

This procedure is intended to help answer:

- Which queries are contributing to **memory pressure**, even when DMV statistics are unstable?
- Which query patterns remain visible despite plan cache churn?
- Which statements should be prioritized for execution plan analysis?
- How can we identify problematic workload patterns when traditional aggregation fails?

---

## Technical Focus

The procedure is specifically oriented toward **query memory load** and related execution characteristics.

It emphasizes signals associated with:

- Memory grant behavior
- Sort-intensive operators
- Hash-intensive operators
- Query structures that tend to consume workspace memory
- Execution characteristics that remain observable despite cache churn
- Patterns that may indicate inefficient grant sizing or memory-heavy plan design

---

## Data Sources

The procedure relies on SQL Server DMVs, including:

- `sys.dm_exec_query_stats`
- `sys.dm_exec_sql_text`
- `sys.dm_exec_query_plan`

These sources are used with the understanding that, in plan cache bloat scenarios:

- Data may be incomplete
- Aggregation windows may be short-lived
- Important queries may disappear quickly

---

## Usage

EXEC dbo.sp_LynxTopQueryMemoryLoad;


## Typical scenarios

- Suspected plan cache bloat  
- Memory pressure without clear top queries  
- Inconsistent or “random” performance degradation  
- High-variation or ad hoc-heavy workloads  
- Production incident analysis where DMV history is unreliable  
- Situations where traditional “top CPU / reads / duration” analysis fails  

---

## Interpretation Notes

- Results represent a partial, time-sensitive view  
- Absence of a query does not imply low impact  
- Findings should be validated via execution plans and waits  

---

## Design Principles

- Focused diagnostics  
- Signal over completeness  
- Minimal overhead  
- Production-first  

---

## SQLynx Performance Kit

👉 https://sqlynx.no/sqlynx-performance-kit/
