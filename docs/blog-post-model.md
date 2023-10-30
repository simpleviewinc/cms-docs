## Blog Post Model
  - [Blog Post Fields](#blog-post-fields)
  - [Example Blog Post Endpoint URLs](#example-blog-post-endpoint-urls)
  - [Example Blog Post Data](#example-blog-post-data)

Blog Posts are where blog page content is stored. Multiple blogs can be present on a site and the individual endpoints make use of the individual blog identifier, e.g. articles, leisure_blog.

### Blog Post Fields

**Model: `plugins_blog_[blog_identifier]_posts`**

- `title` - `String`
- `slug` - `String`
- `image_id` - `ObjectId`
- `enabled` - `Boolean`
- `active` - `Boolean`
- `publish_start` - `Date`
- `created` - `Date`
- `updated` - `Date`
- `publish_start` - `Date`
- `publish_end` - `Date`
- `version_id` - `ObjectId`
- `description_raw` - `String` - HTML content for the post
- `teaser_raw` - `String` - Teaser excerpt for the post
- `url` - `String`
- `absoluteUrl` - `String`
- `blog` - `String` - the [blog_identifier] this post belongs to
- `open_graph_title` - `String`
- `open_graph_description` - `String`
- `meta_title` - `String`
- `meta_description` - `String`
- `metaTitle` - `String`
- `metaDescription` - `String`

### Main Blog Post Endpoint URLs

```
https://[client_identifier].simpleviewinc.com/includes/rest_v2/plugins_blog_[blog_identifier]_posts/find/?json=%7B%22options%22%3A%7B%22limit%22%3A%2010,%22fields%22%3A%7B%7D%7D%7D&token=c9946f31c4ea64b601cb3268feff0c8f
https://[client_identifier].simpleviewinc.com/includes/rest_v2/plugins_blog_[blog_identifier]_posts/count/?&token=c9946f31c4ea64b601cb3268feff0c8f
```


### Example Blog Post Endpoint URLs

```
https://rc.simpleviewinc.com/includes/rest_v2/plugins_blog_leisure_blog_posts/find/?json=%7B%22options%22%3A%7B%22limit%22%3A%2010,%22fields%22%3A%7B%7D%7D%7D&token=c9946f31c4ea64b601cb3268feff0c8f
https://rc.simpleviewinc.com/includes/rest_v2/plugins_blog_leisure_blog_posts/count/?&token=c9946f31c4ea64b601cb3268feff0c8f
```

### Example Blog Post Data

```json
{
      "_id": "601c874d4966cb17dfb5b2cb",
      "author_id": "5ef651937ec41b1002f28d61",
      "title": "10 Most Instagrammable Spots",
      "slug": "most-instagrammable-spots1",
      "image_id": "5ea88f805c34192cb9e779b0",
      "meta_title": "Simpleville's Best Spots for Instagram Selfies",
      "meta_description": "Find your next selfie pic that's guaranteed some extra hearts.",
      "open_graph_title": "Discover Your Best Instagram Spot in Simpleville",
      "open_graph_description": "Is your feed looking for a boost? Come discover the best spots for your next selfie to get those extra hearts you crave.",
      "publish_start": "2021-02-04T23:00:00.000Z",
      "enable_comments": false,
      "categories_ids": [
        "5ea0a6c65c34192cb9e776d1",
        "5f6b83081d55b75572f04052"
      ],
      "tags_ids": [
        "5ea0a6e25c34192cb9e776d4",
        "5ea0a6b35c34192cb9e776cb"
      ],
      "cms_tags_ids": [
        "5ea0a6ea5c34192cb9e776dd",
        "5ea0a6e75c34192cb9e776d7",
        "5ea0a6e95c34192cb9e776da"
      ],
      "enabled": true,
      "blog": "leisure_blog",
      "_title_sort": "10 most instagrammable spots",
      "updated": "2021-02-04T23:46:26.679Z",
      "active": true,
      "created": "2021-02-04T23:46:21.716Z",
      "description_raw": "<p>Lorem ipsum dolor sit amet, consectetuer adipiscing elit. Aenean commodo ligula eget dolor. Aenean massa. Cum sociis natoque penatibus et magnis dis parturient montes, nascetur ridiculus mus. Donec quam felis, ultricies nec, pellentesque eu, pretium quis, sem. Nulla consequat massa quis enim. Donec pede justo, fringilla vel, aliquet nec, vulputate eget, arcu. In enim justo, rhoncus ut, imperdiet a, venenatis vitae, justo. Nullam dictum felis eu pede mollis pretium.</p>\r\n\r\n<p>Integer tincidunt. Cras dapibus. Vivamus elementum semper nisi. Aenean vulputate eleifend tellus. Aenean leo ligula, porttitor eu, consequat vitae, eleifend ac, enim. Aliquam lorem ante, dapibus in, viverra quis, feugiat a, tellus. Phasellus viverra nulla ut metus varius laoreet. Quisque rutrum. Aenean imperdiet. Etiam ultricies nisi vel augue. Curabitur ullamcorper ultricies nisi. Nam eget dui. Etiam rhoncus. Maecenas tempus, tellus eget condimentum rhoncus, sem quam semper libero, sit amet adipiscing sem neque sed ipsum. Nam quam nunc, blandit vel, luctus pulvinar, hendrerit id, lorem. Maecenas nec odio et ante tincidunt tempus. Donec vitae sapien ut libero venenatis faucibus. Nullam quis ante. Etiam sit amet orci eget eros faucibus tincidunt. Duis leo. Sed fringilla mauris sit amet nibh. Donec sodales sagittis magna. Sed consequat, leo eget bibendum sodales, augue velit cursus nunc.</p>\r\n <p>Quis gravida magna mi a libero. Fusce vulputate eleifend sapien. Vestibulum purus quam, scelerisque ut, mollis sed, nonummy id, metus. Nullam accumsan lorem in dui. Cras ultricies mi eu turpis hendrerit fringilla. Vestibulum ante ipsum primis in faucibus orci luctus et ultrices posuere cubilia Curae; In ac dui quis mi consectetuer lacinia. Nam pretium turpis et arcu. Duis arcu tortor, suscipit eget, imperdiet nec, imperdiet iaculis, ipsum. Sed aliquam ultrices mauris. Integer ante arcu, accumsan a, consectetuer eget, posuere ut, mauris. Praesent adipiscing.</p>\r\n\r\n<p>Phasellus ullamcorper ipsum rutrum nunc. Nunc nonummy metus. Vestibulum volutpat pretium libero. Cras id dui. Aenean ut eros et nisl sagittis vestibulum. Nullam nulla eros, ultricies sit amet, nonummy id, imperdiet feugiat, pede. Sed lectus. Donec mollis hendrerit risus. Phasellus nec sem in justo pellentesque facilisis. Etiam imperdiet imperdiet orci. Nunc nec neque. Phasellus leo dolor, tempus non, auctor et, hendrerit quis, nisi. Curabitur ligula sapien, tincidunt non, euismod vitae, posuere imperdiet, leo. Maecenas malesuada. Praesent congue erat at massa. Sed cursus turpis vitae tortor. Donec posuere vulputate arcu. Phasellus accumsan cursus velit.</p>\r\n\r\n<p>Vestibulum ante ipsum primis in faucibus orci luctus et ultrices posuere cubilia Curae; Sed aliquam, nisi quis porttitor congue, elit erat euismod orci, ac placerat dolor lectus quis orci. Phasellus consectetuer vestibulum elit. Aenean tellus metus, bibendum sed, posuere ac, mattis non, nunc. Vestibulum fringilla pede sit amet augue. In turpis. Pellentesque posuere. Praesent turpis. Aenean posuere, tortor sed cursus feugiat, nunc augue blandit nunc, eu sollicitudin urna dolor sagittis lacus. Donec elit libero, sodales nec, volutpat augue.</p>\r\n\r\n<p>Suscipit non, turpis. Nullam sagittis. Suspendisse pulvinar, augue ac venenatis condimentum, sem libero volutpat nibh, nec pellentesque velit pede quis nunc. Vestibulum ante ipsum primis in faucibus orci luctus et ultrices posuere cubilia Curae; Fusce id purus. Ut varius tincidunt libero. Phasellus dolor. Maecenas vestibulum mollis diam. Pellentesque ut neque. Pellentesque habitant morbi tristique senectus et netus et malesuada fames ac turpis egestas. In dui magna, posuere eget, vestibulum et, tempor auctor, justo. In ac felis quis tortor malesuada pretium. Pellentesque auctor neque nec urna. Proin sapien ipsum, porta a, auctor quis, euismod ut, mi. Aenean viverra rhoncus pede.</p>\r\n <p>Pellentesque habitant morbi tristique senectus et netus et malesuada fames ac turpis egestas. Ut non enim eleifend felis pretium feugiat. Vivamus quis mi. Phasellus a est. Phasellus magna. In hac habitasse platea dictumst. Curabitur at lacus ac velit ornare lobortis. Curabitur a felis in nunc fringilla tristique. Morbi mattis ullamcorper velit. Phasellus gravida semper nisi. Nullam vel sem. Pellentesque libero tortor, tincidunt et, tincidunt eget, semper nec, quam. Sed hendrerit. Morbi ac felis. Nunc egestas, augue at pellentesque laoreet, felis eros vehicula leo, at malesuada velit leo quis pede.</p>\r\n\r\n<p>Donec interdum, metus et hendrerit aliquet, dolor diam sagittis ligula, eget egestas libero turpis vel mi. Nunc nulla. Fusce risus nisl, viverra et, tempor et, pretium in, sapien. Donec venenatis vulputate lorem. Morbi nec metus. Phasellus blandit leo ut odio. Maecenas ullamcorper, dui et placerat feugiat, eros pede varius nisi, condimentum viverra felis nunc et lorem. Sed magna purus, fermentum eu, tincidunt eu, varius ut, felis. In auctor lobortis lacus. Quisque libero metus, condimentum nec, tempor a, commodo mollis, magna. Vestibulum ullamcorper mauris at ligula. Fusce fermentum. Nullam cursus lacinia erat. Praesent blandit laoreet nibh.</p>\r\n\r\n<p>Fusce convallis metus id felis luctus adipiscing. Pellentesque egestas.</p>\r\n\r\n<blockquote>\r\n<p>Neque sit amet convallis pulvinar, justo nulla eleifend augue, ac auctor orci leo non est. Quisque id mi. Ut tincidunt tincidunt erat. Etiam feugiat lorem non metus. Vestibulum dapibus nunc ac augue. Curabitur vestibulum aliquam leo. Praesent egestas neque eu enim. In hac habitasse platea dictumst. Fusce a quam. Etiam ut purus mattis mauris sodales aliquam. Curabitur nisi. Quisque malesuada placerat nisl. Nam ipsum risus, rutrum vitae, vestibulum eu, molestie vel, lacus. Sed augue ipsum, egestas nec, vestibulum et, malesuada adipiscing, dui.</p>\r\n</blockquote>\r\n\r\n<p>Vestibulum facilisis, purus nec pulvinar iaculis, ligula mi congue nunc, vitae euismod ligula urna in dolor. Mauris sollicitudin fermentum libero. Praesent nonummy mi in odio. Nunc interdum lacus sit amet orci. Vestibulum rutrum, mi nec elementum vehicula, eros quam gravida nisl, id fringilla neque ante vel mi. Morbi mollis tellus ac sapien. Phasellus volutpat, metus eget egestas mollis, lacus lacus blandit dui, id egestas quam mauris ut lacus. Fusce vel dui. Sed in libero ut nibh placerat accumsan. Proin faucibus arcu quis ante. In consectetuer turpis ut velit. Nulla sit amet est. Praesent metus tellus, elementum eu, semper a, adipiscing nec, purus.</p>\r\n\r\n<p>Cras risus ipsum, faucibus ut, ullamcorper id, varius ac, leo. Suspendisse feugiat. Suspendisse enim turpis, dictum sed, iaculis a, condimentum nec, nisi. Praesent nec nisl a purus blandit viverra. Praesent ac massa at ligula laoreet iaculis. Nulla neque dolor, sagittis eget, iaculis quis, molestie non, velit. Mauris turpis nunc, blandit et, volutpat molestie, porta ut, ligula. Fusce pharetra convallis urna. Quisque ut nisi. Donec mi odio, faucibus at, scelerisque quis, convallis in, nisi. Suspendisse non nisl sit amet velit hendrerit rutrum. Ut leo. Ut a nisl id ante tempus hendrerit. Proin pretium, leo ac pellentesque mollis, felis nunc ultrices eros, sed gravida augue augue mollis justo.</p>\r\n\r\n<p>Suspendisse eu ligula. Nulla facilisi. Donec id justo. Praesent porttitor, nulla vitae posuere iaculis, arcu nisl dignissim dolor, a pretium mi sem ut ipsum. Curabitur suscipit suscipit tellus. Praesent vestibulum dapibus nibh. Etiam iaculis nunc ac metus. Ut id nisl quis enim dignissim sagittis. Etiam sollicitudin, ipsum eu pulvinar rutrum, tellus ipsum laoreet sapien, quis venenatis ante odio sit amet eros. Proin magna. Duis vel nibh at velit scelerisque suscipit. Curabitur turpis. Vestibulum suscipit nulla quis orci. Fusce ac felis sit amet ligula pharetra condimentum. Maecenas egestas arcu quis ligula mattis placerat. Duis lobortis massa imperdiet quam. Suspendisse potenti.</p>\r\n\r\n<p>Pellentesque commodo eros a enim. Vestibulum turpis sem, aliquet eget, lobortis pellentesque, rutrum eu, nisl. Sed libero. Aliquam erat volutpat. Etiam vitae tortor. Morbi vestibulum volutpat enim. Aliquam eu nunc. Nunc sed turpis. Sed mollis, eros et ultrices tempus, mauris ipsum aliquam libero, non adipiscing dolor urna a orci. Nulla porta dolor. Class aptent taciti sociosqu ad litora torquent per conubia nostra, per inceptos hymenaeos. Pellentesque dapibus hendrerit tortor. Praesent egestas tristique nibh. Sed a libero. Cras varius. Donec vitae orci sed dolor rutrum auctor. Fusce egestas elit eget lorem. Suspendisse nisl elit, rhoncus eget, elementum ac, condimentum eget, diam. Nam at tortor in tellus interdum sagittis.</p>\r\n",
      "teaser_raw": "<p>Lorem ipsum dolor sit amet, consectetuer adipiscing elit. Aenean commodo ligula eget dolor. Aenean massa. Cum sociis natoque penatibus et magnis dis parturient montes, nascetur ridiculus mus. Donec quam felis, ultricies nec, pellentesque eu, pretium quis, sem. Nulla consequat massa quis enim. Donec</p>",
      "version_id": "601c87524966cb17dfb5b2df",
      "publish_start_moment": "2021-02-04T23:00:00.000Z",
      "publish_end_moment": "2023-08-24T14:51:38.265Z",
      "id": "601c874d4966cb17dfb5b2cb",
      "url": "/blog/post/most-instagrammable-spots1/",
      "commentsUrl": "/blog/post/most-instagrammable-spots1/#comments",
      "absoluteUrl": "https://rc.simpleviewinc.com/blog/post/most-instagrammable-spots1/",
      "amp_url": "https://rc.simpleviewinc.com/blog/post/most-instagrammable-spots1/amp/",
      "metaTitle": "Simpleville's Best Spots for Instagram Selfies",
      "metaDescription": "Find your next selfie pic that's guaranteed some extra hearts."
}
```