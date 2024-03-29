# RETCONnect: A human- and machine- readable language for describing interconnected personal IoT systems and platforms.

Our world is now full of connected devices that help us run our daily lives.  Behind the scenes, these devices interact with many cloud services and systems, in complex ways that are not made evident to end-users. Yet, it can be important to know how the important devices in our worlds are connected, and the services upon which they rely, for many reasons.  

For instance, at a basic level, you might want to know whether a system you need to rely on uses a particular intermediary.  This might be, for iexample, if an intermediary is known for unscrupulous business practices or has been hacked/compromised, you might want to avoid use of systems that use that.  Similarly, if a vital service that your system relies on lies in a hostile government or entity's jurisdiction, you might need to know this in order to assess what would happen if the hostile entity takes advantage of its position within your system.

More importantly, you might want to know all the services or entities that the systems you use use as data processors and/or controllers. For instance, you might want to prevent your data from being processed by a company that has a known record for human rights abuses.

## Background

There are many kinds of system modelling languages used by various stakeholders in the industry for documenting complex systems and reasoning about their properties and capabilities.  Although some are general-purpose, most such languages are often designed for particular domains (such as industrial control systems) or particular purposes (such as supporting software documentation).

One such modelling langauge is [SysML](https://sysml.org/sysml-partners/), which is a profile of UML (Unified Modelling Language) and is used by many large, complex systems integrators.  However, being general purpose it is very verbose and not very easy for people to read, write, and edit. Thus, while inspired by it, we decided to create a new lightweight language which could, in theory, be easily converted to SysML or other system moelling langauges, but better suits its intended purpose.

### Software Bills of Materials (SBOMs)

Closely related to RETCONnect are [Software Bill of Materials](https://www.cisa.gov/sbom) which is an initiative led by the US Government's [Cybersecurity & Infrastructure Security Agency (CISA)](https://cisa.gov) to get manufacturers to provide a list of all the components of a software product, including the names of the suppliers and versions of the components. The intended purpose of SBOMs is to track the software components used in a product, for the purpose of identifying potential security risks. 

SBOMs have been proposed to support many different aspects of software quality and cybersecurity–for example:

- Risk management: SBOMs can be used to identify and assess the risks associated with using open source software. This can help organizations to mitigate the risks of supply chain attacks, intellectual property theft, and compliance violations.
- Compliance: SBOMs can be used to comply with regulations such as the Cybersecurity and Infrastructure Security Agency (CISA)'s Software Supply Chain Risk Management (SCRM) requirements.
- Security: SBOMs can be used to improve the security of software by identifying known vulnerabilities and providing information on how to patch them.
- DevOps: SBOMs can be used to improve the efficiency of DevOps processes by providing information on the components used in software. This can help to reduce the risk of errors and improve the quality of software.
- Communication: To inform customers, suppliers, and other stakeholders of such dependencies.
  
The use of Software Bills of Materials (SBOMs) is projected to increase; Forrester Research found that 74% of organizations have SBOMs on their 2-year roadmap. 

Despite their potential, adoption of SBOMs present significant hurdles.  One of the biggest  is collecting and managing the data needed to create an SBOM. This data can come from a variety of sources, including software vendors, open source projects, and internal systems. It can be difficult to identify all of the components that make up a software system, and it can be even more difficult to track changes to those components over time.

Another challenge is in finding the right representations for SBOMS in order for them to successfully serve all of the purposes envisioned. Thus far, there has been a lack of standardisation of SBOM representations, libraries, and tooling for supporting the reading and writing of SBOMs. Without such standardisation, it is likely that different vendors will produce SBOMs that represent wildly different information at different levels of detail that may greatly hinder their usefulness.

Finally, there a lack of end-user applciations that use SBOMs means that the perceived benefits are seen as hpypothetical, which means that the real tangible cost of creating SBOMs cannot be immediatley compared to the benefits they afford. 

### How does RETConnect relate to SBOMs?

RETCONnect was invented independently from CISA and others' SBON efforts, and grew out of the PETRAS X-Ray projects at the University of Oxford. The X-Ray projects aimed to empower end-user individuals to understand the hidden data infrastructures that run behind the scenes of the digital apps and services they use on the daily basis.  First in the series was App X-Ray, the first large-scale effort to map the Android app data ecosystem--using static code analysis.  This subsequently rendered obsolete by other similar projects like [Exodus](https://exodus-privacy.eu.org/en/). Other follow-up X-Ray projects include [X-Ray Refine](https://dl.acm.org/doi/10.1145/3173574.3173967), [TrackerControl](https://trackercontrol.org/), and, finally, [IoT Refine](https://dl.acm.org/doi/10.1145/3313831.3376264) for smart homes.  IoT Refine used network traffic analysis only to try to infer what data processors home IoT devices used, including identifying potential 3rd-party service libraries.

One problem with these projects was that while these analyses were used to create UIs that helped people understand the services behind their devices and apps, this knowledge could not further propagated downstream for other purposes.  It is in this that we created RETCONnect – a simple machine- and human-readable software bill of materials and device/service descriptor that provides not only descriptions for devices themselves but for the constituent libraries and companies behind them.

## RETCOnnect Examples

This vocabulary makes it possible for information about systems to be easily declared, read, modified, and shared by people and by machines. 

In the following example, we have a specification for a smart tv that both has dependencies at the OS/system level and also supports installation of apps which, each, in turn, have their own dependencies.  Note that this description is specific to the OS version and model. 

```yaml
id: Acme-SmartTV-2000-42
type: device
subtype: smarttv
networkid: SmartTV 2000-42
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
    - id: acme-usage
    - datatype: viewing stats, programmes
```

Let's go through this example step by step.

The first three keys identify the particular device. "name" is a global identifier (we could have called it `id`).  These keys help to uniquely distinguish records for slightly different variants of devices (versions, OSes) etc, since different versions could have characteristics worth noting.

The `dependencies` key contains a list of 1st and 3rd party dependencies that the core TV OS itself depends on. This dependencies could provide functionality key to essential services (such as providing software updates), or optional/unnecessary services such as marketing.

The `software` key represents apps installed either by the manufacturer or the user. Here we differentiate these dependencies because they are not considered essential to the base system functionality.

Each library, app, or service, in turn, can list its dependencies and has essential information about the jurisdictions and types of shared. It's shown in a separate file. For instnace, if we look at `appsflyer-android` under the `libs` directory:

```yaml
id: appsflyer-android
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
id: appsflyer.com
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

In this example, there are articles that have been crowdsourced by individuals who think that these articles can provide vital background information about the companies. Ideally this list would contain links to important information, including histories of data breaches, information on data protectiona audits and other vital information that can relate to its trustworthiness.

## Specification

Instead of creating a fixed specification, RETCONnect is designed to evolve, with the vocabulary itself defined in files that can be updated and versioned.  We take inspiration from XML namespaces to do this.

```yaml
_vid: vocabs/retconnect-apps-v1 # this sets the vocabulary, can be a URL to a vocabulary listed anywhere on the internet.
id: languagewizard.com
type: app
### .. rest of the file here 
```

The specifications itself look like this:

```yaml
id: retconnect-apps-v1
type: vocab
fields:
  - name: the name/title of the app
    - type: string
  - description: a human readable description of the app
    - type: string
  - platform: the platform of the app
    - type: iOS | Android
  - dependencies: the list of libraries used by the application
    - type: [ library ]
```

## Envisioned Applications

There are many possible applications that we have envisioned for RETCONnect. Here we describe a few that have been proposed, focused on those that could improve the security and privacy of individual citizens. 

- *Legible SBOMs for Guiding Buyers of Home IoT*:  Many "smart home" and connected devices are available to purchase at discount prices on various e-Retail sites, however it is difficult to understand whether these devices are built secureily, including where they will send their data and who their core data processors are.  Having a buyer's guide that is informed by actual data about devices would empower consumers to make more informed purchases.

- *Has My Data Been Leaked?*: Data breaches happen every day, and it is impractical for everyday citizens to try to keep track of what data breaches are relevant to them.  A repository of RETCONnects could enable a custom deamon to watch for published data breaches, and to match them against the dependencies of all  devices a person uses.  If a data controller used by a user's devices was experienced in a security vulnerability or data breach, then it is possible they were affected; this could be used to inform a mitigation strategy that is appropriate given the kind of data potentially compromised.

- *Blocking without Breaking*: Increasingly, home broadband routers are becoming capable of providing privacy protections to users including by blocking certain services and sites at will.  With RETCONnect, these routers could provide more granular blocking controls to end users; do users want to block all services for instance, of a particular type, or with a particular reputation? Or perhaps only those with a geopolitical designation, such as blocking data controllers in regions with governments known for human rights violations or for which certain lifestyles are prohibited by law.

- *AI Red-Teaming Applications*: We envision that the next generation of proactive cyber-security applications will be able to perform various kinds of monitoring and testing to help bolster individual cybersecurity. One such application is [ARETHA-RT](https://github.com/OxfordHCC/arethart), which is a conceptual prototype of a future AI Red Teaming assistant.

## License 

RETCONnect is licensed under an MIT License–please adopt, remix, and use as you see fit; however we do not guarantee its suitability for use in your application.

## Acknowledgements

RETCONnect was funded by the UKRI through the PETRAS2 Centre of Excellence, under the project RETCON: Red Teaming the Connected Internet of Things.