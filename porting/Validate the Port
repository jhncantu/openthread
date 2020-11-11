

# Validate the Port

Basic validation is necessary to verify a successful port of OpenThread to a new
hardware platform example.

<h2 class="numbered">Compile for the target platform</h2>

Demonstrate a successful build by compiling the example OpenThread application
for the target platform.

<pre class="devsite-click-to-copy"><code class="devsite-terminal">./bootstrap</code>
<code class="devsite-terminal">make -f examples/Makefile-efr32 COMMISSIONER=1 JOINER=1</code>
</pre>

<h2 class="numbered">Interact with the CLI</h2>

Demonstrate successful OpenThread execution and UART capability by interacting
with the CLI.

Open a terminal to `/dev/ttyACM0` (serial port settings: 115200 8-N-1). Type
`help` for a list of commands.

Note: The set of CLI commands will vary based on the features enabled in a
particular build. The majority of them have been elided in the example output
below.

<pre class="devsite-click-to-copy"><code class="devsite-terminal" data-terminal-prefix="&gt; ">help
help
autostart
bufferinfo
...
version
whitelist</code></pre>
</pre>

<h2 class="numbered">Form a Thread network</h2>

Demonstrate successful protocol timers by forming a Thread network and verifying
the node has transitioned to the Leader state.

<pre class="devsite-click-to-copy"><code class="devsite-terminal" data-terminal-prefix="&gt; ">dataset init new</code>
Done
<code class="devsite-terminal" data-terminal-prefix="&gt; ">dataset</code>
Active Timestamp: 1
Channel: 13
Channel Mask: 07fff800
Ext PAN ID: d63e8e3e495ebbc3
Mesh Local Prefix: fd3d:b50b:f96d:722d/64
Master Key: dfd34f0f05cad978ec4e32b0413038ff
Network Name: OpenThread-8f28
PAN ID: 0x8f28
PSKc: c23a76e98f1a6483639b1ac1271e2e27
Security Policy: 0, onrcb
Done
<code class="devsite-terminal" data-terminal-prefix="&gt; ">dataset commit active</code>
Done
<code class="devsite-terminal" data-terminal-prefix="&gt; ">ifconfig up</code>
Done
<code class="devsite-terminal" data-terminal-prefix="&gt; ">thread start</code>
Done</pre>

Wait a couple of seconds...

<pre class="devsite-click-to-copy"><code class="devsite-terminal" data-terminal-prefix="&gt; ">state
leader
Done</code></pre>

<h2 class="numbered">Attach a second node</h2>

Demonstrate successful radio communication by attaching a second node to the
newly formed Thread network, using the same Thread Master Key and PAN ID from
the first node:

<pre class="devsite-click-to-copy"><code class="devsite-terminal" data-terminal-prefix="&gt; ">dataset masterkey dfd34f0f05cad978ec4e32b0413038ff</code>
Done
<code class="devsite-terminal" data-terminal-prefix="&gt; ">dataset panid 0x8f28</code>
Done
<code class="devsite-terminal" data-terminal-prefix="&gt; ">dataset commit active</code>
Done
<code class="devsite-terminal" data-terminal-prefix="&gt; ">routerselectionjitter 1</code>
Done
<code class="devsite-terminal" data-terminal-prefix="&gt; ">ifconfig up</code>
Done
<code class="devsite-terminal" data-terminal-prefix="&gt; ">thread start</code>
Done</pre>

Wait a couple of seconds...

<pre class="devsite-click-to-copy"><code class="devsite-terminal" data-terminal-prefix="&gt; ">state
router
Done</code></pre>

<h2 class="numbered">Ping between devices</h2>

Demonstrate successful data path communication by sending/receiving ICMPv6 Echo
request/response messages.

List all IPv6 addresses of Leader:

<pre class="devsite-click-to-copy"><code class="devsite-terminal" data-terminal-prefix="&gt; ">ipaddr
fdde:ad00:beef:0:0:ff:fe00:fc00
fdde:ad00:beef:0:0:ff:fe00:800
fdde:ad00:beef:0:5b:3bcd:deff:7786
fe80:0:0:0:6447:6e10:cf7:ee29
Done</code></pre>

Send an ICMPv6 ping from Router to Leader's Mesh-Local EID IPv6 address:

<pre class="devsite-click-to-copy"><code class="devsite-terminal" data-terminal-prefix="&gt; ">ping fdde:ad00:beef:0:5b:3bcd:deff:7786
16 bytes from fdde:ad00:beef:0:5b:3bcd:deff:7786: icmp_seq=1 hlim=64 time=24ms</code></pre>

<h2 class="numbered">Reset a device and validate reattachment</h2>

Demonstrate non-volatile functionality by resetting the device and validating
its reattachment to the same network without user intervention.

Start a Thread network:
<pre class="devsite-click-to-copy"><code class="devsite-terminal" data-terminal-prefix="&gt; ">dataset init new</code>
Done
<code class="devsite-terminal" data-terminal-prefix="&gt; ">dataset</code>
Active Timestamp: 1
Channel: 13
Channel Mask: 07fff800
Ext PAN ID: d63e8e3e495ebbc3
Mesh Local Prefix: fd3d:b50b:f96d:722d/64
Master Key: dfd34f0f05cad978ec4e32b0413038ff
Network Name: OpenThread-8f28
PAN ID: 0x8f28
PSKc: c23a76e98f1a6483639b1ac1271e2e27
Security Policy: 0, onrcb
Done
<code class="devsite-terminal" data-terminal-prefix="&gt; ">dataset commit active</code>
Done
<code class="devsite-terminal" data-terminal-prefix="&gt; ">ifconfig up</code>
Done
<code class="devsite-terminal" data-terminal-prefix="&gt; ">thread start</code>
Done</pre>

Wait a couple of seconds and verify that the active dataset has been stored in
non-volatile storage:

<pre class="devsite-click-to-copy"><code class="devsite-terminal" data-terminal-prefix="&gt; ">dataset active</code>
Active Timestamp: 1
Channel: 13
Channel Mask: 07fff800
Ext PAN ID: d63e8e3e495ebbc3
Mesh Local Prefix: fd3d:b50b:f96d:722d/64
Master Key: dfd34f0f05cad978ec4e32b0413038ff
Network Name: OpenThread-8f28
PAN ID: 0x8f28
PSKc: c23a76e98f1a6483639b1ac1271e2e27
Security Policy: 0, onrcb
Done</pre>

Reset the device:

<pre class="devsite-click-to-copy"><code class="devsite-terminal" data-terminal-prefix="&gt; ">reset</code>
<code class="devsite-terminal" data-terminal-prefix="&gt; ">ifconfig up
Done</code>
<code class="devsite-terminal" data-terminal-prefix="&gt; ">thread start
Done</code>
</pre>

Wait a couple of seconds and verify that the device has successfully reattached
to the network:

<pre class="devsite-click-to-copy"><code class="devsite-terminal" data-terminal-prefix="&gt; ">panid</code>
0x8f28
Done
<code class="devsite-terminal" data-terminal-prefix="&gt; ">state
router
Done</code></pre>

<h2 class="numbered">Verify random number generation</h2>

Demonstrate random number generation by executing the `factoryreset` command and
verifying a new random extended address.

<pre class="devsite-click-to-copy"><code class="devsite-terminal" data-terminal-prefix="&gt; ">extaddr
a660421703f3fdc3
Done</code>
<code class="devsite-terminal" data-terminal-prefix="&gt; ">factoryreset</code></pre>

Wait a couple of seconds...
<pre class="devsite-click-to-copy"><code class="devsite-terminal" data-terminal-prefix="&gt; ">extaddr
9a8ed90715a5f7b6
Done</code></pre>



