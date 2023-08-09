# Registry

## Domain

There are three domains under which the registry operates.

* Core
* Distributor
* Collector

### Core

The domain `Core` represents everything that is shared between the distributor and the collector. Most prominent are the [data processor plugins](../data-processor/index.md), but there are also other shared plugins and shared services.

### Distributor

The domain `Distributor` encompasses everything that is used by the distributor but not by the collector. Most prominent are [routes](../../distributor/routes.md), [data providers](../../distributor/data-providers.md) and [data dispatchers](../../distributor/data-dispatchers.md).

### Collector

The domain `Collector` encompasses everything that is used by the collector but not by the distributor. Most prominent are the [data collectors](../../collector/data-collectors.md).

## Update Types

There are three different types of registry updates.

* Global Configuration
* Services
* Plugins

### Global Configuration

The global configuration is not coming from [configuration documents](). It is system-wide configuration that is added from the surrounding system and it is empty by default. In TYPO3, for example, we use extension settings as global configuration.

Services and plugins may need to use the global configuration which is why we have to ensure that the global configuration is updated first.

### Services

Services are singleton classes which server a specific purpose that is needed in multiple locations.

They may rely on global configuration to be present, which why they come second.

There are default implementations for all services, though some may be dummy implementations without any actual functionality.

### Plugins

Every plugin implements a specific interface which defines the type of the plugin.

Plugins are not singletons and they are created on demand. Which implementation is used depends on the given keyword from the requesting party.

Plugins may need global configuration and services, which is why they are registered last.

## Implementing Registry Updates

### PHP

The plain PHP packages do not add any global configuration automatically. This is something that the surrounding system will have to do.

The plain PHP packages also do not change the registered services. The registry already has a default implementation for all services. If a more specific version of a service should be used, that would have to be coming from the surrounding system.

The `Initialization` classes from PHP packages are mostly there to add plugins to the system. The class name should be a shorthand for the package name with the suffix `Initialization`, e.g. `DistributorPardotInitialization`.

The plugin classes are grouped by domain and by plugin type.

```
protected const PLUGINS = [
	RegistryDomain::CORE => [
		ValueSourceInterface::class => [
			ConstantValueSource::class,
			FieldValueSource::class,
			// ...
		],
		ValueModifierInterface::class => [
			TrimValueModifier::class,
			// ...
		],
		EvaluationInterface::class => [
			AndEvaluation::class,
			OrEvaluation::class,
			// ...
		],
	],
	RegistryDomain::DISTRIBUTOR => [
		RouteInterface::class => [
			MailRoute::class,
			JsonRequestRoute::class,
			// ...
		],
		DataProviderInterface::class => [
			TimestampDataProvider::class,
			// ...
		],
		DataDispatcherInterface::class => [
			MailDataDispatcher::class,
			RequestDataDispatcher::class,
			// ...
		],
	],
	RegistryDomain::COLLECTOR => [
		DataCollectorInterface::class => [
			PardotDataCollector::class,
			// ...
		],
	],
];
```

Besides the plugin information of the given package the `Initialization` class also requires as constructor arguments the package name and the current schema version. The latter is needed to build targeted migrations in the future.

```
public function __construct()
{
	parent::__construct('my-package', '3.5.1');
}
```

When a schema has changed, a [migration](../configuration/migrations.md) can be implemented and registered in the `Initialization` class as well. The class name has no higher meaning. The source and target versions are held in the class and not derived from the class name.

```
protected const SCHEMA_MIGRATIONS = [
	MyMigration1::class,
	MyMigration2::class,
];
```

### TYPO3

In TYPO3 are three different [events](../events.md) that can be listened to to update the registry - one fore each domain.

* `CoreRegistryUpdateEvent`
* `DistributorRegistryUpdateEvent`
* `CollectorRegistryUpdateEvent`

Depending what a new package needs to register, it might be necessary to listen to more than one of those event. Though it should never be necessary for a package to listen to both the distributor and collector update event.

Each event provides the registry, that should be updated, and the type of update that is currently requested - global configuration, services or plugins.

There are already classes to make the event listening easier.

* `AbstractCoreRegistryUpdateEventListener`
* `AbstractDistributorRegistryUpdateEventListener`
* `AbstractCollectorRegistryUpdateEventListener`

Each of those event listeners needs the corresponding `Initialization` class as constructor argument.

```
class MyCoreRegistryUpdateEventListener extends AbstractCoreRegistryUpdateEventListener
{
	public function __construct()
	{
		parent::__construct(new MyInitialization());
	}
}
```
By extending the abstract listener class, plugins defined in the `Initialization` class will be registered automatically in all domains. For global configuration updates, service updates or special cases for plugin updates, there are corresponding methods that can be extended in the listener classes.

```
protected function initGlobalConfiguration(RegistryInterface $registry): void
{
	parent::initGlobalConfiguration($registry);
	// add your global configuration updates here
}

protected function initServices(RegistryInterface $registry): void
{
	parent::initServices($registry);
	// add your service updates here
}

protected function initPlugins(RegistryInterface $registry): void
{
	parent::initPlugins($registry);
	// add your plugin updates here
}
```

As a rule of thumb, a distributor package should always implement the `CoreRegistryUpdateEventListener` and the `DistributorRegistryUpdateEventListener`. And a collector package should always implement the `CoreRegistryUpdateEventListener` and the `CollectorRegistryUpdateEventListener`. Technically it may not always be necessary to implement both listeners but when we always implement both listeners, we won't forget to register a plugin in all needed domains.
