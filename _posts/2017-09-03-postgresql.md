---
layout: post
title: "PostgreSQL"
description: ""
category:
tags: [database]
---
{% include JB/setup %}

# Installation
## MAC
```
# It will create a PostgreSQL for the current logined user.
brew install postgres
# postgres is a built-in database.
psql postgres
```

Use `pg_ctl -D /usr/local/var/postgres start` to start PostgreSQL.

Remove PostgreSQL files:
```
rm -rf /usr/local/var/postgres
```

```
createdb mydb
psql mydb
```
