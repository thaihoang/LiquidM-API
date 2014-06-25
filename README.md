LiquidM API
==============



| Version | Date     | Editor         |
| --------| -------- | -------------- |
| 3.0     | 27.03.14 | Thomas Hille   |
|         |          | Siddhi Saraiya |
|         |          |                |


## Introduction

LiquidM offers a HTTP based API to publishers. We offer two different types of API-access. Please have a closer look at section "Ad request" to find out which is the best solution for your needs.


## Ad request

The HTTP request should:

*   use the simple ad request or the extended ad request service URL
*   be a POST request (Why not GET?)
*   set the preferred response format using the HTTP Accept Header (see also the table below)
*   have a x-www-form-urlencoded encoded POST body
*   have a POST body which contains all mandatory and zero or more optional parameters

### Ad Request URL

There are two different api-access-types that only differ in the schema of the ad request URLs.


#### Simple Ad Request

If you have a countable number of sites and/or apps we recommend that you use the simple ad request.
 
Please use the US or European URL schema depending on the location of your ad servers. 

The URL schema for this request is:
*European data center*: ad.eu1.lqm.io/site/SITE_TOKEN 
OR

*US data center*: ad.us1.lqm.io/site/SITE_TOKEN

The SITE_TOKEN represents a single placements. You can get a SITE_TOKEN by logging in to liquidm.com and creating a new website or app, or requesting it from your account manager.

#### Extended Ad Request

The extended ad request is designed for ad requests from publishers or networks that have to manage a large amount of sites and/or apps. With the help of this interface, placements can be generated automatically. The placements are categorized by the terms “publisher”, “site” and “sub”(-site). 
The URL schema for this request is:
*European data center*: 
http://ad.eu1.lqm.io/partner/PARTNER_ID/[publisher/PUBLISHER_ID][/site/SITE_ID[/sub/SUBSITE_ID]]/ad

or
*US data center*: 
http://ad.us1.lqm.io/partner/PARTNER_ID/[publisher/PUBLISHER_ID][/site/SITE_ID[/sub/SUBSITE_ID]]/ad

The ad request URL can be read as a RESTful URI of an ad with the respective scopes for the publisher, the site and the sub-site id. If one of the parameters is not relevant, the whole scope should be omitted – these are marked as optional via square brackets in the URL (see below for examples). The placements are generated automatically and named following the schema:
“Publisher:PUBLISHER_ID, Site:SITE_ID, SUB:SUBSITE_ID”


#### URL parts and scopes


| URL part | Possible value | Mandatory | Description | Example |
| -------- | -------------- | --------- | ----------- | ------- |
| PARTNER_ID | valid string | Yes | Your partner id. It will be provided on request. | 123451Ab4FC |
| PUBLISHER_ID | alphanumeric and “_”, “-“, “.” | No | Id of the publisher in your system, should be unique for your partner id. | 12345,1Ab4FC | 
| SITE_ID | alphanumeric and “_”, “-“, “.” | Yes, if SUBSITE_ID is provided | Id of the site in your system, should be unique for your partner id. | 12345,1Ab4FC, m.test.org, asdf-awsd |
| SUBSITE_ID | alphanumeric and “_”, “-“, “.” | No, requires SITE_ID | Id of the subsite identifying the place where the ad will be placed. Should be unique for the given SITE_ID. | login_view_top,home_view_bottom,AB26868CG |

Examples
The following URLs are correct URLs for partners with US-based data centers:
http://ad.us1.lqm.io/partner/my_partner_id/site/my_site_1/sub/login_page_top/ad
http://ad.us1.lqm.io/partner/my_partner_id/publisher/my_publisher/site/my_site_1/ad
http://ad.us1.lqm.io/partner/my_partner_id/site/my_site_1/ad
http://ad.us1.lqm.io/partner/demo/publisher/super_pub/site/m.demo.org/ad
                                             
The following URLs are correct URLs for partners with Europe-based data centers:

http://ad.eu1.lqm.io/partner/my_partner_id/publisher/my_publisher/site/my_site_1/ad
http://ad.eu1.lqm.io/partner/my_partner_id/site/my_site_1/sub/login_page_top/ad
http://ad.eu1.lqm.io/partner/my_partner_id/site/my_site_1/ad
http://ad.eu1.lqm.io/partner/demo/publisher/super_pub/site/m.demo.org/ad


## LiquidM API parameters


### Definitions

*       **Boolean**: true = true|t|1, false = false|f|0
*       **Valid String**: any combination of characters
*       **Range**: Consist of two positive natural numbers, separated by a dash. The first value has to be smaller or equal then the second value. Example: 25-45


### List of parameters

| Parameter name | Possible value | Mandatory | Description | Example | 
| -------------- |:-------------- | --------- | ----------- | ------- |
| ua | valid string | yes | User agent string of the requesting device | Mozilla/5.0 (iPod touch; U; CPU iPhone OS 2_2_1 like Mac OS X; en-us) AppleWebKit/525.18.1 (KHTML, like Gecko) Mobile/5H1 | 
| ip | valid ipv4 address | yes | IP of the requesting device | 210.12.13.13 | 
| requester | valid string | yes | string which identifies the requesting entity, please use the example-value | liquidm_api | 
| version | valid string | yes | string which gives information about the version of the requesting entity, please use the example-value | api_3.0 | 
| lqm_cookie | valid string | yes | The “LiquidM” cookie which was passed by the requesting device. If the device doesn’t support cookies, send nothing. | UU-34m16x3 | 
| category | IAB category and subcategory, separated by dash; IAB2-1 | no | one or more strings, seperated by ','. Use blind in the sense of all | IAB2-1,IAB3,IAB14 | 
| **Parameter name** | **Possible value** | **Mandatory** | **Description** | **Example** | 
| banner_type | widthxheight (request for any size you want by simply using the dimensions of the banner. )  For videos: video_pre_roll, video_mid_roll, video_post_roll | yes | You may request several kinds of banner_types. The order indicates the priority. You may request banners as widthxheight (e.g. 640x100) as well. You will not get more than one banner, but you may define fallback types if the first type is not available. Common/Recommended banner sizes  320x50 (xx_large),  300x250 (medium_rectangle)  728x90 (leaderboard),  1024x66 (landscape), full page interstitial (request as rich_media)  Video  video_pre_roll, video_mid_roll, video_post_roll | 320x50 | 
| uid | valid string | no | unique id for an user (beyond the session). i.E: SHA1 | cba13 | 
| udid_md5 | valid string | no | Equivalent of Android ID, or similar ID for other platforms unique id which identifies the device, hashed with MD5 | d41d8cd98f00b204e9800998ecf8427e | 
| udid_sha1 | valid string | no | Equivalent of Android ID, or similar ID for other platforms unique id which identifies the device, hashed with MD5, hashed with SHA1 | da39a3ee5e6b4b0d3255bfef95601890afd80709 | 
| mac_md5 | valid string | no | mac adress which identifies the device (hashed with MD5) | d41d8cd98f00b204e9800998ecf8427e | 
| mac_sha1 | valid string | no | mac adress which identifies the device (hashed with SHA1) | da39a3ee5e6b4b0d3255bfef95601890afd80709 | 
| idfa | valid string | no | IDFA which identifies the device | aa79a3ee5e6b4b0d3255bfef95601890afd80709 | 
| MSID | valid string | no | MSISDN of the device | 85291830654 | 
| lat | dd.dddddd (degrees,decimal degrees) | no | User location latitude | 34.131623 | 
| lng | dd.dddddd (degrees,decimal degrees) | no | User location longitude | 17.414113 | 
| coordinates_precision | on of the following values:exact, city, country | no | Accuracy level of the given latitude and longitude pair | city | 
| zip | valid string | no | ZIP code of the device user | 15391 | 
| street | valid string | no | Street of the device user | Janway 15 | 
| city | valid string | no | City of the device user | Berlin | 
| country | ISO 3166-1-alpha-2 Code | no | Country of the device user | DE | 
| carrier | valid string | no | Name of the carrier | O2 | 
| age | single natural number (0-120) or range(0-120 to 0-120) | no | Age of the device user | 17 or 12-24 |
| gender | F or M | no | Gender of the device user | M | 
| incentivized | boolean | no | Is the app/mobile website user rewarded for a click/download of the advertised product or brand? | 1 | 
| site_url | valid URL | no | The url of the requesting site. | http://admachines.mobi/stats | 
| keywords | valid String | no | Keywords which are associated with the requesting site content. | gaming
 
 
## Ad response
  
Depending on the given parameters the HTTP response has different HTTP status codes and a different body.

The ad response may have different HTTP status codes:

| HTTP Code | Explanation |
| --------- | ----------- |
| 200       | Everything OK. The response body contains the ad in HTML |
| 204       | Everything OK, but no ad is available at the moment |
| 404       | Nothing found - this may have different reasons. The passed token could be invalid or mandatory parameters are missing (ua, ip) |


If the ad response status is 200 (OK) the response body will contain the ad. The response body can contain a large set of useful information.
 
### Examples
* In all examples below, the user agent is an iPhone
* The IP is a LAN IP however this works for testing purpose
* We use the std. curl tool which ships with most Unix's
* We only use a minimal number of parameters
* hd_url is included in the response if the ad has an original HD version 
* Key: 
  request
  response

Invalid site token and just show headers.
 
curl -s -d 'ua=Mozilla%2F5.0+%28iPhone%3B+U%3B+CPU+iPhone+OS+3_0+like+Mac+OS+X%3B+en-us%29+AppleWebKit%2F528.18+%28KHTML%2C+like+Gecko%29+Version%2F4.0+Mobile%2F7A341+Safari%2F528.16&ip=10.0.0.1' -D- http://ad.us1.lqm.io/site/GIBT_ES_NICHT -o /dev/null
 
HTTP/1.1 404 Not Found Server: nginx/0.8.24 Date: Wed, 16 Jun 2010 23:02:50 GMT Content-Type: text/plain Connection: keep-alive Content-Length: 0

Valid request and just show headers.
 
curl -s -d 'ua=Mozilla%2F5.0+%28iPhone%3B+U%3B+CPU+iPhone+OS+3_0+like+Mac+OS+X%3B+en-us%29+AppleWebKit%2F528.18+%28KHTML%2C+like+Gecko%29+Version%2F4.0+Mobile%2F7A341+Safari%2F528.16&ip=10.0.0.1' -D- http://ad.us1.lqm.io/site/TestTokn -o /dev/null

HTTP/1.1 200 OK Server: nginx/0.8.24 Date: Wed, 16 Jun 2010 23:04:22 GMT Content-Type: text/html Connection: keep-alive Content-Length: 234 Set-Cookie: lqm_cookie=UU-a553ydp2

Valid request - this time show body.
 
curl  -d 'ua=Mozilla%2F5.0+%28iPhone%3B+U%3B+CPU+iPhone+OS+3_0+like+Mac+OS+X%3B+en-us%29+AppleWebKit%2F528.18+%28KHTML%2C+like+Gecko%29+Version%2F4.0+Mobile%2F7A341+Safari%2F528.16&ip=10.0.0.1' http://ad.us1.lqm.io/site/TestTokn  
 
<script>if ("localStorage" in window && window["localStorage"] !== null) { localStorage.setItem("liquidm", "31mepGsXr20"); } (function() {
      xhr = new XMLHttpRequest();
      xhr.open('GET', 'http://ad.us1.lqm.io/ed42/site/TestTokn/ad/VgcZ0cox/pingback/nwtac9z8', true);
      xhr.send();
     }());</script>
<a href='http://ad.us1.lqm.io/ed42/track/nwtac9z8/site/TestTokn/ad/VgcZ0cox' target='_parent'><img src='https://asset.us1.lqm.io/banners/46546/mma/iphone_f85260b5e60f2e81cae8e34f7063f6a5.jpg' alt=''/>MMA 300x50 Test Banner</a><img src='http://ad.us1.lqm.io/ed42/site/TestTokn/ad/VgcZ0cox/voxel.gif?nwtac9z8' width='1' height='1' alt=''/>

Invalid request (missing parameter - IP).
curl -s -d 'ua=Mozilla%2F5.0+%28iPhone%3B+U%3B+CPU+iPhone+OS+3_0+like+Mac+OS+X%3B+en-us%29+AppleWebKit%2F528.18+%28KHTML%2C+like+Gecko%29+Version%2F4.0+Mobile%2F7A341+Safari%2F528.16' -D- http://ad.us1.lqm.io/site/TestTokn -o /dev/null
 
HTTP/1.1 404 Not Found Server: nginx/0.8.24 Date: Wed, 16 Jun 2010 23:06:29 GMT Content-Type: text/plain Connection: keep-alive Content-Length: 0

HD banner responses

LiquidM does not work on a request basis on HD banners, instead it is taken care by the hd_url parameter in the response (see below).

{"click_url":"http://127.0.0.1:9292/track/8xt9eqjz/site/cpc/ad/YR8hhxDc","tracking":["http://127.0.0.1:9292/site/cpc/ad/YR8hhxDc/voxel.gif?8xt9eqjz"],"text":"CPC ad","has_banner":true,"should_open_in_app":true,"cookies":{"lqm_cookie":"0"},"banner":{"url":"https://lqm-assets-development.s3.amazonaws.com/banners/4/mma/iphone_ac7c8b869ef328dbadb942e368f302a0.jpg","type":"mma","hd_url":"https://lqm-assets-development.s3.amazonaws.com/banners/4/mma/hd_iphone_ac7c8b869ef328dbadb942e368f302a0.jpg"},"banner_type":"mma"}

This is a response for an mma standard banner (320x53) request. Since there is an HD banner uploaded, hd_url is also sent to be used where needed. If a known HD version of an MMA standard banner (320x50, leaderboard, 320x480) is requested specifically, this URL is not sent. Below, a response for “640x106” request, for the same ad.

{"click_url":"http://127.0.0.1:9292/track/84xl3f7w/site/cpc/ad/YR8hhxDc","tracking":["http://127.0.0.1:9292/site/cpc/ad/YR8hhxDc/voxel.gif?84xl3f7w"],"text":"CPC ad","has_banner":true,"should_open_in_app":true,"cookies":{"lqm_cookie":"0"},"banner":{"url":"https://lqm-assets-development.s3.amazonaws.com/banners/4/mma/hd_iphone_ac7c8b869ef328dbadb942e368f302a0.jpg","type":"mma"},"banner_type":"640x106"}
