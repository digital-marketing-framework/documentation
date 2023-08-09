
# Namespaces and Package Names

While it is technically possible to use any package name and any namespace prefixes, here are the general rules to keep at least the official packages consistent and easy to navigate.

## Package Name

The vendor of the packages is always `digital-marketing-framework`.

The rest of the package name depends on its purpose.

### PHP

The package name is composed of the domain (`distributor` or `collector`) and a descriptive name of the package. Unfortunately it is not possible to have more than one `/` as separator in a composer package name, which is why we use `-` as additional separator.

`digital-marketing-framework/<DOMAIN>-<DESCRIPTIVE-NAME>`

If a package adds a distributor route called `json-request`, the name would be `digital-marketing-framework/distributor-json-request`. If a package adds a data collector called `pardot`, the name would be `digital-marketing-framework/collector-pardot`.

If a package is adding something other than a route or a data collector, additional keywords should imply what is added. If a package is adding a distributor data provider called `timestamp`, the name would be `digital-marketing-framework/distributor-timestamp-provider`.

* `digital-marketing-framework/distributor-json-request`
* `digital-marketing-framework/collector-pardot`
* `digital-marketing-framework/distributor-timestamp-provider`

### TYPO3

For TYPO3 extensions the package naming conventions are very similar to conventions of plain PHP packages. The only difference is that the first keyword after the vendor name is `typo3`. Everything else stays the same.

* `digital-marketing-framework/typo3-distributor-json-request`
* `digital-marketing-framework/typo3-collector-pardot`
* `digital-marketing-framework/typo3-distributor-timestamp-provider`

### Drupal (WIP)

## Namespaces

The namespace prefix is derived directly from the package name. Fortunately we don't need to use the `-` character here.

### PHP

* `DigitalMarketingFramework\Distributor\JsonRequest`
* `DigitalMarketingFramework\Collector\Pardot`
* `DigitalMarketingFramework\Distributor\TimestampProvider`

### TYPO3

* `DigitalMarketingFramework\Typo3\Distributor\JsonRequest`
* `DigitalMarketingFramework\Typo3\Collector\Pardot`
* `DigitalMarketingFramework\Typo3\Distributor\TimestampProvider`

### Drupal (WIP)
