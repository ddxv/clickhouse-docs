---
title: How to output send logs level traces to file using the clickhouse-client
description: “How to output send logs level traces to file using the clickhouse-client“
date: 2024-11-21
---

# How to output send logs level traces to file using the clickhouse-client

### Question

How do I output the send_logs_level output to a file using the ClickHouse Client for multiple statements and multiple lines?

### Answer

- Create a SQL file with the statements, for example, `send_logs_level_example.sql`:
```
SET send_logs_level = 'trace';
SELECT * FROM db1.table1;
```

- Run command to write to the screen and to the file:
```
cat send_logs_level_example.sql | ./clickhouse client -n -m --host abc123.us-west-2.aws.clickhouse.cloud --secure --port 9440 --password ABC123 --send_logs_level=trace 2>&1 | tee send_log_results.txt
```

- Example results:
```
[c-azure-gk-72-server-tojnalg-0] 2024.08.13 21:31:28.437263 [ 1247 ] {4b347939-db0f-48f8-9550-1ccc7d591c44} <Debug> executeQuery: (from 71.56.215.107:47946) SET send_logs_level = 'trace'; (stage: Complete)
[c-azure-gk-72-server-tojnalg-0] 2024.08.13 21:31:28.437546 [ 1247 ] {4b347939-db0f-48f8-9550-1ccc7d591c44} <Debug> TCPHandler: Processed in 0.000727434 sec.
[c-azure-gk-72-server-tojnalg-0] 2024.08.13 21:31:28.508066 [ 1247 ] {3406aa56-20e8-44e0-b5de-8cb7715861f3} <Debug> executeQuery: (from 71.56.215.107:47946) SELECT * FROM db1.table1; (stage: Complete)
[c-azure-gk-72-server-tojnalg-0] 2024.08.13 21:31:28.508437 [ 1247 ] {3406aa56-20e8-44e0-b5de-8cb7715861f3} <Trace> InterpreterSelectQuery: FetchColumns -> Complete
[c-azure-gk-72-server-tojnalg-0] 2024.08.13 21:31:28.508530 [ 1247 ] {3406aa56-20e8-44e0-b5de-8cb7715861f3} <Debug> db1.table1 (781f25db-3cd1-47c6-a76e-701945a67485) (SelectExecutor): Key condition: unknown
[c-azure-gk-72-server-tojnalg-0] 2024.08.13 21:31:28.508581 [ 1247 ] {3406aa56-20e8-44e0-b5de-8cb7715861f3} <Trace> db1.table1 (781f25db-3cd1-47c6-a76e-701945a67485) (SelectExecutor): Filtering marks by primary and secondary keys
[c-azure-gk-72-server-tojnalg-0] 2024.08.13 21:31:28.508994 [ 1247 ] {3406aa56-20e8-44e0-b5de-8cb7715861f3} <Debug> db1.table1 (781f25db-3cd1-47c6-a76e-701945a67485) (SelectExecutor): Selected 2/2 parts by partition key, 2 parts by primary key, 2/2 marks by primary key, 2 marks to read from 2 ranges
[c-azure-gk-72-server-tojnalg-0] 2024.08.13 21:31:28.509034 [ 1247 ] {3406aa56-20e8-44e0-b5de-8cb7715861f3} <Trace> db1.table1 (781f25db-3cd1-47c6-a76e-701945a67485) (SelectExecutor): Spreading mark ranges among streams (default reading)
[c-azure-gk-72-server-tojnalg-0] 2024.08.13 21:31:28.509102 [ 1247 ] {3406aa56-20e8-44e0-b5de-8cb7715861f3} <Debug> MergeTreePrefetchedReadPool(db1.table1 (781f25db-3cd1-47c6-a76e-701945a67485)): Increasing prefetch step from 0 to 24
[c-azure-gk-72-server-tojnalg-0] 2024.08.13 21:31:28.509146 [ 1247 ] {3406aa56-20e8-44e0-b5de-8cb7715861f3} <Debug> MergeTreePrefetchedReadPool(db1.table1 (781f25db-3cd1-47c6-a76e-701945a67485)): Part: all_0_3_2, sum_marks: 1, approx mark size: 0, prefetch_step_bytes: 0, prefetch_step_marks: 24, (ranges: (0, 1))
[c-azure-gk-72-server-tojnalg-0] 2024.08.13 21:31:28.509180 [ 1247 ] {3406aa56-20e8-44e0-b5de-8cb7715861f3} <Debug> MergeTreePrefetchedReadPool(db1.table1 (781f25db-3cd1-47c6-a76e-701945a67485)): Increasing prefetch step from 0 to 24
[c-azure-gk-72-server-tojnalg-0] 2024.08.13 21:31:28.509218 [ 1247 ] {3406aa56-20e8-44e0-b5de-8cb7715861f3} <Debug> MergeTreePrefetchedReadPool(db1.table1 (781f25db-3cd1-47c6-a76e-701945a67485)): Part: all_4_4_0, sum_marks: 1, approx mark size: 0, prefetch_step_bytes: 0, prefetch_step_marks: 24, (ranges: (0, 1))
[c-azure-gk-72-server-tojnalg-0] 2024.08.13 21:31:28.509251 [ 1247 ] {3406aa56-20e8-44e0-b5de-8cb7715861f3} <Debug> MergeTreePrefetchedReadPool(db1.table1 (781f25db-3cd1-47c6-a76e-701945a67485)): Sum marks: 2, threads: 2, min_marks_per_thread: 1, min prefetch step marks: 24, prefetches limit: 200, total_size_approx: 0
[c-azure-gk-72-server-tojnalg-0] 2024.08.13 21:31:28.509312 [ 1247 ] {3406aa56-20e8-44e0-b5de-8cb7715861f3} <Debug> db1.table1 (781f25db-3cd1-47c6-a76e-701945a67485) (SelectExecutor): Reading approx. 8 rows with 2 streams
1	a
2	b
3	test, test
4	test, "test"
1	a
2	b
3	a
4	b
[c-azure-gk-72-server-tojnalg-0] 2024.08.13 21:31:28.510601 [ 1247 ] {3406aa56-20e8-44e0-b5de-8cb7715861f3} <Debug> executeQuery: Read 8 rows, 152.00 B in 0.002588 sec., 3091.190108191654 rows/sec., 57.36 KiB/sec.
[c-azure-gk-72-server-tojnalg-0] 2024.08.13 21:31:28.510663 [ 1247 ] {3406aa56-20e8-44e0-b5de-8cb7715861f3} <Debug> TCPHandler: Processed in 0.002984367 sec.
```

Reference link:
https://clickhouse.com/docs/en/operations/settings/settings#send_logs_level