You can take a look at what's happening on a Postgres database by logging on and running this:

```
SELECT pid, age(query_start, clock_timestamp()), usename, query, state
FROM pg_stat_activity
WHERE state != 'idle' AND query NOT ILIKE '%pg_stat_activity%'
ORDER BY query_start desc;
```

This will show a list of all queries which Postgres is still doing something with. The `active` state means that it's still working. [From the docs](https://www.postgresql.org/docs/9.2/static/monitoring-stats.html):

```
  Possible values are:
  * active: The backend is executing a query.
  * idle: The backend is waiting for a new client command.
  * idle in transaction: The backend is in a transaction, but is not currently executing a query.
  * idle in transaction (aborted): This state is similar to idle in transaction, except one of the statements in the transaction caused an error.
  * fastpath function call: The backend is executing a fast-path function.
  * disabled: This state is reported if track_activities is disabled in this backend.
```

The `age` column shows how long the query has lived for.
