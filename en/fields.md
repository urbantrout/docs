Fields
=======

All of the content on your site will get entered into fields.

Fields are defined globally from Settings → Fields. They are organized into “field groups”, but field groups have very little relevance anywhere else in the system.

All fields share a few common settings:

* The field group it belongs to
* Its name
* Its template-facing handle
* Instruction text

## Translatable Fields

If you’re running Craft Pro and you have more than one site locale, you will also have the option to mark the field as translatable.

If that setting is checked, the field’s values will be stored on a per-locale basis. Otherwise, the values will always be copied across all locales.

## Field Types

The final setting that all fields have is the “Field Type” setting. This determines what type of field it is – what its input UI is going to look like, what type of data it can store, and how you’ll be able to interact with that data from your templates.

`{% set children = entry.getChildren() %}`

Craft comes with `{{ children|length }}` built-in field types:

```{% for child in children %}
{% set title = (child.title|slice(child.title|length - 7, 7) == ' Fields') ? child.title|slice(0, child.title|length-7) : child.title %}
* [{{ title }}]({{ child.getUrl() }})
{% endfor %}
```

## Field Layouts

Once you’ve created your fields, you can get it to show up in your edit forms by adding them to a “field layouts”.

Everything in Craft that has content associated with it will provide a field layout for selecting fields:

* [Entries](en/sections-and-entries.md) use the field layout defined by their entry type in Settings → Sections → Edit Entry Types → [Entry Type name] → Field Layout.
* [Global sets](en/globals.md) each get their own field layout, defined in Settings → Globals → [Global Set name] → Field Layout.
* [Assets](en/assets.md) use the field layout defined by their asset source in Settings → Assets → [asset source name] → Field Layout.
* [Categories](en/categories) use the field layout defined by their Category Group in Settings → Categories → [Category Group name] → Field Layout.
* [Tags](en/tags) use the field layout defined by their Tag Group in Settings → Tags → [Tag Group name] → Field Layout.
* [Users](en/users) all share a single field layout defined in Settings → Users → Fields.

When editing a field layout, you will find a “Content” tab at the top, and a list of all of your site’s fields, grouped into their field groups, at the bottom. Selecting a field is as simple as dragging it from the bottom area to the top, positioning it wherever you want alongside the other selected fields. You can also drag selected fields around to change their order.

Once a field is selected, a gear icon will appear beside it. Clicking on it will reveal a context menu with two options:

* Make required
* Remove

Clicking “Make required” will add an asterisk (`*`) beside the field’s name, indicating that it’s now required. Subsequent gear icon clicks will reveal a new “Make not required” option which does as you’d expect.

Field layouts for entry types have another feature: they let you define the actual content tabs that contain the fields. You can create as many content tabs as you want, and use them to organize similar fields together. Each content tab will get its own gear icon allowing you to rename or delete it.
