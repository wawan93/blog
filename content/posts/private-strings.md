---
date: '2025-01-16T18:19:40+01:00'
draft: false
title: 'Private strings in Golang'
lang: en-US
tags:
  - programming
posts:
  - programming
---

Why you should use private string types in Go programming language:

```go
package db  
  
type privateString string  
  
type DB struct{}  
  
func (db *DB) Query(query privateString) {  
   print(query)  
}
```

You can only pass string literal into this `Query` method. 
It prevents you from passing user's input here:

```go
package main  
  
import (  
   "wawan/gotest/pkg/db"  
)  
  
func main() {  
   conn := &db.DB{}  
  
   // this works  
   conn.Query(`SELECT * FROM users`)  
  
   userID := "4939c773-78ef-4eb8-aa8d-6fcc7e3521e8"  
	// error: cannot use `SELECT * FROM users WHERE user_id="` + userID + `"`  
	// (value of type string) as type db.privateString in argument to conn.Query
   conn.Query(`SELECT * FROM users WHERE user_id="` + userID + `"`)  
}
```

You can also use private string types to pass only pre-defined constants inside your functions

```go
package tgbot

type kind string

const (
	PlainText kind = "plain"
	Photo kind = "photo"
)

// You can only pass Select or Insert constants here
func Handle(upadte *tgbotapi.Update, event kind) {
	// handlers[event].exec(update)
}
```
