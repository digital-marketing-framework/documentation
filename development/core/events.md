# Events

## Registry

Registry update events are used to register plugins and services for the DMF. Since the DMF is divided into two parts - distributor and collector - each of which can work without the other, there will be two different registries if the core packages of both parts are installed.

This is why we need to use events to register plugins and other classes and objects. It might be necessary to register them in more than one registry.

We distinguish between three different domains of registries:
* `RegistryDomain::CORE` - The core registry is able to register everything that both, the distributor registry and the collector registry, can use.
* `RegistryDomain::DISTRIBUTOR` - The distributor registry can register all distributor-specific items.
* `RegistryDomain::COLLECTOR` - The collector registry can register all collector-specific items.

Regardless of which registry domain we are talking about, there are three different types of updates which need to be distinguished because the order in which updates are registered is important.

* `RegistryUpdateType::GLOBAL_CONFIGURATION` - Is there to register system-specific global configuration, like the extension configuration in TYPO3.

* `RegistryUpdateType::SERVICE` - Is there to register services, that will be used by either the system or other services and plugins. Services are treated as Singletons.
* `RegistryUpdateType::GLOBAL_PLUGIN` - Is there to register plugins. A plugin is always newly instanciated whenever it is requested.

The update types are not represented by different events because along with the domain distinction this would result in nine different events, which seems excessive and unnecessary. Instead the events of the three domains are triggered three times each, one time for each update type. The update type of the current event can be requested by calling the method `getUpdateType()`.

The suggested class name of a listener is similar, except that the own package name is already included in the namespace by default and therefore does not need to be part of the class name.

[REGISTRY-TYPE]RegistryUpdateEvent

Example: CoreRegistryUpdateEvent

### CoreRegistryUpdateEvent
* package: typo3-core
* class: DigitalMarketingFramework\Typo3\Core\Registry\Event\CoreRegistryUpdateEvent
* domain: Registry

### CollectorRegistryUpdateEvent
* package: typo3-collector-core
* class: DigitalMarketingFramework\Typo3\Collector\Core\Registry\Event\CollectorRegistryUpdateEvent
* domain: Registry

### DistributorRegistryUpdateEvent
* package: typo3-distributor-core
* class: DigitalMarketingFramework\Typo3\Distributor\Core\Registry\Event\DistributorRegistryUpdateEvent
* domain: Registry


## ConfigurationDocument

Whenever any part of the DMF is doing something, a configuration document is involved to configure the behaviour of the process.

The configuration structure is described in a schema document, consisting of value sets, custom schema types and the schema of the configuration document itself.

While it is possible for every package to add to the schema document, it is usually not necessary to add anything, because all registries (distributor and collector) are already contributing schema data for all services and plugins that have been registered. For a plugin it is enough to provide a schema definition for its custom configuration and it will be taken into account by the registry automatically.

### ConfigurationDocumentMetaDataUpdateEvent
* package: typo3-core
* class: DigitalMarketingFramework\Typo3\Core\ConfigurationDocument\Event\ConfigurationDocumentMetaDataUpdateEvent
* domain: ConfigurationDocument


## ConfigurationDocument\Storage

A configuration document is usually embedded in another component like a form definition or a system plugin. However, since a configuration document can include parent documents to inherit their configuration, there is also a central place to hold configuration documents that can be re-used from different locations.

A document can inherit from multiple documents, which in turn can all inherit from multiple other documents. The result will be a configuration stack, which is then merged to a single configuration object when used.

There are two kinds of configuration documents that can be inherited. The first kind is the one that is built by the user in a central location. The second kind is called static confguration document and it consists of read-only documents that are provided by different packages. For TYPO3 those can either be actual configuration document files or it can be programmatically generated configuration documents. The most famous generated document is SYS:defaults, which is always having the latest default values of all packages currently installed.

For static configuration documents there are events with which DMF packages can make their documents publicly available. There is a collection event which is requesting all identifiers of static configuration documents and a load event which is requesting the actual configuration document.

### StaticConfigurationDocumentIdentifierCollectionEvent
* package: typo3-core
* class: DigitalMarketingFramework\Typo3\Core\ConfigurationDocument\Storage\Event\StaticConfigurationDocumentIdentifierCollectionEvent
* domain: ConfigurationDocument\Storage

### StaticConfigurationDocumentLoadEvent
* package: typo3-core
* class: DigitalMarketingFramework\Typo3\Core\ConfigurationDocument\Storage\Event\StaticConfigurationDocumentLoadEvent
* domain: ConfigurationDocument\Storage


## Controller

The TYPO3 backend module has only one section by default, which is the central configuration document management. However, other sections can be added by additional packages. In order to allow other controllers and their actions to be available in the module, there is a backend controller update event, which is requesting all controllers and actions so that they can be allowed in the module definition.

### BackendControllerUpdateEvent
* package: typo3-core
* class: DigitalMarketingFramework\Typo3\Core\Controller\Event\BackendControllerUpdateEvent
* domain: Controller


## Extensions\Form

The distributor package has one default entry point which is a form finisher for the integration in the TYPO3 system extension sysext:form. In order to produce a valid data object from a form submission, the package needs to be able to process form elements. All standard elements are recognised and processed automatically. For custom form elements, it is possible to add a form element processor in form of an event.

### FormElementProcessorEvent
* package: typo3-distributor-core
* class: DigitalMarketingFramework\Typo3\Distributor\Core\Extensions\Form\FormElementProcessorEvent
* domain: Extensions\Form
