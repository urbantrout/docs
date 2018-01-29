# Extending Twig

Craft provides two ways for plugins to extend its Twig templating environment.

- [Extend the Global `craft` Variable](#extend-the-craft-global-variable)
- [Register a Twig Extension](#register-a-twig-extension)

## Extend the Global `craft` Variable

The global `craft` template variable can be extended using [behaviors](http://www.yiiframework.com/doc-2.0/guide-concept-behaviors.html) or [services](http://www.yiiframework.com/doc-2.0/guide-concept-service-locator.html).

- Use a **behavior** to add custom properties or methods directly onto the `craft` variable (e.g. `craft.foo()`).
- Use a **service** to add a sub-object to the `craft` variable, which will be lazy-loaded the first time it is actually requested by a template (e.g. `craft.foo.*`).

You can attach your behavior or service to the `craft` variable by registering an [`EVENT_INIT`](https://docs.craftcms.com/api/v3/craft-web-twig-variables-craftvariable.html#EVENT_INIT-detail) event handler from your plugin’s `init()` method:

```php
use craft\web\twig\variables\CraftVariable;
use yii\base\Event;

public function init()
{
    parent::init();
    
    Event::on(CraftVariable::class, CraftVariable::EVENT_INIT, function(Event $e) {
        /** @var CraftVariable $variable */
        $variable = $e->sender;
        
        // Attach a behavior:
        $variable->attachBehaviors([
            MyBehavior::class,
        ]);
        
        // Attach a service:
        $variable->set('serviceId', MyService::class);
    });
}
```

## Register a Twig Extension

If you want to add new global variables, functions, filters, tags, operators, or tests to Twig, you can do that by creating a custom [Twig extension](https://twig.symfony.com/doc/2.x/advanced.html#creating-an-extension).

Twig extensions can be registered for Craft’s Twig environment by calling [craft\web\View::registerTwigExtension()](https://docs.craftcms.com/api/v3/craft-web-view.html#registerTwigExtension()-detail) from your plugin’s `init()` method:

```php
public function init()
{
    parent::init();
    
    if (Craft::$app->request->getIsSiteRequest()) {
        // Add in our Twig extension
        $extension = new MyTwigExtension();
        Craft::$app->view->registerTwigExtension($extension);
    }
}
```
