
<div class="text-right" markdown="1">

# NIST Internal Report 8112

# Attribute Metadata:
## *A Proposed Schema for Evaluating Federated Attributes*   

Paul A. Grassi  
Naomi B. Lefkowitz  
Ellen M. Nadeau  
Ryan J. Galluzzo  
Abhiraj T. Dinh  

{::comment}

This publication is available free of charge from:
https://doi.org/10.6028/NIST.SP.XXX  

{:/comment}

![](media/csd.png)    
![](media/nist_logo.png) 

</div><div class="breaker text-right" markdown="1">


# NIST Internal Report 8112

# Attribute Metadata:
## *A Proposed Schema for Evaluating Federated Attributes*   

Paul A. Grassi  
Naomi B. Lefkowitz  
Ellen M. Nadeau  
*Applied CyberSecurity Division  
Information Technology Laboratory*

Ryan J. Galluzzo  
Abhiraj T. Dinh  
*Deloitte & Touche LLP  
Rosslyn, VA*

{::comment}

This publication is available free of charge from:
https://doi.org/10.6028/NIST.SP.XXX  

{:/comment}

Month TBD 2017

![](media/commerce_logo.png)

U.S. Department of Commerce  
*Penny Pritzker, Secretary*

National Institute of Standards and Technology  
*Kent Rochford, Acting Under Secretary of Commerce for Standards and
Technology and Director*

</div>

<div class="breaker"/>

<div class="text-center" markdown="1">

National Institute of Standards and Technology Internal Report 8112  
Natl. Inst. Stand. Technol. NISTIR 8112, xxx pages (MonthTBD 2017)  
CODEN: NSPUE2


{::comment}

This publication is available free of charge from:
https://doi.org/10.6028/NIST.SP.XXX  

{:/comment}

>Certain commercial entities, equipment, or materials may be identified in this document in order to describe an experimental procedure or concept adequately. Such identification is not intended to imply recommendation or endorsement by NIST, nor is it intended to imply that the entities, materials, or equipment are necessarily the best available for the purpose.
There may be references in this publication to other publications currently under development by NIST in accordance with its assigned statutory responsibilities. The information in this publication, including concepts and methodologies, may be used by federal agencies even before the completion of such companion publications. Thus, until each publication is completed, current requirements, guidelines, and procedures, where they exist, remain operative. For planning and transition purposes, federal agencies may wish to closely follow the development of these new publications by NIST.
Organizations are encouraged to review all draft publications during public comment periods and provide feedback to NIST. Many NIST cybersecurity publications, other than the ones noted above, are available at [http://csrc.nist.gov/publications](http://csrc.nist.gov/publications).  

**Comments on this publication may be submitted to: <nsticworkshop@nist.gov>  **  
All comments are subject to release under the Freedom of Information Act (FOIA).

National Institute of Standards and Technology  
Attn: Applied Cybersecurity Division, Information Technology Laboratory  
100 Bureau Drive (Mail Stop 2000) Gaithersburg, MD 20899-8930  
Email: <nsticworkshop@nist.gov>


### Reports on Computer Systems Technology

The Information Technology Laboratory (ITL) at the National Institute of Standards and Technology (NIST) promotes the U.S. economy and public welfare by providing technical leadership for the Nation’s measurement and standards infrastructure. ITL develops tests, test methods, reference data, proof of concept implementations, and technical analyses to advance the development and productive use of information technology. ITL’s responsibilities include the development of management, administrative, technical, and physical standards and guidelines for the cost-effective security and privacy of other than national security-related information in Federal information systems.

### Abstract
This NIST Internal Report contains a metadata schema for attributes that may be asserted about an individual during an online transaction. The schema can be used by relying parties to enrich access control policies, as well as during run-time evaluation of an individual's ability to access protected resources. Attribute metadata could also create the possibility for data sharing permissions and limitations on individual data elements. There are other possible applications of attribute metadata, such as evaluation and execution of business logic in decision support systems; however the metadata contained herein is focused on supporting an organization's risk-informed authorization policies and evaluation.

### Keywords
Access control, assertions, attributes, attribute metadata, attribute schema metadata, attribute values, attribute value metadata, authorization, federation, identity, identity federation, information security, metadata, privacy, risk, risk management, security, trust

### Acknowledgements
The authors would like to thank Josh Freedman for his significant contributions to this report, as well as Sean Brooks for his considerate inclusion of privacy related content.  In addition, we would like to thank Anil John and the Federal Identity, Credential, and Access Management (FICAM) Attribute Tiger Team for their leadership in developing the initial set of attribute metadata necessary for federal systems. Finally, we express significant gratitude to Darran Rolls of SailPoint Technologies, Inc., as well as Gerry Gebel and David Brossard of Axiomatics, for their insightful review of this report.

{::comment}

### Audience

### Compliance with NIST Standards and Guidelines

### Conformance Testing

### Note to Reviewers

### Note to Readers

### Trademark Information

{:/comment}

</div>


## Executive Summary
This NIST Internal Report proposes attribute schema metadata and attribute value metadata as part of an overall schema intended to convey information about a subject's attribute(s) to allow for a relying party (RP) to:

* Obtain greater understanding of how the attribute and its value were obtained, determined, and vetted;
* Have greater confidence in applying appropriate authorization decisions to subjects external to the domain of a protected system or data;
* Develop more granular access control policies;
* Make more effective authorization decisions;
* Manage rules about the processing of data more effectively; and
* Promote federation of attributes.

This document defines a set of optional elements to support cross-organization confidence in attribute assertions as well as the semantics and syntax required to support interoperability. The schema contains two core components, `attribute schema metadata` and `attribute value metadata` which, along with their suggested elements, are described below:

* **Attribute Schema Metadata (ASM)** - Metadata for the attribute itself, not the specific attribute’s value. For example, this metadata may describe the `format` in which the attribute will be transmitted, such as that height will always be sent in inches regardless of what the actual value may be (e.g., `height= 72`). This schema provides a set of attribute metadata from which to choose when constructing and executing an attribute sharing agreement (often called trust-time) and the rationale for their inclusion.

| Metadata            | Description                                                                          | Recommended Values                                        |
| ------------------- |------------------------------------------------------------------------------------|---------------------------------------------|
| **Description** | An informative description of the attribute |   Any |
| **Allowed Values** | A defined set of allowed values for the attribute  |   Any  |     
|**Format**| A defined format in which the attribute will be expressed| Any |
| **Verification Frequency** |The frequency at which the Attribute Provider will re-verify the attribute| Any |
| **Data Processing** | Describes the basis for processing attributes and attribute values | Any |

* **Attribute Value Metadata (AVM)** - These elements focus on the asserted value for the attribute. Following the same example as above, the attribute value would be the actual height. A possible AVM for the height could be the name of the originating organization that provisioned the height, for example the DMV in the subject's home state. This schema provides a set of AVM, proposed values for those metadata fields, and rationale for their inclusion.



**Metadata Element**|**Description**|**Values**
--------------------|--------------|------------
**Origin** |The name of the entity that issues or creates the initial attribute value| -\<Origin's Name> <br> -"None"
**Provider** |The name of the entity that is providing the attribute|-\<Provider's Name> <br> -"None"
**Pedigree** |Description of the attribute value's relationship to the authoritative source of the value| -"Authoritative" <br> -"Sourced" <br> -"Self-Asserted" <br> -"Derived"
**Verifier** |The entity that verified the attribute's value<br>| -"Origin" <br> -"Provider" <br> -"Not Verified"
**Verification Method** |The method by which the attribute value was verified as true and belonging to the specific individual| -"Document Verification" <br> -"Record Verification" <br> -"Document Verification with Record Verification" <br> -"Proof of Possession" <br> -"Probabilistic Verification" <br> -"Not Verified"
**Last Verification** |The date and time when the attribute value was last verified as being true and belonging to the specified individual|No restrictions
**Last Refresh** |The date and time when the attribute was last refreshed |No restrictions
**Expiration Date** |The date an attribute’s value is considered to be no longer valid|No restrictions
**Date Consented** | The date on which subject consent for release of the attribute value was acquired | No restrictions
**Consent Type** | Indicates the type of consent | No restrictions
**Acceptable Uses** |Allowed use conditions for entities that receive attributes| No restrictions
**Cache Time To Live** |The length of time for which an attribute value may be cached| No restrictions
**Data Deletion Date** | Indicates the date the attribute is to be deleted from records| No restrictions
**Classification** | The security classification level of the attribute| -"Unclassified" <br> -"Controlled Unclassified" <br> -"Confidential" <br> -"Secret" <br> -"Top Secret" <br> -"Company Confidential"
**Releasability** |  The restrictions regarding to whom an attribute value may be released | -"NATO" <br> -"NOFORN" <br> -"FVEY" <br> -"Public Release" <br> -"Externally Releasable for Business Purposes" <br> -"Do Not Release" <br> -"None"


The schema in this document is intended to demonstrate the value of attribute schema and attribute value metadata in supporting U.S. federal government use cases. NIST envisions that the core set of metadata proposed here can serve as a library or menu from which both commercial and federal implementers can draw common semantics, syntaxes, and values to support their specific needs. This will serve as a jumping off point for the development of a metadata standard that can enable greater federation across markets and sectors.

Though this is a finalized document, this schema will be developed further in future revisions, based upon implementation feedback received by the community.

## Table of Contents


[1. Introduction](#sec1)

[2. Definitions and Acronyms](#sec2)

[3. Metadata](#sec3)

[4. Use Cases](#sec4)

[5. References](#sec5)
