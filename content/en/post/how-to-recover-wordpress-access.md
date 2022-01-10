---
date: 2017-04-09T10:58:08-04:00
description: "How to recover wordpress access with terminal and sql."
featured_image: "/images/Pope-Edouard-de-Beaumont-1844.jpg"
tags: ["Wordpress","SQL","Terminal"]
title: "How to recover wordpress access"
---
 Once uppon a time I had forgotten a password to one of my wordpress login. I need it because I need to update wordpress in case I had interesting read about new Critical Log4Shell (Apache Log4j) Zero-Day Attack. After no luck searching in lastpass records and keep cards I decided to go next step of difficulity. Also, level up or remember some of my devops oldtimes.

So I managed a simple steps to recover access:

- Connect to VPS by ssh and get access to SQL server.
- Backup sql server data.
- Gett hash of password I know from other wordpress directly from sql database.
- Update hash of lost password with known one. That\'s it.

First of all, I got access to VPS server.

## Connecting to server... or servers.

There is not just a server with linux and httpd, mysql and wordpress installed. There is [HTTPS-PORTAL](https://github.com/SteveLTN/https-portal)! It is a fully automated HTTPS server powered by [Nginx](http://nginx.org/), [Let\'s Encrypt](https://letsencrypt.org/) and [Docker](https://www.docker.com/). By using it, you can run any existing web application over HTTPS, with only one extra line of configuration. The SSL certificates are obtained, and renewed from Let\'s Encrypt automatically. That\'s amazing how I see it. Pure miracle there. There must be some kind of magic inside. By the way, it is working great more than 5 years for now.

Connecting to VPS by ssh:

```
sudo ssh -l noone molokov.de
```

Now I need to backup database. It\'s URGENT!

```
sudo mysqldump -u dbuser -p --all-databases > dumpsql.sql
```

Connecting to sql server with username and password:

```
mysql -u dbuser -p
```

Success! Now let's see what is inside the sql server:

```
SHOW DATABASES;
```

Good news everyone! There is some databases. Actually, there are more than 3. Maybe there are some old one's from previous RIP projects. I guess, the database named MYBLOG03 is where known encoded password can be got. Probably from user table in column like user_passwd. Let's see:

Selecting the database:

```
USE MYBLOG03;
```

Listing tables in it:

```
SHOW TABLES;
```

Output:

```
Database changed
MariaDB [MYBLOG03]> show tables;
+-------------------------+
| Tables_in_devpress      |
+-------------------------+
| wp_commentmeta        |
| wp_comments           |
| wp_links              |
| wp_options            |
| wp_postmeta           |
| wp_posts              |
| wp_term_relationships |
| wp_term_taxonomy      |
| wp_termmeta           |
| wp_terms              |
| wp_usermeta           |
| wp_users              |
+-------------------------+
12 rows in set (0.000 sec)
```

Hurragh! There are some tables. The last one might be table with records about registered users. 

Let's look inside its data:

```
select * from wp_users;
```

Nice! There is only one user with an encoded password in user_pass column. Copied!

Now getting access to database LSTPWDB01 the same way as before:

```
USE LSTPWDB01;
SHOW TABLES;
SELECT * FROM wp_users;
```

Brilliant, the database and the table structure are equivalent.

Finaly, updating a password:

```
UPDATE wp_users SET user_pass=SnVzdCBhbm90aGVyIFdvcmRQcmVzcyBzaXRl WHERE user_name=admin 
```

Signing in wordpress with know password.... success! There are some kind of unknown magician or shadow of sourceror near here!