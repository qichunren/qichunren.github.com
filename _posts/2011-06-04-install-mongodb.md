---
layout: single
title: Install mongo on mac
date: 2011-06-04 22:01
comments: true
categories: [nosql, mongodb]
---

```
caojinhuamatoMacBook-Pro:code caojinhua$ brew install mongodb
==> Downloading http://fastdl.mongodb.org/osx/mongodb-osx-x86_64-1.8.1.tgz
######################################################################## 100.0%
==> Caveats
If this is your first install, automatically load on login with:
    mkdir -p ~/Library/LaunchAgents
    cp /usr/local/Cellar/mongodb/1.8.1-x86_64/org.mongodb.mongod.plist ~/Library/LaunchAgents/
    launchctl load -w ~/Library/LaunchAgents/org.mongodb.mongod.plist

If this is an upgrade and you already have the org.mongodb.mongod.plist loaded:
    launchctl unload -w ~/Library/LaunchAgents/org.mongodb.mongod.plist
    cp /usr/local/Cellar/mongodb/1.8.1-x86_64/org.mongodb.mongod.plist ~/Library/LaunchAgents/
    launchctl load -w ~/Library/LaunchAgents/org.mongodb.mongod.plist

Or start it manually:
    mongod run --config /usr/local/Cellar/mongodb/1.8.1-x86_64/mongod.conf
MongoDB 1.8+ includes a feature for Write Ahead Logging (Journaling), which has been enabled by default.
This is not the default in production (Journaling is disabled); to disable journaling, use --nojournal.
==> Summary
/usr/local/Cellar/mongodb/1.8.1-x86_64: 16 files, 93M, built in 2 seconds
```