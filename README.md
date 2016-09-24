# FCC Search API
Documentation and resources for the Forum Communications Company (FCC) search API powered by Elasticsearch.

- [FCC Search API Proxy](https://fccpublicsearch.herokuapp.com/fccnn/_search)
- [Sift Search UI](http://fccsearch.herokuapp.com/search)

## Overview
This public search proxy is an experimental API that is being made available just for this hackathon. Please be respectful and don’t abuse it.

## History
- The search API was developed as a replacement for Google Custom Search, and is currently live in production as the basic search functionality on all FCC online news properties.
- Sift, the React-based search UI was originally developed as a testing tool and proof-of-concept of headless front-end implementations, but will be expanded for internal use by journalists for research and data-mining purposes.
- In the future, this decoupled content index will allow us easy and fast client-agnostic data access for anything from headless front-end sites, progressive web apps, chatbots and more, in an easy and unified API.

## Quick Facts
- Real-time indexing of published articles on over 40 FCC news properties.
- The index and API are still currently under development and should be considered in the alpha phase.
- Documents in the index currently include news articles from 01/01/2015-present, but content will ultimately span back to 1999 and will expand to include additional document types such as obituaries, celebrations, legal notices and more.
- The 350+ sites on the AreaVoices network (which are currently on a separate index) will be migrated to the same cluster for centralized, cross-network search and data aggregation.
- Access to the index is achieved via a custom node.js proxy app which restricts public access to GET and POST requests only to help maintain data integrity during development.


## Index & Endpoint Basics
- Default Endpoint: `https://fccpublicsearch.herokuapp.com/fccnn/_search`
- Specific Article: `https://fccpublicsearch.herokuapp.com/fccnn/article/4104775/`
- Index: `fccnn`
- Type: `article`

## Postman
Postman is the swiss army knife of API tools, allowing you to design, build, test, document and monitor REST API services, all in one place. Postman is available for Chrome, Mac OSX and Windows.
- Download the Chrome, Mac, or Windows.
  - [Chrome](https://chrome.google.com/webstore/detail/postman-rest-client/fhbjgbiflinjbdggehcddcbncdddomop)
  - [Mac](https://www.getpostman.com/app/postman-osx)
  - [Windows x64](https://app.getpostman.com/app/download/win64)
  - [Windows x86](https://app.getpostman.com/app/download/win32)
- Download the example Postman collection and environment for the API (.zip or raw json).
  - [.zip collection](https://github.com/openfcci/FCC-Search-API/raw/master/postman-examples/postman-examples.zip)
  - [API Collection](https://raw.githubusercontent.com/openfcci/FCC-Search-API/master/postman-examples/FCC%2520Search%2520API%2520Examples.postman_collection.json) (right-click/save-as)
  - [API Environment](https://raw.githubusercontent.com/openfcci/FCC-Search-API/master/postman-examples/FCC%2520Search%2520API%2520Example%2520Environment.postman_environment.json) (right-click/save-as)
- Import the collection and environment into Postman and select the `FCC Search API Example Environment`
- [Postman Documentation](https://www.getpostman.com/docs/)

![Postman Overview](https://raw.githubusercontent.com/openfcci/FCC-Search-API/master/img/postman-overview.png)

## Elasticsearch Basics
- [Elasticsearch v2.3 reference and documentation](https://www.elastic.co/guide/en/elasticsearch/reference/2.3/index.html)
- [Official Elastic Github Repositories](https://github.com/elastic): contains source repos for Elasticsearch and various multi-language clients, SDK's, libraries and plugins.

## Document Structure & Fields

#### Basic URI Search Example
```
https://fccpublicsearch.herokuapp.com/fccnn/_search?q=weezer&size=1
```

#### Basic Query DSL Search Example
POST request to `https://fccpublicsearch.herokuapp.com/fccnn/_search`
```json
{
  "query": {
    "query_string": {
      "query": "weezer",
      "fields": [
        "title"
      ]
    }
  },
  "size": 1,
  "sort": [
    {
      "created": {
        "order": "desc"
      }
    }
  ]
}
```

#### Basic cURL Example
```json
curl -X POST -H "Content-Type: application/json" -H "Cache-Control: no-cache" -d '{
  "query": {
    "query_string": {
      "query": "weezer",
      "fields": [
        "title"
      ]
    }
  },
  "size": 1,
  "sort": [
    {
      "created": {
        "order": "desc"
      }
    }
  ]
}' "https://fccpublicsearch.herokuapp.com/fccnn/_search"
```

### Example Response

```json
{
  "took": 3,
  "timed_out": false,
  "_shards": {
    "total": 4,
    "successful": 4,
    "failed": 0
  },
  "hits": {
    "total": 16,
    "max_score": 2.437505,
    "hits": [
      {
        "_index": "fccnn",
        "_type": "article",
        "_id": "4045294",
        "_score": 2.437505,
        "_source": {
          "nid": "4045294",
          "type": "article",
          "post_date": "2016-06-01 11:24:08",
          "title": "Weezer, moe to perform in Moorhead in September",
          "excerpt": "MOORHEAD - Two new shows featuring Weezer and moe with Yonder Mountain String Band have been added to the summer concert series at Bluestem Center for the Arts.",
          "author": {
            "realname": "John Lamb",
            "first_name": "John",
            "last_name": "Lamb"
          },
          "url": "www.grandforksherald.com/node/4045294",
          "search_thumb": "www.fccnn.com/sites/default/files/styles/square_300/public/field/image/060216.F.FF_.weezer_0_0.jpg",
          "thumb_filename": "060216.F.FF_.weezer_0.jpg",
          "uid": "85481",
          "name": "jlamb",
          "picture": "0",
          "created": "1464798248",
          "published": "1464798248",
          "date_terms": {
            "year": 2016,
            "month": 6,
            "week": 22,
            "dayofyear": 152,
            "day": 1,
            "dayofweek": 3,
            "dayofweek_iso": 3,
            "hour": 11,
            "minute": 24,
            "second": 8,
            "m": 201606
          },
          "revision_timestamp": "1464798248",
          "revision_uid": "213959",
          "vid": "4323734",
          "status": "1",
          "promote": "1",
          "sticky": "0",
          "newspaper": "Grand Forks Herald",
          "parent_domain": "www.grandforksherald.com",
          "path": {
            "pid": "4666422",
            "source": "node/4045294",
            "alias": "news/region/4045294-weezer-moe-perform-moorhead-september"
          },
          "domains": [
            "12"
          ],
          "subdomains": [
            "Grand Forks Herald"
          ],
          "domain_site": false,
          "video": null,
          "audio": null,
          "poll": null,
          "google_drive_document_id": null,
          "body": {
            "value": "<p>MOORHEAD - Two new shows featuring Weezer and moe with Yonder Mountain String Band have been added to the summer concert series at Bluestem Center for the Arts.</p><p dir=\"ltr\">The jam band double bill plays the outdoor space on Sept. 1.</p><p dir=\"ltr\">Weezer returns to the area on Sept. 2 for the first time since playing the Fargodome in 2002.</p><p dir=\"ltr\">Tickets for both shows go on sale at 11 a.m.&nbsp;Friday, June 10.</p><p dir=\"ltr\">Tickets range from $19.50 to $39.50 for moe and Yonder Mountain String Band. Tickets for Weezer range from $39.50 to $85.</p><p dir=\"ltr\">For more information go to&nbsp;<a href=\"http://jadepresents.com/\" target=\"_blank\">http://jadepresents.com</a>.</p>",
            "summary": "",
            "safe_value": "MOORHEAD - Two new shows featuring Weezer and moe with Yonder Mountain String Band have been added to the summer concert series at Bluestem Center for the Arts.\nThe jam band double bill plays the outdoor space on Sept. 1.\nWeezer returns to the area on Sept. 2 for the first time since playing the Fargodome in 2002.\nTickets for both shows go on sale at 11 a.m.Friday, June 10.\nTickets range from $19.50 to $39.50 for moe and Yonder Mountain String Band. Tickets for Weezer range from $39.50 to $85.\nFor more information go tohttp://jadepresents.com.",
            "safe_summary": "",
            "format": "tinymce"
          },
          "category": [
            {
              "tid": "1",
              "name": "News"
            },
            {
              "tid": "4",
              "name": "region"
            }
          ],
          "tags": [
            {
              "tid": "318282",
              "name": "moorhead"
            },
            {
              "tid": "220353",
              "name": "Music"
            },
            {
              "tid": "220248",
              "name": "Entertainment"
            },
            {
              "tid": "62480",
              "name": "weezer"
            },
            {
              "tid": "5335",
              "name": "Moe"
            }
          ],
          "images": [
            {
              "fid": "2571617",
              "uid": "213959",
              "filename": "060216.F.FF_.weezer_0.jpg",
              "uri": "public://field/image/060216.F.FF_.weezer_0_0.jpg",
              "filemime": "image/jpeg",
              "filesize": "141543",
              "status": "1",
              "timestamp": "1464798248",
              "origname": "060216.F.FF_.weezer_0.jpg",
              "type": "image",
              "focus_rect": null,
              "crop_rect": null,
              "field_file_image_alt_text": [],
              "field_file_image_title_text": [],
              "image_dimensions": {
                "width": "1000",
                "height": "666"
              },
              "alt": "Weezer will play Bluestem on Sept. 2. Special to The Forum",
              "title": "",
              "width": "1000",
              "height": "666",
              "full": "www.fccnn.com/sites/default/files/field/image/060216.F.FF_.weezer_0_0.jpg",
              "full_1000": "www.fccnn.com/sites/default/files/styles/full_1000/public/field/image/060216.F.FF_.weezer_0_0.jpg",
              "16x9_860": "www.fccnn.com/sites/default/files/styles/16x9_860/public/field/image/060216.F.FF_.weezer_0_0.jpg",
              "16x9_430": "www.fccnn.com/sites/default/files/styles/16x9_430/public/field/image/060216.F.FF_.weezer_0_0.jpg",
              "square_200": "www.fccnn.com/sites/default/files/styles/square_200/public/field/image/060216.F.FF_.weezer_0_0.jpg"
            }
          ]
        }
      }
    ]
  }
}
```

## Newspaper Domain IDs

Domain ID | News Property
--------- | ------------------------------------
1					| 	Forum Communications Company News Network
2	 				| 	Bemidji Pioneer
3	 				| 	Ag Week
4	 				| 	Daily Globe
5	 				| 	Detroit Lakes Online
6				 	| 	Duluth Budgeteer
7	 				| 	Duluth News Tribune
8	 				| 	Echo Press
9	 				| 	Perham Focus
10	 			| 	Farmington Independent
11	 			| 	Grandforks
12	 			| 	Grandforks Herald
13	 			| 	Hastings Star Gazette
14	 			| 	Hudson Star Observer
15	 			| 	Fargo Forum
16	 			| 	Jamestown Sun
17	 			| 	Mitchell Advisor
18	 			| 	Mitchell Republic
19	 			| 	Morris Sun Tribune
20	 			| 	New Richmond News
21	 			| 	Northland Outdoors
22	 			| 	Park Rapids Enterprise
23	 			| 	Pierce County Herald
24	 			| 	Pine Journal
25	 			| 	Prairie Biz Mag
26	 			| 	Republican Eagle
27	 			| 	Riverfalls Journal
28	 			| 	River Towns
29	 			| 	Rosemount Town Pages
30	 			| 	Superior Telegram
31	 			| 	SWC Bulletin
32	 			| 	The Dickinson Press
33	 			| 	The Osakis Review
34	 			| 	The Bakken Today
35	 			| 	Two Harbors MN
36	 			| 	Wadena PJ
37	 			| 	West Central Tribune
38	 			| 	WDAY
39	 			| 	WDAZ
40	 			| 	West Fargo Pioneer
41	 			| 	Woodbury Bulletin

#### Additional Resources
- [2016 Civic Engagement Hackathon Overview Documentation](https://docs.google.com/document/d/1138foSkmqywcB4RKaWwErOvi_YH-X0G6UvXFhHW_Yj4/edit#)
- Questions can be sent to ryan.veitch`@`forumcomm.com
