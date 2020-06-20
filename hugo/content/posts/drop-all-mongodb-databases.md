---
title: "Drop every MongoDB database [snippet]"
date: 2020-06-20T09:44:31-03:00
draft: false
categories:
- MongoDB
---
Occasionally useful during development:

<code>
db.getMongo().getDBNames().forEach(dbName => db.getSiblingDB(dbName).dropDatabase())
</code>
