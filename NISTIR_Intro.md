<a name="sec1"></a>

<div class="breaker"/>

## 1. Introduction

Access control policy increasingly depends on evaluating the attributes of the individual or subject that is attempting to access a protected resource \[[Special Publication (SP) 800-162](#SP 800-162)\]. As enterprise domains continue to expand, architectures become further distributed, business relationships become more complex, and organizations increasingly depend on federated identities, methods are needed for evaluating externally asserted attributes to make the best and most appropriate authorization decision possible.  Such mechanisms will increase the ability of organizations to consume attributes as well as enrich and enforce critical access control policies. At the "Advanced Identity Workshop: Applying Measurement Science in the Identity Ecosystem" (hereafter "workshop") held at NIST in Gaithersburg on January 12 and 13, 2016, NIST proposed an initial set of attribute metadata as a step towards enabling greater federation and trust of identity attributes among identity ecosystem participants. This NIST Internal Report (NISTIR) represents a refined list of optional metadata that may be adopted when participating in a federated environment.  

### 1.1.	Purpose

This NISTIR proposes attribute schema metadata and attribute value metadata to convey information about a subject's attribute(s) to allow for a relying party (RP) to:

* Achieve greater understanding of how the attribute and its value were obtained, determined, and vetted;
* Promote greater confidence in applying appropriate authorization decisions to subjects external to the domain of a protected system or data (i.e., external users);
* Enable more effective authorization decisions; and 
* Promote federation of attributes.

The model proposed in this document allows RPs to determine the most appropriate attribute metadata elements for a given transaction. In the future, it could serve as a foundation for an attribute confidence scoring structure to simplify further the process of aligning attribute based authorization decisions with the risk environment.

In addition, as a NISTIR, this document is intended to be treated as an "implementers' draft" so that developers and business owners can determine the efficacy and required adjustments of the attribute metadata elements. By issuing this as an implementers' draft, NIST seeks to obtain feedback on agencies' and industries' experiences with this approach in order to identify next steps, such as potentially transitioning this document to a NIST SP or a contribution to a private sector standards developer.

### 1.2.	Scope

This NISTIR defines a set of optional elements of an attribute metadata schema to support cross-organization decision making, such as two executive branch agencies, in attribute assertions. It also provides the semantics and syntax required to support interoperability. NIST does not intend to make any of this schema required in federal systems and attribute-based information sharing.  Rather, this schema represents a compendium of possible metadata elements to assist in risk-based decision making by an RP. This schema is focused on subjects (individual users); objects and data tagging, while related, are out of scope.

Specifically, this document addresses the following:  

* **Attribute Schema Metadata (ASM)** - Metadata for the attribute itself, not the specific attribute’s value. For example, this metadata may describe the format in which the attribute will be transmitted, such as that height will always be sent in inches regardless of what the actual value may be (e.g., `height= '72'`). This schema provides a set of attribute metadata from which to choose when establishing and executing an attribute sharing agreement (i.e., trust-time) and the rationale for their inclusion.
* **Attribute Value Metadata (AVM)** - These elements focus on the asserted value for the attribute. Following the same example as above, the attribute value would be the actual height (72). A possible AVM for the height could be the name of the originating organization that provisioned the height, for example the DMV in the subject's home state. This schema provides a set of AVM, proposed values for those metadata fields, and rationale for their inclusion.
* **Use Cases** - To demonstrate the applicability of the proposed metadata schema, this document also provides example use cases in which the application of the proposed schema would be used to support authorization decision making, thus allowing for greater confidence in federated identities and attributes.
* **Example Assertions** - Finally, this report includes example assertions illustrating what a technical implementation of the schema would look like leveraging standards such as Extensible Access Control Markup Language (XACML).

![](media/Generic.png)


While the schema in this document is intended to demonstrate the value of attribute metadata in supporting U.S. federal government use cases, the ideal metadata schema could be used in both commercial and public sector implementations, thus serving as a foundation to enable greater federation across markets and sectors. Furthermore, NIST intends for the schema to be protocol and technology agnostic, thus capable of being supported across the spectrum of modern run-time access control architectures.
