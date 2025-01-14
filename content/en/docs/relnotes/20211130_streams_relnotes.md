---
title: Streams November 2021 Release Notes
linkTitle: Streams November 2021 Release Notes
weight: 145
date: 2021-09-03
---

## Summary

Streams is available as a set of Docker containers deployable in Kubernetes by using a Helm chart. For a summary of the system requirements, see [Install Streams](/docs/install/).

## Important changes
<!-- Use this section to describe any changes in the behavior of the product (as a result of features or fixes), for example, new Java system properties in the jvm.xml file. This section could also be used for any important information that doesn't fit elsewhere. -->

It is important, especially when upgrading from an earlier version, to be aware of the following changes in the behavior or operation of the product in this new version.

### Streams third-parties

The following third-party library has been upgraded:

* MariaDB : `10.4.21`
* Apache Kafka : `2.8.1`

### Embedded Kafka configuration

To address security vulnerabilities, Apache Kafka has been upgraded to version 2.8.1. This upgrade introduces breaking changes in the configuration of embedded kafka - the SASL
configuration is now in the `embeddedKafka.auth.sasl` block of the Helm values.  For more information, see [Helm parameters](/docs/install/helm-parameters/#kafka-parameters).

We have also changed some internal configuration of the Kafka containers to improve security. There is no visible change unless you have disabled or customized some security of embedded kafka as
described in [the installation](/docs/install/#embedded-kafka-configuration). In that case, be aware
that `embeddedKafka.auth.sasl.jaas.clientPasswordSecret` has replaced `embeddedKafka.auth.jaas.existingSecret`.

## Deprecated features
<!-- As part of our software development life cycle, we constantly review our Streams offering. -->

As part of this review, no capabilities have been deprecated.

## Removed features
<!-- To stay current and align our offerings with customer demand and best practices, Axway might discontinue support for some capabilities. -->

As part of this review, no features have been deprecated.

## Fixed issues

There are no fixed issues in this version.

## Documentation

To find all available documents for this product version:

Go to Manuals on the Axway Documentation portal.

1. Go to [Product Manuals](https://docs.axway.com/bundle), in the Axway Documentation portal.
2. In the left pane Filters list, select your product or product version.

Customers with active support contracts need to log in to access restricted content.

## Support services

The Axway Global Support team provides worldwide 24 x 7 support for customers with active support agreements.

Email [support@axway.com](mailto:support@axway.com) or visit Axway Support at <https://support.axway.com>.
