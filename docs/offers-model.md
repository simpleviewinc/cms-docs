## Offers Model
  - [Offer Fields](#offer-fields)
  - [Example Offer Data](#example-offer-data)
  - [Offer Meta Fields](#offer-meta-fields)
  - [Example Offers Meta Data](#example-offers-meta-data)

### Main Offers Endpoints:
```
- https://[client_identifier].simpleviewinc.com/includes/rest_v2/plugins_offers_offers/find/
- https://[client_identifier].simpleviewinc.com/includes/rest_v2/plugins_offers_offers/count/
- https://[client_identifier].simpleviewinc.com/includes/rest_v2/plugins_offers_offers/aggregate/
- https://[client_identifier].simpleviewinc.com/includes/rest_v2/plugins_offers_offermeta/find/
- https://[client_identifier].simpleviewinc.com/includes/rest_v2/plugins_offers_offermeta/aggregate/
```

### Offer Fields

**Model: `plugins_offers_offers`**

- `recid` - `String` - unqiue ID for this offer
- `title` - `String` - offer title
- `title_sort` - `String` - lowercase/normalized version of the title for case-insensitive sorting
- `description` - `String`
- `listing_id` - `Array[Int]` - recid(s) of listing(s) related to this offer
- `offerlink` - `String` - URL for the offer
- `mediafile` - `String`
- `mediathumburl` - `String`
- `medianame` - `String`
- `mediatype` - `String`
  `updated` - `Date` - date/time the offer was last updated in CRM
- `categories` - `Array[Object]` - list of categories assigned to this offer
  - `catName` - `String`
  - `catId` - `Int`
- `acctid` - `Int` - ID for associated CRM account
- `udfs_object` - `Object` - CRM User defined field data, keys are field IDs (varies per client)
- `Media` - `Array[Object]` - media files associated with this offer
  - `mediaurl` - `String`
  - `mediatype` - `String`
  - `medianame` - `String`
  - `sortorder` - `Int`
- `source` - `String`
- `sites` - `Array[String]` - list of all sites this offer can be displayed on
- `primary_site` - main site for this offer
- `personas_ids` - `Array[String]` - personas associated with this offer
- `filter_tags` - `Array[String]` - tags used for optimized filtering
- `custom` - `Object` - custom offer data (varies per client)
- `redeemstart` - `Date`
- `redeemend` - `Date`
- `poststart` - `Date`
- `postend` - `Date`


### Example Offer Data

Note: some fields and data have been removed for the sake of brevity.

```json
{
    "_id" : ObjectId("62687ade255832496f441a29"),
    "crmtracking" : {
        "core_offer_view" : "5_282"
    },
    "mediafile" : "original_8545328107_9d6261b940_c0-53a8dbd95056a36.jpg",
    "mediathumburl" : "https://assets.simpleviewinc.com/simpleview/image/upload/crm/demo/8545328107_9d6261b940_c0-53a8dbd95056a36_53a8dea8-5056-a36a-09b2da8d5031463b.jpg",
    "medianame" : "Hotel hallway",
    "udfs" : [ 
        {
            "name" : "msqa - text area",
            "value" : "approved coupon x-4 image",
            "digits" : 0,
            "fieldid" : 3396,
            "typeid" : 9,
            "type" : "Text Area",
            "value_raw" : "approved coupon x-4 image",
            "value_string" : "approved coupon x-4 image"
        }, 
        {
            "name" : "coupon fileUpload image Req",
            "value" : "https://demo.simpleviewcrm.com/sched/getfilebykey.cfm?filekey=e39b23a9-4580-428c-b8be-b7366c851c82",
            "digits" : 0,
            "fieldid" : 3711,
            "typeid" : 15,
            "type" : "File Upload",
            "value_raw" : "https://demo.simpleviewcrm.com/sched/getfilebykey.cfm?filekey=e39b23a9-4580-428c-b8be-b7366c851c82",
            "value_string" : "https://demo.simpleviewcrm.com/sched/getfilebykey.cfm?filekey=e39b23a9-4580-428c-b8be-b7366c851c82"
        }
    ],
    "mediatype" : "Image",
    "title" : "Stay two nights, get one free",
    "mediaid" : 644,
    "listings_ids" : [ 
        42586
    ],
    "mediaurl" : "https://assets.simpleviewinc.com/simpleview/image/upload/crm/demo/8545328107_9d6261b940_c0-53a8dbd95056a36_53a8dea8-5056-a36a-09b2da8d5031463b.jpg",
    "description" : "<p>Stay two nights, get one free</p>",
    "redeemstart" : ISODate("2018-11-01T07:00:00.000Z"),
    "categories" : [ 
        {
            "categoryid" : 2,
            "categoryname" : "Where to Stay"
        }, 
        {
            "categoryid" : 8,
            "categoryname" : "Winter Deals"
        }
    ],
    "redeemend" : ISODate("2025-03-02T06:59:59.000Z"),
    "postend" : ISODate("2025-03-02T06:59:59.000Z"),
    "poststart" : ISODate("2018-09-01T07:00:00.000Z"),
    "acctid" : 125192,
    "recid" : "282",
    "udfs_object" : {
        "3396" : {
            "name" : "msqa - text area",
            "value" : "approved coupon x-4 image",
            "digits" : 0,
            "fieldid" : 3396,
            "typeid" : 9,
            "type" : "Text Area",
            "value_raw" : "approved coupon x-4 image",
            "value_string" : "approved coupon x-4 image"
        },
        "3711" : {
            "name" : "coupon fileUpload image Req",
            "value" : "https://demo.simpleviewcrm.com/sched/getfilebykey.cfm?filekey=e39b23a9-4580-428c-b8be-b7366c851c82",
            "digits" : 0,
            "fieldid" : 3711,
            "typeid" : 15,
            "type" : "File Upload",
            "value_raw" : "https://demo.simpleviewcrm.com/sched/getfilebykey.cfm?filekey=e39b23a9-4580-428c-b8be-b7366c851c82",
            "value_string" : "https://demo.simpleviewcrm.com/sched/getfilebykey.cfm?filekey=e39b23a9-4580-428c-b8be-b7366c851c82"
        }
    },
    "media" : [ 
        {
            "mediaurl" : "https://assets.simpleviewinc.com/simpleview/image/upload/crm/demo/8545328107_9d6261b940_c0-53a8dbd95056a36_53a8dea8-5056-a36a-09b2da8d5031463b.jpg",
            "mediatype" : "Image",
            "medianame" : "Hotel hallway",
            "sortorder" : 1
        }
    ],
    "source" : "crm",
    "updated" : ISODate("2022-07-04T21:33:15.258Z"),
    "title_sort" : "stay two nights, get one free",
    "cms_title" : "Stay two nights, get one free (282)",
    "cms_title_sort" : "stay two nights, get one free (282)",
    "sites" : [ 
        "primary", 
        "multi"
    ],
    "primary_site" : "primary",
    "filter_tags" : [ 
        "site_primary", 
        "site_multi"
    ],
    "qualityScore" : 20
}
```


### Offer Meta Fields

This table contains a single record which stores various meta data relating to offers. Useful data includes all possible categories, and a list of all CRM User Defined Fields (UDFs) for offers.

**Model: `plugins_offers_offermeta`**

- `categories` - `Array[Object]` - list of all possible categories
  - `calendarid` - `Int` - ID of the calendar this category belongs to
  - `sortOrder` - `Int`
  - `label` - `String` - e.g. "Concerts & Festivals"
  - `active` - `Boolean`
- `udfs_object` - `Object` - contains list of all offer UDFs in CRM. Keys are field IDs.
  - `fieldtype` - `String` - describes the type of data stored in the field e.g. "Text"
  - `numeric` - `Boolean` - whether or not the field contains a boolean value
  - `label` - `String` - the field label, e.g. "TicketMaster URL"
  - `array` - `Boolean` - whether or not the field contains an array of values
  - `fieldid` - `Int` - the unique ID of the field, same as the parent object key for this item

### Example Offers Meta Data

Note: some fields/data have been removed for the sake of brevity.

```json
{
    "_id" : ObjectId("62687add255832496f441a11"),
    "udfs" : [ 
        {
            "fieldtype" : "File Upload",
            "sortorder" : 1,
            "numeric" : false,
            "label" : "coupon fileUpload image",
            "digits" : 0,
            "array" : false,
            "showonweb" : true,
            "typeid" : 15,
            "fieldid" : 3389
        }, 
        {
            "fieldtype" : "Text Area",
            "description" : "text area field",
            "sortorder" : 2,
            "numeric" : false,
            "label" : "msqa - text area",
            "digits" : 0,
            "array" : false,
            "showonweb" : true,
            "typeid" : 9,
            "fieldid" : 3396
        }, 
        {
            "fieldtype" : "Yes/No",
            "description" : "msqa - yes/no field",
            "sortorder" : 3,
            "numeric" : false,
            "label" : "msqa - yes/no field",
            "digits" : 0,
            "array" : false,
            "showonweb" : true,
            "typeid" : 11,
            "fieldid" : 3397
        }
    ],
    "categories" : [ 
        {
            "sortorder" : 10,
            "categoryid" : 2,
            "categoryname" : "Where to Stay",
            "active" : true
        }, 
        {
            "sortorder" : 8,
            "categoryid" : 3,
            "categoryname" : "What to Do",
            "active" : true
        }, 
        {
            "sortorder" : 9,
            "categoryid" : 4,
            "categoryname" : "Where to Eat",
            "active" : true
        }, 
        {
            "sortorder" : 5,
            "categoryid" : 5,
            "categoryname" : "Family Friendly",
            "active" : true
        }
    ],
    "recid" : "singleton",
    "updated" : ISODate("2022-07-06T19:33:31.000Z"),
    "udfs_object" : {
        "3389" : {
            "fieldtype" : "File Upload",
            "sortorder" : 1,
            "numeric" : false,
            "label" : "coupon fileUpload image",
            "digits" : 0,
            "array" : false,
            "showonweb" : true,
            "typeid" : 15,
            "fieldid" : 3389
        },
        "3396" : {
            "fieldtype" : "Text Area",
            "description" : "text area field",
            "sortorder" : 2,
            "numeric" : false,
            "label" : "msqa - text area",
            "digits" : 0,
            "array" : false,
            "showonweb" : true,
            "typeid" : 9,
            "fieldid" : 3396
        },
        "3397" : {
            "fieldtype" : "Yes/No",
            "description" : "msqa - yes/no field",
            "sortorder" : 3,
            "numeric" : false,
            "label" : "msqa - yes/no field",
            "digits" : 0,
            "array" : false,
            "showonweb" : true,
            "typeid" : 11,
            "fieldid" : 3397
        }
    }
}
```
