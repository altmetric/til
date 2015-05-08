# Analysing Sidekiq scheduled queue

[Sidekiq](http://sidekiq.org/)'s scheduled jobs are kept in Redis in a `<your-namespace>:schedule` set.

That set is a sorted set (ZSET) where:

* values are JSON-serialized job details;
* scores values are timestamps when a particular job should be executed.

The way Sidekiq schedules the jobs is by removing "expired" jobs from the set and pushing them to a work queue: https://github.com/mperham/sidekiq/blob/v3.3.4/lib/sidekiq/scheduled.rb#L15-L25

Also see [ZRANGEBYSCORE](http://redis.io/commands/zrangebyscore) Redis docs.
