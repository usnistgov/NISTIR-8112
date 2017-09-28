<a name="sec4"></a>

<div class="breaker"/>

## 4. Use Cases

This section details three use cases as a means of demonstrating the ways in which ASM and AVM can be leveraged to enrich authorization decisions, facilitate cross boundary interoperability and trust, and enable adoption of federated attributes. Each use case carries with it a set of authorization and privacy considerations as well as suggested metadata necessary to fulfill evaluation of the requisite authorization policy, in addition to an example of what an AVM assertion may look like.

The use cases are:  
1. Federated Access to Classified Documents in an Information Sharing Environment    
2. Citizen Access to Federal Benefits  
3. Law Enforcement Access to Intelligence Database  


### 4.1. Federated Access to Classified Document in an Information Sharing Environment

#### Overview

Monique, an Army employee with a current Secret clearance, attempts to access an information system that stores classified information, hosted on a shared Secure Internet Protocol Router Network (SIPRNet) site by the Air Force. Furthermore, the system, due to its sensitivity and the number of possible individuals that have legitimate need to access it, is protected using Attribute Based Access Control (ABAC) principles. ABAC evaluates access policy to enforce decisions based on attributes specific to the user and the resource (not addressed in the schema). 

When Monique attempts access to the resource, an attribute query is routed to her agency, the Army, to obtain the attributes needed to grant or deny access. The Army then asserts the requested set of attributes, which are evaluated against the access control policy of the Air Force hosted site so a decision can be made. 

While in an actual implementation there may be many different attributes required to access the protected resource, for the purposes of illustration, this use case will only focus on the *clearance* attribute. Furthermore, in this scenario it is assumed that the semantics and syntax associated with the attribute itself are established.

|**Attribute**                  |**Value**       |
|---------------------------------------|----------------------|
| Clearance | Secret |

#### Authorization Considerations

In a traditional ABAC scenario, the assertion from the Army system would only provide the value that they maintain within their own records. As a result, the receiving agency’s access control system is only able to make a decision based upon the asserted attribute value and nothing more (i.e., the employee’s clearance is Secret therefore they are authorized for access). Information such as: the recency of the clearance issuance, when it was last verified by the asserting agency, and from where the value originated are not factored into the process. 

With the inclusion of attribute metadata, the relying agency is able to make an informed, risk-based decision by adding the evaluation of the attribute metadata into their ABAC policies. For example, they could determine that anyone accessing this specific resource must have a Secret clearance that: originated from a DoD entity, has been verified in the last six months, and was verified by the providing entity against an authoritative database.

|**Authorization Policy**|
|-------------------------------|
| 1. Origin MUST be an organization that is part of the Department of Defense |
| 2. Verification of clearance MUST have been done in the last six months |
| 3. Verification of clearance MUST have been done against an authoritative database |

Through the establishment of AVM, these further considerations and requirements can be expressed in policy and compared to asserted information.

#### Privacy Considerations

In this scenario privacy considerations factoring into the selection of AVM are limited, the selected information is an absolute for access based on national security requirements and only the requested value and metadata are being returned to a trusted party as part of the assertion.

#### Suggested Attribute Value Metadata

Based on the scenario’s authorization and privacy considerations, the table below illustrates the AVM that is applied to support appropriate authorization decisions by the relying agency. It also provides notional values.

| **Element** | **Value** |
|------|------|
| **Verifier** | **Origin** - The clearance was verified by the originating entity—which in this case is the same as the provider|  
| **Verification Method**| **Record Check** - The attribute value was verified against the sponsoring agency's clearance database|
| **Last Verification** |	**6/10/16** (assume an access request date of 7/1/2016)|
| **Origin** | **United States Army** |
|**Pedigree**| **Authoritative** - The attribute’s value was generated and in this case asserted as well by the authoritative source|

#### XACML Example Policy

Attribute and metadata names, and valid values, are fictional.  These will ultimately depend on the technologies of the attribute sources that is being queried to evaluate policy.  URI's and namespaces, in some cases, have been removed for brevity.


~~~xml
  <xacml3:Policy Version="1.0" RuleCombiningAlgId="urn:oasis:names:tc:xacml:3.0:rule-combining-algorithm:deny-overrides" PolicyId="http://www.axiomatics.com/automatic-unique-id/50f5b25e-dc7f-4672-a673-1a482e53f023">
    <xacml3:Description>Use Case #1</xacml3:Description>
	<xacml3:PolicyDefaults><xacml3:XPathVersion>http://www.w3.org/TR/1999/REC-xpath-19991116</xacml3:XPathVersion></xacml3:PolicyDefaults>
    <xacml3:Target/>
    <xacml3:Rule RuleId="c01d7519-be21-4985-88d8-10941f44590a" Effect="Permit">
      <xacml3:Description>isTSClearance</xacml3:Description>
      <xacml3:Target>
        <xacml3:AnyOf>
          <xacml3:AllOf>
            <xacml3:Match MatchId="urn:oasis:names:tc:xacml:3.0:function:string-equal-ignore-case">
              <xacml3:AttributeValue DataType="http://www.w3.org/2001/XMLSchema#string">Secret</xacml3:AttributeValue>
              <xacml3:AttributeDesignator Category="urn:oasis:names:tc:xacml:1.0:subject-category:access-subject" AttributeId="clearance.value" MustBePresent="false" DataType="http://www.w3.org/2001/XMLSchema#string"/>
            </xacml3:Match>
          </xacml3:AllOf>
        </xacml3:AnyOf>
        <xacml3:AnyOf>
          <xacml3:AllOf>
            <xacml3:Match MatchId="urn:oasis:names:tc:xacml:3.0:function:string-equal-ignore-case">
              <xacml3:AttributeValue DataType="http://www.w3.org/2001/XMLSchema#string">ORIGIN</xacml3:AttributeValue>
              <xacml3:AttributeDesignator Category="urn:oasis:names:tc:xacml:1.0:subject-category:access-subject" AttributeId="clearance.verifier" MustBePresent="false" DataType="http://www.w3.org/2001/XMLSchema#string"/>
            </xacml3:Match>
          </xacml3:AllOf>
        </xacml3:AnyOf>
        <xacml3:AnyOf>
          <xacml3:AllOf>
            <xacml3:Match MatchId="urn:oasis:names:tc:xacml:3.0:function:string-equal-ignore-case">
              <xacml3:AttributeValue DataType="http://www.w3.org/2001/XMLSchema#string">authoritative</xacml3:AttributeValue>
              <xacml3:AttributeDesignator Category="urn:oasis:names:tc:xacml:1.0:subject-category:access-subject" AttributeId="clearance.pedigree" MustBePresent="false" DataType="http://www.w3.org/2001/XMLSchema#string"/>
            </xacml3:Match>
          </xacml3:AllOf>
        </xacml3:AnyOf>
        <xacml3:AnyOf>
          <xacml3:AllOf>
            <xacml3:Match MatchId="urn:oasis:names:tc:xacml:3.0:function:string-equal-ignore-case">
              <xacml3:AttributeValue DataType="http://www.w3.org/2001/XMLSchema#string">records check</xacml3:AttributeValue>
              <xacml3:AttributeDesignator Category="urn:oasis:names:tc:xacml:1.0:subject-category:access-subject" AttributeId="clearance.verification_method" MustBePresent="false" DataType="http://www.w3.org/2001/XMLSchema#string"/>
            </xacml3:Match>
          </xacml3:AllOf>
        </xacml3:AnyOf>
      </xacml3:Target>
    </xacml3:Rule>
  <xacml3:Rule RuleId="4bae1384-729b-4e3e-895e-ea8dfefe5704" Effect="Permit">
    <xacml3:Description>isOriginDOD</xacml3:Description>
    <xacml3:Target>
      <xacml3:AnyOf>
        <xacml3:AllOf>
          <xacml3:Match MatchId="urn:oasis:names:tc:xacml:3.0:function:string-equal-ignore-case">
            <xacml3:AttributeValue DataType="http://www.w3.org/2001/XMLSchema#string">DOD</xacml3:AttributeValue>
            <xacml3:AttributeDesignator Category="urn:oasis:names:tc:xacml:1.0:subject-category:access-subject" AttributeId="clearance.origin" MustBePresent="false" DataType="http://www.w3.org/2001/XMLSchema#string"/>
          </xacml3:Match>
        </xacml3:AllOf>
        <xacml3:AllOf>
          <xacml3:Match MatchId="urn:oasis:names:tc:xacml:3.0:function:string-equal-ignore-case">
            <xacml3:AttributeValue DataType="http://www.w3.org/2001/XMLSchema#string">ARMY</xacml3:AttributeValue>
            <xacml3:AttributeDesignator Category="urn:oasis:names:tc:xacml:1.0:subject-category:access-subject" AttributeId="clearance.origin" MustBePresent="false" DataType="http://www.w3.org/2001/XMLSchema#string"/>
          </xacml3:Match>
        </xacml3:AllOf>
        <xacml3:AllOf>
          <xacml3:Match MatchId="urn:oasis:names:tc:xacml:3.0:function:string-equal-ignore-case">
            <xacml3:AttributeValue DataType="http://www.w3.org/2001/XMLSchema#string">NAVY</xacml3:AttributeValue>
            <xacml3:AttributeDesignator Category="urn:oasis:names:tc:xacml:1.0:subject-category:access-subject" AttributeId="clearance.origin" MustBePresent="false" DataType="http://www.w3.org/2001/XMLSchema#string"/>
          </xacml3:Match>
        </xacml3:AllOf>
        <xacml3:AllOf>
          <xacml3:Match MatchId="urn:oasis:names:tc:xacml:3.0:function:string-equal-ignore-case">
            <xacml3:AttributeValue DataType="http://www.w3.org/2001/XMLSchema#string">MARINES</xacml3:AttributeValue>
            <xacml3:AttributeDesignator Category="urn:oasis:names:tc:xacml:1.0:subject-category:access-subject" AttributeId="clearance.origin" MustBePresent="false" DataType="http://www.w3.org/2001/XMLSchema#string"/>
          </xacml3:Match>
        </xacml3:AllOf>
        <xacml3:AllOf>
          <xacml3:Match MatchId="urn:oasis:names:tc:xacml:3.0:function:string-equal-ignore-case">
            <xacml3:AttributeValue DataType="http://www.w3.org/2001/XMLSchema#string">AIR FORCE</xacml3:AttributeValue>
            <xacml3:AttributeDesignator Category="urn:oasis:names:tc:xacml:1.0:subject-category:access-subject" AttributeId="clearance.origin" MustBePresent="false" DataType="http://www.w3.org/2001/XMLSchema#string"/>
          </xacml3:Match>
        </xacml3:AllOf>
        <xacml3:AllOf>
          <xacml3:Match MatchId="urn:oasis:names:tc:xacml:3.0:function:string-equal-ignore-case">
            <xacml3:AttributeValue DataType="http://www.w3.org/2001/XMLSchema#string">USCG</xacml3:AttributeValue>
            <xacml3:AttributeDesignator Category="urn:oasis:names:tc:xacml:1.0:subject-category:access-subject" AttributeId="clearance.origin" MustBePresent="false" DataType="http://www.w3.org/2001/XMLSchema#string"/>
          </xacml3:Match>
        </xacml3:AllOf>
      </xacml3:AnyOf>
    </xacml3:Target>
  </xacml3:Rule>
  <xacml3:Rule RuleId="a17ecf55-77c0-4ddc-ab81-fcff342bcf7f" Effect="Permit">
    <xacml3:Description>verificationDateWithinYear</xacml3:Description>
    <xacml3:Target/>
      <xacml3:Condition xmlns:xacml3="urn:oasis:names:tc:xacml:3.0:core:schema:wd-17">
        <xacml3:Apply FunctionId="urn:oasis:names:tc:xacml:1.0:function:dateTime-less-than">
          <xacml3:Apply FunctionId="urn:oasis:names:tc:xacml:1.0:function:dateTime-one-and-only">
            <xacml3:AttributeDesignator Category="urn:oasis:names:tc:xacml:3.0:attribute-category:environment" AttributeId="urn:oasis:names:tc:xacml:1.0:environment:current-dateTime" MustBePresent="false" DataType="http://www.w3.org/2001/XMLSchema#dateTime"/>
          </xacml3:Apply>
          <xacml3:Apply FunctionId="urn:oasis:names:tc:xacml:3.0:function:dateTime-add-yearMonthDuration">
            <xacml3:Apply FunctionId="urn:oasis:names:tc:xacml:1.0:function:dateTime-one-and-only">
              <xacml3:AttributeDesignator Category="urn:oasis:names:tc:xacml:1.0:subject-category:access-subject" AttributeId="clearance.last_verification" MustBePresent="false" DataType="http://www.w3.org/2001/XMLSchema#dateTime"/>
            </xacml3:Apply>
            <xacml3:AttributeValue DataType="http://www.w3.org/2001/XMLSchema#yearMonthDuration">P6M</xacml3:AttributeValue>
          </xacml3:Apply>
        </xacml3:Apply>
      </xacml3:Condition>
    </xacml3:Rule>
  </xacml3:Policy>
~~~


### 4.2. Citizen Access to Federal Benefits 

#### Overview

Jane is a veteran and she is in the process of establishing an online account to manage her Veterans Affairs (VA) educational benefits. The VA system leverages a federated identity model that is integrated with multiple trusted Identity Providers (IDPs), which offer high assurance credentials and identity attributes. Furthermore, the VA system leverages the asserted attributes to both populate the  online registration form and to make an initial eligibility determination when establishing an account. 

When Jane initiates the registration process she is notified by her IDP which attributes are being asserted to the VA, for what they are going to be used, and what type of metadata is being provided. Failure to enroll via the online process (if, for example the attribute value metadata is not within policy) triggers a backup offline verification process conducted by the VA.

|**Attribute**                  |**Value**       |
|---------------------------------------|----------------------|
| Veteran | Yes |

#### Authorization Considerations

For this transaction, the VA has identified the attribute *Veteran Status* as critical to making an initial authorization decision. Though the VA is likely to have an existing record for Jane, it may not be easily accessible to the application. To ease the process of online enrollment for the service the VA has determined and external assertion of veteran status is sufficient to open an account if the following policy is met:

|**Authorization Policy**|
|-------------------------------|
| 1. Veteran status must have been verified by the provider or the originating authority |
| 2. Veteran status must have been verified through document verification and against an authoritative database |

#### Privacy Considerations

In this use case, some metadata elements with privacy implications, such as `provider`, are necessary for the transaction. Since this must be included, it’s important to ensure that Jane is aware of the fact that her information is being transferred as metadata in transactions. By gaining explicit consent from Jane before releasing her veteran status (as required by the authorization policy), Jane is notified of the transfer of this attribute value, and she gives her permission for the transfer. Other metadata elements with privacy implications, such as `origin`, are not needed in this transaction, technically or policy-wise. Thus, they should be excluded since they are not necessary and their inclusion would potentially reveal a broad profile of Jane (e.g., related to her associations with certain organizations).

#### Suggested Attribute Value Metadata

Based on the scenario’s authorization and privacy considerations, the table below illustrates the AVM that is applied to support appropriate decisions by the VA system. It also provides notional values.

|**Element**  |**Value** |
|------|------|
| **Verifier** | **Provider** - The clearance was verified by the IDP (also acting as the AP in this instance)|  
| **Verification Method**| **Document verification with Record Check** - The attribute value was verified against a DD-214 provided by Jane and was checked against a National Archives and Records Administration database|

#### XACML Example Policy

Attribute and metadata names, and valid values, are fictional.  These will ultimately depend on the technologies of the attribute sources that is being queried to evaluate policy.  URI's and namespaces, in some cases, have been removed for brevity.


~~~xml
<xacml3:Policy Version="1" RuleCombiningAlgId="urn:oasis:names:tc:xacml:3.0:rule-combining-algorithm:deny-overrides" PolicyId="9458137c-535b-4f2e-9907-2e8c7d5881ad">
  <xacml3:Description>Use Case #2</xacml3:Description>
  <xacml3:PolicyDefaults>
    <xacml3:XPathVersion>http://www.w3.org/TR/1999/REC-xpath-19991116</xacml3:XPathVersion>
  </xacml3:PolicyDefaults>
  <xacml3:Target/>
  <xacml3:Rule RuleId="35e6f270-5504-4596-9786-431d7de04402" Effect="Permit">
    <xacml3:Description>isVeteran</xacml3:Description>
    <xacml3:Target>
      <xacml3:AnyOf>
        <xacml3:AllOf>
          <xacml3:Match MatchId="urn:oasis:names:tc:xacml:1.0:function:boolean-equal">
            <xacml3:AttributeValue DataType="http://www.w3.org/2001/XMLSchema#boolean">true</xacml3:AttributeValue>
            <xacml3:AttributeDesignator Category="UC2" AttributeId="veteran.value" MustBePresent="false" DataType="http://www.w3.org/2001/XMLSchema#boolean"/>
          </xacml3:Match>
        </xacml3:AllOf>
      </xacml3:AnyOf>
      <xacml3:AnyOf>
        <xacml3:AllOf>
          <xacml3:Match MatchId="urn:oasis:names:tc:xacml:3.0:function:string-equal-ignore-case">
            <xacml3:AttributeValue DataType="http://www.w3.org/2001/XMLSchema#string">PROVIDER</xacml3:AttributeValue>
            <xacml3:AttributeDesignator Category="UC2" AttributeId="veteran.verifier" MustBePresent="false" DataType="http://www.w3.org/2001/XMLSchema#string"/>
          </xacml3:Match>
        </xacml3:AllOf>
      </xacml3:AnyOf>
      <xacml3:AnyOf>
        <xacml3:AllOf>
          <xacml3:Match MatchId="urn:oasis:names:tc:xacml:3.0:function:string-equal-ignore-case">
            <xacml3:AttributeValue DataType="http://www.w3.org/2001/XMLSchema#string">&gt;Document Verification with Record Verification</xacml3:AttributeValue>
            <xacml3:AttributeDesignator Category="UC2" AttributeId="veteran.verification_method" MustBePresent="false" DataType="http://www.w3.org/2001/XMLSchema#string"/>
          </xacml3:Match>
        </xacml3:AllOf>
      </xacml3:AnyOf>
    </xacml3:Target>
  </xacml3:Rule>
</xacml3:Policy>
~~~

### 4.3. Law Enforcement Access to a Government Database

#### Overview

Claude is with the Los Angeles Police Department (LAPD) and is attempting to access an FBI criminal justice database in order to gather additional information for a high-profile case. This database uses a federated identity model with multiple IDPs across affiliated law enforcement agencies. Due to the sensitive information retained within the database, access is protected based by ABAC. The attributes are asserted by the appropriate law enforcement agency (in this case the LAPD) to the FBI, who is then able to evaluate the attributes and make an access decision.

|**Attribute**                  |**Value**       |
|---------------------------------------|----------------------|
| Sworn Law Enforcement Officer | Yes |
| CJIS Privacy Training | Yes |

#### Authorization Considerations

We assume in this example that the access request was sent on 7/1/16. The FBI allows access to this database based around two major requirements. The first requirement is that Claude must be a Sworn Law Enforcement Officer (LEO), verified at least quarterly in order to prevent granting access to retired users. The second requirement is that the Claude must have completed Criminal Justice Information System (CJIS) Privacy Training. Verification of the completion of this training must be done within 12 months.

|**Authorization Policy**|
|-------------------------------|
| 1. Origin MUST be FBI or an affiliated law enforcement agency |
| 2. User MUST be a Sworn LEO with status validated within the last quarter (3 months) |
| 3. CJIS Privacy Training MUST have been completed within the last 12 months |

#### Privacy Considerations

In this use case, certain metadata elements are necessary to demonstrate compliance with access requirements for this database. However, excessive metadata collection that extends beyond these requirements could unnecessarily reveal information about law enforcement officials accessing the system. For example, provider metadata is not necessary for this transaction, and could reveal unintended information about Claude by divulging his relationship with the provider organization. Other metadata elements (e.g., origin) are necessary, but might still have privacy implications for Claude by revealing information about him. In these instances, it is important to—when possible—ensure that Claude is aware of which information is being transferred.

#### Suggested Attribute Value Metadata

Based on the scenario’s authorization and privacy considerations, the table below illustrates the AVM that is applied to support appropriate authorization decisions by the FBI. It also provides notional values.

|**Element**  |**Value** |
|------|------|
| **Verifier** | **Origin** - The statuses and verification dates for both Sworn LEO and CJIS Privacy Training would be verified by the originating entity (LAPD) |  
| **Last Verification (Sworn LEO)** |	**6/15/16** |
| **Last Verification (CJIS Privacy Training)** |	**6/1/15** |
| **Origin (both)** | **Los Angeles Police Department** |
|**Pedigree (both)**| **Authoritative** - The attribute’s value was generated and in this case asserted as well by the authoritative source|

Based on information about the user sent to the FBI by the LAPD IDP, the user is a Sworn LEO and has been verified as such within the last month (6/15/16). The user has also completed CJIS Privacy Training. However, the last verified date for the CJIS Privacy Training value was 13 months ago (6/1/15). In accordance with policy and based on interrogation of AVM, Claude is denied access based on the amount of time since the value for CJIS Privacy Training was verified. Here, the FBI has maintained its policy that simply taking the CJIS Privacy Training is not enough; it must have also been completed and verified within the last year as well. Similar to the “Federated Access to Classified Document in an Information Sharing Environment” example, the inclusion of AVM allows for more informed and fine grained access control decisions than in a traditional ABAC instance.

#### XACML Example Policy

Attribute and metadata names, and valid values, are fictional.  These will ultimately depend on the technologies of the attribute sources that is being queried to evaluate policy.  URI's and namespaces, in some cases, have been removed for brevity.


~~~xml
<xacml3:Policy Version="1" RuleCombiningAlgId="urn:oasis:names:tc:xacml:3.0:rule-combining-algorithm:deny-overrides" PolicyId="72537098-66a9-4283-8790-9c567eb2be1d">
  <xacml3:Description>Use Case #3</xacml3:Description>
  <xacml3:PolicyDefaults>
    <xacml3:XPathVersion>http://www.w3.org/TR/1999/REC-xpath-19991116</xacml3:XPathVersion>
  </xacml3:PolicyDefaults><xacml3:Target/><xacml3:Rule RuleId="1db3d77b-7467-42d3-82cc-0ae61facdad4" Effect="Permit">
  <xacml3:Description>isSLEO</xacml3:Description>
  <xacml3:Target>
    <xacml3:AnyOf>
      <xacml3:AllOf>
        <xacml3:Match MatchId="urn:oasis:names:tc:xacml:1.0:function:boolean-equal">
          <xacml3:AttributeValue DataType="http://www.w3.org/2001/XMLSchema#boolean">true</xacml3:AttributeValue>
          <xacml3:AttributeDesignator Category="UC3" AttributeId="sleo.value" MustBePresent="false" DataType="http://www.w3.org/2001/XMLSchema#boolean"/>
        </xacml3:Match>
      </xacml3:AllOf>
    </xacml3:AnyOf>
    <xacml3:AnyOf>
      <xacml3:AllOf>
        <xacml3:Match MatchId="urn:oasis:names:tc:xacml:3.0:function:string-equal-ignore-case">
          <xacml3:AttributeValue DataType="http://www.w3.org/2001/XMLSchema#string">ORIGIN</xacml3:AttributeValue>
          <xacml3:AttributeDesignator Category="UC3" AttributeId="sleo.verifier" MustBePresent="false" DataType="http://www.w3.org/2001/XMLSchema#string"/>
        </xacml3:Match>
      </xacml3:AllOf>
    </xacml3:AnyOf>
    <xacml3:AnyOf>
      <xacml3:AllOf>
        <xacml3:Match MatchId="urn:oasis:names:tc:xacml:3.0:function:string-equal-ignore-case">
          <xacml3:AttributeValue DataType="http://www.w3.org/2001/XMLSchema#string">LAPD</xacml3:AttributeValue>
          <xacml3:AttributeDesignator Category="UC3" AttributeId="sleo.origin" MustBePresent="false" DataType="http://www.w3.org/2001/XMLSchema#string"/>
        </xacml3:Match>
      </xacml3:AllOf>
    </xacml3:AnyOf>
    <xacml3:AnyOf>
      <xacml3:AllOf>
        <xacml3:Match MatchId="urn:oasis:names:tc:xacml:3.0:function:string-equal-ignore-case">
          <xacml3:AttributeValue DataType="http://www.w3.org/2001/XMLSchema#string">AUTHORITATIVE</xacml3:AttributeValue>
          <xacml3:AttributeDesignator Category="UC3" AttributeId="sleo.pedigree" MustBePresent="false" DataType="http://www.w3.org/2001/XMLSchema#string"/>
        </xacml3:Match>
      </xacml3:AllOf>
    </xacml3:AnyOf>
  </xacml3:Target>
  <xacml3:Condition xmlns:xacml3="urn:oasis:names:tc:xacml:3.0:core:schema:wd-17">
    <xacml3:Apply FunctionId="urn:oasis:names:tc:xacml:1.0:function:dateTime-less-than">
      <xacml3:Apply FunctionId="urn:oasis:names:tc:xacml:1.0:function:dateTime-one-and-only">
        <xacml3:AttributeDesignator Category="urn:oasis:names:tc:xacml:3.0:attribute-category:environment" AttributeId="urn:oasis:names:tc:xacml:1.0:environment:current-dateTime" MustBePresent="false" DataType="http://www.w3.org/2001/XMLSchema#dateTime"/>
      </xacml3:Apply>
      <xacml3:Apply FunctionId="urn:oasis:names:tc:xacml:3.0:function:dateTime-add-yearMonthDuration">
        <xacml3:Apply FunctionId="urn:oasis:names:tc:xacml:1.0:function:dateTime-one-and-only">
          <xacml3:AttributeDesignator Category="UC3" AttributeId="sleo.last_verification" MustBePresent="false" DataType="http://www.w3.org/2001/XMLSchema#dateTime"/>
        </xacml3:Apply>
        <xacml3:AttributeValue DataType="http://www.w3.org/2001/XMLSchema#yearMonthDuration">P3M</xacml3:AttributeValue>
      </xacml3:Apply>
    </xacml3:Apply>
  </xacml3:Condition>
</xacml3:Rule>
<xacml3:Rule RuleId="0ef722ee-1b81-4c4b-98fa-34fbc5f17ea3" Effect="Permit">
  <xacml3:Description>isPrivTrained</xacml3:Description>
  <xacml3:Target>
    <xacml3:AnyOf>
      <xacml3:AllOf>
        <xacml3:Match MatchId="urn:oasis:names:tc:xacml:1.0:function:boolean-equal">
          <xacml3:AttributeValue DataType="http://www.w3.org/2001/XMLSchema#boolean">true</xacml3:AttributeValue>
          <xacml3:AttributeDesignator Category="UC3" AttributeId="cjis_privacy_training.value" MustBePresent="false" DataType="http://www.w3.org/2001/XMLSchema#boolean"/>
        </xacml3:Match>
      </xacml3:AllOf>
    </xacml3:AnyOf>
    <xacml3:AnyOf>
      <xacml3:AllOf>
        <xacml3:Match MatchId="urn:oasis:names:tc:xacml:3.0:function:string-equal-ignore-case">
          <xacml3:AttributeValue DataType="http://www.w3.org/2001/XMLSchema#string">ORIGIN</xacml3:AttributeValue>
          <xacml3:AttributeDesignator Category="UC3" AttributeId="cjis_privacy_training.verifier" MustBePresent="false" DataType="http://www.w3.org/2001/XMLSchema#string"/>
        </xacml3:Match>
      </xacml3:AllOf>
    </xacml3:AnyOf>
    <xacml3:AnyOf>
      <xacml3:AllOf>
        <xacml3:Match MatchId="urn:oasis:names:tc:xacml:3.0:function:string-equal-ignore-case">
          <xacml3:AttributeValue DataType="http://www.w3.org/2001/XMLSchema#string">LAPD</xacml3:AttributeValue>
          <xacml3:AttributeDesignator Category="UC3" AttributeId="cjis_privacy_training.origin" MustBePresent="false" DataType="http://www.w3.org/2001/XMLSchema#string"/>
        </xacml3:Match>
      </xacml3:AllOf>
    </xacml3:AnyOf>
    <xacml3:AnyOf>
      <xacml3:AllOf>
        <xacml3:Match MatchId="urn:oasis:names:tc:xacml:3.0:function:string-equal-ignore-case">
          <xacml3:AttributeValue DataType="http://www.w3.org/2001/XMLSchema#string">AUTHORITATIVE</xacml3:AttributeValue>
          <xacml3:AttributeDesignator Category="UC3" AttributeId="cjis_privacy_training.pedigree" MustBePresent="false" DataType="http://www.w3.org/2001/XMLSchema#string"/>
        </xacml3:Match>
      </xacml3:AllOf>
    </xacml3:AnyOf>
  </xacml3:Target>
  <xacml3:Condition xmlns:xacml3="urn:oasis:names:tc:xacml:3.0:core:schema:wd-17">
    <xacml3:Apply FunctionId="urn:oasis:names:tc:xacml:1.0:function:dateTime-less-than">
      <xacml3:Apply FunctionId="urn:oasis:names:tc:xacml:1.0:function:dateTime-one-and-only">
        <xacml3:AttributeDesignator Category="urn:oasis:names:tc:xacml:3.0:attribute-category:environment" AttributeId="urn:oasis:names:tc:xacml:1.0:environment:current-dateTime" MustBePresent="false" DataType="http://www.w3.org/2001/XMLSchema#dateTime"/>
      </xacml3:Apply>
      <xacml3:Apply FunctionId="urn:oasis:names:tc:xacml:3.0:function:dateTime-add-yearMonthDuration">
        <xacml3:Apply FunctionId="urn:oasis:names:tc:xacml:1.0:function:dateTime-one-and-only">
          <xacml3:AttributeDesignator Category="UC3" AttributeId="cjis_privacy_training.last_verification" MustBePresent="false" DataType="http://www.w3.org/2001/XMLSchema#dateTime"/>
        </xacml3:Apply>
        <xacml3:AttributeValue DataType="http://www.w3.org/2001/XMLSchema#yearMonthDuration">P1Y</xacml3:AttributeValue>
      </xacml3:Apply>
    </xacml3:Apply>
  </xacml3:Condition>
</xacml3:Rule>
</xacml3:Policy>
~~~
