## Listings Model
  - [Listings Fields](#listings-fields)
  - [Example Listing Queries](#example-listing-queries)
  - [Example Listings Docs](#example-listings-docs)
  - [Listing Meta Fields](#listing-meta-fields)
  - [Example Listing Meta data](#example-listing-meta-data)

### Main Listings Endpoints:

Note: all examples are provided using "rc.simpleviewinc.com", which is a website
with sample data. You'll obviously want your own data, so substitute it with
your own website's URL.

```
- https://[client_identifier].simpleviewinc.com/includes/rest_v2/plugins_listings_listings/find/
- https://[client_identifier].simpleviewinc.com/includes/rest_v2/plugins_listings_listings/count/
- https://[client_identifier].simpleviewinc.com/includes/rest_v2/plugins_listings_listings/aggregate/
- https://[client_identifier].simpleviewinc.com/includes/rest_v2/plugins_listings_listingmeta/find/
- https://[client_identifier].simpleviewinc.com/includes/rest_v2/plugins_listings_listingmeta/aggregate/
```

### Listings Fields

**Model: `plugins_listings_listings`**

This is the main listings database table.

Note: some (less useful/common) fields are excluded in order to keep this list as terse as possible. In some cases, there may be more than one way to access the same data, so we've ommitted the less-useful ways. Please don't hesitate to reach out with questions.

* `recid` - `Int` - listing ID for general use
* `_id` - `ObjectId` - standard mongo unique ID. not for general use.
* `title` - `String` - listing/product name
* `sortcompany` - `String` - normalized lowercase version of the company/listing/product name/title to be used for case-insensitive sorting operations
* `alpha` - `String` - first letter of the listing for alpha-filtering purposes
* `description` - `String` - full product/listing description
* `acctid` - `Int` - ID for associated CRM account
* `typeid` - `Int` - 1 for normal listings, 2 for pakkedatabase listings
* `filter_tags` - `Array[String]` - tags used for optimized filtering on site, categories, subcategories, and other criteria. Examles will be provided below. **This is an important field for making fast queries.**
* `locale_code` - the listing's intended locale
* `primary_site` - `String` - The name of the main site this listing is displayed on, based on locale. Most commonly, our clients have a single site named "primary" (or similar).
* `sites` - `Array[String]` - a list of all the sites this listing is available on
* `locale_related` - `Array[Object]` - array of localized variations of this listing (not common)
  * `locale_code` - `String` - the related listing's locale
  * `recid` - `Int` - the unique ID for the related listing
* `locale_items_ids` - `Array[Int]` - contains just the recids for locale related items, for convenient filtering
* `media` - `Array[Object]`
  * `mediaid` - `Int`
  * `mediaurl` - `String` - original image URL
  * `medianame` - `String` - used for image title/name
  * `mediadesc` - `String` - used for image credits
* `primary_image_url` - `String` - URL for the primary listing image
* `listingudfs_object` - `Object` - CRM listing-level user defined fields, as an object, with the `fieldid` as the key for each. These will vary per client.
* `accountudfs_object` - `Object` - CRM account-level user defined fields, as an object, with the `fieldid` as the key for each. These will vary per client.
* `custom` - `Object` - custom listing data. This will vary per client.
* `categories` - `Array[Object]` - array of all subcategories assigned to the listing
  * `primary` - `boolean` - indicates whether this is the listing's primary category
  * `subcatid` - `Int` - unique ID for the subcategory
  * `subcatname` - `String` - name of the subcategory
  * `catid` - `Int` - unique ID for the category that this subcategory belongs to
  * `catname` - `String` - name of the category this subcategory belongs to
* `primary_category` - `Object` - contains the same data as the `categories` array, but only for the primary category - a convenient way to get the primary category information
* `primarycatid` - `Int`
* `primarysubcatid` - `Int`
* `updated` - `Date` - the date this listing was last updated in CRM
* `loc` - `Object` - Geo-location data
  * `coordinates` - `Array[Double]` - latitude [0] and longitude [1] for this listing
* `weburl` - `String` - URL for listing's website
* `address1` - `String`
* `city` - `String`
* `state` - `String` - used for region??
* `zip` - `String` - zip/postal code
* `phone` - `String`
* `email` - `String`
* `fax` - `String`
* `rankorder` - `Int` - rank score determined by the quality algorithm - used for sorting by "recommended"

### Example Listing Queries

#### Find a single listing by recid, return the `recid` and `title` fields (JavaScript/jQuery)

```javascript
$.get("/includes/rest_v2/plugins_listings_listings/find/", {
  json : JSON.stringify({ filter : { recid : 41849 }, options : { castDocs: false, fields : { recid : 1, title : 1 }, limit : 1 } }),
  token : "d2f5ba5eaab52cb4ce80280b5afffdd8"
}, function(data) {
  console.log(data);
});
```

**Response:**
```json
{
  "docs": [
    {
      "recid": 41849,
      "title": "Simpleville Pizzeria"
    }
  ]
}
```


#### Query by site and a specific subcategory

This example shows how `filter_tags` can be used to query by both site and subcategory. This is a very efficient method to query because `filter_tags` is an indexed field. Most of your queries should use at least one filter tag, as the **first** filter parameter.

```javascript
$.get("/includes/rest_v2/plugins_listings_listings/find/", {
  json : JSON.stringify({ filter_tags : { $in : ['site_primary_subcatid_161'] }, options : { castDocs: false, fields : { recid : 1, title : 1 }, limit : 3 } }),
  token : "d2f5ba5eaab52cb4ce80280b5afffdd8"
}, function(data) {
  console.log(data);
});
```

**Response:**
```json
{
  "docs": [
    {
      "recid": 5,
      "title": "Maritime Tours AS"
    },
    {
      "recid": 8,
      "title": "Voss Active"
    },
    {
      "recid": 12,
      "title": "The Whaling Museum"
    }
  ]
}
```


#### Find 3 listings on the primary site filtered by a UDF (user-defined field) value

```javascript
var filter = {
  filter_tags: { $in: ['site_primary']},
  'listingudfs_object.1354.listid' : 770
};

var options = {
  castDocs: false,
  fields : {
    recid : 1,
    title : 1
  },
  limit : 3
}

$.get("/includes/rest_v2/plugins_listings_listings/find/", {
  json : JSON.stringify({ filter: filter, options : options }),
  token : "d2f5ba5eaab52cb4ce80280b5afffdd8"
}, function(data) {
  console.log(data);
});
```

**Response:**
```json
{
  "docs": [
    {
      "recid": 956,
      "title": "Pearl Continental Hotel East"
    },
    {
      "recid": 41896,
      "title": "101 Riverviews"
    },
    {
      "recid": 42629,
      "title": "101 Riverviews"
    }
  ]
}
```



### Example Listings Docs

Note: some duplicate or irrelevant data has been excluded from the examples for the sake of brevity.

```json
{
	"_id": "62c604e4df86489a35c2e451",
	"social": [
		{
			"fieldname": "URL",
			"value": "https://www.yelp.com/biz/",
			"smfieldid": 8,
			"smserviceid": 5
		}
	],
	"taoptin": false,
	"zip": "00123",
	"contact_email": "joe@example.com",
	"address1": "14000 N. Dust Devil Dr.",
	"region": "Downtown District",
	"fax": "520-399-8086",
	"typeid": 1,
	"company": "DHE",
	"listingudfs": [
		{
			"listid": 1030,
			"name": "Sample UDF - Dropdown",
			"value": "Sample List Item 4",
			"digits": 0,
			"fieldid": 1354,
			"typeid": 7,
			"type": "Dropdown",
			"value_raw": {
			"listid": 1030,
			"value": "Sample List Item 4"
			},
			"value_string": "Sample List Item 4"
		},
		{
			"name": "Sample UDF - Multi-Select",
			"valuearray": [
			{
				"listid": 901,
				"value": "Sample List Item 3"
			}
			],
			"digits": 0,
			"fieldid": 1359,
			"typeid": 12,
			"type": "Multi-Select",
			"value_raw": [
			{
				"listid": 901,
				"value": "Sample List Item 3"
			}
			],
			"value_string": "Sample List Item 3"
		}
	],
	"description": "Our rooms are tastefully decorated and overlook a courtyard and swimming pool. A gazebo, bird aviary, and waterfall compliment the flower-filled oasis and lawn.  All rooms have refrigerators, coffeemakers, hairdryers, two phones with data ports, and cable television with HBO and closed caption capability. You will drift to sleep in Papago's extra-long beds. Environmentally enhanced \"green rooms\" and modified rooms for guests with a disability are available.  We are located in Scottsdale where guests can enjoy the world-famous shops, restaurants, golf courses, and tennis course nearby.  Fashion Square Mall, the Phoenix Zoo, Desert Botanical Gardens, and Scottsdale Stadium are just minutes away.",
	"fullname": "Joe Example",
	"city": "Tucson",
	"acctid": 214,
	"sortcompany": "delta hotel east",
	"typename": "Website",
	"state": "AZ",
	"fname": "Joe",
	"recid": 214,
	"weburl": "http://www.simpleviewinc.com",
	"status": "Active",
	"meetingfacility": {
		"numrooms": 75,
		"totalsqft": 12000,
		"sleepingrooms": 34
	},
	"phone": "520-678-9198",
	"addressid": 179542,
	"amenities_array": [
		{
			"tabshortname": "amenitiesandguestservices",
			"amenitytabid": 1,
			"value": "true",
			"label": "Airport Transportation",
			"shortname": "airporttransportation",
			"amenitygroupid": 2,
			"digits": 0,
			"fieldid": 32,
			"typeid": 11,
			"type": "Yes/No",
			"value_raw": true,
			"value_string": "Yes",
			"uniquename": "amenitiesandguestservices_airporttransportation"
		},
		{
			"tabshortname": "amenitiesandguestservices",
			"amenitytabid": 1,
			"value": "2 miles",
			"label": "Convention Center Distance",
			"shortname": "conventioncenterdistance",
			"amenitygroupid": 2,
			"digits": 0,
			"fieldid": 27,
			"typeid": 8,
			"type": "Text",
			"value_raw": "2 miles",
			"value_string": "2 miles",
			"uniquename": "amenitiesandguestservices_conventioncenterdistance"
		},
		{
			"tabshortname": "amenitiesandguestservices",
			"amenitytabid": 1,
			"value": "true",
			"label": "Elevators",
			"shortname": "elevators",
			"amenitygroupid": 2,
			"digits": 0,
			"fieldid": 22,
			"typeid": 11,
			"type": "Yes/No",
			"value_raw": true,
			"value_string": "Yes",
			"uniquename": "amenitiesandguestservices_elevators"
		},
		{
			"tabshortname": "amenitiesandguestservices",
			"amenitytabid": 1,
			"value": "true",
			"label": "Fitness Center",
			"shortname": "fitnesscenter",
			"amenitygroupid": 2,
			"digits": 0,
			"fieldid": 38,
			"typeid": 11,
			"type": "Yes/No",
			"value_raw": true,
			"value_string": "Yes",
			"uniquename": "amenitiesandguestservices_fitnesscenter"
		}
	],
	"email": "sales@example.com",
	"crmtracking": {
		"core_itinerary": "58_214",
		"core_booking_click": "7_214",
		"core_map_view": "59_214",
		"core_listing_view": "1_214",
		"core_mobile_click": "16_214",
		"custom_core_placeholder": "125_214",
		"core_listing_click": "4_214",
		"custom_visitapps_passport_check_in": "110_214",
		"custom_instagram_click_thrus": "115_214",
		"custom_threshold_360_views": "120_214",
		"core_mobile_view": "17_214",
		"core_facebook_view": "14_214",
		"custom_instagram_views": "114_214",
		"custom_youtube_click_thrus": "117_214",
		"core_mobile_call": "18_214",
		"custom_visitapps_listing_view": "111_214",
		"core_facebook_click": "15_214",
		"custom_book_direct_activities": "113_214",
		"custom_youtube_views": "116_214",
		"custom_book_direct_lodging": "112_214",
		"custom_pinterest_click_thrus": "119_214",
		"custom_pinterest_views": "118_214",
		"core_twitter_view": "12_214",
		"core_twitter_click": "13_214"
	},
	"country": "UNITED STATES",
	"tollfree": "800-853-1652",
	"lname": "Example",
	"meetingrooms": [
		{
			"roomname": "Pacific A",
			"roomid": "45",
			"banquet": "60",
			"height": "12",
			"classroom": "50",
			"theater": "100",
			"reception": "100",
			"length": "12",
			"sqft": "900",
			"width": "45"
		}
	],
	"contactid": 61923,
	"addresstype": "Physical",
	"statusid": 32,
	"categories": [
		{
			"primary": true,
			"subcatid": 71,
			"subcatname": "Luxury Resorts",
			"catname": "Hotels",
			"catid": 1571
		},
		{
			"primary": false,
			"subcatid": 23,
			"subcatname": "Apartments & Condos",
			"catname": "Hotels",
			"catid": 1571
		},
		{
			"primary": false,
			"subcatid": 50,
			"subcatname": "Guest Ranches",
			"catname": "Hotels",
			"catid": 1571
		}
	],
	"regionid": 1,
	"title": "DHE",
	"listingudfs_object": {
		"1354": {
			"listid": 1030,
			"name": "Sample UDF - Dropdown",
			"value": "Sample List Item 4",
			"digits": 0,
			"fieldid": 1354,
			"typeid": 7,
			"type": "Dropdown",
			"value_raw": {
			"listid": 1030,
			"value": "Sample List Item 4"
			},
			"value_string": "Sample List Item 4"
		},
		"1359": {
			"name": "Sample UDF - Multi-Select",
			"valuearray": [
			{
				"listid": 901,
				"value": "Sample List Item 3"
			}
			],
			"digits": 0,
			"fieldid": 1359,
			"typeid": 12,
			"type": "Multi-Select",
			"value_raw": [
			{
				"listid": 901,
				"value": "Sample List Item 3"
			}
			],
			"value_string": "Sample List Item 3"
		}
	},
	"amenities": {
		"amenitiesandguestservices_airporttransportation": {
			"tabshortname": "amenitiesandguestservices",
			"amenitytabid": 1,
			"value": "true",
			"label": "Airport Transportation",
			"shortname": "airporttransportation",
			"amenitygroupid": 2,
			"digits": 0,
			"fieldid": 32,
			"typeid": 11,
			"type": "Yes/No",
			"value_raw": true,
			"value_string": "Yes",
			"uniquename": "amenitiesandguestservices_airporttransportation"
		},
		"amenitiesandguestservices_conventioncenterdistance": {
			"tabshortname": "amenitiesandguestservices",
			"amenitytabid": 1,
			"value": "2 miles",
			"label": "Convention Center Distance",
			"shortname": "conventioncenterdistance",
			"amenitygroupid": 2,
			"digits": 0,
			"fieldid": 27,
			"typeid": 8,
			"type": "Text",
			"value_raw": "2 miles",
			"value_string": "2 miles",
			"uniquename": "amenitiesandguestservices_conventioncenterdistance"
		}
	},
	"updated": "2022-07-06T21:55:48.103Z",
	"contacttitle": "General Manager/Owner",
	"alpha": "d",
	"loc": {
		"type": "Point",
		"coordinates": [
			-110.92407,
			32.460432
		]
	},
	"sites": [
		"primary",
		"multi"
	],
	"primary_site": "primary",
	"rankorder": 9,
	"primary_category": {
		"primary": true,
		"subcatid": 71,
		"subcatname": "Luxury Resorts",
		"catname": "Hotels",
		"catid": 1571
	},
	"primarycatid": 1571,
	"primarysubcatid": 71,
	"cms_title": "DHE - Hotels - Luxury Resorts (214)",
	"cms_title_sort": "dhe - hotels - luxury resorts (214)",
	"primary_image_url": "https://res.cloudinary.com/simpleview/image/upload/v1463070008/listings_default_tppucp.jpg",
	"primary_image_is_default": true,
	"filter_tags": [
		"site_primary",
		"catid_1571",
		"subcatid_71",
		"site_primary_catid_1571",
		"site_primary_subcatid_71",
		"site_primary_catid_1571_subcatid_71",
		"subcatid_23",
		"site_primary_subcatid_23",
		"site_primary_catid_1571_subcatid_23",
		"subcatid_50",
		"site_primary_subcatid_50",
		"site_primary_catid_1571_subcatid_50",
		"subcatid_100",
		"site_primary_subcatid_100",
		"site_primary_catid_1571_subcatid_100",
		"subcatid_104",
		"site_primary_subcatid_104",
		"site_primary_catid_1571_subcatid_104",
		"site_multi",
		"site_multi_catid_1571",
		"site_multi_subcatid_71",
		"site_multi_catid_1571_subcatid_71",
		"site_multi_subcatid_23",
		"site_multi_catid_1571_subcatid_23",
		"site_multi_subcatid_50",
		"site_multi_catid_1571_subcatid_50",
		"site_multi_subcatid_100",
		"site_multi_catid_1571_subcatid_100",
		"site_multi_subcatid_104",
		"site_multi_catid_1571_subcatid_104"
	],
	"detail_type": "hotels",
	"qualityScore": -43,
	"id": "62c604e4df86489a35c2e451",
	"detailURL": "/hotel/dhe/214/",
	"genericUrl": "/listing/dhe/214/",
	"url": "/hotel/dhe/214/",
	"absolute_url": "https://rc.simpleviewinc.com/hotel/dhe/214/",
	"absolute_primary_url": "https://rc.simpleviewinc.com/hotel/dhe/214/",
	"amp_url": "https://rc.simpleviewinc.com/hotel/dhe/214/amp/",
	"isListing": true,
	"longitude": -110.92407,
	"latitude": 32.460432,
	"hasTripAdvisor": false,
	"hasYelp": false
	}
```


### Listing Meta Fields

**Model: `plugins_listings_listingmeta`**

This table contains listing meta data, such as all categories and subcategories, all possible list values for select and multiselect UDF fields, and more.

This table contains a single document (singleton) which holds all the metadata. For this reason, it's important to use `options.fields` to include exactly what you need, rather than pulling all the data every time. This also means there's no purpose for using a filter with its endpoints, since it will always return a single doc.

Practical uses include listing categories, subcategories, or amenities (i.e. for creating filters), or getting translations of amenity names. It is also useful during development for referencing listing and account user-defined fields (UDFs).

Since this is a singleton table, which contains a large amount of data, you'll always need to specify a small set of `options.fields`.

The best way to understand this data is to look at some responses, such as this one:

```
https://[client_identifier].simpleviewinc.com/includes/rest_v2/plugins_listings_listingmeta/find/?json={%22options%22:{%22fields%22:{%22listingudfs_object%22:true}}}&token=6db0fd820e20e7295515f5657dd75e6f
```

Try replacing `listingudfs_object` with `accountudfs_object` or `listingcats`, etc. You'll of course need to grab a fresh access token to replace the one in the URL above.

Note: some (less useful/common) fields are excluded in order to keep this list as terse as possible.

* `listingcats` - `Array[Object]` - A list of all available listing categories
  * `catname` - `String`
  * `catid` - `Int` - catagory unique ID
  * `typeid` - `Int` - type of listing associated with this category, 1 for normal listings, 2 for pakkedatabase
  * `sortorder` - `Int` - number describing where this category should appear in the list
* `listingudfs_object` - `Object` - object containing all listing-level UDFs, where keys are the UDF field ID.
  * `fieldtype` - `String` - the type of field (Common values: "Date", "Text", "Yes/No", "Text Area", "Dropdown", "Multi-Select", "Number")
  * `fieldid` - `Int` - the unique ID for this UDF (same as the parent object key for each item)
  * `label` - `String` - the field label, e.g. "Booking URL"
  * `numeric` - `Boolean` - whether the value of the field is numeric
  * `array` - `Boolean` - whether the field value is an array
* `accountudfs_object` - `Object` - object containing all account-level UDFs, where keys are the UDF field ID. The schema is the same as `listingudfs_object`.
* `custom` - custom meta data (varies per client)
* `amenityfields_object` - `Object` - another way to access amenity data, in a slightly different structure.
* `amenitygroups` - `Array[Object]` - same as above
* `amenitytabs` - `Array[Object]` - same as above
* `amenityfields` - `Array[Object]` - same as above
* `meetingfacilityadditional` - `Object` - like amenities, but specifically for meeting facility listing data
* `socialfields` - `Array[Object]` - list of possible social media field data
  * `smservice` - `String` - social media service name, e.g. "Twitter"
  * `fieldname` - `String` - social media data point name, e.g. "UserID"
  * `smfieldid` - `Int` - ID for this social media field
  * `smserviceid` - `Int` - ID for this social media service


### Example Listing Meta data

Note: much data has been ommitted for the sake of brevity.


```json
{
    "_id" : ObjectId("60b7edfd273dd4bd800761de"),
    "listingranks" : [ 
        {
            "sortorder" : 4,
            "rankid" : 2,
            "active" : true,
            "rankname" : "Priority"
        }, 
        {
            "sortorder" : 1,
            "rankid" : 3,
            "active" : true,
            "rankname" : "Basic"
        }, 
        {
            "sortorder" : 5,
            "rankid" : 4,
            "active" : true,
            "rankname" : "Silver"
        }, 
        {
            "sortorder" : 3,
            "rankid" : 5,
            "active" : true,
            "rankname" : "Non-Member"
        }, 
        {
            "sortorder" : 8,
            "rankid" : 7,
            "active" : true,
            "rankname" : "Visitors Guide 50 Characters"
        }, 
        {
            "sortorder" : 7,
            "rankid" : 8,
            "active" : true,
            "rankname" : "Visitors Guide 200 Characters"
        }
    ],
    "listingudfs" : [
        {
            "fieldtype" : "Number",
            "sortorder" : 1,
            "numeric" : true,
            "label" : "Sample Number",
            "digits" : 2,
            "array" : false,
            "showonweb" : false,
            "typeid" : 4,
            "fieldid" : 1351
        },
        {
            "fieldtype" : "Multi-Select",
            "sortorder" : 1,
            "numeric" : false,
            "label" : "Days Open",
            "digits" : 0,
            "array" : true,
            "showonweb" : true,
            "values" : [ 
                {
                    "listid" : 1824,
                    "sortorder" : 1,
                    "listvalue" : "Monday"
                }, 
                {
                    "listid" : 1825,
                    "sortorder" : 2,
                    "listvalue" : "Tuesday"
                }, 
                {
                    "listid" : 1826,
                    "sortorder" : 3,
                    "listvalue" : "Wednesday"
                }, 
                {
                    "listid" : 1827,
                    "sortorder" : 4,
                    "listvalue" : "Thursday"
                }, 
                {
                    "listid" : 1828,
                    "sortorder" : 5,
                    "listvalue" : "Friday"
                }, 
                {
                    "listid" : 1829,
                    "sortorder" : 6,
                    "listvalue" : "Saturday"
                }, 
                {
                    "listid" : 1830,
                    "sortorder" : 7,
                    "listvalue" : "Sunday"
                }
            ],
            "typeid" : 12,
            "fieldid" : 3068
        }, 
        {
            "fieldtype" : "File Upload",
            "sortorder" : 20,
            "numeric" : false,
            "label" : "Deploy -3-570 UploadFile",
            "digits" : 0,
            "array" : false,
            "showonweb" : true,
            "typeid" : 15,
            "fieldid" : 3712
        }
    ],
    "regions" : [ 
        {
            "sortorder" : 1,
            "region" : "Downtown District",
            "has_listings" : true,
            "active" : true,
            "regionid" : 1
        }, 
        {
            "sortorder" : 3,
            "region" : "Outlying Areas",
            "has_listings" : true,
            "active" : true,
            "regionid" : 2
        }, 
        {
            "sortorder" : 4,
            "region" : "East Valley District",
            "has_listings" : true,
            "active" : true,
            "regionid" : 3
        }, 
        {
            "sortorder" : 2,
            "region" : "Airport",
            "has_listings" : true,
            "active" : true,
            "regionid" : 30
        }, 
        {
            "sortorder" : 5,
            "region" : "Oro Valley District",
            "has_listings" : true,
            "active" : true,
            "regionid" : 83
        }, 
        {
            "sortorder" : 16,
            "region" : "Southside District",
            "has_listings" : true,
            "active" : true,
            "regionid" : 84
        }, 
        {
            "sortorder" : 17,
            "region" : "Northside District",
            "has_listings" : false,
            "active" : true,
            "regionid" : 85
        }, 
        {
            "sortorder" : 18,
            "region" : "Westside District",
            "has_listings" : true,
            "active" : true,
            "regionid" : 86
        }
    ],
    "listingcats" : [ 
        {
            "sortorder" : 9,
            "catname" : "Hotels",
            "has_listings" : true,
            "catid" : 1571,
            "active" : true,
            "typeid" : 1,
            "fullname" : "Website: Hotels",
            "sites" : [ 
                "primary", 
                "multi"
            ]
        }, 
        {
            "sortorder" : 2,
            "catname" : "Convention & Group Services",
            "has_listings" : true,
            "catid" : 1574,
            "active" : true,
            "typeid" : 1,
            "fullname" : "Website: Convention & Group Services",
            "sites" : [ 
                "primary", 
                "multi"
            ]
        }, 
        {
            "sortorder" : 3,
            "catname" : "Dining",
            "has_listings" : true,
            "catid" : 1575,
            "active" : true,
            "typeid" : 1,
            "fullname" : "Website: Dining",
            "sites" : [ 
                "primary", 
                "multi"
            ]
        }
    ],
    "meetingroomsudfs" : [ 
        {
            "fieldtype" : "Number",
            "sortorder" : 1,
            "numeric" : true,
            "label" : "Total Exhibitor Booths Needed",
            "digits" : 0,
            "array" : false,
            "showonweb" : false,
            "typeid" : 4,
            "fieldid" : 3468
        }, 
        {
            "fieldtype" : "Dropdown",
            "description" : "This is a custom field for the account Record",
            "sortorder" : 1,
            "numeric" : false,
            "label" : "Clovis Custom Dropsown Field",
            "digits" : 0,
            "array" : true,
            "showonweb" : false,
            "values" : [ 
                {
                    "listid" : 2311,
                    "sortorder" : 1,
                    "listvalue" : "4"
                }, 
                {
                    "listid" : 2310,
                    "sortorder" : 2,
                    "listvalue" : "3"
                }, 
                {
                    "listid" : 2309,
                    "sortorder" : 3,
                    "listvalue" : "2"
                }, 
                {
                    "listid" : 2308,
                    "sortorder" : 4,
                    "listvalue" : "1"
                }
            ],
            "typeid" : 7,
            "fieldid" : 3654
        },  
        {
            "fieldtype" : "Yes/No",
            "sortorder" : 9,
            "numeric" : false,
            "label" : "Sample UDF - Yes/No",
            "digits" : 0,
            "array" : false,
            "showonweb" : false,
            "typeid" : 11,
            "fieldid" : 1688
        }
    ],
    "amenityfields" : [ 
        {
            "description" : "Describe Eco Friendly Efforts of the property or business",
            "sortorder" : 1,
            "label" : "Green Practices",
            "amenitygroupid" : 70,
            "tabshortname" : "activities",
            "amenitytabid" : 1024,
            "fieldtype" : "Text Area",
            "numeric" : false,
            "shortname" : "green",
            "array" : false,
            "fieldid" : 240,
            "typeid" : 9,
            "uniquename" : "activities_green"
        }, 
        {
            "description" : "Will you store guest luggage before and/or after the guest checks in or checks out?",
            "sortorder" : 4,
            "label" : "Short Term Luggage Storage",
            "amenitygroupid" : 70,
            "tabshortname" : "activities",
            "amenitytabid" : 1024,
            "fieldtype" : "Yes/No",
            "numeric" : false,
            "shortname" : "luggagestorage",
            "array" : false,
            "fieldid" : 223,
            "typeid" : 11,
            "uniquename" : "activities_luggagestorage"
        },  
        {
            "altlabels" : [ 
                {
                    "displayname" : "English",
                    "locale" : "en-US",
                    "value" : "File Upload \"Health & Safety Policy\""
                }
            ],
            "sortorder" : 3,
            "label" : "File Upload - Health & Safety Policy",
            "amenitygroupid" : 159,
            "tabshortname" : "businesscontinuity",
            "amenitytabid" : 1072,
            "fieldtype" : "File Upload",
            "numeric" : false,
            "shortname" : "healthsafetypolicy",
            "array" : false,
            "fieldid" : 590,
            "typeid" : 15,
            "uniquename" : "businesscontinuity_healthsafetypolicy"
        }
    ],
    "socialfields" : [ 
        {
            "smservice" : "Yelp",
            "fieldname" : "Phone",
            "smfieldid" : 8,
            "smserviceid" : 5
        }, 
        {
            "smservice" : "Yelp",
            "fieldname" : "URL",
            "smfieldid" : 9,
            "smserviceid" : 5
        }, 
        {
            "smservice" : "OpenTable",
            "fieldname" : "ID",
            "smfieldid" : 2,
            "smserviceid" : 6
        }, 
        {
            "smservice" : "OpenTable",
            "fieldname" : "URL",
            "smfieldid" : 7,
            "smserviceid" : 6
        }, 
        {
            "smservice" : "Instagram",
            "fieldname" : "URL",
            "smfieldid" : 7,
            "smserviceid" : 13
        }
    ],
    "listingtypes" : [ 
        {
            "typename" : "Website",
            "sortorder" : 17,
            "active" : true,
            "typeid" : 1,
            "sites" : [ 
                "primary", 
                "multi"
            ]
        }, 
        {
            "typename" : "Meeting Planner Guide",
            "sortorder" : 6,
            "active" : true,
            "typeid" : 9,
            "sites" : [ 
                "primary", 
                "multi"
            ]
        }, 
        {
            "typename" : "Restaurant Week",
            "sortorder" : 10,
            "active" : true,
            "typeid" : 22,
            "sites" : [ 
                "primary", 
                "multi"
            ]
        }
    ],
    "accountudfs" : [ 
        {
            "fieldtype" : "Date",
            "sortorder" : 1,
            "numeric" : false,
            "label" : "Expiration Date",
            "digits" : 0,
            "array" : false,
            "showonweb" : true,
            "typeid" : 2,
            "fieldid" : 3065
        }, 
        {
            "fieldtype" : "Multi-Select",
            "description" : "Please select all the Partner Programs you plan on attending this year.",
            "sortorder" : 1,
            "numeric" : false,
            "label" : "2021 Marketing Communications Events",
            "digits" : 0,
            "array" : true,
            "showonweb" : false,
            "values" : [ 
                {
                    "listid" : 1963,
                    "sortorder" : 1,
                    "listvalue" : "Summer 2021: Experiential Marketing Tour (TBD) "
                }, 
                {
                    "listid" : 1962,
                    "sortorder" : 2,
                    "listvalue" : "1/2021 - 12/2021: Partner Education/Training ($500/in-kind) "
                }, 
                {
                    "listid" : 1961,
                    "sortorder" : 3,
                    "listvalue" : "1/2021 - 12/2021: Individual Press Trips (In-Kind) "
                }
            ],
            "typeid" : 12,
            "fieldid" : 3212
        }
    ],
    "meetingfacilityadditional" : {
        "fields" : [ 
            {
                "fieldtype" : "Yes/No",
                "description" : "Convention space",
                "sortorder" : 1,
                "numeric" : false,
                "label" : "Convention Space",
                "shortname" : "conspace",
                "amenitygroupid" : 117,
                "array" : false,
                "typeid" : 11,
                "fieldid" : 358
            }, 
            {
                "altlabels" : [ 
                    {
                        "displayname" : "English",
                        "locale" : "en-US",
                        "value" : "Small Group Rates"
                    }
                ],
                "fieldtype" : "Yes/No",
                "sortorder" : 2,
                "numeric" : false,
                "label" : "Small Group Rates",
                "shortname" : "smgrprate",
                "amenitygroupid" : 117,
                "array" : false,
                "typeid" : 11,
                "fieldid" : 645
            }, 
            {
                "altlabels" : [ 
                    {
                        "displayname" : "English",
                        "locale" : "en-US",
                        "value" : "description review"
                    }
                ],
                "fieldtype" : "Yes/No",
                "description" : "description review",
                "sortorder" : 2,
                "numeric" : false,
                "label" : "description review",
                "shortname" : "descrev",
                "amenitygroupid" : 119,
                "array" : false,
                "typeid" : 11,
                "fieldid" : 465
            }
        ],
        "groups" : [ 
            {
                "amenitytabid" : 999,
                "sortorder" : 15,
                "amenitygroupname" : "Boutique Hotels",
                "amenitygroupid" : 117
            }, 
            {
                "amenitytabid" : 999,
                "sortorder" : 83,
                "amenitygroupname" : "RD Test",
                "amenitygroupid" : 119
            }, 
            {
                "amenitytabid" : 999,
                "sortorder" : 92,
                "amenitygroupname" : "Site Profile",
                "amenitygroupid" : 26
            }
        ],
        "fields_object" : {
            "conspace" : {
                "fieldtype" : "Yes/No",
                "description" : "Convention space",
                "sortorder" : 1,
                "numeric" : false,
                "label" : "Convention Space",
                "shortname" : "conspace",
                "amenitygroupid" : 117,
                "array" : false,
                "typeid" : 11,
                "fieldid" : 358
            },
            "smgrprate" : {
                "altlabels" : [ 
                    {
                        "displayname" : "English",
                        "locale" : "en-US",
                        "value" : "Small Group Rates"
                    }
                ],
                "fieldtype" : "Yes/No",
                "sortorder" : 2,
                "numeric" : false,
                "label" : "Small Group Rates",
                "shortname" : "smgrprate",
                "amenitygroupid" : 117,
                "array" : false,
                "typeid" : 11,
                "fieldid" : 645
            },
            "descrev" : {
                "altlabels" : [ 
                    {
                        "displayname" : "English",
                        "locale" : "en-US",
                        "value" : "description review"
                    }
                ],
                "fieldtype" : "Yes/No",
                "description" : "description review",
                "sortorder" : 2,
                "numeric" : false,
                "label" : "description review",
                "shortname" : "descrev",
                "amenitygroupid" : 119,
                "array" : false,
                "typeid" : 11,
                "fieldid" : 465
            }
        }
    },
    "amenitytabs" : [ 
        {
            "tabshortname" : "activities",
            "amenitytabname" : "Activities",
            "altlabels" : [ 
                {
                    "displayname" : "English",
                    "locale" : "en-US",
                    "value" : "Activities"
                }
            ],
            "amenitytabid" : 1024,
            "sortorder" : 6
        }, 
        {
            "tabshortname" : "adacompliance",
            "amenitytabname" : "ADA Compliance",
            "amenitytabid" : 1057,
            "sortorder" : 7
        }, 
        {
            "tabshortname" : "businesscontinuity",
            "amenitytabname" : "Business Continuity",
            "altlabels" : [ 
                {
                    "displayname" : "English",
                    "locale" : "en-US",
                    "value" : "Business Continuity"
                }
            ],
            "amenitytabid" : 1072,
            "sortorder" : 10
        }
    ],
    "amenitygroups" : [ 
        {
            "amenitytabid" : 1024,
            "sortorder" : 33,
            "amenitygroupname" : "General",
            "amenitygroupid" : 70
        }, 
        {
            "altlabels" : [ 
                {
                    "displayname" : "English",
                    "locale" : "en-US",
                    "value" : "Accessibility"
                }
            ],
            "amenitytabid" : 1057,
            "sortorder" : 2,
            "amenitygroupname" : "Accessibility",
            "amenitygroupid" : 143
        }, 
        {
            "amenitytabid" : 1057,
            "sortorder" : 18,
            "amenitygroupname" : "Communication Services",
            "amenitygroupid" : 145
        }
    ],
    "listingsubcats" : [ 
        {
            "schemaitemtype" : "EntertainmentBusiness",
            "subcatid" : 1,
            "sortorder" : 1,
            "categoryid" : 1,
            "subcatname" : "Joss QA SubCat",
            "has_listings" : true,
            "active" : true,
            "fullname" : "Joss QA Cat: Joss QA SubCat",
            "sites" : [ 
                "primary", 
                "multi"
            ],
            "typeid" : 1,
            "catname" : "Joss QA Cat"
        }, 
        {
            "subcatid" : 17,
            "sortorder" : 1,
            "categoryid" : 1586,
            "subcatname" : "Aircraft Charters & Services",
            "has_listings" : false,
            "active" : true,
            "fullname" : "Transportation: Aircraft Charters & Services",
            "sites" : [ 
                "primary", 
                "multi"
            ],
            "typeid" : 1,
            "catname" : "Transportation"
        }, 
        {
            "subcatid" : 19,
            "sortorder" : 1,
            "categoryid" : 1575,
            "subcatname" : "American",
            "has_listings" : true,
            "active" : true,
            "fullname" : "Dining: American",
            "sites" : [ 
                "primary", 
                "multi"
            ],
            "typeid" : 1,
            "catname" : "Dining"
        }
    ],
    "acctstatus" : [ 
        {
            "statusid" : 14,
            "sortorder" : 5,
            "status" : "Partner",
            "active" : true
        }, 
        {
            "statusid" : 32,
            "sortorder" : 1,
            "status" : "Active",
            "active" : true
        }, 
        {
            "statusid" : 44,
            "sortorder" : 1,
            "status" : "Active",
            "active" : true
        }
    ],
    "recid" : "singleton",
    "updated" : ISODate("2022-07-07T17:16:59.000Z"),
    "listingudfs_object" : {
        "1350" : {
            "fieldtype" : "Email",
            "sortorder" : 4,
            "numeric" : false,
            "label" : "Sample UDF - Email",
            "digits" : 0,
            "array" : false,
            "showonweb" : false,
            "typeid" : 3,
            "fieldid" : 1350
        },
        "1351" : {
            "fieldtype" : "Number",
            "sortorder" : 1,
            "numeric" : true,
            "label" : "Sample Number",
            "digits" : 2,
            "array" : false,
            "showonweb" : false,
            "typeid" : 4,
            "fieldid" : 1351
        },
        "1352" : {
            "fieldtype" : "Percent",
            "sortorder" : 3,
            "numeric" : true,
            "label" : "Sample UDF - Percent",
            "digits" : 0,
            "array" : false,
            "showonweb" : false,
            "typeid" : 5,
            "fieldid" : 1352
        }
    },
    "accountudfs_object" : {
        "2984" : {
            "fieldtype" : "File Upload",
            "sortorder" : 4,
            "numeric" : false,
            "label" : "Partner Fact Sheet",
            "digits" : 0,
            "array" : false,
            "showonweb" : true,
            "typeid" : 15,
            "fieldid" : 2984
        },
        "3044" : {
            "fieldtype" : "Currency",
            "sortorder" : 5,
            "numeric" : true,
            "label" : "Example Currency UDF Field",
            "digits" : 0,
            "array" : false,
            "showonweb" : false,
            "typeid" : 1,
            "fieldid" : 3044
        },
        "3064" : {
            "fieldtype" : "Dropdown",
            "sortorder" : 2,
            "numeric" : false,
            "label" : "Membership Level",
            "digits" : 0,
            "array" : true,
            "showonweb" : true,
            "values" : [ 
                {
                    "listid" : 1911,
                    "sortorder" : 1,
                    "listvalue" : "Gold"
                }, 
                {
                    "listid" : 1912,
                    "sortorder" : 2,
                    "listvalue" : "Silver"
                }
            ],
            "typeid" : 7,
            "fieldid" : 3064
        }
    },
    "meetingroomsudfs_object" : {
        "1678" : {
            "fieldtype" : "Currency",
            "sortorder" : 3,
            "numeric" : true,
            "label" : "Sample UDF - Currency",
            "digits" : 0,
            "array" : false,
            "showonweb" : false,
            "typeid" : 1,
            "fieldid" : 1678
        },
        "1688" : {
            "fieldtype" : "Yes/No",
            "sortorder" : 9,
            "numeric" : false,
            "label" : "Sample UDF - Yes/No",
            "digits" : 0,
            "array" : false,
            "showonweb" : false,
            "typeid" : 11,
            "fieldid" : 1688
        },
        "1689" : {
            "fieldtype" : "Dropdown",
            "sortorder" : 10,
            "numeric" : false,
            "label" : "Sample UDF - Multi-Select",
            "digits" : 0,
            "array" : true,
            "showonweb" : false,
            "values" : [ 
                {
                    "listid" : 685,
                    "sortorder" : 1,
                    "listvalue" : "Sample List Item 1"
                }, 
                {
                    "listid" : 815,
                    "sortorder" : 2,
                    "listvalue" : "Sample List Item 2"
                }, 
                {
                    "listid" : 945,
                    "sortorder" : 3,
                    "listvalue" : "Sample List Item 3"
                }, 
                {
                    "listid" : 1075,
                    "sortorder" : 4,
                    "listvalue" : "Sample List Item 4"
                }
            ],
            "typeid" : 7,
            "fieldid" : 1689
        }
    },
    "amenityfields_object" : {
        "activities_green" : {
            "description" : "Describe Eco Friendly Efforts of the property or business",
            "sortorder" : 1,
            "label" : "Green Practices",
            "amenitygroupid" : 70,
            "tabshortname" : "activities",
            "amenitytabid" : 1024,
            "fieldtype" : "Text Area",
            "numeric" : false,
            "shortname" : "green",
            "array" : false,
            "fieldid" : 240,
            "typeid" : 9,
            "uniquename" : "activities_green"
        },
        "activities_luggagestorage" : {
            "description" : "Will you store guest luggage before and/or after the guest checks in or checks out?",
            "sortorder" : 4,
            "label" : "Short Term Luggage Storage",
            "amenitygroupid" : 70,
            "tabshortname" : "activities",
            "amenitytabid" : 1024,
            "fieldtype" : "Yes/No",
            "numeric" : false,
            "shortname" : "luggagestorage",
            "array" : false,
            "fieldid" : 223,
            "typeid" : 11,
            "uniquename" : "activities_luggagestorage"
        },
        "activities_ofskiruns" : {
            "altlabels" : [ 
                {
                    "displayname" : "English",
                    "locale" : "en-US",
                    "value" : "# of Ski Runs"
                }
            ],
            "sortorder" : 5,
            "label" : "# of Ski Runs",
            "amenitygroupid" : 70,
            "tabshortname" : "activities",
            "amenitytabid" : 1024,
            "fieldtype" : "Number",
            "numeric" : true,
            "shortname" : "ofskiruns",
            "array" : false,
            "fieldid" : 695,
            "typeid" : 4,
            "uniquename" : "activities_ofskiruns"
        },
        "famprofile_w" : {
            "altlabels" : [ 
                {
                    "displayname" : "English",
                    "locale" : "en-US",
                    "value" : "What Month Would You Like?"
                }
            ],
            "sortorder" : 2,
            "label" : "What Month Would You Like?",
            "amenitygroupid" : 152,
            "values" : [ 
                {
                    "listid" : 156,
                    "sortorder" : 1,
                    "listvalue" : "Jan"
                }, 
                {
                    "listid" : 157,
                    "sortorder" : 2,
                    "listvalue" : "Feb"
                }
            ],
            "tabshortname" : "famprofile",
            "amenitytabid" : 1067,
            "fieldtype" : "Multi-Select",
            "numeric" : false,
            "shortname" : "w",
            "array" : true,
            "fieldid" : 530,
            "typeid" : 12,
            "uniquename" : "famprofile_w"
        },
        "amenitiesandguestservices_airporttransportation" : {
            "sortorder" : 2,
            "label" : "Airport Transportation",
            "amenitygroupid" : 2,
            "tabshortname" : "amenitiesandguestservices",
            "amenitytabid" : 1,
            "fieldtype" : "Yes/No",
            "numeric" : false,
            "shortname" : "airporttransportation",
            "array" : false,
            "fieldid" : 32,
            "typeid" : 11,
            "uniquename" : "amenitiesandguestservices_airporttransportation"
        },
        "shopping_localsource" : {
            "altlabels" : [ 
                {
                    "displayname" : "English",
                    "locale" : "en-US",
                    "value" : "Locally Sourced"
                }
            ],
            "sortorder" : 2,
            "label" : "Locally Sourced",
            "amenitygroupid" : 122,
            "tabshortname" : "shopping",
            "amenitytabid" : 1049,
            "fieldtype" : "Yes/No",
            "numeric" : false,
            "shortname" : "localsource",
            "array" : false,
            "fieldid" : 545,
            "typeid" : 11,
            "uniquename" : "shopping_localsource"
        }
    },
    "amenities" : [ 
        {
            "tabshortname" : "activities",
            "amenitytabname" : "Activities",
            "altlabels" : [ 
                {
                    "displayname" : "English",
                    "locale" : "en-US",
                    "value" : "Activities"
                }
            ],
            "amenitytabid" : 1024,
            "sortorder" : 6,
            "groups" : [ 
                {
                    "amenitytabid" : 1024,
                    "sortorder" : 33,
                    "amenitygroupname" : "General",
                    "amenitygroupid" : 70,
                    "fields" : [ 
                        {
                            "description" : "Describe Eco Friendly Efforts of the property or business",
                            "sortorder" : 1,
                            "label" : "Green Practices",
                            "amenitygroupid" : 70,
                            "tabshortname" : "activities",
                            "amenitytabid" : 1024,
                            "fieldtype" : "Text Area",
                            "numeric" : false,
                            "shortname" : "green",
                            "array" : false,
                            "fieldid" : 240,
                            "typeid" : 9,
                            "uniquename" : "activities_green"
                        }, 
                        {
                            "description" : "Will you store guest luggage before and/or after the guest checks in or checks out?",
                            "sortorder" : 4,
                            "label" : "Short Term Luggage Storage",
                            "amenitygroupid" : 70,
                            "tabshortname" : "activities",
                            "amenitytabid" : 1024,
                            "fieldtype" : "Yes/No",
                            "numeric" : false,
                            "shortname" : "luggagestorage",
                            "array" : false,
                            "fieldid" : 223,
                            "typeid" : 11,
                            "uniquename" : "activities_luggagestorage"
                        }, 
                        {
                            "altlabels" : [ 
                                {
                                    "displayname" : "English",
                                    "locale" : "en-US",
                                    "value" : "# of Ski Runs"
                                }
                            ],
                            "sortorder" : 5,
                            "label" : "# of Ski Runs",
                            "amenitygroupid" : 70,
                            "tabshortname" : "activities",
                            "amenitytabid" : 1024,
                            "fieldtype" : "Number",
                            "numeric" : true,
                            "shortname" : "ofskiruns",
                            "array" : false,
                            "fieldid" : 695,
                            "typeid" : 4,
                            "uniquename" : "activities_ofskiruns"
                        }
                    ]
                }
            ]
        }, 
        {
            "tabshortname" : "adacompliance",
            "amenitytabname" : "ADA Compliance",
            "amenitytabid" : 1057,
            "sortorder" : 7,
            "groups" : [ 
                {
                    "altlabels" : [ 
                        {
                            "displayname" : "English",
                            "locale" : "en-US",
                            "value" : "Accessibility"
                        }
                    ],
                    "amenitytabid" : 1057,
                    "sortorder" : 2,
                    "amenitygroupname" : "Accessibility",
                    "amenitygroupid" : 143,
                    "fields" : [ 
                        {
                            "altlabels" : [ 
                                {
                                    "displayname" : "English",
                                    "locale" : "en-US",
                                    "value" : "Audio Hearing Aids"
                                }
                            ],
                            "description" : "Does the facility provide audio aids for the blind and visually impaired?",
                            "sortorder" : 1,
                            "label" : "Audio Hearing Aids",
                            "amenitygroupid" : 143,
                            "tabshortname" : "adacompliance",
                            "amenitytabid" : 1057,
                            "fieldtype" : "Yes/No",
                            "numeric" : false,
                            "shortname" : "aa",
                            "array" : false,
                            "fieldid" : 476,
                            "typeid" : 11,
                            "uniquename" : "adacompliance_aa"
                        }, 
                        {
                            "sortorder" : 4,
                            "label" : "Number of Wheelchair Accessible Restrooms",
                            "amenitygroupid" : 143,
                            "tabshortname" : "adacompliance",
                            "amenitytabid" : 1057,
                            "fieldtype" : "Number",
                            "numeric" : true,
                            "shortname" : "whl",
                            "array" : false,
                            "fieldid" : 481,
                            "typeid" : 4,
                            "uniquename" : "adacompliance_whl"
                        }, 
                        {
                            "description" : "Does this property have access for guest in wheelchairs? ",
                            "sortorder" : 7,
                            "label" : "Wheelchair Accessible",
                            "amenitygroupid" : 143,
                            "tabshortname" : "adacompliance",
                            "amenitytabid" : 1057,
                            "fieldtype" : "Yes/No",
                            "numeric" : false,
                            "shortname" : "wc",
                            "array" : false,
                            "fieldid" : 473,
                            "typeid" : 11,
                            "uniquename" : "adacompliance_wc"
                        }
                    ]
                }, 
                {
                    "amenitytabid" : 1057,
                    "sortorder" : 18,
                    "amenitygroupname" : "Communication Services",
                    "amenitygroupid" : 145,
                    "fields" : [ 
                        {
                            "description" : "Do Televisions and Monitors provide Closed Caption Services?",
                            "sortorder" : 1,
                            "label" : "Closed Captioning",
                            "amenitygroupid" : 145,
                            "tabshortname" : "adacompliance",
                            "amenitytabid" : 1057,
                            "fieldtype" : "Yes/No",
                            "numeric" : false,
                            "shortname" : "bvis",
                            "array" : false,
                            "fieldid" : 477,
                            "typeid" : 11,
                            "uniquename" : "adacompliance_bvis"
                        }, 
                        {
                            "sortorder" : 2,
                            "label" : "TTY Telephones",
                            "amenitygroupid" : 145,
                            "tabshortname" : "adacompliance",
                            "amenitytabid" : 1057,
                            "fieldtype" : "Yes/No",
                            "numeric" : false,
                            "shortname" : "tty",
                            "array" : false,
                            "fieldid" : 482,
                            "typeid" : 11,
                            "uniquename" : "adacompliance_tty"
                        }
                    ]
                }, 
                {
                    "amenitytabid" : 1057,
                    "sortorder" : 82,
                    "amenitygroupname" : "Public Accommodations",
                    "amenitygroupid" : 144,
                    "fields" : [ 
                        {
                            "description" : "Does the property provide access to blind and visually impaired guests?",
                            "sortorder" : 1,
                            "label" : "Blind and Visually Impaired Services",
                            "amenitygroupid" : 144,
                            "values" : [ 
                                {
                                    "listid" : 143,
                                    "sortorder" : 1,
                                    "listvalue" : "Large Print Format Material"
                                }, 
                                {
                                    "listid" : 144,
                                    "sortorder" : 2,
                                    "listvalue" : "Verbal Instructions"
                                }, 
                                {
                                    "listid" : 145,
                                    "sortorder" : 3,
                                    "listvalue" : "Brail Materials"
                                }
                            ],
                            "tabshortname" : "adacompliance",
                            "amenitytabid" : 1057,
                            "fieldtype" : "Multi-Select",
                            "numeric" : false,
                            "shortname" : "bvi",
                            "array" : true,
                            "fieldid" : 474,
                            "typeid" : 12,
                            "uniquename" : "adacompliance_bvi"
                        }, 
                        {
                            "description" : "Is wheelchair accessible transportation available?",
                            "sortorder" : 2,
                            "label" : "Wheelchair Transportation",
                            "amenitygroupid" : 144,
                            "tabshortname" : "adacompliance",
                            "amenitytabid" : 1057,
                            "fieldtype" : "Yes/No",
                            "numeric" : false,
                            "shortname" : "wct",
                            "array" : false,
                            "fieldid" : 478,
                            "typeid" : 11,
                            "uniquename" : "adacompliance_wct"
                        }
                    ]
                }
            ]
        }
    ],
    "singleton" : true
}
```
