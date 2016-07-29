<div class="text-right" markdown="1">

# NIST Internal Report 8112 (Draft)

# Attribute Metadata

Paul A. Grassi  
Ellen M. Nadeau  
Ryan J. Galluzzo   
Abhiraj T. Dinh

{::comment}

This publication is available free of charge from:
http://dx.doi.org/10.6028/NIST.SP.XXX  

{:/comment}

![](media/csd.png)    
![](media/nist_logo.png)  

# NIST Internal Report 8112 Draft

# Attribute Metadata

Paul A. Grassi  
Ellen M. Nadeau  
*Applied CyberSecurity Division  
Information Technology Laboratory*

Ryan J. Galluzzo  
Abhiraj T. Dinh  
*Deloitte & Touche LLP  
Rosslyn, VA*

{::comment}

This publication is available free of charge from:
http://dx.doi.org/10.6028/NIST.SP.XXX  

{:/comment}

July 2016

![](media/commerce_logo.png)

U.S. Department of Commerce  
*Penny Pritzker, Secretary*

National Institute of Standards and Technology  
*Willie E. May, Under Secretary of Commerce for Standards and
Technology and Director*

</div>

<div class="text-center" markdown="1">

National Institute of Standards and Technology Special Publication 800-63-3  
Natl. Inst. Stand. Technol. Spec. Publ. 800-63-3, xxx pages (MonthTBD 2016)  
CODEN: NSPUE2


{::comment}

This publication is available free of charge from:
http://dx.doi.org/10.6028/NIST.SP.XXX  

{:/comment}

>Certain commercial entities, equipment, or materials may be identified in this document in order to describe an experimental procedure or concept adequately. Such identification is not intended to imply recommendation or endorsement by NIST, nor is it intended to imply that the entities, materials, or equipment are necessarily the best available for the purpose.
There may be references in this publication to other publications currently under development by NIST in accordance with its assigned statutory responsibilities. The information in this publication, including concepts and methodologies, may be used by federal agencies even before the completion of such companion publications. Thus, until each publication is completed, current requirements, guidelines, and procedures, where they exist, remain operative. For planning and transition purposes, federal agencies may wish to closely follow the development of these new publications by NIST.
Organizations are encouraged to review all draft publications during public comment periods and provide feedback to NIST. Many NIST cybersecurity publications, other than the ones noted above, are available at [http://csrc.nist.gov/publications](http://csrc.nist.gov/publications).  

**Comments on this publication may be submitted to: <nsticwks@nist.gov>  
Public comment period: 01 August 2016 - 30 September 2016**  
All comments are subject to release under the Freedom of Information Act (FOIA).

National Institute of Standards and Technology  
Attn: Applied Cybersecurity Division, Information Technology Laboratory  
100 Bureau Drive (Mail Stop 2000) Gaithersburg, MD 20899-8930  
Email: <nsticworkshop@nist.gov>


### Reports on Computer Systems Technology

The Information Technology Laboratory (ITL) at the National Institute of
Standards and Technology (NIST) promotes the U.S. economy and public
welfare by providing technical leadership for the Nation’s measurement
and standards infrastructure. ITL develops tests, test methods,
reference data, proof of concept implementations, and technical analyses
to advance the development and productive use of information technology.
ITL’s responsibilities include the development of management,
administrative, technical, and physical standards and guidelines for the
cost-effective security and privacy of other than national
security-related information in Federal information systems. The Special
Publication 800-series reports on ITL’s research, guidelines, and
outreach efforts in information system security, and its collaborative
activities with industry, government, and academic organizations.

### Abstract
This NIST Internal Report contains a metadata schema for attributes that may be asserted about an individual during an online transaction. The schema can be used by relying parties to enrich access control policies, as well as during runtime evaluation of an individual's ability to access protected resources, and for an individual's. Attribute metadata could also create the possibility for data sharing permissions and limitations on individual data elements. There are other possible applications of attribute metadata, such as evaluation and execution of business logic in decision support systems; however the metadata contained herein is focused on supporting an organization's risk-informed authorization policies and evaluation.

### Keywords
Access control, assertions, attributes, attribute metadata, attribute values, attribute value metadata, authorization, federation, identity, identity federation, information security, metadata, privacy, risk, risk management, security, trust

### Acknowledgements
The authors would like to thank Josh Freedman for his significant contributions to this report, as well as Sean Brooks and Naomi Lefkovitz for their considerate inclusion of privacy related content.  In addition, we would like to thank Anil John and the Federal Identity, Credential, and Access Management (FICAM) Attribute Tiger Team for their leadership in developing the initial set of attribute metadata necessary for federal systems. Finally, we express significant gratitude to Darran Rolls of SailPoint Technologies, Inc., as well as Gerry Gebel and David Brossard of Axiomatics, for their insightful review of this report.

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
This NIST IR proposes a schema for attribute metadata and attribute value metadata intended to convey information about a subject's attribute(s) to allow for a relying party (RP) to:

* Obtain greater understanding of how the attribute and its value were obtained, determined, and vetted;
* Have greater confidence in applying appropriate authorization decisions to subjects external to the domain of a protected system or data;
* Develop more granular access control policies;
* Make more effective authorization decisions; and
* Promote federation of attributes.

This document defines a set of optional elements to support cross-organization confidence in attribute assertions as well as the semantics and syntax required to support interoperability. The schema contains two core components, `attribute metadata` and `attribute value metadata` which, along with their suggested elements, are described below:

* **Attribute Metadata** - Metadata for the attribute itself, not the specific attribute’s value. For example, this metadata may describe the `format` in which the attribute will be transmitted, height will always be sent in inches regardless of what the actual value may be (e.g., `height= 72`). This schema provides a set of attribute metadata from which to choose when constructing an attribute sharing agreement (trust-time) and the rationale for their inclusion.

| Metadata            | Description                                                                          | Recommended Values                                        |
| ------------------- |------------------------------------------------------------------------------------|---------------------------------------------|
| **Description** | An informative description of the attribute. |   None |
| **Allowed Values** | A defined set of allowed values for the attribute.  |   None  |     
|**Format**| A defined format in which the attribute will be expressed.| None |
| **Verification Frequency** |The frequency at which the Attribute Provider will re-verify the attribute.|None |

* **Attribute Value Metadata** - These elements focus on the asserted value for the attribute. Following the same example as above, the attribute value would be the actual height. A possible attribute value metadata for the height could be the name of the originating organization that provisioned the height, for example the DMV in the subject's home state. This schema provides a set of attribute value metadata, proposed values for those metadata fields, and rationale for their inclusion.



**Metadata Element**|**Description**|**Values**
--------------------|--------------|------------
**Verifier** |The entity that verified the attribute's value.<br>| -Origin <br> -Provider <br> -Not Verified
**Verification Method** |The method by which the attribute value was verified as true, and belonging to the specific individual.| -Document Verification <br> -Record Verification <br> -Document Verification with Record Verification <br> -Proof of Possession <br> -Not Verified
**Last Update** |The date and time when the attribute was last updated. |No restrictions
**Expiration Date** |The date an attribute’s value is considered to be no longer valid.|No restrictions
**Last Verification** |The date and time when the attribute value was last verified as being true and belonging to the specified individual.|No restrictions
**Origin** |The legal name of the entity that issues or creates the initial attribute value.| -Origin's name <br> -None
**Provider** |The legal name of the entity that is providing the attribute.|-Provider's Name <br> -None
**Pedigree** |Description of the attribute value's relationship to the authoritative source of the value.| -Authoritative <br> -Sourced <br> -Self-Asserted <br> -Derived
**Individual Consented** |Captures whether the user has expressly consented to providing the attribute value.| -Yes <br> -No <br> -Unknown
**Date Consented** | The date on which express consent for release of the attribute value was acquired. | No restrictions
**Acceptable Uses** |Allowed uses for entities that ingest attributes.| -Authorization <br> -Secondary Use <br> -No Further Disclosure
**Cache Time To Live** |The length of time for which an attribute value may be cached.| No restrictions
**Data Deletion Date** | Indicates the date a certain attribute should be deleted from records.| No restrictions
**Classification** | The U.S. Federal Government security classification level of the attribute.| -Unclassified <br> -Controlled Unclassified <br> -Confidential <br> -Secret <br> -Top Secret
**Releasability** |  The U.S. Federal Government restrictions regarding to whom an attribute value may be released. | -NATO <br> -FVEY <br> -NOFORN <br> -Public Release <br> -None


The schema in this document is intended to demonstrate the value of attribute and attribute value metadata in supporting U.S. Federal Government use cases and it is envisioned that the core set of metadata proposed here can serve as a library or menu from which both commercial and federal implementers can draw common semantics, syntaxes, and values to support their specific needs. This will serve as a jumping off point for the development of a metadata standard that can enable greater federation across markets and sectors.

## Table of Contents


[1. Introduction](#sec1)

[2. Definitions and Acronyms](#sec2)

[3. Metadata](#sec3)

[4. Use Cases](#sec4)

[5. References](#sec5)
