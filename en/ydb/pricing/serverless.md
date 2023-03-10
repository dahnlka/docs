# Pricing policy for {{ ydb-name }} Serverless mode



{% include [currency-choice](../_includes/pricing/currency-choice.md) %}


When you use {{ ydb-name }} in Serverless mode, you are billed for each request made to the database. Users don't have to indicate the resources they need: the database quickly adapts to changes in the user load. Apart from requests, the user pays for the data stored in {{ ydb-name }} on an hourly basis. Other operations, such as restoring data from backups, are charged additionally.

## What goes into the cost of using {{ ydb-name }} Serverless mode {#rules}

When using {{ ydb-name }} in Serverless mode, you pay for the following:
* Data operations.
* The amount of stored data, including service data, such as indexes.
* Additional user operations, like restoring data from backups.

Other consumed resources to be additionally paid for:

* The space used in {{ objstorage-full-name }} to store on-demand backups.
* Egress traffic from {{ yandex-cloud }}.

{% include [pricing-gb-size](../_includes/pricing/pricing-gb-size.md) %}

### Data operations and request units {#rules-ru}

The serverless mode of {{ ydb-name }} supports multiple ways to work with data:
* YQL is an SQL-like language for accessing relational tables, supported by the [SDK](https://ydb.tech/en/docs/reference/ydb-sdk/), [CLI](https://ydb.tech/en/docs/reference/ydb-cli/), and management console in {{ ydb-short-name }}.
* Document API is the Amazon DynamoDB-compatible HTTP API. You can use this API to perform operations on document tables.

You can also query the database through the special APIs available as stand-alone features in the {{ ydb-short-name }} SDK, CLI, or management console.

To calculate the cost of requests in {{ ydb-short-name }}, we use so-called _request units (RU)_. Each executed request, depending on its type, complexity, and data size, consumes a certain number of RU. The total cost of all executed requests to {{ ydb-short-name }} is the sum of the RU costs for each request.

Rules for calculating the cost of requests to {{ ydb-short-name }} in RU:
* [YQL](ru-yql.md).
* [Document API](ru-docapi.md).
* [Special APIs](ru-special.md).
* [Topic operations](ru-topics.md).

#### Pricing for Request Unit consumption {#prices-ru}

* **Actual consumption (on-demand)**. The cost of using {{ ydb-short-name }} for a certain period is the number of spent Request Units multiplied by the price per Request Unit minus the monthly [free package](../../billing/concepts/serverless-free-tier.md#ydb). If there were no requests to the database or if the amount of RU spent in the current month is less than the free package, there is no charge for actual RU consumption.
* **Provisioned capacity**. If your load is predictable and has a fixed component, you can reduce the cost of using {{ ydb-short-name }} by paying part (or all) of RU spent at an hourly rate. To do this, set a non-zero value of the database parameter "Provisioned capacity, RU/s". After that, you are charged according to a special pricing plan, the rate for which is set to RU/s × hour, with billing per second. The consumption of RU within the specified capacity isn't added to the RU spent at the on-demand rate.

### Amount of data stored {#rules-storage}

In Serverless mode, data storage capacity is allocated automatically. The amount of stored data is calculated as the total amount of user and service data stored in the database. For example, creating a global index increases the total storage size by the index size.

For topics with on-demand pricing, you also pay for the actually used disk space. Its usage starts once each message is published in a topic and it's released once the retention period set for the topic expires.

### Creating backups {#rules-backup-storage}

#### Automatic backups {#rules-auto-backup-storage}

{{ ydb-name }} automatically creates and stores two full backups for the last two days free of charge. No fee is charged for storing automatic backups.

#### On-demand backups {#rules-auto-backup-storage}

You can force a database backup, saving a copy to [{{ objstorage-name }}](../../storage/). The cost of this operation depends on the size of copied data and is calculated similarly to the [ReadTable](ru-special.md#readtable) operation. When calculating the cost, the actual amount is rounded up to a multiple of 1 GB.

{% note warning %}

If you export data using the `ydb tools dump` utility, billing is based on the rates for the `ReadTable` operation.

{% endnote %}

#### Example of calculating the cost of creating an on-demand backup {#example-of-cost-calculation-backup-storage}

Below, you can find sample cost calculations for creating backups of 1GB and 10GB databases.

**Cost calculation for a database of 1 GB**

Actual (on-demand) consumption of RUs per month will be:

> 128 RU × 1024 = 131072 RU

Where:
* 128: Cost of request for 1 MB of data.
* 1024: Amount of data copied, in MB.

The number of RUs used (131072) is [less than 1000000](#prices), so creating a backup will be free of charge.

**Cost calculation for a database of 10 GB**

Actual (on-demand) consumption of RUs per month will be:

> 128 RU × 1024 × 10 = 1310720 RU

Where:
* 128: Cost of request for 1 MB of data.
* 1024 × 10: Amount of data copied, in MB.

The number of RUs used (1310720) [exceeds 1000000](#prices), so the cost of creating a backup will be $0.171040 for 1000000 RU.

> (1310720 RU - 1000000 RU) / 1000000 × $0.171040 = $0.0531455488
>
> Total: $0.053146 is the cost of creating a backup of 10 GB of data.

Where:
* 1310720 RU is actual (on-demand) consumption of RUs per month.
* $0.171040: Cost of 1 million RUs.

### Restoring data from backups {#rules-backup-restore}

You can restore databases and individual tables from the backups stored in {{ objstorage-name }}. The cost of this operation depends on the size of recovered data and is calculated similarly to the [BulkUpsert](ru-special.md#bulkupsert) operation. When calculating the cost, the actual amount is rounded up to a multiple of 1 GB.

{% note warning %}

If you restore data using the `ydb tools restore` utility, billing is based on the cost of writing a row to the DB for each restored row.

{% endnote %}

#### Example of calculating the cost of backup recovery {#example-of-cost-calculation-backup-restore}

Below, you can find sample cost calculations for backup recovery for 1GB and 10GB databases.

**Cost calculation for a database of 1 GB**

Actual (on-demand) consumption of RUs per month will be:

> 0.5 RU × 1024 × 1024 = 524288 RU

Where:
* 0.5: Cost of request for 1 KB of data.
* 1024 × 1024: Amount of data recovered, in KB.

The number of RUs used (524288) is [less than 1000000](#prices), so recovery from a backup will be free of charge.

**Cost calculation for a database of 10 GB**

Actual (on-demand) consumption of RUs per month will be:

> 0.5 RU × 1024 × 1024 × 10 = 5242880 RU

Where:
* 0.5: Cost of request for 1 KB of data.
* 1024 × 1024 × 10: Amount of data recovered, in KB.

The number of RUs used (5242880) [exceeds 1000000](#prices), so the cost of recovery from a backup will be $0.171040 for 1000000 RUs.

> (5242880 RU - 1000000 RU)/ 1000000 × $0.171040 = $0.7257021952
>
> Total: $0.725702 is the cost of recovery from a backup of 10 GB of data.

Where:
* 5242880 RU: Actual (on-demand) consumption of RUs per month.
* $0.171040: Cost of 1 million RUs.

## Pricing  {#prices}





{% include notitle [usd-serverless](../../_pricing/ydb/usd-serverless.md) %}





{% include notitle [usd-egress-traffic.md](../../_pricing/usd-egress-traffic.md) %}



