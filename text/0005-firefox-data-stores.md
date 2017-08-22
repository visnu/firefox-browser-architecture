
# Problem

There are a large number of data stores being used to hold data across all Firefox instances and platforms. There is currently nowhere that documents all these data stores and how they share information between them. We have 3 platforms, desktop, Android and iOS and each of these platforms shares data with the others. But not all the information is synced completely across all platforms, and the data itself is not stored in the same way everywhere. When starting to think about how we might provide a holistic user data solution for Mozilla, it is important to understand the scope of the problem and the specifics of the data that is being handled. 

We therefore need to look at all of the data stores that we are currently using and document their purpose, structure, the kind of data that they hold and how they share that data.


# Solution

The extensive documentation can be found in the [Firefox Data Stores](https://github.com/mozilla/firefox-data-store-docs) repository.

* There are a total of 46 separate data stores in Firefox for desktop. Within these 46 stores, there are 10 different storage formats. Very few of these stores are well documented in the code. 
* Firefox for Android has only 2 stores on the frontend, but shares many of the same stores Gecko stores.
* Firefox for iOS has 5 data stores. One of these is historical as the Reading List store was reused from a another Mozilla app. One of the stores is used to store metadata for use in Activity Stream and has been created separately to make syncing of this data at a later data easier. 
* Many of the data stores on desktop are synchronous, for example prefs.js. prefs.js is a simple key value file written to disk, but it contains well over 1000 entries. It's synchronous nature means that every time a value is changed every single value is written out. For a data store that is the default for storing single values for the entire app, this proves to be highly inefficient.
* Firefox desktop has an entire SQLite database, Storage.sqlite, that contains no tables. It is used entirely to reference the schema version value and uses that value to create the version number for the Storage directory. The Storage directory contains all of the files and stores used by the app.
* Of the data fields stored by desktop, only 8% are available to Firefox Sync.
* A visual overview of the [fields made available to Firefox Sync](https://docs.google.com/spreadsheets/d/1k9_K7Dc3q2h3SDV0vwjTgJou-ndza6WuobyJ1bbemtc/edit?ts=5977ab9d#gid=1269587388) and how each sync record is implemented on each platform is also available.

