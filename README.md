# RETCONnect: A human- and machine- readable language for describing interconnected personal IoT systems and platforms.

Our world is now full of connected devices that help us run our daily lives.  Behind the scenes, these devices interact with many cloud services and systems, in complex ways that are not made evident to end-users. Yet, it can be important to know how the important devices in our worlds are connected, and the services upon which they rely, for many reasons.  

For instance, at a basic level, you might want to know whether a system you need to rely on uses a particular intermediary.  This might be, for iexample, if an intermediary is known for unscrupulous business practices or has been hacked/compromised, you might want to avoid use of systems that use that.  Similarly, if a vital service that your system relies on lies in a hostile government or entity's jurisdiction, you might need to know this in order to assess what would happen if the hostile entity takes advantage of its position within your system.

More importantly, you might want to know all the services or entities that the systems you use use as data processors and/or controllers. For instance, you might want to prevent your data from being processed by a company that has a known record for human rights abuses.

## Background

There are many kinds of system modelling languages used by various stakeholders in the industry for documenting complex systems and reasoning about their properties and capabilities.  Although some are general-purpose, most such languages are often designed for particular domains (such as industrial control systems) or particular purposes (such as supporting software documentation).

One such modelling langauge is [SysML](https://sysml.org/sysml-partners/), which is a profile of UML (Unified Modelling Language) and is used by many large, complex systems integrators.  However, being general purpose it is very verbose and not very easy for people to read, write, and edit. Thus, while inspired by it, we decided to create a new lightweight language which could, in theory, be easily converted to SysML or other system moelling langauges, but better suits its intended purpose.

Closely related to RETCONnect are [Software Bill of Materials](https://www.cisa.gov/sbom) which is an initiative to get manufacturers to provide a list of all the components of a software product, including the names of the suppliers and versions of the components. The intended purpose of SBOMs is to track the software components used in a product, for the purpose of identifying potential security risks. 

SBOMs have been proposed as a way to: 
- Identify potential security risks in a product.
- Track the provenance of software components.
- Comply with regulations that require disclosure of software components.
- Improve the security of software products.
- To inform customers, suppliers, and other stakeholders of such dependencies.

## Examples

This vocabulary makes it possible for information about systems to be easily declared, read, modified, and shared by people and by machines. 

In the following example, we have a specification for a smart tv that both has dependencies at the OS/system level and also supports installation of apps which, each, in turn, have their own dependencies.  Note that this description is specific to the OS version and model. 

```yaml
name: Acme-SmartTV-2000-42
type: device
subtype: smarttv
networkname: SmartTV 2000-42
manufacturer: Acme
model: SM652000-42-2022
osversion: 1.0.22
dependencies:
  - libs/optimizely-android
  - libs/samsung-voice-recognition-saas-android
  - libs/nielsen-ratings-android
  - libs/appsflyer-android
software:
  - apps/bbciplayer-android
  - apps/netflix-android
  - apps/4od-android
  - apps/itv-android
disclosures:
  - destination: 
    - name: acme-usage
    - datatype: viewing stats, programmes
```

Let's go through this example step by step.

The first three keys identify the particular device. "name" is a global identifier (we could have called it `id`).  These keys help to uniquely distinguish records for slightly different variants of devices (versions, OSes) etc, since different versions could have characteristics worth noting.

The `dependencies` key contains a list of 1st and 3rd party dependencies that the core TV OS itself depends on. This dependencies could provide functionality key to essential services (such as providing software updates), or optional/unnecessary services such as marketing.

The `software` key represents apps installed either by the manufacturer or the user. Here we differentiate these dependencies because they are not considered essential to the base system functionality.

Each library, app, or service, in turn, can list its dependencies and has essential information about the jurisdictions and types of shared. It's shown in a separate file. For instnace, if we look at `appsflyer-android` under the `libs` directory:

```yaml
name: appsflyer-android
type: lib
libtype: [analytics, advertising]
org: appsflyer.com
platform: android
code_identifier: com.appsflyer
version: 1.0.xx
exodus_info: https://reports.exodus-privacy.eu.org/en/trackers/12/
```

Notice the org tag here; this means that there should be another page containing information about the company.

```yaml
name: appsflyer.com
type: company
companyurl: https://www.appsflyer.com/
nationality: US
description: AppsFlyer is a SaaS mobile marketing analytics and attribution platform, headquartered in San Francisco, California
moreinfo:
  - wikipedia:
    - url: https://en.wikipedia.org/wiki/AppsFlyer
    - type: wikipeida
  - itwire-1-april-2022:
    - url: https://itwire.com/business-it-news/security/$72-million-lost-in-mobile-app-ad-fraud-appsflyer.html
    - type: news
```

## But why?

There are several applications of 
