# Listings (Apex version)

***

**Heads up**
This is still in progress and some data is incomplete or incorrect.

***

- [Listings API](#listings-api)
	- [Listings Endpoints](#listings-endpoints)
 	- [Example Listings Queries](#example-listings-queries)
	- [Listings Fields](#listings-fields)
   	- [Example Listings Docs](#example-listings-docs)
- [Properties API](#properties-api)
	- [Properties Endpoints](#properties-endpoints)
	- [Example Properties Queries](#example-properties-queries)
	- [Properties Fields](#properties-fields)
   	- [Example Properties Docs](#example-properties-docs)

## Listings API

**Model: `plugins_listings_listings`**

This is the main listings database table. For the most part, data is provided exactly as it comes from Apex, but a few CMS-specific fields are added, such as `last_sync`, `detail_url`, etc.

### Listings Endpoints:

Note: all examples are provided using "rc-apex.simpleviewinc.com", which is a website
with sample data. You'll obviously want your own data, so substitute it with
your own website's URL.

```
- https://[client_identifier].simpleviewinc.com/includes/rest_v2/plugins_listings_listings/find/
- https://[client_identifier].simpleviewinc.com/includes/rest_v2/plugins_listings_listings/count/
- https://[client_identifier].simpleviewinc.com/includes/rest_v2/plugins_listings_listings/aggregate/
```

### Listings Fields

* `listing_id` - `String` - unique identifier for this listing
* `_id` - `ObjectId` - internal identifier not intended for general use. Use `listing_id` instead, as `_id` can change.
* `id` - `String` - String representation of the `_id` field, also not intended for general use. Use `listing_id` instead, as `id` can change. 
* `title` - `String` - listing/product name
* `title_sort` - `String` - normalized lowercase version of the title used for case-insensitive filtering/sorting operations
* `description` - `String` - full product/listing description
* `keywords` - `Array[String]`
* `is_featured` - `Boolean`
* `summary_url` - `String` - URL for listing summary page in Apex
* `post_from_at` - `Date` - date/time from which a listing can be posted
* `created_at` - `Date` - Date the listing was created in Apex
* `last_updated_at` - `Date` - Date the listing was last updated in Apex
* `is_deleted` - `Boolean` - indicates if the listings has been deleted in Apex
* `subcategory_ids` - `Array[String]`
* `media` - `Array[Object]`
  * `account_media_id` - `String`
  * `related_id` - `String` 
  * `type` - `String` - what type of media file this is, e.g. "image" 
  * `title` - `String`
  * `url` - `String`  
  * `sort_order` - `Number` - number indicating the sort position of this item in the media array
  * `created_at` - `Date`
  * `last_updated_at` - `Date`
* `rank_id` - `String`
* `property_id` - `String` - ID for the property this listing is associated with
* `property` - `Object` - The listing's property (see Properties model below for specification)
* `channel_ids` - `Array[String]` - channels this listing is assigned to
* `last_sync` - `Date` - date the listing was last synced from Apex
* `active` - `Boolean` - indicates whether the listing should be shown on any sites. Typically, this will be `false` if a listing has been assigned to no sites, based on channel to site mappings.
* `sites` - `Array[String]` - a list of all the sites this listing is available on
* `primary_site` - `String` - The name of the main site this listing is displayed on. Most commonly, our clients have a single site named "primary" (or similar). Listings can be displayed on multiple sites (see `sites`) but one site is considered its canonical version.
* `rankorder` - `Number` - indicates the sort position of this listing based on its ranking in Apex
* `filter_tags` - `Array[String]` - tags used for optimized filtering on site, categories, subcategories, and other criteria. Examles will be provided below. **This is an important field for making performant queries.**
* `created` - `Date` - indicates when the listing record was created **in CMS**.
* `detail_url` - `String`
* `generic_url` - `String`
* `url` - `String`
* `absolute_url` - `String`
* `absolute_primary_url` - `String`


### Example Listings Queries

#### Example 1: Query by `listing_id`

Find a single listing by its `listing_id`, and return ONLY the `listing_id` and `title` fields. Note 
that the `castDocs: false` and limited `fields` keeps the returned data very concise. This is a bit
of an extreme/contrived example, but is meant to showcase these features.

```javascript
const query = {
	filter: {
 		listing_id: 41849
   	},
    	options: {
     		castDocs: false,
		fields: {
  			listing_id: 1,
     			title: 1
		}
	}
};

$.get("/includes/rest_v2/plugins_listings_listings/find/", {
  json : JSON.stringify(query),
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
      "listing_id": 41849,
      "title": "Simpleville Pizzeria"
    }
  ]
}
```


#### Example 2: Query by site and a specific subcategory

This example shows how `filter_tags` can be used to query by both site and subcategory. This query is very efficient because `filter_tags` is an indexed field in the database. Most of your queries should use at least one filter tag as the **first** filter parameter (the order of filter parameters counts).

This query also passes a `limit` to specify the maximum number of items to be returned.

```javascript
const query = {
	filter_tags: {
		$in: ['site_primary_subcatid_161']
	},
 	options: {
  		castDocs: false,
    		fields: {
      			listing_id: 1,
			title: 1
   		},
     		limit: 3
	}
};

$.get("/includes/rest_v2/plugins_listings_listings/find/", {
  json : JSON.stringify(query),
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
      "listing_id": 5,
      "title": "Maritime Tours AS"
    },
    {
      "listing_id": 8,
      "title": "Voss Active"
    },
    {
      "listing_id": 12,
      "title": "The Whaling Museum"
    }
  ]
}
```


#### Find a randomly selected listing and include the property data

```javascript
const query = {
	filter: {
		filter_tags: {
			$in: ['site_primary']
		},
	},
	options: {
		castDocs: false,
		fields : {
			listing_id: 1,
			title: 1,
			detail_url: 1
		},
		random : 1,
		hooks: [
			"afterFind_property"
		]
	}
};

$.get("/includes/rest_v2/plugins_listings_listings/find/", {
  json : JSON.stringify(query),
  token : "d2f5ba5eaab52cb4ce80280b5afffdd8"
}, function(data) {
  console.log(data);
});
```

**Response:**
```json

```



### Example Listings Docs

#### Example 1

```json
{
	"_id": "671bc3532bf24b4afcc730f1",
	"listing_id": "20",
	"title": "Potomac River Boat Company",
	"description": "For more than 30 years Potomac Riverboat Company has been offering sightseeing tours up and down the Potomac River.Both open and chartered tours are available at the Potomac Riverboat Company on a variety of different vessels, such as the Cherry Blossom, a re-creation of a 19th century Victorian riverboat and several double-deckers.",
	"is_featured": false,
	"summary_url": "/listings/summary/20/",
	"post_from_at": "2023-01-01T07:00:00.000Z",
	"listing_public_id": "dms://models/listings_public/20",
	"ui_label": "Potomac River Boat Company",
	"created_at": "2024-10-23T18:29:41.140Z",
	"last_updated_at": "2024-10-23T18:29:41.140Z",
	"is_deleted": false,
	"subcategories_ids": [
	  "7",
	  "17"
	],
	"rank_id": "2",
	"property_id": "20",
	"channels_ids": [
	  "1",
	  "3",
	  "4"
	],
	"last_sync": "2024-10-25T16:12:03.938Z",
	"title_sort": "potomac river boat company",
	"ui_label_sort": "potomac river boat company",
	"active": true,
	"sites": [
	  "primary"
	],
	"primary_site": "primary",
	"rank_order": 2,
	"filter_tags": [
	  "site_primary",
	  "catid_3",
	  "subcatid_7",
	  "site_primary_catid_3",
	  "site_primary_subcatid_7",
	  "site_primary_catid_3_subcatid_7",
	  "catid_6",
	  "subcatid_17",
	  "site_primary_catid_6",
	  "site_primary_subcatid_17",
	  "site_primary_catid_6_subcatid_17"
	],
	"created": "2024-10-25T16:12:03.947Z",
	"id": "671bc3532bf24b4afcc730f1",
	"detail_url": "/listing/potomac-river-boat-company/20/",
	"generic_url": "/listing/potomac-river-boat-company/20/",
	"url": "/listing/potomac-river-boat-company/20/",
	"absolute_url": "https://primary-rc-apex.qa.simpleviewcms.com/listing/potomac-river-boat-company/20/",
	"absolute_primary_url": "https://primary-rc-apex.qa.simpleviewcms.com/listing/potomac-river-boat-company/20/"
}
```

#### Example 2

```json
{
        "_id": "671bc3532bf24b4afcc73129",
        "listing_id": "27",
        "title": "Hiland Terrace Motel",
        "description": "In Irwin (North Huntingdon) With a stay at Hiland Terrace Hotel in Irwin (North Huntingdon), you'll be within 14.4 mi (23.1 km) of Carnegie Mellon University and 14.8 mi (23.8 km) from University of Pittsburgh. Make yourself at home in one of the 12 air-conditioned guestrooms. Complimentary wireless internet access keeps you connected, and cable programming is available for your entertainment. Bathrooms have shower/tub combinations and complimentary toiletries. ",
        "is_featured": false,
        "summary_url": "/listings/summary/27/",
        "post_from_at": "2023-01-01T07:00:00.000Z",
        "listing_public_id": "dms://models/listings_public/27",
        "ui_label": "Hiland Terrace Motel",
        "created_at": "2024-10-23T18:29:41.140Z",
        "last_updated_at": "2024-10-23T18:29:41.140Z",
        "is_deleted": false,
        "subcategories_ids": [
          "1"
        ],
        "rank_id": "2",
        "property_id": "27",
        "channels_ids": [
          "1"
        ],
        "last_sync": "2024-10-25T16:12:03.939Z",
        "title_sort": "hiland terrace motel",
        "ui_label_sort": "hiland terrace motel",
        "active": false,
        "rank_order": 2,
        "created": "2024-10-25T16:12:03.947Z",
        "id": "671bc3532bf24b4afcc73129",
        "detail_url": "/listing/hiland-terrace-motel/27/",
        "generic_url": "/listing/hiland-terrace-motel/27/",
        "url": "/listing/hiland-terrace-motel/27/",
        "absolute_url": "https://primary-rc-apex.qa.simpleviewcms.com/listing/hiland-terrace-motel/27/"
}
```

## Properties API

**Model: `plugins_listings_properties`**

Every listing is associated with a single property. A property may be associated with one or many listings, and holds data shared between all its associated listings.

While the properties model has its own API and endpoints, it's possible to use the "afterFind_property" hook when querying the listings API to include each listing's most relevant property data. See the provided examples.

Like listings, most of this data is provided exactly as it comes from Apex.


### Properties Endpoints:

Note: all examples are provided using "rc-apex.simpleviewinc.com", which is a website
with sample data. You'll obviously want your own data, so substitute it with
your own website's URL.

```
- https://[client_identifier].simpleviewinc.com/includes/rest_v2/plugins_listings_properties/find/
- https://[client_identifier].simpleviewinc.com/includes/rest_v2/plugins_listings_properties/count/
- https://[client_identifier].simpleviewinc.com/includes/rest_v2/plugins_listings_properties/aggregate/
```

### Properties Fields

* `property_id` - `String` - unique identifier for this property
* `_id` - `ObjectID` - internal identifier not intended for general use. Use `property_id` instead, as `_id` can change.
* `id` - `String` - plain string representation of the `_id` field, also not for general use. Use the `property_id` field instead, as `id` can change.
* `name` - `String`
* `name_sort` - `String` - normalized lowercase version of the `name` for case-insensitive sorting/filtering operations
* `weburl` - `String` - the URL for the property's external website
* `brand` - `String`
* `accessible_parking_spaces` - `Number`
* `health_and_safety_guidelines` - `String`
* `number_of_floors` - `Number`
* `facilities` - `String`
* `email` - `String`
* `geo_coordinates` - `Object`
	* `type` - `String`
	* `coordinates` - `Array[Numbers]`
* `opentable_id` - `String`
* `tripadvisor_id` - `String`
* `yelp_id` - `String`
* `viator_id` - `String`
* `fareharbor_id` - `String`
* `txusa_id` - `String`
* `threshold360_id` - `String`
* `bandwango_id` - `String`
* `ticketmaster_id` - `String`
* `social_facebook` - `String`
* `social_instagram` - `String`
* `social_youtube` - `String`
* `social_x` - `String`
* `social_linkedin` - `String`
* `social_tiktok` - `String`
* `summary_url` - `String` - URL for this property's summary page in Apex.
* `ui_label` - `String`
* `created_at` - `Date` - when the property was created in Apex
* `last_updated_at` - `Date` - when the property was last updated in Apex
* `is_deleted` - `Boolean` - indicates whether this property has been deleted in Apex
* `amenities` - `Array[Object]`
	* `core_amenity_id` - `String` - core amenity slug
	* `label` - `String` - amenity name/label text
	* `is_deleted` - `Boolean` - whether the amenity has been deleted in Apex
	* `amenity` - `Object`
		* `amenity_id` - `String` - ID for this amenity
	* `amenity_type` - `Object`
		* `amenity_type_id` - `String`
		* `label` - `String`
* `health_and_safety_booklets` - `Array[Object]`
	* `file_id` - `String`
	* `filestorage_id` - `String`
	* `guid` - `String`
	* `name` - `String`
	* `is_public` - `Boolean`
	* `url` - `String`
	* `public_url` - `String`
	* `private_url` - `String`
	* `file_size` - `Number`
	* `is_image` - `Boolean`
	* `is_deleted` - `Boolean`
* `properties_normalized` - `Object` - contains data for this property which has been formatted for general use
	* `property_address` - `Object`
		* `address_line_1` - `String`
		* `address_line_2` - `String`
		* `city` - `String`
		* `postal_code` - `String`
		* `state` - `Object`
			* `state_id` - `String` - full state identifier including country, such as "US_AZ for Arizona or "CA_ON" for Ontario Canada
			* `code` - `String` - short state code, such as "CA" for California or "ON" for Ontario
			* `country` - `Object`
				* `country_id` - `String` - full country identifier, such as "US" for United States or "CA" for Canada
				* `code` - `String` - country code, such as "US" or "CA" for Canada
				* `name` - `String` - name of country, such as "United States" or "Candada"
		* `country` - `Object`
			* `country_id` - `String`
			* `code` - `String`
			* `name` - `String`
		* `region` - `Object`
			* `region_id` - `String` - identifier for this properties region
	* `property_phones` - `Array[Object]`
		* `phone_id` - `Number`
		* `phone_number` - `String`
		* `phone_extension` - `String`
		* `is_primary` - `Boolean`
		* `phone_type`
			* `phone_type_id` - `String`
			* `label` - `String`
			* `is_deleted` - `Boolean`

* `account_id` - `String` - ID for the account this property is associated with
* `property_type_id` - `String`
* `parking_ids` - `Array[String]`
* `languages_spoken_ids` - `Array[String]`
* `facility_types_ids` - `Array[String]`
* `dining_facility_ids`
* `accomodation_facility_id` - `String`
* `sports_facility_ids`
* `region_id` - `String`
* `neighborhood_id` - `String`
* `listings_ids` - `Array[String]` - ids for all listings at this property
* `latitude`
* `longitude`
* `last_sync` - `Date` - when this property was last synced from Apex
* `created` - `Date` - when this property record was created in CMS

### Example Properties Queries

#### Example 1: Query by `property_id`

```javascript
const query = {
	filter: {
 		property_id: "9"
   	},
    	options: {
     		castDocs: false,
		fields: {
  			property_id: 1,
     			name: 1,
			weburl: 1
		}
	}
};

$.get("/includes/rest_v2/plugins_listings_properties/find/", {
  json : JSON.stringify(query),
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
      "property_id": "9",
      "name": "Hacienda Del Sol Guest Ranch Resort",
      "weburl": "https://www.haciendadelsol.com/?utm_source=google%20my%20business&utm_medium=listing&utm_campaign=visit%20website"
    }
  ]
}
```


### Example Properties Docs

```json
{
  "docs": [
    {
      "_id": "671bc3552bf24b4afcc73435",
      "property_id": "5",
      "name": "A Cottage Property",
      "summary_url": "/properties/summary/5/",
      "ui_label": "A Cottage Property",
      "created_at": "2024-10-23T18:29:40.814Z",
      "last_updated_at": "2024-10-23T18:29:40.814Z",
      "is_deleted": false,
      "properties_normalized": {
        "property_id": "5",
        "property_address": {
          "address_id": "108",
          "address_line_1": "1723 Fort Hill Rd",
          "city": "Fort Hill",
          "postal_code": "29715",
          "is_physical": true,
          "is_shipping": false,
          "is_billing": false,
          "created_at": "2024-10-23T18:29:40.543Z",
          "last_updated_at": "2024-10-23T18:29:40.543Z",
          "state": {
            "state_id": "US_SC",
            "code": "SC",
            "name": "South Carolina",
            "country": {
              "country_id": "US",
              "code": "US",
              "name": "United States"
            }
          },
          "country": {
            "country_id": "US",
            "code": "US",
            "name": "United States"
          }
        },
        "region": {
          "region_id": "2"
        }
      },
      "account_id": "115",
      "property_type_id": "1",
      "listings_ids": [
        "5"
      ],
      "last_sync": "2024-10-25T16:12:05.874Z",
      "name_sort": "a cottage property",
      "ui_label_sort": "a cottage property",
      "created": "2024-10-25T16:12:05.884Z",
      "id": "671bc3552bf24b4afcc73435"
    },


    {
      "_id": "671bc3552bf24b4afcc735bb",
      "property_id": "49",
      "name": "Saguaro Dude Ranch",
      "facilities": "Accommodation",
      "summary_url": "/properties/summary/49/",
      "ui_label": "Saguaro Dude Ranch",
      "created_at": "2024-10-23T18:29:40.814Z",
      "last_updated_at": "2024-10-23T18:29:40.814Z",
      "is_deleted": false,
      "properties_normalized": {
        "property_id": "49",
        "total_sleeping_rooms": 42,
        "property_address": {
          "address_id": "45",
          "address_line_1": "5896 Bumblebee Rd",
          "city": "Bumblebee ",
          "postal_code": "85324",
          "is_physical": true,
          "is_shipping": true,
          "is_billing": true,
          "created_at": "2024-10-23T18:29:40.543Z",
          "last_updated_at": "2024-10-23T18:29:40.543Z",
          "state": {
            "state_id": "US_AZ",
            "code": "AZ",
            "name": "Arizona",
            "country": {
              "country_id": "US",
              "code": "US",
              "name": "United States"
            }
          },
          "country": {
            "country_id": "US",
            "code": "US",
            "name": "United States"
          }
        },
        "region": {
          "region_id": "7"
        }
      },
      "account_id": "52",
      "property_type_id": "1",
      "facility_types_ids": [
        "accommodation_facility"
      ],
      "accommodation_facility_id": "25",
      "region_id": "7",
      "neighborhood_id": "43",
      "listings_ids": [
        "49"
      ],
      "last_sync": "2024-10-25T16:12:05.877Z",
      "name_sort": "saguaro dude ranch",
      "ui_label_sort": "saguaro dude ranch",
      "created": "2024-10-25T16:12:05.889Z",
      "id": "671bc3552bf24b4afcc735bb"
    }
  ]
}
```

