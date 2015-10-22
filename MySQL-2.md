MySQL-2 Optimalization guide
==================================

## MySQL Server Hardware and OS Tuning:

- Have enough physical memory to load your entire InnoDB file into memory – InnoDB is much faster when the file can be accessed in memory rather than from disk.
- Avoid Swap at all costs – swapping is reading from disk, its slow.
- Use Battery-Backed RAM.
- Use an advanced RAID – preferably RAID10 or higher.
- Avoid RAID5 – the checksum needed to ensure integrity is costly.
- Separate your OS and data partitions, not just logically, but physically – costly OS writes and reads will impact your database performance.
- Put your mysql temp space and replication logs on a separate partition than your data – background writes will impact your database when it goes to write/read from disk.
- More disks equals more speed.
- Faster disks are better.
- Use SAS over SATA.
- Smaller disks are faster than larger disks, especially in RAID configs.
- Use Battery-Backed Cache RAID controllers.
- Avoid software raids.
- Consider using Solid State IO Cards (not disk drives) for your data partition – these cards can sustain over 2GB/s writes for almost any amount of data.
- On Linux set your swappiness value to 0 – no reason to cache files on a database server, this is more of a web server or desktop advantage.
- Mount filesystem with noatime and nodirtime if available – no reason to update database file modification times for access.
- Use XFS filesystem – a faster, smaller filesystem than ext3 and has more options for journaling, also ext3 has been shown to have double buffering issues with MySQL.
- Tune your XFS filesystem log and buffer variables – for maximum performance benchmark.
- On Linux systems, use NOOP or DEADLINE IO scheduler – the CFQ and ANTICIPATORY scheduler have been shown to be slow vs NOOP and DEADLINE scheduler.
- Use a 64-bit OS – more memory addressable and usable to MySQL.
- Remove unused packages and daemons from servers – less resource stealing.
- Put your host that use MySQL and your MySQL host in a hosts file – no dns lookups.
- Never force kill a MySQL process – you will corrupt your database and be running for the backups.
- Dedicate your server to MySQL – background processes and other services can steal from the db cpu time.

## MySQL Configuration:

- Use innodb_flush_method=O_DIRECT to avoid a double buffer when writing.
- Avoid O_DIRECT and EXT3 filesystem – you will serialize all your writes.
- Allocate enough innodb_buffer_pool_size to load your entire InnoDB file into memory – less reads from disk.
- Do not make innodb_log_file_size too big, with faster and more disks – flushing more often is good and lowers the recovery time during crashes.
- Do not mix innodb_thread_concurrency and thread_concurrency variables – these two values are not compatible.
- Allocate a minimal amount for max_connections – too many connections can use up your RAM and lock up your MySQL server.
- Keep thread_cache at a relatively high number, about 16 – to prevent slowness when opening connections.
- Use  skip-name-resolve – to remove dns lookups.
- Use query cache if your queries are repetitive and your data does not change often – however using query cache on data that changes often will give you a performance hit.
- Increase temp_table_size – to prevent disk writes.
- Increase max_heap_table_size – to prevent disk writes.
- Do not set your sort_buffer_size too high – this is per connection and can use up memory fast.
- Monitor key_read_requests and key_reads to determine your key_buffer size – the key read requests should be higher than your key_reads, otherwise you are not efficiently using your key_buffer
- Set innodb_flush_log_at_trx_commit = 0 will improve performance, but leaving it to default (1), you will ensure data integrity, you will also ensure replication is not lagging
- Have a test environment where you can test your configs and restart often, without affecting production.

## MySQL Schema Optimization:

- Keep your database trim.
- Archive old data – to remove excessive row returns or searches on queries.
- Put indexes on your data.
- Do not overuse indexes, compare with your queries.
- Compress text and blob data types – to save space and reduce number of disk reads.
- UTF 8 and UTF16 is slower than latin1.
- Use Triggers sparingly.
- Keep redundant data to a minimum – do not duplicate data unnecessarily.
- Use linking tables rather than extending rows.
- Pay attention to your data types, use the smallest one possible for your real data.
- Separate blob/text data from other data if other data is often used for queries when blob/text are not.
- Check and optimize tables often.
- Rewrite InnoDB tables often to optimize.
- Sometimes, it is faster to drop indexes when adding columns and then add indexes back.
- Use different storage engines for different needs.
- Use ARCHIVE storage engine for Logging tables or Auditing tables – this is much more efficient for writes.
- Store session data in memcache rather than MySQL – memcache allows for auto-expiring values and prevents you from having to create costly reads and writes to MySQL for temporal data.
- Use VARCHAR instead CHAR when storing variable length strings – to save space since CHAR is fixed length and VARCHAR is not (utf8 is not affected by this).
- Make schema changes incrementally – a small change can have drastic effects.
- Test all schema changes in a development environment that mirrors production.
- Do NOT arbitrarily change values in your config file, it can have disastrous affects.
- Sometimes less is more in MySQL configs.
- When in doubt use a generic MySQL config file.

## MySQL Backup Procedures:
