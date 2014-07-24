# About
This document is part of the Integrated Award Environment (IAE) iae-global repository available at github.com/gsa/iae-global. The repository contains information on the architecture, design, implementation and operation of the IAE to-be enviornment. 

Questions about the repository can be directed through github or by email to iaeoutreach@gsa.gov.

This document provides an overview of the Solution Concept, which is a foundational document that is part of the IAE Vision, which in turn is part of the overall TOGAF-based Enterprise Architecture for IAE.

# Solution Concept
The IAE Solution Concept identifies the principle architectural features of the target (To-Be) IAE environment. The Solution Concept is a high-level view that illustrates the business, technical and organizational goals of the IAE program. The Solution Concept does not identify systems, contracts, development teams or other lower level artifacts.

The Solution Concept Diagram is available at github.com/gsa/iae-global/architecture/vision/IAE-solution-concept-diagram.png

# Solution Components
 - **Common Services**. The Common Services component provides foundational technical and business capabilities to support the operation of IAE. Foundational capabilities include Data Stores, Identity and Access Management, Hosting and Help Desk.
 - **Interface Management**. IAE is envisioned as a service oriented architecture (SOA) where users and systems access business transactions through multiple channels. IAE will build and operate one of these channels - a general user interface that allows Users access to business functionality. At the same time, and through the features of the 'Interface Management' component, other organizations will be able to build functionally equivalent (or enhanced) user interfaces that have the same access to business transactions implemented by IAE. A critical function of the Interface Management component is to ensure secure and audited use of the interfaces published by IAE.
 - **Users**. Users are any government or non-government person who access the IAE systems through the IAE built and maintained user interface.
 - **Systems**. A system is any government or non-government system that accesses the business or technical interfaces to IAE. 
 - **Post Award Processes**. The grants, loans and acquisition processes supported by IAE can be split into three distinct gropus of business functions. The Post Award (business) Processes  gather together all business processes, functions, reports and other functions that are required after an Award is made. 
 - **Pre Award Processes**. Pre-Award processes cover all business activities prior to an award being made.
 - **Entity Management**. All organizations and individual who receive payment from the government via a grant, loan or contract award must be registered in IAE. Anyone or any organization that completes such a registration is called an Entity. The Entity Management component support business processes for entity registration as well as the documenting of parties excluded from doing business with the Federal government. 
 - **BI**. The overall architecture for IAE includes a centralized data reporting capability tuned to provide business related information to support the operation and on-going improvement of IAE.

The Solution Concept Diagram is also known as the "IAE Target Architecture", "the green field picture" and the "space ship".
