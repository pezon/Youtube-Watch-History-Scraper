## What is it?
Command line YouTube watch history scraper written in Python 2.7.
Uses pre logged in session cookies extracted from chrome.

This is just an experimental project. The database design isn't 
well thought out and there are not implemented methods for getting
information back out of the database.

## Python library dependencies
* [scrapy](http://scrapy.org/)
* [lxml](http://lxml.de/)
* [sqlalchemy](http://www.sqlalchemy.org/)

## How to use

### Ensure dependencies are met:

	pip install scrapy lxml sqlalchemy

### Getting session cookies and headers:
	
* Open chrome / chromium

* Make sure your logged into youtube.com

* Open the inspector (F12)

* Open the network tab, check the "Preserve History" box

* Visit https://youtube.com/feed/history

* Click the request for "/feed/history" in the network tab

* Copy the section "Request Headers", below the title "Request Headers"

* Paste into a new file called "youtube_request_headers.txt" in this directory

### Run Scrapy:

	scrapy crawl yth_spider

## Profit?

You can search it manually in your favorite database browser
See [DB Browser for SQLite](http://sqlitebrowser.org/) to make use of db.

Or use ipython and sqlalchemy:

	from db_api import *
	db = AppDatabase()
	with db._session_scope() as s:
		h_entries = s.query(HistoryEntry).filter(HistoryEntry.time > 7 * 60).limit(5)
	for he in h_entries.all():                                            
		print "{t} (http://youtube.com/watch?v={v})".format(t=he.title, v=he.vid) 


#### You can avoid the database output entirely
Just remove the line:

    'youtube_history.pipelines.DbOutputPipeline': 800,

From the ***settings.py*** file.