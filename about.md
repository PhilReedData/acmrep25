---
title: About
---

## Abstract
[RO-Crate](https://www.researchobject.org/ro-crate/) is a lightweight method to package research outputs along with their metadata. [Bioschemas](https://bioschemas.org/) provides metadata schemas to add structured metadata to webpages on Life Science. [Schemas.science](https://schemas.science/) extends this provision into non-bio domains. 

The combination of RO-Crates, Bioschemas and schemas.science make resources easy to navigate by machines, provide an unambiguous way for machines to access [FAIR](https://www.go-fair.org/fair-principles/) metadata and content in a single request, and reduce content-negotiation hassle that can give unpredictable results. 

This combination is of benefit for researchers and data providers whenever they want to improve FAIRability of their resources as they can support RO-Crate imports and align with Bioschemas specifications, making FAIR-compliant digital objects achievable with existing technologies such as Web Linking over HTTP.

The benefits of adopting RO-Crates are evident in a diverse range of applications. For example, *Five Safes RO-Crate* [https://w3id.org/5s-crate/](https://w3id.org/5s-crate/) specifies a profile of RO-Crate for the purpose of workflow execution in a distributed trusted research environment (TRE). The TRE-FX Project (delivered with HDR-UK) is providing safe, federated access to TREs, using the [Five Safes](https://www.gov.uk/data-ethics-guidance/the-five-safes-framework) RO-Crate standard.

Following a Bring Your Own Resource hands-on style, in this tutorial we will showcase how to use RO-Crates, and Bioschemas to improve FAIRability and will accompany attendees to apply the technologies to their own resources.


## Learning objectives

Following a Bring Your Own Resource hands-on style, in this tutorial we will showcase how to use RO-Crates and Bioschemas to improve FAIRability and will accompany attendees to apply the technologies to their own resources.

After completing the Bioschemas section of the tutorial, learners will be able to:

* Describe how schema.org and Bioschemas markup can be embedded to GitHub pages  
* Use schema.org and Bioschemas profiles on GitHub pages  
* Use schema and Bioschemas validators

After completing the RO-Crates section of the tutorial, learners will be able to:

* Construct an RO-Crate by hand using JSON  
* Describe each part of the Research Object  
* Use basic JSON-LD to create FAIR metadata  
* Connect different parts of the Research Object using identifiers

These learning outcomes are most relevant to the ACM reproducibility concepts, including: “Software and artifact packaging and container-related reproducibility methods”, “Data versioning and preservation”, and “Approaches for advancing reproducibility”.


## Intended audience

This introductory tutorial targets researchers and infrastructure managers who are producing file-based datasets. The audience will learn the method RO-Crate for publishing such datasets using a pragmatic approach to structure and provide FAIR metadata. 

## Audience prerequisites

Before attending the Bioschemas section of the tutorial, learners should already have:

* Familiarity on how to use GitHub; Basic knowledge on how to use GitHub Pages  
* Familiarity with JSON-LD  
* Basic knowledge on Markdown  
* Knowledge of developer tools on a browser

Before attending the RO-Crates section of the tutorial, learners should already have:

* Files and folder organization and using an editor/IDE  
* Familiarity with JSON file format

This part of the tutorial is meant to be read along with the [RO-Crate specification](https://www.researchobject.org/ro-crate/1.1/), which learners are suggested to browse in advance.

## Software needs for the tutorial

Participants should prepare by ensuring they can access their own free GitHub account. If they have not used GitHub for a while, they may need to configure two-factor authentication. You will need to use a text editor such as Notepad (Windows) or TextEdit (macOS). Any plain text editor will work, however, for a better experience, we recommend an IDE such as [VSCode](https://code.visualstudio.com/download) (free and built on open source). VSCode will provide features such as syntax highlighting and formatting for JSON documents. 

## About the instructors
**Phil Reed, The University of Manchester (UK)**

Research Community and Training Manager for The University of Manchester. Computer Science Research Associate, Library Data Specialist and Teaching and Learning Librarian (2012-2024), eScience Lab since 2024. Member of the Library Carpentry Curriculum Advisory Committee (2019-2024). Senior Fellow of Advance HE (the UK higher education academy), Fellow of the Software Sustainability Institute. Member of the Bioschemas community, co-leading hackathon events across Europe that improve the usability of Bioschemas resources.

**Abigail Miller, The Jackson Laboratory (USA)**

Principal Software Engineer for JAX Data Science
Architect and technical lead for BioConnect, a metadata indexing system that promotes data standards across the organization and leverages community and open source frameworks such as the RO Crate python library, Frictionless Data, and OpenFGA
Participant in ISA and RO Crate working groups and member of RO Crate steering committee
Has been developing research workflow software systems since 2003

