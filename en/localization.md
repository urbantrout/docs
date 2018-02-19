Guide: Setting Up a Localized Site
=============

This guide will walk you through all of the steps that are typically involved in setting up a localized site, using Craft’s deep localization features.

## Step 1: Defining Your Sites and Languages

The first step to creating localized site is to decide which languages you need to support. After that, create a new Site in Craft for each language you need to support using the [guide on configuring a multi-site setup in Craft](sites.md).

## Step 2: Update Your Sections

After creating a new site for a language, you need to enable the new site. In Settings -> Sites, choose the site you just created.

Enable the site in the Site Settings table and fill out the Entry URI Format (for Channel and Structure sections) or URI (for Single sections).

## Step 3: Define Your Translatable Fields

In Settings -> Fields, choose the fields you want to have translatable. Under Translation Method, choose "Translate for each language."

Craft will allow you to update this field's content in each entry on a per-language basis. 

## Step 4: Update Your Templates

If you have any templates that you only want to serve from a specific site, you can create a new subfolder in your templates folder, named after your site's handle, and place the templates in there.

For example, if you wanted to give your German site its own homepage template, you might set your templates folder up like this:

```
templates/
    index.twig      --> default homepage template
    de/
        index.twig  --> German homepage template
```

Each template is aware of the current language via `craft.app.language`, which can be used to toggle specific parts of your templates, depending on the language:

```
{% if craft.app.language == 'de' %}
    <p>I like bread and beer.</p>
{% endif %}
```

You will also probably want to take advantage of Craft’s static translation support, for various strings throughout your templates.

```
{{ "Welcome!"|t }}
```

## Step 5: Give your authors access to the sites

As soon as you add a second site to your Craft installation, Craft will start checking for site permissions whenever users try to edit content. By default, no users or groups have access to any site, so you need to go assign them.

When you edit a user group or a user account, you will find a new Sites permissions section, which lists all of your sites. Assign them where appropriate.