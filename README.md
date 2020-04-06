# Sitegeist.Silhouettes

> Centralized property configuration for the Neos.ContentRepository

It is common that properties in various NodeTypes are expected to behave
identically. This is usually achieved with mixins but those are bound to
a fixed property name do not cover the case where properties with
different names share similarities.

The `Sitegeist.Silhouettes` package uses preconfigured
property-configurations from the settings in multiple NodeTypes. This
adds a way to centralize property-configuration for cases where mixins
are not sufficient and settings shall be synchronized betweeen
properties with different names.

It is also possible to create silhouettes for childNode constraint
configurations, e.g. to apply the same centralized constraints to different
childNodes following the dry principle.
This can also be useful when using a single NodeType package in different
neos instances where the constraints may differcenate.

The settings from the configured silhouette are merged with the
configuration that is found in the nodeType with the local configuration
taking precedence over the silhouette.

## Authors & Sponsors

* Martin Ficzel - ficzel@sitegeist.de

*The development and the public-releases of this package is generously sponsored
by our employer http://www.sitegeist.de.*

## Usage

Settings.yaml

```yaml
Sitegeist:
  Silhouettes:
    properties:
      vendor:
        text:
          block:
            type: string
            defaultValue: ''
            ui:
              inlineEditable: TRUE
              aloha:
                placeholder: i18n
                autoparagraph: TRUE
                'format':
                  'strong': TRUE
                  'em': TRUE
                  'u': FALSE
                  'sub': FALSE
                  'sup': FALSE
                  'del': FALSE
                  'p': TRUE
                  'h1': TRUE
                  'h2': TRUE
                  'h3': TRUE
                  'pre': TRUE
                  'removeFormat': TRUE
                'table':
                  'table': TRUE
                'list':
                  'ol': TRUE
                  'ul': TRUE
                'link':
                  'a': TRUE
      childNodes:
        vendor:
          defaultConstraints:
            constraints:
              'Neos.Neos:Content': TRUE
              'Neos.NodeTypes.BaseMixins:TitleMixin': TRUE
              'Neos.Demo:Constraint.Content.Carousel': TRUE
              'Neos.Demo:Constraint.Content.Column': FALSE
```

NodeTypes.yaml

```yaml
'Vendor.Package:NodeTypeName':
  childNodes:
    column1:
      options:
        silhouette: 'vendor.defaultConstraints'    
    column2:
      options:
        silhouette: 'vendor.defaultConstraints'    
  properties:
    description:
      ui:
        label: 'Description'
        aloha:
          placeholder: 'please add description ... '
      options:
        silhouette: 'vendor.text.block'
```

### Predefined silhouettes

- `text.plain`: An inline editable string where no formatting is allowed.
- `text.block`: An inline editable string where only inline formatting is enabled.
- `text.free`: An inline editable string where all formatting including blocks is allowed.

## Installation

Sitegeist.Silhouettes is available via packagist. Add `"sitegeist/silhouettes" : "^1.0"`
to the require section of the composer.json or run `composer require sitegeist/silhouettes`.

We use semantic-versioning so every breaking change will increase the major-version number.

## Contribution

We will gladly accept contributions. Please send us pull requests.
