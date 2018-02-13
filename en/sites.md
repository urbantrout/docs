Sites
======================

Sites allows you to set up multiple sites in a single Craft installation. You can define one or more sites at different domains, using a different set of templates, and different versions of entry content.

The multi-site feature in Craft is designed for sites with similar content structures. You manage the multi-site content at the entry level, with the ability to the enable Sections you want included in a site.

## Sites

When you install Craft you automatically get a default site. The site name is what you defined at time of installation, and the handle is `default`.

You add additional sites using the Sites settings in Settings -> Sites.

A site has the following attributes:

* Group
* Name
* Handle
* Language
* Is Primary Site?
* Base URL


### Site Groups

Site Groups allow you to organize your sites together by commonality, like language or site type.

Craft creates the first Site Group for you--named after the default site--and assigns the default site to that group.

Similar to [Field Groups](), Site Groups are for organization only.

### Language

Choosing the language for the site tells Craft which language to use when generating date and time formatting and system messages.

In your templates, you can also access the language setting via `craft.app.language`. You can use this in a conditional:

```
{% if craft.app.language == 'de' %}
    <p>Guten Tag!</p>
{% endif %}
```

Or as a way to automatically include the proper template for each language:

```
{% include '_share/footer-' ~ craft.app.language %}
```

where your template name would be, for example, `_share/footer-de`. 



### Primary Site

Craft sets the default site as the Primary site, meaning Craft will load it by default on the front end. If you only have one site then you cannot disable it as the Primary site. 

You can change the Primary site once you create additional sites using the settings for the new site. Craft will automatically toggle the current Primary site.

### Site URL

Any additional site can have its own Base URL apart from the URL you defined when creating the first site.

This comes in handy for hosting the site at a different domain instead of in a subdirectory. Instead of using `https://craftcms.com/de`, you can have a different domain like `https://de.craftcms.com` or `https://craftcms.de`.

Defining a Base URL for your site requires that you have an additional `web` directory with a different name (like `web-de`), and that you configure your web server to route requests for that domain to the new web directory.


## Propagating Entries Across All Enabled Sites

In the settings for each Channel or Structure Section is an option to propagate entries in that section across all sites. When enabled, Craft will create the new entry in each site enabled for that section using the submitted content.

## Setting Up and New Site

In this short guide we'll walk through the steps of setting up a new site in Craft. 
