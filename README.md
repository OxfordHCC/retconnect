# RETCONnect: A human- and machine- readable language for describing interconnected personal IoT systems and platforms.

Our world is now full of connected devices that help us run our daily lives.  Behind the scenes, these devices interact with many cloud services and systems, in complex ways that are not made evident to end-users. Yet, it can be important to know how the important devices in our worlds are connected, and the services upon which they rely, for many reasons.  

For instance, at a basic level, you might want to know whether a system you need to rely on uses a particular intermediary.  This might be, for iexample, if an intermediary is known for unscrupulous business practices or has been hacked/compromised, you might want to avoid use of systems that use that.  Similarly, if a vital service that your system relies on lies in a hostile government or entity's jurisdiction, you might need to know this in order to assess what would happen if the hostile entity takes advantage of its position within your system.

More importantly, you might want to know all the services or entities that the systems you use use as data processors and/or controllers. For instance, you might want to prevent your data from being processed by a company that has a known record for human rights abuses.

## Background

There are many kinds of system modelling languages used by various stakeholders in the industry for documenting complex systems and reasoning about their properties and capabilities.  Although some are general-purpose, most such languages are often designed for particular domains (such as industrial control systems) or particular purposes (such as supporting software documentation).

One such modelling langauge is [SysML](https://sysml.org/sysml-partners/), which is a profile of UML (Unified Modelling Language) and is used by many large, complex systems integrators.  However, being general purpose it is very verbose and not very easy for people to read, write, and edit. Thus, while inspired by it, we decided to create a new lightweight language which could, in theory, be easily converted to SysML or other system moelling langauges, but better suits its intended purpose.

## Examples

This vocabulary makes it possible for information about systems to be easily declared, read, modified, and shared by people and by machines. 

```yaml

```

