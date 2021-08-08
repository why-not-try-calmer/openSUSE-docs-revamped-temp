# Rationale

To comply with the newly added license, we are required to name the top 5 "principal" contributors to the wikis. If the top 5 (+ 1 for good measure) users with the most edits is a valid application of this notion, we have:
```
As per MariaDB [wiki_en]> select user_id, count(rev_user) as edits_count, user_name from revision, user where rev_user=user_id group by rev_user order by edits_count desc limit 20;
+---------+---------------+
| edits_count | user_name |
+---------+---------------+
|  4358 | Hennevogel      |
|  3286 | Rajko m         |
|  2444 | Jsmeix          |
|  2264 | Saigkill        |
|  2227 | Spyhawk         |
|  1824 | Ddemaio         |
+---------+---------------+
```
