---
title: direct integration with the id service
description: implementation for customers using id service on devices that cannot accept or work with our javascript or sdk code.
seo-title: direct integration with the adobe experience cloud id service
seo-description: implementation for customers using adobe experience cloud id service on devices that cannot accept or work with our javascript or sdk code.
short-title: direct integration
doc-type: reference
audience: admin
index: true
translate: true
version: false
private-feature-pack: false
beta: false
redirect: false
product: core services id service
cloud: experience cloud
archetype: admin
user-guide: id service admin guide
git-edit: https://git.corp.adobe.com/AdobeDocs/core-services.en/tree/master/help/id-service/implementation/implementation-other/implementation-other-direct-integration.md
git-issue: https://git.corp.adobe.com/AdobeDocs/core-services.en/issues/new
git-repo: https://git.corp.adobe.com/adobedocs/core-services.en
---
<!--Meta Data Values

**Required Meta for search optimization and page data**

title: free text string

description: free text string

seo-title: free text string

seo-description: free text string

**Optional Meta for extended capabilities**

audience:
all (default), admin, developer, end-user
 
index: true (default), false
 
translate:
true (default), false
 
doc-type:
reference (default), tutorials

version:
false (default), Classic, Standard, 6.5, 6.4, 6.3, 6.2
 
private-feature-pack:
false (default), true
 
beta:
false (default), true
 
redirect:
false (default), pathname
-->

# Direct Integration with the Experience Cloud ID Service

This implementation lets customers use the ID service on devices that cannot accept or work with our JavaScript or SDK code. This includes devices such as gaming consoles, smart TVs, or other Internet-enabled appliances. 

Refer to this section for syntax, code samples, and definitions.

## Syntax

Devices that cannot use the `VisitorAPI.js` or SDK code libraries can make calls directly to the data collection servers \(DCS\) used by the ID service. 

To do this, you would call `dpm.demdex.net` and format your request as shown below. *Italics* indicates a variable placeholder.

![directSyntax example](../../assets/directSyntax.png) 

In this syntax example, the `d_` prefix identifies the key-value pairs in the call as a system-level variable. You can pass quite a few `d_` parameters to the ID service, but stay focused on the key-value pairs as shown in the code above. For more information about other variables, see [Supported Attributes for DCS API calls](https://marketing.adobe.com/resources/help/en_US/aam/dcs-keys.html).

The ID service supports HTTP and HTTPS calls. Use HTTPS to pass data from a secure page.

## Sample Request

Your request could look similar to the sample shown below. Long variables have been shortened.

![Direct example](../../assets/directExample.png)

## Sample Response

The ID service returns data in a JSON object as shown below. Your response may be different.

```javascript
{
     "d_mid":"12345",
     "dcs_region":"6",
     "id_sync_ttl":"604800",
     "d_blob":"wxyz5432"
}
```

## Parameters Defined: Request

### `dpm.demdex.net`
A legacy domain controlled by Adobe. See [Understanding Calls to the Demdex Domain](https://marketing.adobe.com/resources/help/en_US/aam/demdex-calls.html).

### `d_mid` 
The Experience Cloud visitor ID. See [Cookies and the Experience Cloud ID Service](../../getting-started/getting-started-cookies.md).

### `d_orgid`
Your Experience Cloud Organization ID. For help with finding this ID see, [Requirements for the Experience Cloud ID Service](../../reference/reference-requirements.md).

### `d_cid`
An optional parameter that passes the Data Provider ID \(DPID\), the Unique User ID \(DPUUID\), and an [authenticated state ID](mcvid-authenticated-state.html#) to the ID service. As shown in the code sample, separate the DPID and DPUUID with the non-printing control character, `%01`.

DPID and DPUUID - In the `d_cid` parameter, assign each related DPID and DPUUID combination to the same `d_cid` parameter. This lets you return multiple ID sets in a single request. Also, separate the DPID, DPUUID, and optional authentication flag with the non-printing control character, `%01`. 

In the examples below, the provider and user IDs are highlighted in **bold** text.

| State                | Example                                                                                                                                                                                           |
| -------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Syntax               | `...d_cid=**DPID**%01**DPUUID**%01authentication state...`                                                                                                                                        |
| Example              | `...d_cid=**123**%01**456**%011...`                                                                                                                                                               |
| Authentication State | This is an optional ID in the `d_cid` parameter. Expressed as an integer, it identifies users according to their authentication status as shown:  `0` Unknown, `1` Authenticated, `2` Logged Out. |

To specify an authentication state, you set this flag after the user ID \(UUID\) variable. Separate the UUID and authentication flag with the non-printing control character, `%01`. In the examples below, the authentication IDs are highlighted in **bold** text.

Syntax: `...d_cid=DPID%01DPUUID%01**authentication state**` 
Examples: 

| State         | Parameters                       |
| ------------- | -------------------------------- |
| Unknown       | `...d_cid=123%01456%01**0**...`  |
| Authenticated | `...d_cid=123%01456%01**1**...`  |
| Logged Out    | `...d_cid=123%01456%01**2**...`  |

### `dcs_region` 
The ID service is a geographically distributed and load-balanced system. The ID identifies the region of the data center handling the call. See [DCS Region IDs, Locations, and Host Names](https://marketing.adobe.com/resources/help/en_US/aam/dcs-regions.html).

### `d_cb` 
*\(Optional\)* A callback parameter that lets you execute a JavaScript function in the request body.

### `d_blob`
An encrypted chunk of JavaScript metadata. Size constraints limit the blob to 512 bytes or less.

### `d_ver`
Required. This sets the API version number. Leave this set as `d_ver=2`.

## Parameters Defined: Response

Some response parameters are part of the request and have been defined in the section above.

| Parameter     | Description                                                                                              |
| ------------- | -------------------------------------------------------------------------------------------------------- |
| `id_sync_ttl` | The re-synchronization interval, specified in seconds. The default interval is 604,800 seconds (7-days). |