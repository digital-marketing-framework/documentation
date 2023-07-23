
# Object Awareness

All objects that are created by the Registry service, have their awareness processed.

If you want to create an object yourself and take its awareness into account, you can use the method `Registry::createObject(string $class, array $arguments = []): object`.

Awareness is a method of automatically injecting certain global services and other objects into a class instance. Unlike the classical dependency injection it is not done via the constructor method and it is not restricted to services. All classes can use this kind of injection if they are instanciated via the Registry service and not only services can be injected. A detailed list of injectable objects can be found below.

To make your class aware of a service, you have to implement the corresponding interface. For every `AwareInterface` there is also an `AwareTrait` that is alreday having the standard implementation of the given interface.

## GlobalConfigurationAwareInterface

Injected class: `DigitalMarketingFramework\Core\Model\Configuration\ConfigurationInterface`

Member variable: `$globalConfiguration`

The global configuration has the same structure as the configurations coming from configuration documents, but the implementation is usually very different. Its purpose is to provide system-wide configuration that is not dependent on a given use-case.

In TYPO3 the global configuration object is giving access to the extension settings.

## LoggerAwareInterface

Injected class: `DigitalMarketingFramework\Core\Log\LoggerInterface`

Member variable: `$logger`

An injected logger gives the object the ability to log information. The implementation depends on the surrounding system. In TYPO3 the standard TYPO3 log system is used.

## ContextAwareInterface

Injected class: `DigitalMarketingFramework\Core\Context\ContextInterface`

Member variable: `$context`

The context holds information about the currently processed request like cookies, headers, the IP address, the timestamp and other data that may be specific to the surrounding system, like the current language or country.

Note, that the distributor can work asynchronously, in which case the global context that can be injected with the `ContextAwareInterface` will not reflect the context of the original request. In those cases the relevant parts of the original context have to have been stored as part of the processed job and needs to be retrieved differently.

## DataCacheAwareInterface

Injected class: `DigitalMarketingFramework\Core\Cache\DataCacheInterface`

Member variable: `$cache`

The data cache is used to save the results of collected user data temporarily to reduce the number of requests needed from Collectors. Cache entries are user- and collector-specific.

## DataProcessorAwareInterface

Injected class: `DigitalMarketingFramework\Core\DataProcessor\DataProcessorInterface`

Member variable: `$dataProcessor`

The [data processor](data-processor/index.md) is a service that is able to execute all data processing plugins, namingly:

* [`DataMapper`](data-processor/data-mapper.md)
* [`Value`](data-processor/value.md)
  * [`ValueSource`](data-processor/value-source.md)
  * [`ValueModifier`](data-processor/value-modifier.md)
* [`Evaluation`](data-processor/evaluation.md)
  * [`Comparison`](data-processor/comparison.md)

## FileStorageAwareInterface

Injected class: `DigitalMarketingFramework\Core\FileStorage\FileStorageInterface`

Member variable: `$fileStorage`

The file storage service abstracts file handling and allows the surrounding system to inject its own way of handling files and folders. It operates in strings called identifiers, which can but don't have to be file paths.
