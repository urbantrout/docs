`craft.tags`
============

You can access your site’s [tags](en/tags.md) from your templates via `craft.tags`.

```twig
{% for tag in craft.tags.group('blogTags').all() %}
    <li><a href="{{ url('blog/tags/'~tag.id) }}">{{ tag.title }}</a></li>
{% endfor %}
```

## Parameters

`craft.tags` supports the following parameters:

### `fixedOrder`

If set to `true`, tags will be returned in the same order as the IDs entered in the [`id`](#id) param.

### `group`

Only fetch tags that belong to a given tag group(s), referenced by its handle.

### `groupId`

Only fetch tags that belong to a given tag group(s), referenced by its ID.

### `id`

Only fetch the tag with the given ID(s).

### `indexBy`

Indexes the results by a given property. Possible values include `'id'` and `'title'`.

### `limit`

Limits the results to *X* tags. Defaults to `null` (no limit) if not specified.

### `site`

The site the tags should be returned in. (Defaults to the current site.)

### `offset`

Skips the first *X* tags. For example, if you set `offset(1)`, the would-be second asset returned becomes the first.

### `orderBy`

The order the tags should be returned in. Possible values include `'title'`, `'id'`, `'groupId'`, `'dateCreated'`, and `'dateUpdated'`, as well as any textual custom field handles. If you want the entries to be sorted in descending order, add “`desc`” after the property name (ex: `'title desc'`). The default value is `'title asc'`.

### `relatedTo`

Only fetch tags that are related to certain other elements. (See [Relations](en/relations.md) for the syntax options.)

### `search`

Only fetch tags that match a given search query. (See {entry:docs/searching:link} for the syntax and available search attributes.)

### `slug`

Only fetch the tag with the given slug.

### `title`

Only fetch the tag with the given title(s).