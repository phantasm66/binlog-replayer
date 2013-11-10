MySQL Binlog Replayer
=====================

A MySQL binlog parser and row format event replayer

Usage
-----
    ruby binlog_replayer [OPTIONS]

    Options:

        --binlog        path to locally stored mysql binlog (required)
        --database      database to replay tables for
        --tables        comma delimited list (no spaces) of tables to replay (required)
        --password      local mysql server root password (required)
        --start         start time of events to replay (quoted format: 'YYYY-MM-dd HH:mm:ss')
        --stop          stop time of events to replay (quoted format: 'YYYY-MM-dd HH:mm:ss')

Summary
-------
I wrote this during an emergency and with complete amazement that no such tool existed. I tried the Yelp! [ybinlogp](https://github.com/Yelp/ybinlogp) tool but was unable to get it to make successfully. This tool has a very specific purpose, to parse and replay events from a row format mysql binlog back into a running mysql instance. It should work for any MySQL versions 5.1 and above, but it has only been tested on MySQL 5.6.

This tool can replay all binlog row events matching any combination of the following:

    * single table or list of tables
    * single database
    * specific time frame of events (assuming they exist within that binlog)

Caveats & Disclaimer
--------------------
If you attempt to replay enormous singular row based events, you *will* crash the mysql instance you are replaying into. This was designed specifically for use with row format mysql binlogs. It will *not* work for statement format binlogs. It *might* work for mixed format binlogs. If you do attempt to use it for replaying a mixed format binlog please keep in mind that it will not replay RENAME or DELETE events (i have not yet figured out why).

If you do not specify a --start time argument, the entire binlog will be parsed. Replaying an entire 1GB binlog back into a bored MySQL 5.6 instance takes ~10 minutes. However, if you plan to replay into a MySQL instance that also handles regular requests, it will likely take a bit longer.

I am not a DBA and have very limited knowledge on the formats and internals of MySQL. It is highly probable that this tool will be completely useless to anyone other than myself.

If i'm wrong... great. Glad i could help :)
