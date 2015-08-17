ghdb-scrape
-----------------
Scrape the [Google Hacking Database](https://www.exploit-db.com/google-hacking-database/) for useful Google Dorks from Exploit-DB.com and insert the results into MongoDB. Docs and other stuff coming soon ;)

> " A Google dork query, sometimes just referred to as a dork, is a search string that uses advanced search operators to find information that is not readily available on a website. Google dorking, also known as Google hacking, can return information that is difficult to locate through simple search queries. That description includes information that is not intended for public viewing but that has not been adequately protected. As a passive attack method, Google dorking can return usernames and passwords, email lists, sensitive documents, personally identifiable financial information (PIFI) and website vulnerabilities. That information can be used for any number of illegal activities, including cyberterrorism, industrial espionage, identity theft and cyberstalking." -- [http://whatis.techtarget.com/definition/Google-dork-query](http://whatis.techtarget.com/definition/Google-dork-query)

### Scrape dorks

```
$ chmod +x ./fetch.sh && ./fetch.sh
Downloading 1...
Downloading 2...
Downloading 3...
Downloading 4...
Downloading 5...
Downloading 6...
...
...
...
```

Note that you may end up with some 404s, this just means there was an entry in GHDB that was removed for whatever reason. You can clean these up as you continue to run your fetch script:

```
$ grep -R page-404 .| cut -d : -f1| xargs rm -fv && rm -fv ./*.html && ./fetch.sh
```

### Import records into the database

```
$ php ./ghdb.php
Processing 1...OK!
Processing 10...OK!
Processing 100...OK!
Processing 1000...OK!
Processing 1001...OK!
Processing 1002...OK!
Processing 1003...OK!
Processing 1004...OK!
Processing 1005...OK!
Processing 1006...OK!
```

### Verify records were inserted

```
$ mongo
MongoDB shell version: 2.4.9
connecting to: test

> use ghdb
switched to db ghdb

> db.entries.count()
3598

> db.entries.find().limit(1)
{ "_id" : ObjectId("55d14c17ece0b9e9338b4567"), "1" : { "description" : "squid cache server reports", "search" : "&quot;cacheserverreport for&quot; &quot;This analysis was produced by calamaris&quot;", "submitted" : "2003-06-24", "info" : "These are squid server cache reports. Fairly benign, really except when you consider using them for evil purposes. For example, an institution stands up a proxy server for their internal users to get to the outside world. Then, the internal user surf all over to their hearts content (including intranet pages cuz well, the admins are stupid) Voila, intranet links show up in the external cache report. Want to make matters worse for yourself as an admin? OK, configure your external proxy server as a trusted internal host. Load up your web browser, set your proxy as their proxy and surf your way into their intranet. Not that I&#039;ve noticed any examples of this in this google list. *COUGH* *COUGH* *COUGH*  unresolved DNS lookups give clues *COUGH* *COUGH* (&#039;scuse me. must be a furball) OK, lets say BEST CASE scenario. Let&#039;s say there&#039;s not security problems revealed in these logs. Best case scenario is that outsiders can see what your company/agency/workers are surfing." } }
```



The HTML files this tool downloads are cloned from https://www.exploit-db.com/. I do not claim this content as my own.
