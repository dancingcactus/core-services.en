---
title: experience cloud id service migration scenarios
description: migration scenarios for the adobe experience cloud id service
seo-title: adobe experience cloud id service migration scenarios
seo-description: migration scenarios for the adobe experience cloud id service
short-title: migration scenarios
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
git-edit: https://git.corp.adobe.com/AdobeDocs/core-services.en/tree/master/help/id-service/reference/reference-analytics/reference-analytics-migration-scenarios.md
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

# Experience Cloud ID Service Migration Scenarios

Contains server example configurations and the required migration steps.

## Single Web Property

+ **Customer**: Example Company Inc.
+ **Experience Cloud enabled**: No
+ **Web Properties**: `example.com`
+ **Data collection servers**: `metrics.example.com`, `smetrics.example.com`
+ **Analytics JavaScript file**: A single file for all site pages

Firstly, this customer should get enabled for the Experience Cloud \(see the [requirements](../reference-requirements.md)\). And, because they have a single JavaScript file, this customer does not need a grace period. This customer will also set up visitor migration and then migrate away from their data collection `CNAME`, which is not necessary.

## Multiple JavaScript Files, hard-coded image tags

+ **Customer**: Another Example Company Inc.
+ **Experience Cloud enabled**: Yes
+ **Web Properties**: `anotherexample.com`
+ **Data collection servers**: `anotherexampleco.112.2o7.net`
+ **Analytics JavaScript file**: Multiple JavaScript files. One file for their main site, another file for their support section that is maintained in a separate CMS.
+ **Other data collection methods**: Hard-coded image tags on one site section

Firstly, this customer should find their Adobe Experience Cloud Organization ID \(see the [requirements](../reference-requirements.md)\). Next, they should configure a migration grace period because they are using multiple JavaScript files. This customer will also set up visitor migration and then migrate from `*.2o7.net` to `*.sc.omtrdc.net`.

When this customer updates to the latest Analytics JavaScript code in preparation for the Experience Cloud ID service rollout, they will also update all hard-coded image tags to use JavaScript instead.

## Multiple Web Properties, Multiple JavaScript Files and a Flash-based Video Player

+ **Customer**: A Good Customer LLC
+ **Experience Cloud enabled**: Yes
+ **Web Properties**: `mymainsite.com`, `myothersiteA.com`, `myothersiteB.com`
+ **Data collection servers**: `metrics.mymainsite.com`, `smetrics.mymainsite.com`
+ **Analytics JavaScript file**: Multiple JavaScript files. One file for each web property.
+ **Other data collection methods**: A Flash-based video player

Firstly, this customer should find their Adobe Experience Cloud Organization ID \(see the [requirements](../reference-requirements.md)\). Next, they should configure a migration grace period because they are using multiple JavaScript files. This customer tracks visitors between their primary domain and their sub-domains, so they will continue to use their data collection CNAME with the visitor ID service.

When this customer updates to the latest Analytics JavaScript code in preparation for the Experience Cloud ID service rollout, they will also update their Flash-based video player to the latest version of AppMeasurement for Flash.