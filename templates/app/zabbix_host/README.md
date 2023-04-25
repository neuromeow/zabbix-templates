
# Zabbix host items

## Overview

This template is designed to monitor the number of unsupported and enabled items and their ratio.

## Setup

Link this template to the Zabbix host.

## Zabbix configuration

No specific Zabbix configuration is required.

### Macros used

|Name|Description|Default|
|----|-----------|-------|
|{$ZABBIX.HOST.ITEMS_UNSUPPORTED.MIN.AVG} |<p>Minimum percentage of unsupported items on the host for a trigger expression with average severity.</p> |`10` |
|{$ZABBIX.HOST.ITEMS_UNSUPPORTED.MIN.HIGH} |<p>Minimum percentage of unsupported items on the host for a trigger expression with high severity.</p> |`90` |

## Template links

There are no template links in this template.

## Items collected

|Group|Name|Description|Type|Key and additional info|
|-----|----|-----------|----|---------------------|
|Zabbix host |Zabbix host: Number of enabled items |<p>The number of enabled items on the host (supported and not supported).</p> |INTERNAL |zabbix[host,,items] |
|Zabbix host |Zabbix host: Number of unsupported items |<p>The number of unsupported items on the host.</p> |INTERNAL |zabbix[host,,items_unsupported] |
|Zabbix host |Zabbix host: Ratio of unsupported to enabled items, in % |<p>The percentage of unsupported items relative to enabled items on the host.</p> |CALCULATED |zabbix.host.items.ratio |

## Triggers

|Name|Description|Expression|Severity|Dependencies and additional info|
|----|-----------|----|----|----|
|Zabbix host: More than {$ZABBIX.HOST.ITEMS_UNSUPPORTED.MIN.HIGH}% of items are not supported |<p>The proportion of items in an unsupported state is equal to or greater than a certain percentage, used with {$ZABBIX.HOST.ITEMS_UNSUPPORTED.MIN.HIGH} as percentage threshold.</p> |`last(/Zabbix host items/zabbix.host.items.ratio)>={$ZABBIX.HOST.ITEMS_UNSUPPORTED.MIN.HIGH}` |HIGH |**Manual close**: Yes |
|Zabbix host: More than {$ZABBIX.HOST.ITEMS_UNSUPPORTED.MIN.AVG}% of items are not supported |<p>The proportion of items in an unsupported state is equal to or greater than a certain percentage, used with {$ZABBIX.HOST.ITEMS_UNSUPPORTED.MIN.AVG} as percentage threshold.</p> |`last(/Zabbix host items/zabbix.host.items.ratio)>={$ZABBIX.HOST.ITEMS_UNSUPPORTED.MIN.AVG}` |AVERAGE |**Manual close**: Yes<br>**Depends on**:<br><ul><li>Zabbix host: More than {$ZABBIX.HOST.ITEMS_UNSUPPORTED.MIN.HIGH}% of items are not supported</li></ul> |
|Zabbix host: Unsupported items are present |<p>At least one item is in an unsupported state.</p> |`last(/Zabbix host items/zabbix[host,,items_unsupported])>0` |WARNING |**Manual close**: Yes<br>**Depends on**:<br><ul><li>Zabbix host: More than {$ZABBIX.HOST.ITEMS_UNSUPPORTED.MIN.AVG}% of items are not supported</li></ul> |
