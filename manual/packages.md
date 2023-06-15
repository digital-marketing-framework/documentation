
# Packages

This section consists of all DMF packages that are currently officially available. As the DMF is a plugin system that can be extended by everybody, there may be more unofficial packages, which are not included in this list.

## Core

* [digital-marketing-framework/core](https://github.com/digital-marketing-framework/core)
* [digital-marketing-framework/typo3-core](https://github.com/digital-marketing-framework/typo3-core)

The core system is the backbone of the DMF. While it does not do anything on its own, it provides basic functionality and definitions that will be used by other packages. Is also contains the implementation of the [configuration document management](configuration-documents.md).

* [digital-marketing-framework/typo3-cache](https://github.com/digital-marketing-framework/typo3-cache)

This TYPO3-specific package provides a basic implementation of a data cache. It is using whatever cache system is provided the particular by TYPO3 and therefore does not come with any additional requirements.

## Distributor

* [digital-marketing-framework/distributor-core](https://github.com/digital-marketing-framework/distributor-core)
* [digital-marketing-framework/typo3-distributor-core](https://github.com/digital-marketing-framework/typo3-distributor-core)

The distributor core system is the backbone of the distributor part of the DMF. It is providing basic functionality and definitions to be used by other distributor packages and the TYPO3 package installs a new form finisher for the system extension EXT:form which can be used to process form submissions. However, without any additional distributor packages, the core will not do anything.

* [digital-marketing-framework/distributor-request](https://github.com/digital-marketing-framework/distributor-request)
* [digital-marketing-framework/typo3-distributor-request](https://github.com/digital-marketing-framework/typo3-distributor-request)

This package provides a distributor route for HTTP requests.

* [digital-marketing-framework/distributor-pardot](https://github.com/digital-marketing-framework/distributor-pardot)
* [digital-marketing-framework/typo3-distributor-pardot](https://github.com/digital-marketing-framework/typo3-distributor-pardot)

This package provides a distributor routes for Pardot, using the Pardot Form Handler system.

* [digital-marketing-framework/distributor-collector-data-provider](https://github.com/digital-marketing-framework/distributor-collector-data-provider)

This package provides a distributor data provider that is able to add user data from the collector to the current data set, e.g. a form submission.

* [digital-marketing-framework/distributor-cache](https://github.com/digital-marketing-framework/distributor-cache)
* [digital-marketing-framework/typo3-distributor-cache](https://github.com/digital-marketing-framework/typo3-distributor-cache)
* [digital-marketing-framework/distributor-cache-pardot](https://github.com/digital-marketing-framework/typo3-distributor-cache-pardot)
* [digital-marketing-framework/typo3-distributor-cache-pardot](https://github.com/digital-marketing-framework/distributor-cache-pardot)

The distributor-cache packages enables the system to shortcircuit the user data so that it is not only sent to the tartget system but at the same time saved in the collector data cache, so that it does not have to be requested from the target system once the user data is needed again, probably right on the next page that the user is visiting.

## Collector

* [digital-marketing-framework/collector-core](https://github.com/digital-marketing-framework/collector-core)
* [digital-marketing-framework/typo3-collector-core](https://github.com/digital-marketing-framework/typo3-collector-core)

WIP

* [digital-marketing-framework/collector-pardot](https://github.com/digital-marketing-framework/collector-pardot)
* [digital-marketing-framework/typo3-collector-pardot](https://github.com/digital-marketing-framework/typo3-collector-pardot)

WIP
