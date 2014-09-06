---
layout: post
title: Unix commands
published: True
categories: []
date: 2014-09-06 11:27:10
tags: []
---

<br>
#### mysqldump
Usage: <code>mysqldump -u<user> -p<password> [OPTION] [database]</code>

| | |
| ------------------- | ------------- |
| --add-drop-table    | Add a DROP TABLE statement<br> before each CREATE TABLE statement. |
| --add-drop-database | Add a DROP DATABASE statement<br> before each CREATE DATABASE statement. |
| --add-locks         | Surround each table dump with<br> LOCK TABLES and UNLOCK TABLES statements.<br> This results in faster inserts<br> when the dump file is reloaded. |
| --all-databases     | Dump all tables in all databases |
| --compress          | Compress all information sent<br> between the client and the server if both<br> support compression. |
| --extended-insert   | Use multiple-row INSERT syntax<br> that include several VALUES lists. This<br> results in a smaller dump file and<br> speeds up inserts when the file is reloaded. |
| --no-data           | Do not write any row information<br> for the table. This is very useful if<br> you want to get a dump of only the<br> structure for a table. |
| --quick             | This option is useful for dumping<br> large tables. It forces mysqldump to<br> retrieve rows for a table from the<br> server a row at a time rather than retrieving<br> the entire row set and buffering<br> it in memory before writing it out. |


#### rsync
Usage: <code>rsync [OPTION] SRC DEST</code>

|  |  |
| ----------------- | ------------- |
| --verbose         | increase verbosity |
| -a, --archive     | archive mode |
| -n, --dry-run     | show what would have been transferred |
| --existing        | only update files that already exist |
| --ignore-existing | ignore files that already exist on the receiving side |
| --delete          | delete files that don't exist on the sending side |
| --delete-after    | delete after transferring, not before |
| --max-delete=NUM  | don't delete more than NUM files |
| --size-only       | only use file size when determining if a file should <br> be transferred |
| -z, --compress    | compress file data |
| --exclude=PATTERN | exclude files matching PATTERN |
| --progress        | show progress during transfer |

Example use:
{% highlight bash %}
$ rsync --archive --size-only --compress --progress server:/path localpath
$ rsync --archive --size-only --compress --progress local/path server:/path
{% endhighlight %}

#### Other
Search and replace
{% highlight bash %}
$ sed -n 's/search/replace/gpw output' file.txt
{% endhighlight %}
