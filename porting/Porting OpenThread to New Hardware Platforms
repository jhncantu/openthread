

# Porting OpenThread to New Hardware Platforms

Porting the OpenThread stack to a new hardware platform consists of five steps:

1.  [Set up the build environment](/guides/porting/set-up-the-build-environment)
1.  [Implement Platform Abstraction Layer APIs](/guides/porting/implement-platform-abstraction-layer-apis)
1.  [Implement advanced features (Hardware Abstraction Layer)](/guides/porting/implement-advanced-features)
1.  [Validate the port](/guides/porting/validate-the-port)
1.  [Certification and README](/guides/porting/certification-and-readme)

## Hardware platform requirements

OpenThread requires the following platform services:

-   [IEEE 802.15.4-2006](https://standards.ieee.org/findstds/standard/802.15.4-2006.html)
    2.4 GHz radio
    -   Send and receive IEEE 802.15.4 frames
    -   Generate IEEE 802.15.4 Acknowledgment frames
    -   Provide Received Signal Strength Indicator (RSSI) measurements on
        received frames
-   A millisecond-resolution free-running timer with alarm
-   Non-volatile storage for storing network configuration settings
-   A true random number generator (TRNG)

## Example builds

Several example builds are provided in the OpenThread repository. For more
information, see [Platforms](/platforms/).

For a complete end-to-end example of how to port OpenThread from scratch, see
the [Add support for EFR32]({{ github_repo_ot }}/pull/1592)
pull request.



