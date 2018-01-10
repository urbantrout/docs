# `{% js %}`

This tag will queue up a Javascript snippet for inclusion on the page.

```twig
{% set myJs %}
_gaq.push([ "_trackEvent", "Search", "{{ searchTerm|e('js') }}" ]);
{% endset %}

{% js myJs %}
```

## Parameters

The `{% js %}` tag supports the following parameters:

### Javascript snippet

A string that defines the Javascript that should be included. The string can be typed directly into the tag, or you can set it to a variable beforehand, and just type the variable name.

### `first`

Add `first` at the end of the tag if you want this Javascript to be included before any other Javascript snippets that were included using this tag.

```twig
{% js myJs first %}
```

## Where does it get output?

Your Javascript snippet will be output by the `endBody()` function. If you aren’t calling that function anywhere, Craft will insert it right before the HTML’s `</body>` tag.