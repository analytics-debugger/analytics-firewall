![image](https://github.com/analytics-debugger/analytics-firewall/assets/1494564/2ef31c27-2260-4f0a-bffe-c20b4877f014)


# Analytics Firewall
This application enables anyone to set up a personalized public endpoint capable of receiving **Google Analytics 4 Payloads**, similar to a SGTM (*Server-side Google Tag Manager*) endpoint.

The primary objective of this tool is to ensure the collection of highly accurate and pristine data for your GA4 implementation.

# Why this tool

It's being really frustating working with client on bypassing all the limitations of the new Google Analytics Suite, I started to build this the last year, for being able to export the GA4 to BigQuery without any limits ( 1M hits ), so think this is a replacement for the automatic exporting features. 

At the same time, while I was in the SuperWeek,  though I could also try to fix somehow some of the tracking handicaps on BigQuery like the attribution, and then since we are here, why not adding some extra features, like Bot Spam Scoring, Automated PII Data Scrubbing, Parallel Tracking. Yes I know too many things, Hopefully some people my like the project I may end helping.

# Stack Needed
Don't blame me, I'm using PHP 8+ ( with some Asyncronous Support , need to learn mode about PHP Fibers at this point ). The main reason for using PHP is beacause is the most world-wide available server-side language, so it should allow anyone to run this endpoint with the less efforts possible

Any ports to other languages will be welcome at any point.

# Current Features

## Big Query Exporter

The tool will parse the **GA4 collect payload**, and will generate a JSON file/string following the *GA4 Big Query Format*, that could be imported directly on Big Query.

**Analytics Firewall**, will take of everything for you,

 - It will calculate the current session attribution and apply it to every event occurring within the user session.
 - It will retrieve the current geographical location data and map it to the corresponding geo section using the **GEOLite Free Database.**
 - It will extract browser/device details from the User Agent/Client Hints to populate all the relevant data fields.
 - It will handle the generation of internal events for "*session_start*" and "*first_visit*" automatically.

Data is generated in real-time, meaning that you could get RealTime Insigths if you opt-in for a database with that support. 

![image](https://github.com/analytics-debugger/analytics-firewall/assets/1494564/7aa38637-3533-46cf-8fc4-417df66d1b1c)


## Measurement ID spoofing

You can specify a fictitious Measurement ID cliens-side to safeguard your website from bot crawlers that programmatically generate hits. The fake ID will be overridden with the real one from the tool, ensuring protection against unwanted bot traffic.

## Parallel Tracking

 Effortless parallel tracking implementation. Forward a copy of the hits to any account you want. 

## Geo Ip Data
If you're just forwarding the events to Google Analytilcs, the system will pass throuigh the user's ip address so the geo details keep working, if you're using the BigQuery Model Exporter, it will take care of getting the data.

## Browser Data
If you're just forwarding the events to Google Analytilcs, the system will override the user agent and client hits headers of the hit sent to Google Analytics Server, if you're uing the BigQuery Model Exporter, it wil take care of guessing the browser/device details and pass it to the event data.

## Anti-Adblocker Payloads

Not sending the data to Google Endpoints will take care of some adblockers, but still they may check for  the current payload hints, Analytics Debugger will accept encoded/binary payloads, being able to bypass any ad-blocker.

## MiddleWares
There's some incoming support for using middleware and being able to remap the Big Query JSON Schema to other tools like ClickHouse / Snowplow, etc 

# Incoming Features

## Bot/Spam Scoring

Several rules used to detect bots activities, the system will assign an scoring based on each rule. Think about this like an Email Spam Filtering. Then you can just tag the hits (using an event_parameter) or blocking them .  Some examples of the rules to check

- Is the current IP ASN from a known non residential provider
- IP hits throttling ( too many hits from a single IP )
- User Agent validity Check
- Integration with thirst parties IP blacklists

## PII Scrubber
Filtering Personal Identificable Data is important, and hard at the same, Analytics Firewall, will be able to check the full payload details to find %LIKE%  string in the values ( ie: email like values ) or specific parameters within the URL like values and will scrub them out for you automatically.

## Data Sanity Checking.

I bet that some of you found that at point someone sent you a fake 1B transactions ruining all your reports. Since the the real Measurement ID can he hidden, we could even run some sanity check and nobody will be again be able to send data to your accounts.

 - For example you could defined that if there's a transaction where the value is > 1.000.000 and block it. 
 - Hold a whitelist of event names, skipping the ones that are not on that list
 - Event paremeters white lists. Automatically remove parameters that shouldn't be on the current event 
 - User properties. Automatically remove user properties that shouldn't be on the current event.
 - 
Guaranting that you won't get any data you don't want into your reports. 
