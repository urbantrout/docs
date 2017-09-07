The Pieces of Craft
===================

_What's the common theme here -- University website or something with the cocktail theme?_

Before you get started using Craft CMS to build your website, let's first talk big picture: what are the biggest, most important components of Craft? These are technical components but that pieces of Craft that you will work with. For more depth on each piece, click through to the Core Concepts section of the documentation.

## Sections

Sections are how we organize our content or data in Craft. Let's say you have a university website with various types of content: news, blog, press releases, etc. Each one of those content types would be a Section in Craft:

* News
* Blog
* Press Releases

Because Craft wants to be as flexible as possible, there are different Section Types. Each of these types allows you to do different things. The most popular type is the Channel. This is for a collection of similar content (like news articles). But there's also:

* Structure
* Single

To review the specifics of each type, check out the [Sections and Entries](sections-and-entries.md) page of this documentation.

## Entries

Inside of the Sections (types Structure and Channel) are Entries. Entries are your invidual pieces of content (like a news article). 

Each entry has a unique identifier called a `slug` that Craft uses to retrieve it and display on a web page. The `slug` is also the URI of the entry.

A section can have one or many entries. The entries are typically inputted via the Entry Form in the Craft control panel.

Each entry can only belong to one Section.

## Fields

We use Fields in Craft to define the different content types that are stored in an Entry. Craft provides a bunch of fields for (title, slug, postDate and more) but we can also define our own fields so we can mold Craft _around_ our content.

Let's say we are working on our University website and want to build out the News section. We've already created the Channel Section to hold our entries. Now we need to create the fields to hold our content.

Our news articles are pretty simple but we do need to define some Fields:

* Article Image
* Article Lede
* Article Copy


## Templates


