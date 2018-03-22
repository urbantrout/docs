AssetSource Model
=================

AssetSource Model objects represent your asset sources.

## Simple Output

Outputting an AssetSource Model object without attaching a property or method will return the source’s name:

```twig
Source: {{ source }}
```

## Properties

AssetFolder Model objects have the following properties:

### `id`

The source’s ID.

### `name`

The name of the source.

### `type`

The type of source it is (`'Local'`, `'S3'`, `'Rackspace'`, or `'GoogleCloud'`).
