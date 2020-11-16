
# Inform Previous Parent on Reattach

To allow end devices (EDs) in a Thread network to inform their previous parent
router that they have attached to a new parent router, enable the Inform
Previous Parent on Reattach feature.

This updates the previous parent's child table quicker than the configured
[child timeout
interval](/reference/group/api-thread-general#otthreadgetchildtimeout) and
prevents it from queueing traffic for an ED that it thinks is asleep, but in
actuality has a new parent.

## How it works

After an ED attaches to a new parent router, it sends a single unicast IPv6
message containing the following information to its previous parent router:

*   The [Mesh-Local
    EID](/guides/thread-primer/ipv6-addressing#mesh-local-eid-ml-eid) of the ED
    as the source address.
*   The Mesh-Local EID of the previous parent router as the destination address.
*   An empty payload.

This type of IPv6 message prompts the old parent router to immediately remove
all registered IPv6 addresses for that ED from its child table.

## How to enable

This feature is disabled by default.

To enable Inform Previous Parent on Reattach, define
`OPENTHREAD_CONFIG_MLE_INFORM_PREVIOUS_PARENT_ON_REATTACH` as `1` in the
[`/src/core/config/mle.h`]({{ github_core }}/src/core/config/mle.h)
file, prior to [building OpenThread](/guides/build):

<pre class="devsite-click-to-copy">
#ifndef OPENTHREAD_CONFIG_MLE_INFORM_PREVIOUS_PARENT_ON_REATTACH
#define OPENTHREAD_CONFIG_MLE_INFORM_PREVIOUS_PARENT_ON_REATTACH 1
#endif
</pre>

## Parameters

There are no configurable parameters for this feature.

## API

There is no public API for this feature.

## CLI

There are no CLI commands related to this feature.



