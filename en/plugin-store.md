# Publishing to the Plugin Store

If you want to make your plugin available in the in-app Plugin Store, and on [plugins.craftcms.com](http://plugins.craftcms.com), follow this guide.

## Choose a License

All plugins in the Plugin Store must have one of two licenses:

- **[MIT](https://opensource.org/licenses/MIT)** – Use this if you aren’t planning on ever changing for your plugin, and you’re OK with other people pretty much doing whatever they want with your code, as long as they give credit back to you. ([Example](https://github.com/craftcms/element-api/blob/v2/LICENSE.md))
- **[Craft](https://craftcms.github.io/license/)** – Use this if you are planning on charging for your plugin, and want to prevent people from using your code without paying up. ([Example](https://github.com/craftcms/cms/blob/develop/LICENSE.md))

Create a `LICENSE.md` file at the root of your plugin’s repository, and paste in the license text, beginning with a copyright notice.

```
Copyright © <YOUR_NAME>
```

### Registering your Plugin

To register your plugin, first make sure it’s published to a public GitHub repository. Then create a Craft ID account at [id.craftcms.com](https://id.craftcms.com), and connect it to your GitHub account.

> {note} If your plugins are published to a GitHub organization account, make sure that the organization is checked when authenticating your GitHub account.

From your Craft ID account, go to Plugins → “Add a plugin”, and click the “Select” button next to your plugin’s repository. You will then be able to edit its description, screenshots, and other details.

Once you’re ready to submit the plugin, click the “Submit for approval” button. Once your plugin is approved, it will become visible on [plugins.craftcms.com](http://plugins.craftcms.com). It won’t necessarily be available in the in-app Plugin Store yet, though, unless your plugin already has at least one [release](#plugin-releases).

> {tip} You might want to register your plugin with [Packagist](https://packagist.org/) in addition to the Plugin Store, so that people can install and update your plugin from the command line. But Packagist isn’t a requirement for the Plugin Store.

### Plugin Releases

To release a new version of your plugin, first decide on the version number. The Plugin Store follows the same [Semantic Versioning](https://semver.org/) conventions supported by Composer:

- Versions should have 3 or 4 segments (e.g. `1.2.3` or `1.2.3.4`).
- Pre-release versions should end with a release identifier (`-alpha.X`, `-beta.X`, or `-RCX`). 

Once you’ve decided on a version, follow these steps:

1. If your plugin has a [changelog](changelogs-and-updates.md), make sure the new version has a correctly-formatted heading, including the release date.

   ```markdown
   ## 3.0.0 - 2017-04-04
   ```

2. If your plugin’s `composer.json` file includes a `version` property, make sure that it is set to the new version number.

3. Once everything is good to go and committed to Git, [create a tag](https://git-scm.com/book/en/v2/Git-Basics-Tagging) named after the version number, optionally beginning with `v` (for example, `v3.0.0` or `v3.0.0-beta.1`).

4. Push your latest commits and your new version tag to GitHub. At this point the Plugin Store should automatically get notified about the release, and will start recording it. If all goes well, it will show up in the Plugin Store within a minute or two.
