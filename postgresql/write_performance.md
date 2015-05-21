# Write Performance

We have a write-heavy application which needs to keep up with data streamed from an external system. Apparently our staging environment wasn't able to cope with writing all the data to a PostgreSQL database, so our data backlog grew larger and larger.

We verified it's not a memory- or a CPU-bound issue using `top`, however [iotop](http://guichaz.free.fr/iotop/) showed an unusually high number of I/O operations by `jbd2/md6-8` process:

```
TID  PRIO  USER     DISK READ  DISK WRITE  SWAPIN     IO>    COMMAND
477  be/3  root      0.00 B/s  2778.70 K/s  0.00 % 67.48 % [jbd2/md6-8]
```

The `jbd2` process manages journalling on [ext4](https://ext4.wiki.kernel.org/index.php/Ext4_Howto) filesystems, which conflicts with Postgres's own [Write-Ahead Log](http://www.postgresql.org/docs/9.4/static/wal-intro.html) journalling.

Fixing the write performance was a matter of turning off the former on the partition we store PostgreSQL data on. To do so we:

* stopped PostgreSQL and all other services using the partition (`lsof | grep /srv` came handy)
* unmounted the partition - `umount /srv`
* turned off journalling - `tune2fs -O ^has_journal /dev/md6`
* mounted the parting back - `mount /srv`
* started all the services
