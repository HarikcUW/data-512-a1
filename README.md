# Project goal: 
Construct, Analyze and publish a dataset of monthly traffic (page views) on English Wikipedia and make it fully reproducible using best practices for open scientific research.
A page view is a request for the content of a web page and we are using count of page views delivered to users. You can find more details @ https://meta.wikimedia.org/wiki/Research:Page_view

# License and terms of use:
Wikimedia Foundation REST API terms of use: https://www.mediawiki.org/wiki/REST_API#Terms_and_conditions

Source data license: Data accessible via this endpoint is available under the CC0 1.0 license , CC-BY-SA 3.0 and GFDL licenses.<br/>Reference: [Wikimedia REST API](https://wikimedia.org/api/rest_v1/#/)

Analytics/Data Lake/Traffic/Pageview hourly. (2021, June 23). Wikitech, . Retrieved 04:10, October 7, 2021 from https://wikitech.wikimedia.org/w/index.php?title=Analytics/Data_Lake/Traffic/Pageview_hourly&oldid=1916427.<br/>Reference: [Analytics Data_Lake Traffic Pageview_hourly](https://wikitech.wikimedia.org/w/index.php?title=Special:CiteThisPage&page=Analytics%2FData_Lake%2FTraffic%2FPageview_hourly&id=1916427&wpFormIdentifier=titleform)

Wikimedia REST API. (2021, July 19). MediaWiki, . Retrieved 04:00, October 7, 2021 from https://www.mediawiki.org/w/index.php?title=Wikimedia_REST_API&oldid=4710376.<br/>Reference: [MediaWiki](https://www.mediawiki.org/w/index.php?title=Special:CiteThisPage&page=Wikimedia_REST_API&id=4710376&wpFormIdentifier=titleform)

# Data and API Dcouments:
Source data: Analytics/Data Lake/Traffic/Pageview hourly - Wikitech (wikimedia.org)
https://wikitech.wikimedia.org/wiki/Analytics/AQS/Legacy_Pagecounts

* Wikipedia page traffic is available to public using [Wikimedia REST API](https://www.mediawiki.org/wiki/Wikimedia_REST_API)
   * Data is available from December 2007, and exposed using 2 APIs. Refer API end points listed below for get API format, parameters and how to access.
   * Legacy Page counts API:
        * Documentation: [Analytics/AQS/Legacy Pagecounts](https://wikitech.wikimedia.org/wiki/Analytics/AQS/Legacy_Pagecounts)
        * API endpoint:[Wikimedia REST API-Legacy Pagecounts](https://wikimedia.org/api/rest_v1/#/Pagecounts_data_(legacy)/get_metrics_legacy_pagecounts_aggregate_project_access_site_granularity_start_end)
        * Data availability: December 2007 to July 2016
   * Pageviews API: 
        * Documentation: [Analytics/AQS/Pageviews](https://wikitech.wikimedia.org/wiki/Analytics/AQS/Pageviews)
        * API endpoint: [Wikimedia REST API-Pageviews](https://wikimedia.org/api/rest_v1/#/Pageviews_data/get_metrics_pageviews_aggregate_project_access_agent_granularity_start_end)
        * Data availability:July 2015 to last month

Some key attributes from source data:
  * timestamp: UTC timestamp of the request
  * project  : Project name from requests hostname
  * access_method: Method used to access the pages, can be desktop, mobile web, or mobile app or request was to the "mobile" site or the "desktop" site, or an "app" request
  * agent_type: Agent accessing the pages, can be spider,  user or automated
  * view_count: number of pageviews
  * year : Unpadded year (from timestamp) of pageviews
  * month: Unpadded month (from timestamp) of pageviews

# Final data file details:
* Data file location:data-512-a1/data-final/
* File name: en-wikipedia_traffic_200712-202108.csv
* File format: Tab delimited csv file
* Attributes List:
    * Final dataset has Year, Month and 6 metrics related to page views.
    * year : Year of page view request UTC timestamp
    * Month: Month of page view request UTC timestamp
    * pagecount_all_views: Total page counts using desktop or mobile access methods. It is calculated using sum of pagecount_desktop_views and pagecount_mobile_views. Metric from Legacy API.
    * pagecount_desktop_views: Total page counts requested using desktop. Metric from Legacy API. 
    * pagecount_mobile_views: Total page counts requested using mobile. Metric from Legacy API.
    * pageview_all_views: Total page counts using desktop or mobile access methods. It is calculated using sum of pageview_desktop_views and pageview_mobile_views.
    * pageview_desktop_views: Total page counts requested using desktop. Metric from Legacy API
    * pageview_mobile_views: Total page counts requested using desktop. Metric from Legacy API

# Known Issues:
  * Edits are not counted in page views, because it might be already counted when user initially clicked/opened that page, Also it should not be included as part of consumption metrics, it will fall under production metric.
  * Legacy Pagecounts are sampled web request logs whereas current Pageviews are un-sampled web request logs. 
  * Legacy Pagecount definition is several years old, so it might be less accurate. Also the new definition evolved over time as new ways of viewing content are developed (e.g. a mobile app using and APIs)
  * Current Pageviews definition makes a better effort at counting traffic from real persons and excluding automation
  * Current Pageviews definition detects and flags more spiders (which can be a significant fraction of total traffic on some wikis) and provides option to count traffic from real persons and excluding automation, data from Pagecounts API doesn’t exclude. That why we may observe high volume in Legacy Pagecounts compare to Pageviews though recent traffic is much higher than earlier. 
  * Legacy Pageviews data excludes data from apps 
  * Latest technologies help to improve quality & efficiency, so the Pageviews might be more accurate compare to Pagecounts. The new definition is generated on the analytics cluster using Hadoop and web request logs and the previous definition relied on a tool named webstatscollector: Analytics/Webstatscollector
  * Legacy Pagecounts data refresh was stopped in 2016, so any issues found in data/calculation after that are used to refresh only Pageviews data set.
