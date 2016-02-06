# Introduction #

Transdroid Torrent Search provides access to torrent searches on a variety of sites. Instead of providing an interface itself, it allows Android application to access the data via a content provider.

# Getting search results #

Acquiring search results for a specific query can be as easy as as two lines of code:
```
Uri uri = Uri.parse("content://org.transdroid.search.torrentsearchprovider/search/" + query);
Cursor results = managedQuery(uri, null, null, null, null);
```

The returned Cursor can be used in a ListActivity, AlertDialog or otherwise. The following fields are available:
```
String[] fields = new String[] { "_ID", "NAME", "TORRENTURL", "DETAILSURL", "SIZE", "ADDED", "SEEDERS", "LEECHERS" };
```

**Important:** Querying the content providers is an synchronous operation. If done from your applications UI thread, this operation will stall the interface. Use, for example, an AsyncTask to implement proper threading.

# Customizing search results #

You have control over the search results that are returned. A specific site may be queried and the preferred sort order can be given:
```
Uri uri = Uri.parse("content://org.transdroid.search.torrentsearchprovider/search/" + query);
Cursor results = managedQuery(uri, null, "SITE = ?", new String[] { siteCode }, sortOrder)
```
Here, siteCode is the code of one of the supported torrent sites. The default is Mininova. The orderCode is either BySeeders (default) or Combined. Note that no errors are returned when a site or sort order doesn't exist; an null Cursor is returned instead. (This is a limitation of ContentResolvers.)

# Supported torrent sites #

To get a listing of (the codes of) the support torrent sites, you may use another provider:
```
uri = Uri.parse("content://org.transdroid.search.torrentsitesprovider/sites");
Cursor sites = managedQuery(uri, null, null, null, null);
```

The returned Cursor contains the following fields:
```
String[] fields = new String[] { "_ID", "CODE", "NAME", "RSSURL" };
```