KIP-1020: Move `window.size.ms` and `windowed.inner.serde.class` from
`StreamsConfig` to TimeWindowedDe/Serializer class
================

Created by [Lucia
Cerchie](https://cwiki.apache.org/confluence/display/~cerchie), last
modified by [Matthias J.
Sax](https://cwiki.apache.org/confluence/display/~mjsax) on [May 02,
2024](https://cwiki.apache.org/confluence/pages/diffpagesbyversion.action?pageId=290982804&selectedPageVersions=13&selectedPageVersions=14 "Show changes")

[Status](https://cwiki.apache.org/confluence/pages/viewpage.action?pageId=290982804#KIP1020:Move%60window.size.ms%60and%60windowed.inner.serde.class%60from%60StreamsConfig%60toTimeWindowedDe/Serializerclass-Status)

- [Motivation](https://cwiki.apache.org/confluence/pages/viewpage.action?pageId=290982804#KIP1020:Move%60window.size.ms%60and%60windowed.inner.serde.class%60from%60StreamsConfig%60toTimeWindowedDe/Serializerclass-Motivation)

- [Public
  Interfaces](https://cwiki.apache.org/confluence/pages/viewpage.action?pageId=290982804#KIP1020:Move%60window.size.ms%60and%60windowed.inner.serde.class%60from%60StreamsConfig%60toTimeWindowedDe/Serializerclass-PublicInterfaces)

- [Proposed
  Changes](https://cwiki.apache.org/confluence/pages/viewpage.action?pageId=290982804#KIP1020:Move%60window.size.ms%60and%60windowed.inner.serde.class%60from%60StreamsConfig%60toTimeWindowedDe/Serializerclass-ProposedChanges)

- [Compatibility, Deprecation, and Migration
  Plan](https://cwiki.apache.org/confluence/pages/viewpage.action?pageId=290982804#KIP1020:Move%60window.size.ms%60and%60windowed.inner.serde.class%60from%60StreamsConfig%60toTimeWindowedDe/Serializerclass-Compatibility,Deprecation,andMigrationPlan)

- [Test
  Plan](https://cwiki.apache.org/confluence/pages/viewpage.action?pageId=290982804#KIP1020:Move%60window.size.ms%60and%60windowed.inner.serde.class%60from%60StreamsConfig%60toTimeWindowedDe/Serializerclass-TestPlan)

- [Rejected
  Alternatives](https://cwiki.apache.org/confluence/pages/viewpage.action?pageId=290982804#KIP1020:Move%60window.size.ms%60and%60windowed.inner.serde.class%60from%60StreamsConfig%60toTimeWindowedDe/Serializerclass-RejectedAlternatives)

# Status

**Current state**: *Accepted*

**Discussion thread**:
[*here*](http://mail-archives.apache.org/mod_mbox/kafka-dev/201501.mbox/%3CCAOeJiJh6Vkkca85bWYgkeOZ8rC6%2BKDh7zzq8vMKECL_7PNExTA%40mail.gmail.com%3E)[](https://lists.apache.org/list.html?dev@kafka.apache.org)*  
*

**JIRA**:
[*here*](https://issues.apache.org/jira/projects/KAFKA/issues/KAFKA-16260?filter=allissues)
*  
*

Please keep the discussion on the mailing list rather than commenting on
the wiki (wiki discussions get unwieldy fast).

# Motivation

`window.size.ms and windowed.inner.serdes.class` are not true
KafkaStreams configuration pieces, and are intended to be used for
console client use cases. 

# Public Interfaces

- docs/streams/developer-guide/config-streams

In TimeWindowedSerializer:  
  “windowed.inner.serializer.class”  
In TimeWindowedDeserializer:  
  “windowed.inner.deserializer.class”  
   “[window.size.ms](http://window.size.ms/)”

In TimeWindowedSerializer:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><code>public</code> <code>static</code> <code>final</code>
<code>String WINDOWED_INNER_SERIALIZER_CLASS = "windowed.inner.serializer.class";</code></p>
<p><code>private</code> <code>static</code> <code>final</code>
<code>String WINDOWED_INNER_SERIALIZER_CLASS = "Default serializer for the inner class of a windowed record. Must implement the "</code>
<code>+</code></p>
<p><code>"&lt;code&gt;org.apache.kafka.common.serialization.Serde&lt;/code&gt; interface.";</code></p></td>
</tr>
</tbody>
</table>

In TimeWindowedDeserializer:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><code>/** {@code window.size.ms} */</code></p>
<p><code>public</code> <code>static</code> <code>final</code>
<code>String WINDOW_SIZE_MS_CONFIG = "window.size.ms";</code></p>
<p><code>private</code> <code>static</code> <code>final</code>
<code>String WINDOW_SIZE_MS_DOC = "Sets window size for the deserializer in order to calculate window end times.";</code></p>
<p><code>public</code> <code>static</code> <code>final</code>
<code>String WINDOWED_INNER_DESERIALIZER_CLASS = "windowed.inner.deserializer.class"";</code></p>
<p><code>private</code> <code>static</code> <code>final</code>
<code>String  WINDOWED_INNER_DESERIALIZER_CLASS  = "Default deserializer for the inner class of a windowed record. Must implement the "</code>
<code>+</code></p>
<p><code>"&lt;code&gt;org.apache.kafka.common.serialization.Serde&lt;/code&gt; interface.";</code></p></td>
</tr>
</tbody>
</table>

In StreamsConfig.java:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><code>/** {@code window.size.ms</code></p>
<p><code>* @deprecated since 3.8.0 Use "window.size.ms" in TimeWindowedDeserializer instead.} */</code></p>
<p><code>@Deprecated</code></p>
<p><code>public</code> <code>static</code> <code>final</code>
<code>String WINDOW_SIZE_MS_CONFIG = "window.size.ms";</code></p>
<p><code>private</code> <code>static</code> <code>final</code>
<code>String WINDOW_SIZE_MS_DOC = "Sets window size for the deserializer in order to calculate window end times.";</code></p>
<p><code>...</code></p>
<p><code>/** {@code windowed.value.serde.inner</code></p>
<p><code>* @deprecated since 3.8.0 Use Use "windowed.inner.serializer.class" and "windowed.inner.deserializer.class" in TimeWindowedSerializer and TimeWindowedDeserializer instead. */</code></p>
<p><code>@SuppressWarnings("WeakerAccess")</code></p>
<p><code>@Deprecated</code></p>
<p><code>...</code></p>
<p><code>public</code> <code>static</code> <code>final</code>
<code>String WINDOWED_INNER_CLASS_SERDE = "windowed.inner.class.serde";</code></p>
<p><code>private</code> <code>static</code> <code>final</code>
<code>String WINDOWED_INNER_CLASS_SERDE_DOC = " Default serializer / deserializer for the inner class of a windowed record. Must implement the "</code>
<code>+</code></p>
<p><code>"&lt;code&gt;org.apache.kafka.common.serialization.Serde&lt;/code&gt; interface. Note that setting this config in KafkaStreams application would result "</code>
<code>+</code></p>
<p><code>"in an error as it is meant to be used only from Plain consumer client.";</code></p></td>
</tr>
</tbody>
</table>

# Proposed Changes

I propose we deprecate [`window.size.ms`  in
`StreamsConfig`](https://github.com/apache/kafka/blob/trunk/streams/src/main/java/org/apache/kafka/streams/StreamsConfig.java#L762)
as well as `windowed.inner.serdes.class.`

We will not define either config in the client’s Config class, and
instead opt to define a String variable with the config name in the
relevant de/serializer class. There would be three new public String
variables for three different configs:

In TimeWindowedSerializer:  
  - define a constant for “windowed.inner.serializer.class”  
In TimeWindowedDeserializer:  
  - define a constant for “windowed.inner.deserializer.class”  
  - define a constant for “[window.size.m](http://window.size.ms/)s”

We update the windowed de/serializer implementations to check for the
new configs (ie
“[windowed.inner.de/serializer.class](http://windowed.inner.de/serializer.class)”)
and use the provided de/serializer class, if one was given. If the new
configs are not present, they would fall back to the original/current
logic (ie that based on the old “windowed.inner.serde.class” config). 

If both are specified, the new piece of config takes preference. 

# Compatibility, Deprecation, and Migration Plan

- *Deprecating it in `StreamsConfig`  shouldn’t impact current users
  negatively other than them seeing a warning when they use it in
  KStreams.*

- *We should change the config docs first to deprecate. In the future,
  moving the configuration to ClientConfig could be called for. See
  <https://github.com/apache/kafka/pull/14360> for a PR in progress, as
  well as a discussion of this issue.*

- *Post docs change and deprecation, for major release 5. Deprecation
  itself can be for next minor release.*

# Test Plan

*Could be covered with a unit test to check that a warning is displayed
when used.*

*Also have a unit test to make sure the new
“[windowed.inner.de/serializer.class](http://windowed.inner.de/serializer.class)”
takes preference in the case that both it, and the old
“windowed.inner.serde.class” config are both specified*

# Rejected Alternatives

*Currently N/A.*
