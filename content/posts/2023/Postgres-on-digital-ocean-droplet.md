---
title: "Postgres on Digital Ocean Droplet"
date: 2023-12-30T15:44:27+05:45
draft: true
cover:
  image: "https://i.imgur.com/b2g8ybr.png"
---

Digital ocean allows you to just host the postgres database through it's postgreSQL management tools but this tutorial is for those who wants to setup on their droplet machine. This assumes that you are on Ubuntu Machine

## Step 1 - Install PostgreSQL

```bash
sudo apt update
```

```bash
sudo apt install postgres postgresql-contrib
```

```bash
sudo systemctl start postgresql 
```
## Step 2 - Posgres Roles and Databases

- Switch to postgres account and run PSQL prompt

```bash
sudo -u postgres psql
```

- Exit out of the prompt by typing `\q` 

```
postgres=# \q
```

## Step 3: Crate new User

```
sudo adduser voidash
```


## Step 4 - Create new Role
Rolename should be same as user as per convention, same goes for database

```
sudo -u postgres createuser --interactive
```
Type all the details 

```
Output:
Enter name of role to add: voidash
and other details
```
## Step 5 - Create new database

```
sudo -u postgres createdb voidash
```

## Step 6 - Get to PSQL prompt from new user 

```
sudo -u voidash psql
```

```
voidash=# \c

You are connected to database "voidash" as user "voidash"
```

## Step 7 - Find the configuration file 


```
sudo -u voidash psql -c 'SHOW config_file'

               config_file               
-----------------------------------------
 /etc/postgresql/15/main/postgresql.conf
(1 row)
```

modify the config file

```
vim  /etc/postgresql/15/main/postgresql.conf

```
Uncomment the listen_addresses, remove `#` in front

```
listen_addresses = "*"
```

modify `pg_hba.conf`


```
vim  /etc/postgresql/15/main/pg_hba.conf

```
Add the following line on top of the file

```
host    all             all             0.0.0.0/0               scram-sha-256
```

## Step 8 - Test the stuff

```
psql -h <droplet-ip> -p 5432 -U voidash voidash 
```

