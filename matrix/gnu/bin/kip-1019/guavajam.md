KIP-1019: Expose method to determine Metric Measurability
================

Created by [Apoorv
Mittal](https://cwiki.apache.org/confluence/display/~apoorvmittal10),
last modified on [Apr 19,
2024](https://cwiki.apache.org/confluence/pages/diffpagesbyversion.action?pageId=290982791&selectedPageVersions=6&selectedPageVersions=7 "Show changes")

- [Status](https://cwiki.apache.org/confluence/display/KAFKA/KIP-1019%3A+Expose+method+to+determine+Metric+Measurability#KIP1019:ExposemethodtodetermineMetricMeasurability-Status)

- [Motivation](https://cwiki.apache.org/confluence/display/KAFKA/KIP-1019%3A+Expose+method+to+determine+Metric+Measurability#KIP1019:ExposemethodtodetermineMetricMeasurability-Motivation)

- [Public
  Interfaces](https://cwiki.apache.org/confluence/display/KAFKA/KIP-1019%3A+Expose+method+to+determine+Metric+Measurability#KIP1019:ExposemethodtodetermineMetricMeasurability-PublicInterfaces)

- [Proposed
  Changes](https://cwiki.apache.org/confluence/display/KAFKA/KIP-1019%3A+Expose+method+to+determine+Metric+Measurability#KIP1019:ExposemethodtodetermineMetricMeasurability-ProposedChanges)

- [Compatibility, Deprecation, and Migration
  Plan](https://cwiki.apache.org/confluence/display/KAFKA/KIP-1019%3A+Expose+method+to+determine+Metric+Measurability#KIP1019:ExposemethodtodetermineMetricMeasurability-Compatibility,Deprecation,andMigrationPlan)

- [Test
  Plan](https://cwiki.apache.org/confluence/display/KAFKA/KIP-1019%3A+Expose+method+to+determine+Metric+Measurability#KIP1019:ExposemethodtodetermineMetricMeasurability-TestPlan)

- [Rejected
  Alternatives](https://cwiki.apache.org/confluence/display/KAFKA/KIP-1019%3A+Expose+method+to+determine+Metric+Measurability#KIP1019:ExposemethodtodetermineMetricMeasurability-RejectedAlternatives)

  - [Wrapper Object for exception-based measurability
    checks](https://cwiki.apache.org/confluence/display/KAFKA/KIP-1019%3A+Expose+method+to+determine+Metric+Measurability#KIP1019:ExposemethodtodetermineMetricMeasurability-WrapperObjectforexception-basedmeasurabilitychecks)

# Status

**Current state**: Adopted (3.8.0)

**Discussion thread**:
[*here*](https://www.mail-archive.com/dev@kafka.apache.org/msg137597.html)*  
*

**JIRA**: [here](https://issues.apache.org/jira/browse/KAFKA-16280)*  
*

Please keep the discussion on the mailing list rather than commenting on
the wiki (wiki discussions get unwieldy fast).

# Motivation

Kafka currently lacks a straightforward and efficient mechanism for
determining whether a given metric is `measurable`. Existing approaches
often rely on:

- **Exception-Based Measurability Check:** While
  [measurable](https://github.com/apache/kafka/blob/d24abe0edebad37e554adea47408c3063037f744/clients/src/main/java/org/apache/kafka/common/metrics/KafkaMetric.java#L65)()
  does provide the value provider, it throws an exception if the
  provider isn’t an instance of `Measurable`. This means that checking
  for measurability incurs the overhead of exception handling and stack
  trace creation, in cases where the metric is not measurable.

- **Accessing Value Provider Field:**
  [KafkaMetricsCollector](https://github.com/apache/kafka/blob/5cfcc52fb3fce4a43ca77df311382d7a02a40ed2/clients/src/main/java/org/apache/kafka/common/telemetry/internals/KafkaMetricsCollector.java#L265)
  resort to Java reflection to inspect the underlying value provider
  field by making it accessible outside KafkaMetric class. This approach
  is fragile and can break with future Kafka changes.

This small KIP proposes to introduce a new method, `isMeasurable()`, to
the `KafkaMetric` class, eliminating the need for accessing private
fields or handling exceptions in case of non-measurable metrics.

# Public Interfaces

- Modification of the `KafkaMetric` class to include `isMeasurable()`
  method.

<table>
<colgroup>
<col style="width: 101%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><code>/**</code></p>
<p><code>* The method determines if the metric value provider is of type Measurable.</code></p>
<p><code>*</code></p>
<p><code>* @return true if the metric value provider is of type Measurable, false otherwise.</code></p>
<p><code>*/</code></p>
<p><code>public</code> <code>boolean</code>
<code>isMeasurable();</code></p></td>
</tr>
</tbody>
</table>

# Proposed Changes

1.  **Add isMeasurable() Method in KafkaMetric.java:**

    - Introduce a new method to `KafkaMetric` class:
      `boolean isMeasurable()`.

    - This method would internally check if the metric’s underlying
      value provider implements the `Measurable` interface.

    - Returns `true` if the metric is measurable, `false` otherwise.

# Compatibility, Deprecation, and Migration Plan

- Since this is a completely new API, no backward compatibility concerns
  are anticipated.

- Since nothing is deprecated in this KIP, users have no need to migrate
  unless they want to.

# Test Plan

- Verification of `isMeasurable()`’s behaviour and integration with
  existing functionalities.

# Rejected Alternatives

### Wrapper Object for exception-based measurability checks

*Summary:* This approach considered creating a wrapper object around
`KafkaMetric` to mitigate the performance overhead associated with the
exception-based measurable method.Similar to [Open-Telemetry’s Kafka
Client](https://github.com/open-telemetry/opentelemetry-java-instrumentation/blob/7a044f576fc06e041e07b4315a9afb0e1d081d4b/instrumentation/kafka/kafka-clients/kafka-clients-common/library/src/main/java/io/opentelemetry/instrumentation/kafka/internal/KafkaMetricRegistry.java#L92C46-L97)
adapter, the wrapper would store the measurability information to avoid
repeated exception handling.

*Rejected because:* While the wrapper concept addresses the issue of
exception-based checks, it introduces additional complexity and
potential overheads of maintaining the wrapper objects of KafkaMetric
itself. The proposed `isMeasurable()` method provides a more focused and
efficient solution for determining metric measurability and reducing
unnecessary burden on users with wrapper objects.
