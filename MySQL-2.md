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

document this

## MySQL Schema Optimization:

document this 

## MySQL Backup Procedures:
