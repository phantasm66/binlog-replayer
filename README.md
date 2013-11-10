MySQL Binlog Replayer
=====================

A MySQL binlog parser and row format event replayer

Usage
-----
    ruby binlog_replayer [OPTIONS]

Summary
-------
I wrote this during an emergency and with complete amazement that no such tool existed. I tried the Yelp! [ybinlogp](https://github.com/Yelp/ybinlogp) tool but was unable to get it to make successfully. This tool has a very specific purpose, to parse and replay events from a row format mysql binlog back into a running mysql instance. It should work for any MySQL versions 5.1 and above, but it has only been tested on MySQL 5.6.

This tool can replay all binlog row events matching any combination of the following:

    a single table or list of tables
    a single database
    a specific time frame (within 1 second) of events that exist within a binlog

Caveats & Disclaimer
--------------------
If you attempt to replay enormous singular row based events, you *will* crash the mysql instance you are replaying into. This was designed specifically for use with row format mysql binlogs. It will *not* work for statement format binlogs. It *might* work for mixed format binlogs. If you do attempt to use it for replaying a mixed format binlog please keep in mind that it will not replay RENAME or DELETE operations (i have not yet figured out why).

If you do not specify a --start time argument, the entire binlog will be parsed. Please also keep in mind that parsing and replaying binlogs back into a running mysql instance will take some time. Replaying an entire 1GB binlog back into a running MySQL 5.6 instance took ~45 minutes. If you are replaying back into an instance that is also handling regular requests, then it will likely take much longer.

I am not a DBA and have very limited knowledge on the formats and internals of MySQL. It is highly probable that this tool will be completely useless to anyone other than myself.

If i'm wrong... great. Glad i could help :)
