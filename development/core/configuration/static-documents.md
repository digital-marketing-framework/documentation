# Static Documents

The configuration document management has one configured folder in which all manually created documents will be stored. In some cases, however, we want to provide other documents that can be included by a main document.

Those documents are considered to be read-only and in order to make such extra documents available, there are two events that need to be listened to.

`DigitalMarketingFramework\Typo3\Core\ConfigurationDocument\Storage\Event\StaticConfigurationDocumentIdentifierCollectionEvent`

The event `StaticConfigurationDocumentIdentifierCollectionEvent` is there to request all identifiers of extra documents so that they can be used in various lists.

`DigitalMarketingFramework\Typo3\Core\ConfigurationDocument\Storage\Event\StaticConfigurationDocumentLoadEvent`

The event `StaticConfigurationDocumentLoadEvent` is there to request the content of an extra document. Whatever package makes a new document identifier public also has to make sure that it is loaded correctly.

We distinguish between two types of extra documents: system documents and static files.

## System Documents

A system document does not exist as a file somewhere. It is generated programmatically on demand via the load event.

Its identifier should have a prefix `SYS:` followed by one or more keyowrds describing its purpose. The most famous system document is `SYS:defaults`, which generates the default configuration for all packages that are currently installed.

It is usually not necessary to implement system documents for a new package. If it was ever necessary, there is an example in the core package:

* `DigitalMarketingFramework\Typo3\Core\ConfigurationDocument\Storage\EventListener\CoreSystemConfigurationDocumentEventListener`

## Static Files

Static files refer to actual files with YAML content within a package.

In TYPO3 static file identifiers should have a prefix `EXT:` followed by the extension name and its path within the package.

The suggested path to the static configuration documents of a package is `Resources/Private/ConfigurationDocuments/*.config.yaml`.

Example: `EXT:digitalmarketingframework_distributor_pardot/Resources/Private/ConfigurationDocuments/route-defaults.config.yaml`

There is a default event listener that can be extended. It will automatically make all configuration documents in the mentioned folder publicly available.

`DigitalMarketingFramework\Typo3\Core\ConfigurationDocument\Storage\EventListener\StaticConfigurationDocumentEventListener`

Only the extension key has to be provided:

```
class MyStaticConfigurationDocumentEventListener extends StaticConfigurationDocumentEventListener
{
    protected function getExtensionKey(): string
    {
        return 'digitalmarketingframework_my_package';
    }
}
```

And the listener must be registered for both events:

```
services:
  DigitalMarketingFramework\Typo3\Distributor\MyPackage\ConfigurationDocument\Storage\EventListener\MyStaticConfigurationDocumentEventListener:
    tags:
      - name: event.listener
        event: DigitalMarketingFramework\Typo3\Core\ConfigurationDocument\Storage\Event\StaticConfigurationDocumentIdentifierCollectionEvent
      - name: event.listener
        event: DigitalMarketingFramework\Typo3\Core\ConfigurationDocument\Storage\Event\StaticConfigurationDocumentLoadEvent
```
