---
title: List all Users
---

## List all Users

A REST API call is available that will list all Users. As the JWT used to authenticate this REST API call is tied to a specific Nexmo application, all Users associated with that application will be returned:

``` shell
$ curl "https://api.nexmo.com/beta/users" \
     -H 'Authorization: Bearer eyJ0eXAiOiJKV1Q....7Rckzv3QiOqSjmj1JQGq7smQ' \
     -H 'Content-Type: application/json'
```
