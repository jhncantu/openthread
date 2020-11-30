# Build OpenThread

## Toolchains

The primary supported toolchain for building OpenThread is GNU Autotools.

### GNU Autotools

Instructions on building examples with GNU Autotools can be found in each example's
[platform folder](https://github.com/openthread/openthread/tree/master/examples/platforms). 

> Note: the intent of these examples is to show the minimal code necessary to run OpenThread on each
respective platform. As such, they do not highlight the platform's full
capabilities.

Further configuration during builds might be necessary, depending on your use
case.

### GNU Autotools — Nest Labs build

Nest Labs has created a customized, turnkey build system framework, based on GNU
Autotools. This is used for standalone software packages that need to support:

-  building on and targeting against standalone build host systems
-  embedded target systems using GCC-based or -compatible toolchains

The Nest Labs build of GNU Autotools is recommend for use with OpenThread
because some build host systems might not have GNU Autotools, or might have
different versions and distributions. This leads to inconsistent primary and
secondary Autotools output, which results in a divergent user and support
experience. The Nest Labs build avoids this by providing a pre-built,
qualified set of GNU Autotools with associated scripts that do not rely on the
versions of Autotools on the build host system.

This project is typically subtreed (or git submoduled) into a target project
repository and serves as the seed for that project's build system.

To learn more, or to use this tool for your OpenThread builds, see the
[`README`](https://github.com/openthread/openthread/tree/master/third_party/nlbuild-autotools/repo).

## How to build OpenThread

The steps to build OpenThread vary depending on toolchain, user machine, and
target platform.

The most common workflow is:

1.  Set up the build environment and install the desired toolchain:

    a.  **To build directly on a machine,** see the [Simulation Codelab](https://openthread.io/codelabs/openthread-simulation-posix?index=..%2F..index#1)
        for detailed instructions.
        
    b.  **To use a Docker container with a pre-configured environment,**
        download and run the OpenThread `environment` image: 
   ```     
       $ docker pull openthread/environment:latest
       $ docker run -it --rm openthread/environment bash
   ``` 
1.  Within your chosen environment, clone the OpenThread Git repository:
<pre class="devsite-click-to-copy"><code class="devsite-terminal">git clone {{ github_repo_ot }}</code></pre>
1.  From the cloned repository's root directory:
    1.  Install the GNU toolchain and other dependencies (optional):
<pre class="devsite-click-to-copy"><code class="devsite-terminal">./script/bootstrap</code></pre>
    1.  Set up the environment:
<pre class="devsite-click-to-copy"><code class="devsite-terminal">./bootstrap</code></pre>
    1.  Configure and build, using pre-defined platform examples with optional
            customization via common switches:
        1.  Modify OpenThread compile-time constants in the selected platform's
            <code>/examples/platforms/<var>&lt;platform&gt;</var>/openthread-core-<var>&lt;platform&gt;</var>-config.h</code>
            file
        1.  Build the configuration:
<pre class="devsite-click-to-copy"><code class="devsite-terminal">make -f examples/Makefile-<var>&lt;platform&gt; &lt;switches&gt;</var></code></pre>
1.  Flash the desired binary to the target platform. All generated binaries are
    located in <code>/output/<var>&lt;platform&gt;</var>/bin</code>. When
    using Advanced Mode, the <code><var>&lt;platform&gt;</var></code> may
    be specific to the user's machine. For example, `x86_64-apple-darwin`.

Specific instructions on building supported platforms with GNU Autotools can be
found in each example's [platform folder]({{ github_core }}/examples/platforms).

Note: Between builds in the same repository, run `make clean` or
`make distclean` to ensure a clean build each time.

### Configuration

You can configure OpenThread for different functionality and behavior during the
build process. Available configuration options are detailed at the following
locations:

<table>
  <tbody>
    <tr>
      <th>Type</th><th>Location</th>
    </tr>
    <tr>
      <td>Compile-time constants</td><td>Listed in all the header files in <a href="{{ github_core }}/src/core/config"><code>/src/core/config</code></a></td>
    </tr>
    <tr>
      <td>Makefile build switches</td><td>Listed in <a href="{{ github_core }}/examples/common-switches.mk"><code>/examples/common-switches.mk</code></a></td>
    </tr>
  </tbody>
</table>

Note: Each example platform included in OpenThread specifies some, but not all,
of the constants and flags that the platform supports. Modify the example
platform's <code>/openthread-core-<var>&lt;platform&gt;</var>-config.h</code>
file to enable or disable compile-time constants prior to building.

### Build examples

Use a switch to enable functionality for an example platform. For example, to
build the CC2538 example with Commissioner and Joiner support enabled:

<pre class="devsite-click-to-copy"><code class="devsite-terminal">make -f examples/Makefile-cc2538 COMMISSIONER=1 JOINER=1</code></pre>

Or, to build the nRF52840 example with the [Jam Detection
feature](/guides/build/features/jam-detection) enabled:

<pre class="devsite-click-to-copy"><code class="devsite-terminal">make -f examples/Makefile-nrf52840 JAM_DETECTION=1</code></pre>

### Binaries

The following binaries are generated in
<code>/output/<var>&lt;platform&gt;</var>/bin</code> from the build process. To
determine which binaries are generated, use configure option flags with the
`./configure` command to generate an updated `Makefile` for building. For
example, to build OpenThread and generate only the CLI binaries:

<pre class="devsite-click-to-copy"><code class="devsite-terminal">./configure --enable-cli</code>
<code class="devsite-terminal">make</code></pre>

<table>
  <tbody>
    <tr>
      <th>Binary</th><th>Description</th><th>Configure option flags</th>
    </tr>
    <tr>
      <td><code>ot-cli-ftd</code></td>
      <td>Full Thread device for SoC designs</td>
      <td><code>--enable-cli</code><br><code>--enable-ftd</code></td>
    </tr>
    <tr>
      <td><code>ot-cli-mtd</code></td>
      <td>Minimal Thread device for SoC designs</td>
      <td><code>--enable-cli</code><br><code>--enable-mtd</code></td>
    </tr>
    <tr>
      <td><code>ot-ncp-ftd</code></td>
      <td>Full Thread device for Network Co-Processor (NCP) designs</td>
      <td><code>--enable-ncp</code><br><code>--enable-ftd</code></td>
    </tr>
    <tr>
      <td><code>ot-ncp-mtd</code></td>
      <td>Minimal Thread device for NCP designs</td>
      <td><code>--enable-ncp</code><br><code>--enable-mtd</code></td>
    </tr>
    <tr>
      <td><code>ot-rcp</code></td>
      <td>Radio Co-Processor (RCP) design</td>
      <td><code>--enable-ncp</code><br><code>--enable-radio-only</code></td>
    </tr>
  </tbody>
</table>

If neither these flags nor a platform example are not used, applications are not
built but OpenThread library files are still generated in
<code>/output/<var>&lt;platform&gt;</var>/lib</code> for use in a project.

Check the example Makefiles for each platform to see which flags each platform
supports. For example, [the TI CC2650 does not support
FTDs]({{ github_core }}/examples/Makefile-cc2650#L44). For more information on
FTDs and MTDs, see the
[Thread Primer](/guides/thread-primer/node-roles-and-types#device_types). For
more information on SoC and NCP designs, see [Platforms](/platforms/).

The process to flash these binaries varies across example platforms. See the
READMEs in each platform's
[example folder]({{ github_core }}/examples/platforms) for detailed
instructions.

### OpenThread Daemon

OpenThread Daemon (OT Daemon) is an OpenThread POSIX build mode that runs
OpenThread as a service and is used with the RCP design. For more information on
how to build and use it, see [OpenThread
Daemon](/platforms/co-processor/ot-daemon).

## Build Support Packages

Build Support Packages (BSPs)  are found in
[`/third_party`]({{ github_core }}/third_party). BSPs are additional third-party
code used by OpenThread on each respective platform, generally included when
[porting OpenThread](/guides/porting/) to a new hardware platform.

