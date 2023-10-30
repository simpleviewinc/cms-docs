## Events Model
  - [Events Fields](#events-fields)
  - [Example Event Data](#example-event-data)
  - [Example Event Queries](#example-event-queries)
  - [Event Meta Fields](#event-meta-fields)
  - [Example Event Meta Data](#example-event-meta-data)

Events are different to listings in that there are 2 distinct models, "events" and "events by date".

The events_by_date model allows you to use `filter.date_range` to get a set of events within a given date range. This is useful for display events within a given time frame sorted by the date and time of their occurrences. Keep in mind that filtering and sorting events_by_date is more expensive, due to the table size, and is prone to failure (timeouts) when using non-optimized queries. Filtering on UDFs is especially expensive, since they are non-indexed fields within the database. The larger the date range, the more prone to failure it will be.

Also be aware that there are some subtle inconsistencies between listings and events. For example, listings have `recid` which is an Integer type field, whereas events have `recId` which is a String type field. Listing's have "listingudfs_object" but events just have "udfs_object".

### Main Events Endpoints:
```
- https://[client_identifier].simpleviewinc.com/includes/rest_v2/plugins_events_events/find/
- https://[client_identifier].simpleviewinc.com/includes/rest_v2/plugins_events_events/count/
- https://[client_identifier].simpleviewinc.com/includes/rest_v2/plugins_events_events/aggregate/
- https://[client_identifier].simpleviewinc.com/includes/rest_v2/plugins_events_events_by_date/find/
- https://[client_identifier].simpleviewinc.com/includes/rest_v2/plugins_events_events_by_date/count/
- https://[client_identifier].simpleviewinc.com/includes/rest_v2/plugins_events_events_metadata/find/
- https://[client_identifier].simpleviewinc.com/includes/rest_v2/plugins_events_events_metadata/aggregate/
```

### Events Fields

**Model: `plugins_events_events` and `plugins_events_events_by_date`**

- `startDate` - `Date`
- `endDate` - `Date`
- `startTime` - `String` - note that some events have different starting times per occurrence, so it's better to rely on `custom.DATES` (see below) for start and end times.
- `endTime` - `String` - see note for `startTime` above
- `recId` - `String` - unqiue ID for this event
- `title` - `String` - e.g. "Shellfishfestival in Mandal"
- `title_sort` - `String` - lowercase/normalized version of the title for case-insensitive sorting (e.g. "shellfishfestival in mandal")
- `calendarid` - `Int` - ID of the calendar this event belongs to, 1 for leisure, 2 for business
- `updated` - `Date` - date/time the event was last updated in CRM
- `active` - `Boolean`
- `expired` - `Boolean`
- `past` - `Boolean`
- `linkUrl` - `String` - URL for event's website
- `description` - `String`
- `latitude` - `Double`
- `longitude` - `Double`
- `primarycatid` - `Int` - ID for the primary category of this event
- `dates` - `Array[Object]` - list of only the upcoming occurrences of events.
- `primary_site` - main site for this event, based on locale code.
- `sites` - `Array[String]` - list of all sites this event can be displayed on, including sites where it is used as a fallback due to no localized version for that site's language. (e.g. `["primary", "cn"]` for an event that is displayed on the primary (English) and Chinese sites)
- `locale_code` - `String` - locale of this event, e.g. "en"
- `locale_related` - `Array[Object]` - array of localized variations of this event
  - `locale_code` - `String` - the related event's locale
  - `recid` - `String` - the unique ID for the related event
- `categories` - `Array[Object]` - list of categories assigned to this event
  - `catName` - `String`
  - `catId` - `Int`
- `filter_tags` - `Array[String]` - tags used for optimized filtering on site (e.g. `["site_primary", "site_multi"]`)
- `udfs_object` - `Object` - CRM User defined field data, keys are field IDs (varies per client)
- `custom` - `Object` - custom event data (varies per client)
- `accountId` - `Int` - ID for associated CRM account
- `recurType` - `Int` - ID specifying the type of recurrence.
- `location` - `String`
- `city` - `String`
- `state` - `String` 
- `phone` - `String`


### Example Event Data

Note: some fields and data have been removed for the sake of brevity.

```json

{
    "_id" : ObjectId("61e1c8f578a213e9e54fffe5"),
    "week" : 0,
    "primarycatid" : "9",
    "hostname" : "The Food Truck Finder App",
    "address1" : "601 Commonwealth Place",
    "zip" : "15220",
    "day" : 1,
    "startDate" : ISODate("2021-06-01T07:00:00.000Z"),
    "interval" : 1,
    "month" : 0,
    "recurrence" : "Recurring monthly on the 1st",
    "weekday" : 0,
    "description" : "<p><span style=\"font-size: 11pt;\"> Downtown Pittsburgh is hosting an Earth Day-inspired food truck festival on Smithfield Street, across from the Oliver Building.</span></p>\r\n<p>The food truck festival will feature more than a dozen food trucks (many featuring vegan and vegetarian options). Participating trucks include:</p>\r\n<ul>\r\n<li>The Humble Cookie Stand</li>\r\n<li>Cool Beans Taco Truck</li>\r\n<li>Sugar and Spice Ice Cream Truck</li>\r\n<li>Wok of Life PGH</li>\r\n<li>Kona Ice</li>\r\n<li>BRGR</li>\r\n<li>Billu’s Indian Grill on Wheels</li>\r\n<li>Oh-My-Grill</li>\r\n<li>PGH Halal Truck</li>\r\n<li>PGH Sandwich Society</li>\r\n<li>Evil Swine BBQ</li>\r\n<li>PGH Crepes</li>\r\n<li>The Burgh Bites Cart</li>\r\n<li>Bull Dawgs</li>\r\n<li>Randita’s</li>\r\n<li>Nakama</li>\r\n<li>PGH Pierogi Truck</li>\r\n</ul>",
    "udfs" : [ 
        {
            "name" : "Image Release",
            "value" : "https://demo.simpleviewcrm.com/sched/getfilebykey.cfm?filekey=288f8306-a341-4125-b205-f030d60c5182",
            "digits" : 0,
            "fieldid" : 3546,
            "typeid" : 15,
            "type" : "File Upload",
            "value_raw" : "https://demo.simpleviewcrm.com/sched/getfilebykey.cfm?filekey=288f8306-a341-4125-b205-f030d60c5182",
            "value_string" : "https://demo.simpleviewcrm.com/sched/getfilebykey.cfm?filekey=288f8306-a341-4125-b205-f030d60c5182"
        }
    ],
    "calendarid" : "1",
    "endType" : 1,
    "city" : "Pittsburgh",
    "accountId" : 126802,
    "typeName" : "Silver",
    "eventTypeId" : 2,
    "recId" : "428",
    "state" : "PA",
    "occurrences" : 0,
    "listing_id" : 43347,
    "recurType" : 4,
    "dayMask" : 0,
    "never_expire" : false,
    "dates" : [ 
        {
            "eventDate" : ISODate("2022-07-02T06:59:59.000Z")
        }, 
        {
            "eventDate" : ISODate("2022-08-02T06:59:59.000Z")
        }, 
        {
            "eventDate" : ISODate("2022-09-02T06:59:59.000Z")
        }
    ],
    "featured" : true,
    "email" : "jreichenbach@simpleviewinc.com",
    "startTime" : "12:00:00",
    "location" : "Point State Park",
    "categories" : [ 
        {
            "catName" : "Family",
            "catId" : "2"
        }, 
        {
            "catName" : "Culinary",
            "catId" : "9"
        }, 
        {
            "catName" : "Festivals",
            "catId" : "10"
        }
    ],
    "endTime" : "22:00:00",
    "title" : "Summer Food Truck Fest",
    "udfs_object" : {
        "3546" : {
            "name" : "Image Release",
            "value" : "https://demo.simpleviewcrm.com/sched/getfilebykey.cfm?filekey=288f8306-a341-4125-b205-f030d60c5182",
            "digits" : 0,
            "fieldid" : 3546,
            "typeid" : 15,
            "type" : "File Upload",
            "value_raw" : "https://demo.simpleviewcrm.com/sched/getfilebykey.cfm?filekey=288f8306-a341-4125-b205-f030d60c5182",
            "value_string" : "https://demo.simpleviewcrm.com/sched/getfilebykey.cfm?filekey=288f8306-a341-4125-b205-f030d60c5182"
        }
    },
    "sites" : [ 
        "primary", 
        "multi"
    ],
    "primary_site" : "primary",
    "filter_tags" : [ 
        "site_primary", 
        "site_multi"
    ],
    "title_sort" : "summer food truck fest",
    "rank" : 3,
    "recid" : "428",
    "updated" : ISODate("2022-06-30T19:45:16.950Z"),
    "active" : true,
    "cms_title" : "Summer Food Truck Fest (428)",
    "cms_title_sort" : "summer food truck fest (428)",
    "loc" : {
        "type" : "Point",
        "coordinates" : [ 
            -80.009812, 
            40.4411817
        ]
    },
    "expired" : false,
    "past" : false,
    "nextDate" : ISODate("2022-08-02T06:59:59.000Z")
}

```

### Example Event Queries
coming soon

### Event Meta Fields

This table contains a single record which stores various meta data relating to events. Useful data includes all possible categories, and a list of all CRM User Defined Fields (UDFs) for events.

**Model: `plugins_events_events_metadata`**

- `categories` - `Array[Object]` - list of all possible event categories
  - `calendarid` - `Int` - ID of the calendar this category belongs to
  - `sortOrder` - `Int`
  - `label` - `String` - e.g. "Concerts & Festivals"
  - `active` - `Boolean`
- `udfs_object` - `Object` - contains list of all UDFs for events in CRM. Keys are field IDs.
  - `fieldtype` - `String` - describes the type of data stored in the field e.g. "Text"
  - `numeric` - `Boolean` - whether or not the field contains a boolean value
  - `label` - `String` - the field label, e.g. "TicketMaster URL"
  - `array` - `Boolean` - whether or not the field contains an array of values
  - `fieldid` - `Int` - the unique ID of the field, same as the parent object key for this item

### Example Event Meta Data

Note: some fields/data have been removed for the sake of brevity.

```json
{
    "_id": "5bb7c595675a9730ad14ffa8",
    "calendars": [
      {
        "calendarid": "1",
        "calendarname": "Default",
        "submitevent": true,
        "years_synced": 2
      },
      {
        "calendarid": "2",
        "calendarname": "Meetings/Insights Events",
        "submitevent": true,
        "years_synced": 2
      }
    ],
    "categories": [
      {
        "calendarid": "1",
        "sortOrder": 7,
        "label": "Concerts & Festivals",
        "value": "1",
        "active": true
      },
      {
        "schemaitemtype": "Festival",
        "calendarid": "1",
        "sortOrder": 5,
        "label": "Church & Choir Concerts & Festivals",
        "value": "2",
        "active": true
      },
      /* some values omitted */
      {
        "calendarid": "2",
        "sortOrder": 33,
        "label": "Quiz & Games",
        "value": "66",
        "active": true
      },
      {
        "schemaitemtype": "BusinessEvent",
        "calendarid": "2",
        "sortOrder": 34,
        "label": "Course",
        "value": "67",
        "active": true
      }
    ],
    "eventTypes": [
      {
        "label": "International",
        "value": 1,
        "sortOrder": 1,
        "active": true
      }
    ],
    "recid": "singleton",
    "updated": "2020-02-20T04:45:02.000Z",
    "udfs_object": {
      "35": {
        "fieldtype": "Text",
        "sortorder": 18,
        "numeric": false,
        "label": "TicketMaster URL",
        "digits": 0,
        "array": false,
        "showonweb": false,
        "typeid": 8,
        "fieldid": 35
      },
      "94": {
        "fieldtype": "Text",
        "sortorder": 10,
        "numeric": false,
        "label": "Pre-Price",
        "digits": 0,
        "array": false,
        "showonweb": false,
        "typeid": 8,
        "fieldid": 94
      },
      "95": {
        "fieldtype": "Number",
        "sortorder": 11,
        "numeric": true,
        "label": "Trip Advisor Number of Reviews",
        "digits": 0,
        "array": false,
        "showonweb": false,
        "typeid": 4,
        "fieldid": 95
      },
      "96": {
        "fieldtype": "Number",
        "sortorder": 12,
        "numeric": true,
        "label": "Trip Advisor Rating",
        "digits": 0,
        "array": false,
        "showonweb": false,
        "typeid": 4,
        "fieldid": 96
      }
    },
    "id": "5bb7c595675a9730ad14ffa8"
  }
```
