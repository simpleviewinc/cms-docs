## Nav Model
  - [Nav Item Fields](#nav-item-fields)
  - [Example Nav Endpoint URLs](#example-nav-endpoint-urls)
  - [Example Nav Item Data](#example-nav-item-data)

Nav is where page data is stored.

### Main Nav Endpoints:

```
- https://[client_identifier].simpleviewinc.com/includes/rest_v2/plugins_nav_navItems/find/
- https://[client_identifier].simpleviewinc.com/includes/rest_v2/plugins_nav_navItems/count/
- https://[client_identifier].simpleviewinc.com/includes/rest_v2/plugins_nav_navItems/aggregate/
```

### Nav Item Fields

**Model: `plugins_nav_navItems`**

- `sortorder` - `Int`
- `title` - `String`
- `title_sort` - `String`
- `folder` - `String` - name (as shown in the url) for this nav item
- `section` - `String`
- `type` - `String` - page or link
- `active` - `Boolean`
- `show_on_nav` - `Boolean` - should the nav item be shown in the site navigation
- `publish_start` - `Date` - date the nav item is published
- `publish_end` - `Date` - date the nav item is no longer published
- `searchable` - `String` - yes|no|no_recursive
- `locale_code` - `String`
- `new_window` - `Boolean`
- `version_id` - `ObjectId`
- `created` - `Date`
- `site_name` - `String`
- `updated` - `Date`
- `site_section` - `String`
- `published` - `Boolean`
- `amp` - `Boolean`
- `cms_title` - `String`
- `cms_title_sort` - `String`
- `folderHref` - full path to this nav item including the nav item's own `folder` name
- `image_id` - `ObjectId`
- `description` - `String`
- `parent_id` - `ObjectID` - reference to this item's parent nav item (if any)
- `parent_ids` - `Array[ObjectId]` - array of references to all parent items
- `link`- `Object`
  - `id` - `ObjectId`
  - `type` - `String`
- `link_raw`- `Object`
  - `id` - `ObjectId`
  - `type` - `String`
- `meta_title` - `String`
- `meta_description` - `String`
- `login_type` - `String` - "extranet", "visitors_access_groups"
- `content_owner` - `String`
- `open_graph_title` - `String`
- `open_graph_description` - `String`
- `loc` - `Object` - geographic location data 
  - `coordinates` - `Array[Number]`

### Example Nav Endpoint URLs
```
https://[client_identifier].simpleviewinc.com/includes/rest_v2/plugins_nav_navitems/find/?json=%7B%22options%22%3A%7B%22limit%22%3A%2010,%22fields%22%3A%7B%7D%7D%7D&token=f19942375ec3b67599c82197385c8e19
```

### Example Nav Item Data

```json
{
    "_id" : ObjectId("60fb40f939620237f5292c93"),
    "sortorder" : 4,
    "title" : "Andrea's Test Page",
    "folder" : "andreas-test-page",
    "section" : "landing",
    "type" : "page",
    "active" : true,
    "show_on_nav" : false,
    "publish_start" : ISODate("2021-07-23T22:00:00.000Z"),
    "searchable" : "yes",
    "locale_code" : "en-us",
    "new_window" : false,
    "image_id" : ObjectId("60f98d5039620237f5292594"),
    "description" : "<p>Experience the Colorado Plateau!</p>",
    "meta_title" : "The Colorado Plateau | Experience the Adventure",
    "meta_description" : "Experience the diverse adventures available on the Colorado Plateau, from the Grand Canyon to Mesa Verde, rafing on the Colorado River or climbing Mt. Humphreys near Flagstaff, Arizona.",
    "tags_ids" : [ 
        ObjectId("60f98d3639620237f5292592"), 
        ObjectId("6112c793c55e925ce10b5a88")
    ],
    "personas_ids" : [ 
        "outdoors"
    ],
    "loc" : {
        "type" : "Point",
        "coordinates" : [ 
            -108.4158025, 
            37.3316426
        ]
    },
    "version_id" : ObjectId("615647842a0e23196e97a5a6"),
    "created" : ISODate("2021-07-23T22:21:45.786Z"),
    "content_owner" : "default",
    "site_name" : "primary",
    "updated" : ISODate("2021-09-30T23:25:56.093Z"),
    "title_sort" : "andrea's test page",
    "site_section" : "primary.landing",
    "published" : true,
    "amp" : false,
    "cms_title" : "Andrea's Test Page - /andreas-test-page/",
    "cms_title_sort" : "andrea's test page - /andreas-test-page/",
    "folderHref" : "/andreas-test-page/"
}
```
