# Global Variables

Every single template is going to get loaded with the following variables:

## `now`

A {entry:templating/datetime:link} object set to the current date and time.

```twig
Today is {{ now|date('M j, Y') }}.
```

## `siteName`

The name of your site, as defined in Settings → General.

```twig
<h1>{{ siteName }}</h1>
```

## `siteUrl`

The URL of your site. (See {entry:supportArticles/site-url:link})

```twig
<link rel="home" href="{{ siteUrl }}">
```

## `currentUser`

A {entry:templating/usermodel:link} object set to the currently logged-in user (if there is one).

```twig
{% if currentUser %}
    Welcome, {{ currentUser.friendlyName }}!
{% endif %}
```

## `loginUrl`

The URL to your site’s login page, based on the [loginPath]({entry:docs/config-settings}#loginPath) config setting.

```twig
{% if not currentUser %}
    <a href="{{ loginUrl }}">Login</a>
{% endif %}
```

## `logoutUrl`

The URL Craft uses to log users out, based on the [logoutPath]({entry:docs/config-settings}#logoutPath) config setting. Note that Craft will automatically redirect users to your homepage after going here; there’s no such thing as a “logout _page_”.

```twig
{% if currentUser %}
    <a href="{{ logoutUrl }}">Logout</a>
{% endif %}
```

## Global Set Variables

Each of your site’s [global sets]({entry:docs/globals}) get {entry:templating/globalsetmodel:link} object to represent them.

```twig
<p>{{ companyInfo.companyName }} was established in {{ companyInfo.yearEstablished }}.</p>
```