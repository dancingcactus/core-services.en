---
description: Analytics uses cookies to provide information on variables and components that do not persist between image requests and browser sessions.
keywords: cookies;privacy
seo-description: Analytics uses cookies to provide information on variables and components that do not persist between image requests and browser sessions.
seo-title: First-Party Cookies
solution: Experience Cloud,Analytics
title: First-Party Cookies
index: y
snippet: y
---

# About first-party cookies

Analytics uses cookies to provide information on variables and components that do not persist between image requests and browser sessions. These harmless cookies originate from a domain hosted by Adobe, known as third-party cookies.

Many browsers and anti-spyware applications are designed to reject and delete third-party cookies, including those used in Analytics data collection. In order to circumvent tracking limitations imposed by browsers and programs, you can implement first-party cookies.

Two options are available to implement first party cookies

* The Experience Platform ID Service. The ID Service can set the cookie in the first-party context using JavaScript.
* DNS entries on your companies DNS server.
* If your site has secure pages using the `https:` protocol and you are not using the Experience Platform ID Service, you can work with Adobe to obtain an SSL certificate in order to implement first-party cookies

The SSL certificate issuance process can often be confusing and time consuming. As a result, Adobe established a partnership with DigiCert, an industry leading Certificate Authority (CA), and developed an integrated process by which the purchase and management of these certificates is automated.

With your permission, we will work with our CA to issue, deploy, and manage a new SHA-2 SSL certificate for you. Adobe will continue to manage this certificate and ensure that an unexpected expiration, revocation, or security concern, does do not threaten the availability of your organizations secure collection.

## Adobe Managed Certificate Program

The Adobe Managed Certificate Program is the recommended process for implementing a new first-party SSL certificate for first-party cookies.

The Adobe Managed Certificate program lets you implement a new first-party SSL certificate for first-party cookies at no additional cost. If you currently have your own Customer Managed SSL certificate, speak with Adobe Customer Care about migrating to the Adobe Managed Certificate Program. 

### Implement

Here is how you implement a new first-party SSL certificate for first-party cookies: 

1. Fill out the [First-party cookie request form](/help/interface/cookies/assets/FPC_Request_Form.xlsx) and open a ticket with Customer Care requesting to set up first-party cookies on the Adobe Managed program. Each field is described within the document with examples.

1. Create CNAME records (see instructions below). Upon receiving the ticket, a customer care representative should provide you with a pair of CNAME records. These records must be configured on your company's DNS server before Adobe can purchase the certificate on your behalf. The CNAMES will be similar to the following: **Secure** - For example, the hostname `smetrics.example.com` points to: `example.com.ssl.d1.omtrdc.net`. **Non-secure** - For example, the hostname `metrics.example.com` points to: `example.com.d1.omtrdc.net`. 

1. When these CNAMES are in place, Adobe will work with DigiCert to purchase and install a certificate on Adobe's production servers. If you have an existing implementation, you should consider Visitor Migration to maintain your existing visitors. After the certificate has been pushed live to Adobe’s production environment, you will be able to update your tracking server variables to the new hostnames. Meaning, if the site is not secure (https), update the `s.trackingServer`. If the site is secure (https), update both `s.trackingServer` and `s.trackingServerSecure` variables.

1. Ping the hostname (see below).

1. Update Implementation Code (see below).

### Maintenance and Renewals

SSL certificates expire each year, meaning Adobe must purchase a new certificate for each implementation on a yearly basis. All supported users within your organization will receive an email notification each time an implementation is close to expiration. For Adobe to renew your hostname, one supported user will must reply to the email from Adobe and indicate that you plan to continue using the expiring hostname for data collection. At that point, Adobe automatically purchases and installs a new certificate. 

### Frequently Asked Questions

|Question|Answer|
|---|---|
|**Is this process secure?**|Yes, the Adobe Managed program is more secure than our legacy method as no certificate or private key changes hands outside of Adobe and the issuing certificate authority.|
|**How can Adobe purchase a certificate for our domain?**|The certificate can only be purchased when you have pointed the specified hostname (for example, smetrics.example.com) to an Adobe owned hostname. This is essentially delegating this hostname to Adobe and allows Adobe to purchase the certificate on your behalf.|
|**Can I request that the certificate be revoked?**|Yes, as the owner of the domain, you are entitled to request we have the certificate revoked. You will only need to open a ticket with Customer Care to have this completed.|
|**Will this certificate be using SHA-2 encryption?**|Yes, Adobe will work with DigiCert to issue a SHA-2 certificate.|
|**Does this incur any additional cost?**|No, Adobe is offering this service to all current Analytics customers at no additional cost.|

## Create CNAME records

Your organization's network operations team should configure your DNS servers by creating new CNAME record(s). Each hostname forwards data to Adobe's data collection servers.

The FPC specialist provides you with the configured hostnames and what CNAMEs they are to be pointed to. For example:

* **SSL Hostname**:`smetrics.mysite.com`
* **SSL CNAME**:`mysite.com.ssl.d1.sc.omtrdc.net`
* **Non-SSL Hostname**:`metrics.mysite.com`
* **Non-SSL CNAME**:`mysite.com.d1.sc.omtrdc.net`

As long as implementation code is not altered, this step will not affect data collection and can be done at any time after updating implementation code.

>[!Note:] The Experience Cloud Visitor ID service provides an alternative to configuring a CNAME to enable first-party cookies. 

## Ping the hostname

Ping the hostname to ensure correct forwarding. All hostnames must respond to a ping to prevent data loss.

After CNAME records are properly configured, and Adobe has confirmed installation of the certificate, open a command prompt and ping your hostname(s). Using `mysite.com` as an example: `ping metrics.mysite.com`

If everything is successfully set up, it will return something similar to the following:

```Pinging mysite.com.112.2o7.net [66.235.132.232] with 32 bytes of data:
Reply from 66.235.132.232: bytes=32 time=19ms TTL=246
Reply from 66.235.132.232: bytes=32 time=19ms TTL=246
Reply from 66.235.132.232: bytes=32 time=19ms TTL=246
Reply from 66.235.132.232: bytes=32 time=19ms TTL=246

Ping statistics for 66.235.132.232: Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds: Minimum = 19ms, Maximum = 19ms, Average = 19ms
```

If the CNAME records are not correctly set up or not active, it will return the following:

`Ping request could not find the host. Please check the name and try again.`

>[!Note:] If you are using `https:// protocol`, ping will only respond after the upload date specified by the FPC specialist. An addition, be sure to ping the secure hostname and non-secure hostname to ensure that both are working correctly before updating your implementation.

## Update implementation code

Before you edit code on your site to utilize first-party cookies, complete these prerequisites:

* Request an SSL certificate, as described above in Implementation Steps for the Adobe Managed Certificate Program.
* Create CNAME records (see above).
* Ping the hostname (see above).

After you have verified your hostname(s) are responding and forwarding to Adobe data collection servers, you can alter your implementation to point to your own data collection hostnames.

1. Open your core JavaScript file (`s_code.js/AppMeasurement.js`).
1. If you want to update your code version, replace your entire `s_code.js/AppMeasurement.js` file with the newer version and replace any plugins or customizations (if any). **Or**, if you want to update the code only pertinent to first-party cookies, locate the s.trackingServer and s.trackingServerSecure (if using SSL) variables, and point them to your new data collection hostnames. Using mysite.com as an example:`s.trackingServer = "metrics.mysite.com"` `s.trackingServerSecure = "smetrics.mysite.com"`

1. Upload the updated core JavaScript file to your site.

1. If you are moving to first-party cookies from a long-standing implementation, or changing to a different first-party collection hostname, we recommend migrating visitors from the previous domain to the new domain.

See [Visitor Migration](https://docs.adobe.com/help/en/analytics/implementation/javascript-implementation/visitor-migration.html) in the Analytics Implementation Guide.

After you have uploaded the JavaScript file, everything is configured for first-party cookie data collection. We recommend that you monitor Analytics reporting for the next several hours to ensure that data collection continues as normal. If it does not, verify that all above steps have been completed and have one of your organization's supported users contact Customer Care.
