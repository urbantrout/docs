# Upgrading from Craft 2

- [Updating Craft CMS](#updating-craft-cms)
- [Configuration](#configuration)
  - [Config Settings](#config-settings)
  - [`omitScriptNameInUrls` and `usePathInfo`](#omitscriptnameinurls-and-usepathinfo)
  - [Environment Variables](#environment-variables)
  - [Redactor Configs](#redactor-configs)
- [URL Rules](#url-rules)
- [PHP Constants](#php-constants)
- [Static Translation Files](#static-translation-files)
- [Remote Volumes](#remote-volumes)
- [User Photos](#user-photos)
- [Twig 2](#twig-2)
- [Template Tags](#template-tags)
- [Template Functions](#template-functions)
- [Date Formatting](#date-formatting)
- [Element Queries](#element-queries)
- [Elements](#elements)
- [Models](#models)
- [Locales](#locales)
- [Request Params](#request-params)
- [Memcache](#memcache)
- [Plugins](#plugins)

## Updating Craft CMS

The first step to upgrading your site to Craft 3 is updating the CMS itself.

Before you begin, make sure that:

- your server meets Craft 3’s [**minimum requirements**](requirements.md)
- your site is running at least **Craft 2.6.2788**
- you’ve got a fresh **database backup** in case everything goes horribly wrong
- **Composer** is installed (see step 1 of the [installation instructions](installation.md))

Once everything’s in order, there are two ways you can go about updating Craft, depending on whether you want to [keep your current directory structure](#if-you-want-to-keep-your-current-directory-structure), or [switch things up](#if-you-want-your-directory-structure-to-resemble-a-new-craft-3-project) to be more like a new Craft 3 installation.

### If you want to keep your current directory structure…

To update Craft without making any major changes to your site’s directory structure, follow these instructions.

Note that at the end of this, your “project root” (as referenced in other areas of the documentation) will be your `craft/` folder, _not_ its parent folder.

1. Create a `composer.json` file within your project’s `craft/` folder, and add the following properties:

    ```json
    {
      "minimum-stability": "beta",
      "repositories": [
        {
          "type": "composer",
          "url": "https://asset-packagist.org"
        }
      ]
    }
    ```

2. Open your terminal and go to the `craft/` folder:

        cd /path/to/project/craft

3. Then run the following command to load Craft 3 (this will take a few minutes):

        composer require craftcms/cms:~3.0.0-beta.20

    > {tip} If Composer complains that your system doesn’t have PHP 7 installed, but you know it’s not an issue because Craft will run with a different PHP install (e.g. through MAMP or Vagrant), use the `--ignore-platform-reqs` flag.

4. Once all the files are in place, open your `public/index.php` file, find this line:

    ```php
    // Do not edit below this line
    ```

    …and replace everything below it with:

    ```php
    defined('CRAFT_BASE_PATH') || define('CRAFT_BASE_PATH', realpath($craftPath));

    if (!is_dir(CRAFT_BASE_PATH.'/vendor')) {
      exit('Could not find your vendor/ folder. Please ensure that <strong><code>$craftPath</code></strong> is set correctly in '.__FILE__);
    }

    require_once CRAFT_BASE_PATH.'/vendor/autoload.php';
    $app = require CRAFT_BASE_PATH.'/vendor/craftcms/cms/bootstrap/web.php';
    $app->run();
    ```

5. Point your browser to your Control Panel URL (e.g. `http://example.dev/admin`). If you see the update prompt, you did everything right! Go ahead and click “Finish up” to update your database.

6. Delete your old `craft/app/` folder. It’s no longer needed; Craft 3 is located in `vendor/craftcms/cms/` now.

> {note} If your `craft/` folder lives in a public folder on your server (e.g. within `public_html/`), you will need to make sure the new `craft/vendor/` folder is protected from web traffic. If your server is running Apache, you can do this by creating a `.htaccess` file within it, with the contents `Deny from all`.

### If you want your directory structure to resemble a new Craft 3 project…

To set your site up with the same directory structure (including the [PHP dotenv](https://github.com/vlucas/phpdotenv)-based configuration) as a brand new Craft 3 project, follow these instructions:

1. Follow steps 1 and 2 from the [Installation Instructions](installation.md). (Note that you should create your Craft 3 project in a new location; not in the same place your Craft 2 project is).

2. Configure your `.env` file with with your database connection settings. You can either edit the file manually, or run the `./craft setup` command from your new root project directory in your terminal.

3. Copy any settings from your old `craft/config/general.php` file into your new project’s `config/general.php` file.

4. Copy your old templates from `craft/templates/` over to your new project’s `templates/` folder.

5. If you had made any changes to your `public/index.php` file, copy them to your new project’s `web/index.php` file.

6. Copy any other files in your old `public/` folder into your new project’s `web/` folder.

7. Update your web server to point to your new project’s `web/` folder.

8. Point your browser to your Control Panel URL (e.g. `http://example.dev/admin`). If you see the update prompt, you did everything right! Go ahead and click “Finish up” to update your database.

## Configuration

### Config Settings

The following config settings have been deprecated, and will be removed in Craft 4:

File          | Old Setting                  | New Setting
------------- | ---------------------------- | -----------------------------
`general.php` | `activateAccountFailurePath` | `invalidUserTokenPath`
`general.php` | `backupDbOnUpdate`           | `backupOnUpdate`<sup>1</sup>
`general.php` | `defaultFilePermissions`     | `defaultFileMode`<sup>2</sup>
`general.php` | `defaultFolderPermissions`   | `defaultDirMode`
`general.php` | `restoreDbOnUpdateFailure`   | `restoreOnUpdateFailure`
`general.php` | `useWriteFileLock`           | `useFileLocks`
`general.php` | `validationKey`              | `securityKey`<sup>3</sup>

*<sup>1</sup> Performance should no longer be a major factor when setting `backupOnUpdate` to `false`, since backups aren’t generated by PHP anymore.*

*<sup>2</sup> `defaultFileMode` is now `null` by default, meaning it will be determined by the current environment.*

*<sup>3</sup> `securityKey` is no longer optional. If you haven’t set it yet, set it to the value in `storage/runtime/validation.key` (if the file exists). The auto-generated `validation.key` file fallback will be removed in Craft 4.*

The following config settings have been removed entirely:

File          | Setting
------------- | -----------
`db.php`      | `collation`
`db.php`      | `initSQLs`
`general.php` | `appId`

### `omitScriptNameInUrls` and `usePathInfo`

The `omitScriptNameInUrls` setting can no longer be set to `'auto'`, as it was by default in Craft 2. Which means you will need to explicitly set it to `true` in `config/general.php` if you’ve configured your server to route HTTP requests to `index.php`.

Similarly, the `usePathInfo` setting can no longer be set to `'auto'` either. If your server is configured to support [PATH_INFO](https://craftcms.com/support/enable-path-info), you can set this to `true`. This is only necessary if you can’t set `omitScriptNameInUrls` to `true`, though.

### Environment Variables

The concept of “Environment Variables” has been removed in Craft 3. Previously, they provided a way to define custom token values that would get parsed in a couple places:

- The Site URL setting in Settings → General
- The File System Path and URL settings for “Local Folder” asset sources

You can tell if your site defined any environment variables by searching for `environmentVariables` in your `config/general.php` file. If it’s there, here’s how to handle this:

#### Site URL

The recommended way to set your site URL is with the `siteUrl` config setting in `config/general.php`:

```php
return [
    'siteUrl' => 'https://foo.com/',

    // ...
]
```

For multi-site Craft installs, you can also set that to an array:

```php
return [
    'siteUrl' => [
        'siteHandle1' => 'https://foo.com/',
        'siteHandle2' => 'https://bar.com/',
    ],

    // ...
]
```

#### Asset Volume Settings

Craft 3 makes it possible to completely override all asset volume settings, not just the File System Path and URL settings for “Local” volumes. See [Overriding Volume Settings](configuration.md#overriding-volume-settings) on the Configuration page for more info.

### Redactor Configs

Your Redactor configs in `config/redactor/` must now be valid JSON. That means:

- No comments
- All object properties (the config setting names) must be wrapped in double quotes
- All strings must use double quotes rather than single quotes

```javascript
// Bad:
{
  /* interesting comment */
  buttons: ['bold', 'italic']
}

// Good:
{
  "buttons": ["bold", "italic"]
}
```

## URL Rules

If you have any URL rules saved in `config/routes.php`, you will need to update them to Yii 2’s [pattern-route syntax](http://www.yiiframework.com/doc-2.0/guide-runtime-routing.html#url-rules).

- Named parameters in the pattern should be defined using the format (`<paramName:regex>`) rather than as a regular expression subpattern (`(?P<paramName>regex)`).
- Controller action routes should be defined as a string (`'action/path'`) rather than an array with an `action` key (`['action' => 'action/path']`).
- Template routes should be defined as as an array with a `template` key (`['template' => 'template/path']`) rather than a string (`'template/path'`).

```php
// Old:
'dashboard' => ['action' => 'dashboard/index'],
'settings/fields/new' => 'settings/fields/_edit',
'settings/fields/edit/(?P<fieldId>\d+)' => 'settings/fields/_edit',

// New:
'dashboard' => 'dashboard/index',
'settings/fields/new' => ['template' => 'settings/fields/_edit'],
'settings/fields/edit/<fieldId:\d+>' => ['template' => 'settings/fields/_edit'],
```

## PHP Constants

The following PHP constants have been deprecated, and will no longer be supported in Craft 4:

Old              | New
---------------- | ----------------------------------------
`CRAFT_LOCALE`   | `CRAFT_SITE`
`CRAFT_SITE_URL` | Use the `siteUrl` config setting instead

## Static Translation Files

Craft 3 still supports [static translations](https://craftcms.com/support/static-translations), but the directory structure has changed. Now within your `translations/` folder, you should create subdirectories for each locale, and within them, PHP files for each **translation category**.

The acceptable translation categories are:

Category        | Description
--------------- | -----------------------------------------
`app`           | Craft’s translation messages
`yii`           | Yii’s translation messages
`site`          | custom site-specific translation messages
`plugin-handle` | Plugins’ translation messages

In Craft 3, your `translations/` folder might look something like this:

```
translations/
  en-US/
    app.php
    site.php
```

## Remote Volumes

Support for Amazon S3, Rackspace Cloud Files, and Google Cloud Storage have been moved into plugins. If you have any asset volumes that were using those services in Craft 2, you will need to install the new plugins:

- [Amazon S3](https://github.com/craftcms/aws-s3)
- [Rackspace Cloud Files](https://github.com/craftcms/rackspace)
- [Google Cloud Storage](https://github.com/craftcms/google-cloud)

## User Photos

User photos are stored as assets now. When upgrading to Craft 3, Craft will automatically create a new asset volume called “User Photos”, set to the `storage/userphotos/` folder, where Craft previously stored all user photos. However this folder is be above your web root and inaccessible to HTTP requests, so until you make this volume publicly accessible, user photos will not work on the front end.

Here’s how you can resolve this:

1. Move the `storage/userphotos/` folder somewhere below your web root (e.g. `public_html/userphotos/`)
2. Go to Settings → Assets → Volumes → User Photos and configure the volume based on the new folder location:
    - Update the File System Path setting to point to the new folder location
    - Enable the “Assets in this volume have public URLs” setting
    - Set the correct URL setting for the folder
    - Save the volume

## Twig 2

Craft 3 uses Twig 2, which has its own breaking changes for templates:

#### Macros

Twig 1 let you call macros defined by the same template using `_self.macroName()`:

```twig
{% macro foo %}...{% endmacro %}

{{ _self.foo() }}
```

Twig 2 requires you to explicitly import them first:

```twig
Import just one macro:
{% from _self import foo %}
{{ foo() }}

Or all of them:
{% import _self as macros %}
{{ macros.foo() }}
```

#### Undefined Blocks

Twig 1 let you call `block()` even for blocks that didn’t exist:

```twig
{% if block('foo') is not empty %}
    {{ block('foo') }}
{% endif %}
```

Twig 2 will throw an error unless it’s a `defined` test:

```twig
{% if block('foo') is defined %}
    {{ block('foo') }}
{% endif %}
```

## Template Tags

The following Twig template tags have been removed:

Old                 | New
------------------- | -----------------------------------------
`{% endpaginate %}` | *(no replacement needed; just delete it)*

The following Twig template tags have been deprecated, and will be removed in Craft 4:

Old                             | New
------------------------------- | ---------------------------------------------
`{% includecss %}`              | `{% css %}`
`{% includehirescss %}`         | `{% css %}` *(write your own media selector)*
`{% includejs %}`               | `{% js %}`
`{% includecssfile url %}`      | `{% do view.registerCssFile(url) %}`
`{% includejsfile url %}`       | `{% do view.registerJsFile(url) %}`
`{% includecssresource path %}` | See [Asset Bundles](asset-bundles.md)
`{% includejsresource path %}`  | See [Asset Bundles](asset-bundles.md)

## Template Functions

The following template functions have been removed:

Old                                         | New
------------------------------------------- | ------------------------------------
`craft.hasPackage()`                        | *(n/a)*
`craft.entryRevisions.getDraftByOffset()`   | *(n/a)*
`craft.entryRevisions.getVersionByOffset()` | *(n/a)*
`craft.fields.getFieldType(type)`           | `craft.app.fields.createField(type)`
`craft.fields.populateFieldType()`          | *(n/a)*
`craft.tasks.areTasksPending()`             | `craft.app.queue.getHasWaitingJobs()`<sup>1</sup>
`craft.tasks.getRunningTask()`              | *(n/a)*
`craft.tasks.getTotalTasks()`               | *(n/a)*
`craft.tasks.haveTasksFailed()`             | *(n/a)*
`craft.tasks.isTaskRunning()`               | `craft.app.queue.getHasReservedJobs()`<sup>1</sup>

*<sup>1</sup> Only available if the `queue` component implements `craft\queue\QueueInterface`.*

The following template functions have been deprecated, and will be removed in Craft 4:

Old                                                     | New
------------------------------------------------------- | ---------------------------------------------
`round(num)`                                            | `num\|round`
`getCsrfInput()`                                        | `csrfInput()`
`getHeadHtml()`                                         | `head()`
`getFootHtml()`                                         | `endBody()`
`getTranslations()`                                     | `view.getTranslations()\|json_encode\|raw`
`craft.categoryGroups.getAllGroupIds()`                 | `craft.app.categoryGroups.allGroupIds`
`craft.categoryGroups.getEditableGroupIds()`            | `craft.app.categories.editableGroupIds`
`craft.categoryGroups.getAllGroups()`                   | `craft.app.categoryGroups.allGroups`
`craft.categoryGroups.getEditableGroups()`              | `craft.app.categories.editableGroups`
`craft.categoryGroups.getTotalGroups()`                 | `craft.app.categories.totalGroups`
`craft.categoryGroups.getGroupById(id)`                 | `craft.app.categories.getGroupById(id)`
`craft.categoryGroups.getGroupByHandle(handle)`         | `craft.app.categories.getGroupByHandle(handle)`
`craft.config.[setting]` *(magic getter)*               | `craft.app.config.general.[setting]`
`craft.config.get(setting)`                             | `craft.app.config.general.[setting]`
`craft.config.usePathInfo()`                            | `craft.app.config.general.usePathInfo`
`craft.config.omitScriptNameInUrls()`                   | `craft.app.config.general.omitScriptNameInUrls`
`craft.config.getResourceTrigger()`                     | `craft.app.config.general.resourceTrigger`
`craft.locale()`                                        | `craft.app.language`
`craft.isLocalized()`                                   | `craft.app.isMultiSite`
`craft.deprecator.getTotalLogs()`                       | `craft.app.deprecator.totalLogs`
`craft.elementIndexes.getSources()`                     | `craft.app.elementIndexes.sources`
`craft.emailMessages.getAllMessages()`                  | `craft.emailMessages.allMessages`
`craft.emailMessages.getMessage(key)`                   | `craft.app.emailMessages.getMessage(key)`
`craft.entryRevisions.getDraftsByEntryId(id)`           | `craft.app.entryRevisions.getDraftsByEntryId(id)`
`craft.entryRevisions.getEditableDraftsByEntryId(id)`   | `craft.entryRevisions.getEditableDraftsByEntryId(id)`
`craft.entryRevisions.getDraftById(id)`                 | `craft.app.entryRevisions.getDraftById(id)`
`craft.entryRevisions.getVersionsByEntryId(id)`         | `craft.app.entryRevisions.getVersionsByEntryId(id)`
`craft.entryRevisions.getVersionById(id)`               | `craft.app.entryRevisions.getVersionById(id)`
`craft.feeds.getFeedItems(url)`                         | `craft.app.feeds.getFeedItems(url)`
`craft.fields.getAllGroups()`                           | `craft.app.fields.allGroups`
`craft.fields.getGroupById(id)`                         | `craft.app.fields.getGroupById(id)`
`craft.fields.getFieldById(id)`                         | `craft.app.fields.getFieldById(id)`
`craft.fields.getFieldByHandle(handle)`                 | `craft.app.fields.getFieldByHandle(handle)`
`craft.fields.getAllFields()`                           | `craft.app.fields.allFields`
`craft.fields.getFieldsByGroupId(id)`                   | `craft.app.fields.getFieldsByGroupId(id)`
`craft.fields.getLayoutById(id)`                        | `craft.app.fields.getLayoutById(id)`
`craft.fields.getLayoutByType(type)`                    | `craft.app.fields.getLayoutByType(type)`
`craft.fields.getAllFieldTypes()`                       | `craft.app.fields.allFieldTypes`
`craft.globals.getAllSets()`                            | `craft.app.globals.allSets`
`craft.globals.getEditableSets()`                       | `craft.app.globals.editableSets`
`craft.globals.getTotalSets()`                          | `craft.app.globals.totalSets`
`craft.globals.getTotalEditableSets()`                  | `craft.app.globals.totalEditableSets`
`craft.globals.getSetById(id)`                          | `craft.app.globals.getSetById(id)`
`craft.globals.getSetByHandle(handle)`                  | `craft.app.globals.getSetByHandle(handle)`
`craft.i18n.getAllLocales()`                            | `craft.app.i18n.allLocales`
`craft.i18n.getAppLocales()`                            | `craft.app.i18n.appLocales`
`craft.i18n.getCurrentLocale()`                         | `craft.app.locale`
`craft.i18n.getLocaleById(id)`                          | `craft.app.i18n.getLocaleById(id)`
`craft.i18n.getSiteLocales()`                           | `craft.app.i18n.siteLocales`
`craft.i18n.getSiteLocaleIds()`                         | `craft.app.i18n.siteLocaleIds`
`craft.i18n.getPrimarySiteLocale()`                     | `craft.app.i18n.primarySiteLocale`
`craft.i18n.getEditableLocales()`                       | `craft.app.i18n.editableLocales`
`craft.i18n.getEditableLocaleIds()`                     | `craft.app.i18n.editableLocaleIds`
`craft.i18n.getLocaleData()`                            | `craft.app.i18n.getLocaleById(id)`
`craft.i18n.getDatepickerJsFormat()`                    | `craft.app.locale.getDateFormat('short', 'jui')`
`craft.i18n.getTimepickerJsFormat()`                    | `craft.app.locale.getTimeFormat('short', 'php')`
`craft.request.isGet()`                                 | `craft.app.request.isGet`
`craft.request.isPost()`                                | `craft.app.request.isPost`
`craft.request.isDelete()`                              | `craft.app.request.isDelete`
`craft.request.isPut()`                                 | `craft.app.request.isPut`
`craft.request.isAjax()`                                | `craft.app.request.isAjax`
`craft.request.isSecure()`                              | `craft.app.request.isSecureConnection`
`craft.request.isLivePreview()`                         | `craft.app.request.isLivePreview`
`craft.request.getScriptName()`                         | `craft.app.request.scriptFilename`
`craft.request.getPath()`                               | `craft.app.request.pathInfo`
`craft.request.getUrl()`                                | `url(craft.app.request.pathInfo)`
`craft.request.getSegments()`                           | `craft.app.request.segments`
`craft.request.getSegment(num)`                         | `craft.app.request.getSegment(num)`
`craft.request.getFirstSegment()`                       | `craft.app.request.segments\|first`
`craft.request.getLastSegment()`                        | `craft.app.request.segments\|last`
`craft.request.getParam(name)`                          | `craft.app.request.getParam(name)`
`craft.request.getQuery(name)`                          | `craft.app.request.getQueryParam(name)`
`craft.request.getPost(name)`                           | `craft.app.request.getBodyParam(name)`
`craft.request.getCookie(name)`                         | `craft.app.request.cookies.get(name)`
`craft.request.getServerName()`                         | `craft.app.request.serverName`
`craft.request.getUrlFormat()`                          | `craft.app.config.general.usePathInfo`
`craft.request.isMobileBrowser()`                       | `craft.app.request.isMobileBrowser()`
`craft.request.getPageNum()`                            | `craft.app.request.pageNum`
`craft.request.getHostInfo()`                           | `craft.app.request.hostInfo`
`craft.request.getScriptUrl()`                          | `craft.app.request.scriptUrl`
`craft.request.getPathInfo()`                           | `craft.app.request.getPathInfo(true)`
`craft.request.getRequestUri()`                         | `craft.app.request.url`
`craft.request.getServerPort()`                         | `craft.app.request.serverPort`
`craft.request.getUrlReferrer()`                        | `craft.app.request.referrer`
`craft.request.getUserAgent()`                          | `craft.app.request.userAgent`
`craft.request.getUserHostAddress()`                    | `craft.app.request.userIP`
`craft.request.getUserHost()`                           | `craft.app.request.userHost`
`craft.request.getPort()`                               | `craft.app.request.port`
`craft.request.getCsrfToken()`                          | `craft.app.request.csrfToken`
`craft.request.getQueryString()`                        | `craft.app.request.queryString`
`craft.request.getQueryStringWithoutPath()`             | `craft.app.request.queryStringWithoutPath`
`craft.request.getIpAddress()`                          | `craft.app.request.userIP`
`craft.request.getClientOs()`                           | `craft.app.request.clientOs`
`craft.sections.getAllSections()`                       | `craft.app.sections.allSections`
`craft.sections.getEditableSections()`                  | `craft.app.sections.editableSections`
`craft.sections.getTotalSections()`                     | `craft.app.sections.totalSections`
`craft.sections.getTotalEditableSections()`             | `craft.app.sections.totalEditableSections`
`craft.sections.getSectionById(id)`                     | `craft.app.sections.getSectionById(id)`
`craft.sections.getSectionByHandle(handle)`             | `craft.app.sections.getSectionByHandle(handle)`
`craft.systemSettings.[category]` *(magic getter)*      | `craft.app.systemSettings.getSettings('category')`
`craft.userGroups.getAllGroups()`                       | `craft.app.userGroups.allGroups`
`craft.userGroups.getGroupById(id)`                     | `craft.app.userGroups.getGroupById(id)`
`craft.userGroups.getGroupByHandle(handle)`             | `craft.app.userGroups.getGroupByHandle(handle)`
`craft.userPermissions.getAllPermissions()`             | `craft.app.userPermissions.allPermissions`
`craft.userPermissions.getGroupPermissionsByUserId(id)` | `craft.app.userPermissions.getGroupPermissionsByUserId(id)`
`craft.session.isLoggedIn()`                            | `not craft.app.user.isGuest`
`craft.session.getUser()`                               | `currentUser`
`craft.session.getRemainingSessionTime()`               | `craft.app.user.remainingSessionTime`
`craft.session.getRememberedUsername()`                 | `craft.app.user.rememberedUsername`
`craft.session.getReturnUrl()`                          | `craft.app.user.getReturnUrl()`
`craft.session.getFlashes()`                            | `craft.app.session.getAllFlashes()`
`craft.session.getFlash()`                              | `craft.app.session.getFlash()`
`craft.session.hasFlash()`                              | `craft.app.session.hasFlash()`

## Date Formatting

Craft’s extended DateTime class has been removed in Craft 3. Here’s a list of things you used to be able to do in your templates, and what the Craft 3 equivalent is. (The DateTime object is represented by the `d` variable. In reality it could be `entry.postDate`, `now`, etc.)

Old                               | New
--------------------------------- | ----------------------------------
`{{ d }}` *(treated as a string)* | `{{ d\|date('Y-m-d') }}`
`{{ d.atom() }}`                  | `{{ d\|atom }}`
`{{ d.cookie() }}`                | `{{ d\|date('l, d-M-y H:i:s T')}}`
`{{ d.day() }}`                   | `{{ d\|date('j') }}`
`{{ d.iso8601() }}`               | `{{ d\|date('Y-m-d\\TH:i:sO') }}`
`{{ d.localeDate() }}`            | `{{ d\|date('short') }}`
`{{ d.localeTime() }}`            | `{{ d\|time('short') }}`
`{{ d.month() }}`                 | `{{ d\|date('n') }}`
`{{ d.mySqlDateTime() }}`         | `{{ d\|date('Y-m-d H:i:s') }}`
`{{ d.nice() }}`                  | `{{ d\|datetime('short') }}`
`{{ d.rfc1036() }}`               | `{{ d\|date('D, d M y H:i:s O') }}`
`{{ d.rfc1123() }}`               | `{{ d\|date('D, d M Y H:i:s O') }}`
`{{ d.rfc2822() }}`               | `{{ d\|date('D, d M Y H:i:s O') }}`
`{{ d.rfc3339() }}`               | `{{ d\|date('Y-m-d\\TH:i:sP') }}`
`{{ d.rfc822() }}`                | `{{ d\|date('D, d M y H:i:s O') }}`
`{{ d.rfc850() }}`                | `{{ d\|date('l, d-M-y H:i:s T') }}`
`{{ d.rss() }}`                   | `{{ d\|rss }}`
`{{ d.uiTimestamp() }}`           | `{{ d\|timestamp('short') }}`
`{{ d.w3c() }}`                   | `{{ d\|date('Y-m-d\\TH:i:sP') }}`
`{{ d.w3cDate() }}`               | `{{ d\|date('Y-m-d') }}`
`{{ d.year() }}`                  | `{{ d\|date('Y') }}`

## Element Queries

### Params

The following params have been removed:

Element Type | Old Param          | New Param
------------ | ------------------ | -------------------------
All of them  | `childOf`          | `relatedTo.sourceElement`
All of them  | `childField`       | `relatedTo.field`
All of them  | `parentOf`         | `relatedTo.targetElement`
All of them  | `parentField`      | `relatedTo.field`
All of them  | `depth`            | `level`
Tag          | `name`             | `title`
Tag          | `setId`            | `groupId`
Tag          | `set`              | `group`
Tag          | `orderBy:"name"`   | `orderBy:"title"`

The following params are now deprecated, and will be removed in Craft 4:

Element Type | Old Param                | New Param
------------ | ------------------------ | ----------------------------
All of them  | `order`                  | `orderBy`
All of them  | `locale`                 | `siteId` or `site`
All of them  | `localeEnabled`          | `enabledForSite`
All of them  | `relatedTo.sourceLocale` | `relatedTo.sourceSite`
Asset        | `source`                 | `volume`
Asset        | `sourceId`               | `volumeId`
Matrix Block | `ownerLocale`            | `ownerSite` or `ownerSiteId`

Also note that the `limit` param is now set to `null` (no limit) by default, rather than 100.

### Methods

The following methods have been removed:

Old                           | New
----------------------------- | -------------
`findElementAtOffset(offset)` | `nth(offset)`

The following methods are now deprecated, and will be removed in Craft 4:

Old             | New
--------------- | --------------------------------------------------------
`ids(criteria)` | `ids()` (setting criteria params here is now deprecated)
`find()`        | `all()`<sup>1</sup>
`first()`       | `one()`
`last()`        | `nth(query.count() - 1)`
`total()`       | `count()`

*<sup>1</sup> Looping through an element query as if it’s an array has been deprecated as well, so you will need to start explicitly calling `.all()`.*

## Elements

The following element properties have been removed:

Element Type | Old Property | New Property
------------ | ------------ | ------------
Tag          | `name`       | `title`

The following element properties have been deprecated, and will be removed in Craft 4:

Old      | New
-------- | -------------------------------------------
`locale` | `siteId`, `site.handle`, or `site.language`

## Models

The following model methods have been deprecated, and will be removed in Craft 4:

Old                 | New
------------------- | ------------------------
`getError('field')` | `getFirstError('field')`

## Locales

The following locale methods have been deprecated, and will be removed in Craft 4:

Old                | New
------------------ | ------------------------------------
`getId()`          | `id`
`getName()`        | `getDisplayName(craft.app.language)`
`getNativeName()`  | `getDisplayName()`

## Request Params

Your front-end `<form>`s and JS scripts that submit to a controller action will need to be updated with the following changes.

### `action` Params

`action` params must be rewritten in in `snake-case` rather than `camelCase`.

```twig
Old:
<input type="hidden" name="action" value="entries/saveEntry">

New:
<input type="hidden" name="action" value="entries/save-entry">
```

The following controller actions have been removed:

Old                          | New
---------------------------- | --------------------------
`categories/create-category` | `categories/save-category`
`users/validate`             | `users/verify-email`
`users/save-profile`         | `users/save-user`

### `redirect` Params

`redirect` params must be hashed now.

```twig
Old:
<input type="hidden" name="redirect" value="foo/bar">

New:
<input type="hidden" name="redirect" value="{{ 'foo/bar'|hash }}">
```

The `redirectInput()` function is provided as a shortcut.

```twig
{{ redirectInput('foo/bar') }}
```

The following `redirect` param tokens are no longer supported:

Controller Action               | Old Token     | New Token
------------------------------- | ------------- | ---------
`entries/save-entry`            | `{entryId}`   | `{id}`
`entry-revisions/save-draft`    | `{entryId}`   | `{id}`
`entry-revisions/publish-draft` | `{entryId}`   | `{id}`
`fields/save-field`             | `{fieldId}`   | `{id}`
`globals/save-set`              | `{setId}`     | `{id}`
`sections/save-section`         | `{sectionId}` | `{id}`
`users/save-user`               | `{userId}`    | `{id}`

### CSRF Token Params

CSRF protection is enabled by default in Craft 3. If you didn’t already have it enabled (via the `enableCsrfProtection` config setting), each of your front-end `<form>`s and JS scripts that submit to a controller action will need to be updated with a new CSRF token param, named after your `csrfTokenName` config setting value (set to `'CRAFT_CSRF_TOKEN'` by default).

```twig
{% set csrfTokenName = craft.app.config.general.csrfTokenName %}
{% set csrfToken = craft.app.request.csrfToken %}
<input type="hidden" name="{{ csrfTokenName }}" value="{{ csrfToken }}">
```

The `csrfInput()` function is provided as a shortcut.

```twig
{{ csrfInput() }}
```

## Memcache

If you are using `memcache` for your [cacheMethod](https://craftcms.com/docs/config-settings#cacheMethod) config setting and you did not have `useMemcached` set to `true` in your `craft/config/memcache.php` config file, you'll need to install memcached on your server.  Craft 3 will only use it because there is not a PHP 7 compatible version of memcache available.

## DbCache

If you are using `db` for your [cacheMethod](https://craftcms.com/docs/config-settings#cacheMethod) config setting, you'll need to manually execute some SQL before attempting the Craft 3 update.

*MySQL:*

```
DROP TABLE IF EXISTS craft_cache;

CREATE TABLE craft_cache (
    id char(128) NOT NULL PRIMARY KEY,
    expire int(11),
    data BLOB,
    dateCreated datetime NOT NULL,
    dateUpdated datetime NOT NULL,
    uid char(36) NOT NULL DEFAULT 0
);
```

*PostgreSQL:*

```
DROP TABLE IF EXISTS craft_cache;

CREATE TABLE craft_cache (
    id char(128) NOT NULL PRIMARY KEY,
    expire int4,
    data BYTEA,
    dateCreated timestamp NOT NULL,
    dateUpdated timestamp NOT NULL,
    uid char(36) NOT NULL DEFAULT '0'::bpchar
);
```

Note that these examples use the default `craft/config/db.php` config setting of `craft`.

If you have changed that config setting, you will want to adjust the examples accordingly.



## Plugins

See [Updating Plugins for Craft 3](updating-plugins.md).
