---
RFC: RFC0022
Author: Jody Whitlock
Status: Draft
Area: Commands
Comments Due:
---

# Cross-platform `Test-Connection` cmdlet

`Test-Connection` was introduced in `PowerShell 2.0` under the `Microsoft.PowerShell.Management` class to provide a mechanism for sending `ICMP` echo requests within the PowerShell language and have the results returned as a PowerShell object.

`Test-Connection` was implemented based on `WMI`, which is not supported on non-Windows platforms. The RFC aims to describe cross-platform `Test-Connection`, which will run on all platforms supported by .Net Core.

Network troubleshooting is not limited to ping. `Test-Connection` must support all the basic network diagnostics - ping, trace route, connection test, pathping (to check a quality of the channel), black hole router and MTU size detections.

## Motivation

PowerShell users need an cross-platform, available and power tool to troubleshoot and diagnose a network.

Currently users can use some native utilites such as `nmap`, `ping`, `traceroute`, `pathping`. We want to have a cmdlet that provides most of these capabilities.

## Specification

### Outputs

- (empty yet)

### Parameter Sets

1. Ping - (1) resolve hostname address and ping IPv6 address or fallback to IPv4 address. (2) detect input address type anf ping IPv6 or IPv4 address.

1. TraceRoute

1. PathPing

1. ConnectionByTCPPortName

1. ConnectionByUDPPortName

1. ConnectionByTCPPort

1. ConnectionByUDPPort

1. DetectionOfBlackHole - detect Black Hole Router. See [Diagnoses and treatment of black hole routers](https://support.microsoft.com/en-us/help/159211/diagnoses-and-treatment-of-black-hole-routers)

1. DetectionOfMTUSize - detect MTU size. See [Recommended TCP/IP settings for WAN links with a MTU size of less than 576](https://support.microsoft.com/en-us/help/900926/recommended-tcp-ip-settings-for-wan-links-with-a-mtu-size-of-less-than)

### Common Parameters

1. InformationalLevel

   - Quiet - return true/false or a single value of the response.
   - Detailed - return an result object corresponding the parameterset.

1. TimeOut - timeout for each request (not cmdlet).
    [int]
    ValidatePositive()

1. ComputerName - hostname(s) or IPv4/IPv6 address(es).
    [string[]]

### Parameters

1. Ping

    1. IPv4 or IPv6 - force use the protocol.

    1. Resolve - resolve ip addresses to hostnames.
        [Switch]

    1. Source - source address to use.
        [IPAddress]

    1. MaxHops - (TTL) maximum number of hops to search target.
        [int]
        ValidateRange(1, 30)

    1. Count - number of echo requests to send.
        [int]
        ValidatePositive()

    1. Delay - delay in milliseconds between attempts.
        [int]
        ValidatePositive()

    1. BufferSize - send buffer size.
        [int]
        ValidatePositive()

    1. DontFragment - set Don't Fragment flag for IPv4.
        [Switch]

    1. Continues - ping until stopped by Ctrl-C.
        [Switch]

1. TraceRoute

    1. IPv4 or IPv6 - force use the protocol.

    1. Resolve - resolve ip addresses to hostnames.
        [Switch]

    1. Source - source address to use.
        [IPAddress]

    1. MaxHops - (TTL) maximum number of hops to search target.
        [int]
        ValidateRange(1, 30)

1. PathPing

    1. IPv4 or IPv6 - force use the protocol.

    1. Resolve - resolve ip addresses to hostnames.
        [Switch]

    1. Source - source address to use.
        [IPAddress]

    1. MaxHops - (TTL) maximum number of hops to search target.
        [int]
        ValidateRange(1, 30)

    1. Delay - delay in milliseconds between attempts.
        [int]
        ValidatePositive()

    1. Count - number of echo requests to send per hop.
        [int]
        ValidatePositive()

1. ConnectionByTCPPortName

    1. TCPPortName - test connection by the common port name using TCP protocol.
        [string]
        ValidateSet()

1. ConnectionByUDPPortName

    1. UDPPortName - test connection by the common port name using UDP protocol.
        [string]
        ValidateSet()

1. ConnectionByTCPPort

    1. TCPPort - test connection by the port number using TCP protocol.
        [uint]
        ValidateRange()

1. ConnectionByUDPPort

    1. UDPPort - test connection by the port number using UDP protocol.
        [uint]
        ValidateRange(1, 65535)

1. DetectionOfBlackHole

    1. BlackHoleDetect
        [Switch]

1. DetectionOfMTUSize

    1. MTUSizeDetect
        [Switch]

## Alternate Proposals and Considerations

Underlying .Net Core may have limitations and bugs so not all features can be implemented or may not work on all platforms. See [Issue](https://github.com/PowerShell/PowerShell/issues/4240).
