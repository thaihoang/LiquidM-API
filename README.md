LiquidM-api
==============



LiquidM offers a HTTP based API to publishers. We offer two different types of API-access. Please have a closer look at section "Ad request" to find out which is the best solution for your needs.

h2. Ad request

The HTTP request should:

*   use the simple ad request or the extended ad request service URL
*   be a POST request (Why not GET?)
*   set the preferred response format using the HTTP Accept Header (see also the table below)
*   have a x-www-form-urlencoded encoded POST body
*   have a POST body which contains all mandatory and zero or more optional parameters

There are two different api-access-types with different ad request URLs:

*   If you have a countable number of sites and/or apps please use the simple ad request
*   If you have lots of publishers, apps, and sites (e.g. you are an ad server or ad proxy provider please use the extended ad request

The HTTP Accept Header should be set to one of the following values (response format defaults to HTML):

text/html (HTML response)
application/xml (XML response)
application/json 	(JSON response)

h2. Ad response

Depending on the given parameters the HTTP response has different HTTP status codes and a different body.

The ad response may have different HTTP status codes:

200 	Everything OK. The response body contains the ad in HTML, JSON or XML
204 	Everything OK, but no ad is avaliable at the moment
404 	Nothing found - this may have different reasons. The passed token could be invalid or mandatory parameters are missing (ua, ip)

If the ad response status is 200 (OK) the response body will contain the ad. The response body can have different formats and can contain a large set of useful information.
Format of the ad response and examples

To change the ad response format, the HTTP Accept Header can be set to the following values:

*   text/html (HTML response)
*   application/xml (XML response)
*   application/json 	(JSON response)

h2. API Parameter

Parameter name |Possible value |Mandatory |Description |Example
---------------|:-----------:|:-----------:|:----------:|:-------
ua |valid string |yes |User agent string of the requestinyesg device |Mozilla/5.0 (iPod touch; U; CPU iPhone OS 2_2_1 like Mac OS X; en-us) AppleWebKit/525.18.1 (KHTML, like Gecko) Mobile/5H1
ip |valid ipv4 address |yes |IP of the requesting device |210.12.13.13
requester |valid string |yes |string which identifies the requesting entity, please use the example-value |LiquidM_api
version |valid string |yes |string which gives information about the version of the requesting entity, please use the example-value |api_2.1
hh_{.*} |valid string |yes |HTTP headers sent by the requesting device. The original name should be prefixed with hh_, all chars should be downcase. |hh_http_x_forwarded_for=147.13.13.4
LiquidM_cookie |valid string |yes |The “LiquidM” cookie which was passed by the requesting device. If the device doesn’t support cookies, send nothing. |UU-34m16x3
channels |one of the following: blind, community, games, portals, sports, movie_and_entertainment, music, automotive, travel, business_and_finance,lifestyle, technology_and_computer, erotic_content |no |one or more strings, seperated by ','. Use blind in the sense of all |games,sports,music
banner_type |mma, medium_rectangle, leaderboard, rich_media |no |You may request several kinds of banner_types. The given order equals a prioritization. You will not get more than one banner, but you may define fallback types if the first type is not available. Banner sizes are: mma = 300x50 (or 320x53, depending on the phone), medium_rectangle = 300x250, leaderboard = 728x90, rich_media = Full Screen, Every banner type is available as MRAID. rich media should always use mraid=true
unique_session_id |valid string |no |An id which is unique for the current users session. i.E: SHA1 |234a23e
unique_user_id |valid string |no |unique id for an user (beyond the session). i.E: SHA1 |cba13
udid_md5 |valid string |no |unique id which identifies the device (hashed with MD5) |d41d8cd98f00b204e9800998ecf8427e
udid_sha1 |valid string |no |unique id which identifies the device (hashed with SHA1) |da39a3ee5e6b4b0d3255bfef95601890afd80709
mac_md5 |valid string |no |mac adress which identifies the device (hashed with MD5) |d41d8cd98f00b204e9800998ecf8427e
mac_sha1 |valid string |no |mac adress which identifies the device (hashed with SHA1) |da39a3ee5e6b4b0d3255bfef95601890afd80709
idfa |valid string |no |IDFA which identifies the device |aa79a3ee5e6b4b0d3255bfef95601890afd80709
msisdn |valid string |no |MSISDN of the device |85291830654
imei |valid string |no |IMEI of the device |49015420323751
lat |dd.dddddd (degrees,decimal degrees) |no |User location latitute |34.131623
lng |dd.dddddd (degrees,decimal degrees) |no |User location longitude |17.414113
coordinates_precision |on of the following values:exact, city, country |no |Accuracy level of the given latitude and longitude pair |city
zip |valid string |no |ZIP code of the device user |15391
street |valid string |no |Street of the device user |Janway 15
city |valid string |no |City of the device user |Berlin
country |ISO 3166-1-alpha-2 Code |no |Country of the device user |DE
carrier |valid string |no |Name of the carrier |O2
age |single natural number (0-120) or range(0-120 to 0-120) |no |Age of the device user |17 or 12-24
gender |F or M |no |Gender of the device user |M
income |single natural number (in Euro) or range |no |Income of the device user |24000 or 34000-40000
incentivized |boolean |no |Is the app/mobile website user rewarded for a click/download of the advertised product or brand? |1
site_url |valid URL |no |The url of the requesting site. This is only useful for ad proxies. |http://admachines.mobi/stats
keywords |valid String |no |Keywords which are associated with the requesting site. |gaming
debug |boolean |no |If set to true enables debugging. In every response an additional header ist delivered. It contains information about the reason of a specific response. e.g.: X-LiquidM-Debug: Everything OK., X-LiquidM-Debug: User agent is unknown. |true
must_have_banner |boolean |no |Filtering: ensure the returned ad has a banner/image |false
must_have_text |boolean |no |Filtering: ensure the returned ad has a text |false
deliver_only_text |boolean |no |Filtering: ensures the returned ad has a text + does no deliver the banner even if existing |false

