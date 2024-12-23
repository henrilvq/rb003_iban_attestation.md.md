# IBAN Attestation Data Rulebook

## Table of Contents

1. [Introduction](#introduction)
   
    1.1. [Scope](#scope)  
    1.2. [Background](#background)  
    1.3. [Goal of the IBAN Attestation](#goal-of-the-iban-attestation)  
    1.4. [Key words](#key-words)  
    1.5. [Terminology](#terminology)  
2. [IBAN attestation Issuance process](#iban-attestation-issuance-process)  
3. [IBAN attestation Verification process](#iban-attestation-verification-process)  
4. [IBAN attributes](#iban-attestation-attributes)  
5. [Trust infrastructure details](#trust-infrastructure-details)
   
    5.1. [Trust requirements on the EUCC attestation from the perspective of company registration offices as authentic sources for the EUCC](#trust-requirements-on-the-eucc-attestation-from-the-perspective-of-company-registration-offices-as-authentic-sources-for-the-eucc)  
    5.2. [Trust a signature or seal over a EUCC](#trust-a-signature-or-seal-over-a-eucc)  
    5.3. [EUCC Provider Trusted List](#eucc-provider-trusted-list)  
    5.4. [SD-JWT-compliant](#sd-jwt-compliant)  
8. [References](#references)  

## 1. Introduction

*Disclaimer: This document is a draft and needs to be commented on and completed to be published as EWC WP3 T3.2.3 deliverable. The terminology (part 1.5) have been generated by AI and are only a base for Business Registries to discuss with their legal departments which terminology is the most suitable in our context.*

### 1.1 Scope

This document is the International Bank Account Number (referred to as IBAN) Attestation Rulebook. It contains requirements specific to the IBAN attestation, including its issuance and verification processes. This rulebook addresses the background and attributes of the IBAN attestation, its trust infrastructure, and technical implementation details.

The IBAN attestation is solely destined to be used by legal persons (companies).

[Topic 10/23](https://github.com/eu-digital-identity-wallet/eudi-doc-architecture-and-reference-framework/blob/main/docs/annexes/annex-2/annex-2-high-level-requirements.md#a2310-topic-10---issuing-a-pid-or-attestation-to-the-eudi-wallet) in the ARF 1.4 specifies that attestation must be issued in the [SD-JWT VC] format, among others. This rulebook supports the [SD-JWT VC] requirements.

### 1.2 Background

The need for an IBAN attestation arises from the widespread use of the IBAN system for cross-border banking and payment transactions within the SEPA (Single Euro Payments Area) and beyond. Financial institutions and service providers require a secure and standardised method to verify bank account ownership to reduce fraud, enhance customer trust, and improve process efficiency.

The IBAN attestation will be used by companies in the EWC pilots, and some modifications to this rulebook might be made.

### 1.3 Goal of the IBAN attestation

The IBAN attestation aims to:

- Provide a secure, verifiable credential for proving a legal person's ownership of an IBAN.
- Ensure interoperability and machine-readability for automation in financial systems.
- Offer a single trusted standard to replace varied national verification processes.

### 1.4 Key words

This document uses the capitalized key words 'SHALL', 'SHOULD', and 'MAY' as specified in [RFC 2119], i.e., to indicate requirements, recommendations, and options specified in this document.

In addition, 'must' (non-capitalized) is used to indicate an external constraint, i.e., a requirement that is not mandated by this document, but, for instance, by an external document. The word 'can' indicates a capability, whereas other words, such as 'will', and 'is' or 'are' are intended as statements of fact.

### 1.5 Terminology

This document uses terminology specified in Regulation (EU) 2024/1183.

In addition to the attributes definition necessary to understand the data schema, it’s important to understand:

- **IBAN** (International Bank Account Number): A standardized international numbering system used to uniquely identify a bank account. The format includes a two-letter country code, check digits, and a basic bank account number (BBAN).
- **Holder**: A legal person who owns the bank account associated with the IBAN.
- **ASPSP** (Account Servicing Payment Service Provider): A financial institution that provides and maintains payment accounts accessible via online platforms.
- **PSD2** (Payment Services Directive 2): An EU directive that regulates payment services and providers throughout the European Economic Area, promoting competition and innovation.
- **BIC** (Bank Identifier Code): A unique code assigned to financial institutions used to identify the bank during international transactions. Also known as the SWIFT code.
- **Clearing Number**: A number used to identify a financial institution or a branch within a country’s clearing system.
- **SWIFT** (Society for Worldwide Interbank Financial Telecommunication): A global provider of secure messaging services and financial transactions among banks and financial institutions.
- **Open Banking**: A practice that allows third-party financial service providers to access consumer banking, transaction, and other financial data through APIs, typically implemented under PSD2.


## 2. IBAN Issuance process

Only financial institutions acting as Account Servicing Payment Service Providers (ASPSP) are authorized to issue IBAN attestations. These institutions are considered the authentic source of account ownership information. Issuance may occur directly or through a delegated Qualified Electronic Attestation Authority (QEAA).

The IBAN attestation is exclusively available to legal persons (companies). Applicants must demonstrate valid proof of identity, such as a Legal Person Identifier (LPID), within their organizational wallet.

Legal persons must submit a formal request to the ASPSP via secure and authenticated channels, such as an organizational wallet compatible with the EUDI Wallet Framework.

The ASPSP verifies the provided bank account details against its internal records, ensuring they correspond to the legal entity making the request. Key validation steps include:
- Verifying the IBAN format and association with the applicant.
- Confirming the applicant's LPID and its linkage to the IBAN.

Upon successful verification, the ASPSP issues the IBAN attestation as a verifiable credential. The attestation is signed or sealed using a qualified electronic signature (QES) or seal (QES Seal).

**Requirements**:
- An IBAN attestation can only be issued to organizational wallets compliant with the EUDI framework.
- Attestations are valid solely for legal entities with a valid LPID.

Verifying the IBAN format and association with the applicant.
Confirming the applicant's LPID and its linkage to the IBAN.

Then IBAN is a sensitive data that SHOULD be emitted only by a QTSP

The Regulation specifies who is able to issue the EUCC to companies: “Companies could apply for such an EU Company Certificate, with national business registers or through the system of interconnection of registers […] Such an EU Company Certificate should be issued and certified by the national business registers.”

To comply with the Regulation, only Business Registries are allowed to be the authentic source of the EUCC attestation, and they can decide to use a Pub-EAAs provider to issue it on their behalf.

In the EWC context, a generic attestation issuance process has been described by wallet providers in the pilots. Those controls and generic steps are described in [RFC-001](https://github.com/EWC-consortium/eudi-wallet-rfcs/blob/main/ewc-rfc001-issue-verifiable-credential.md).

While different business registries have national processes, there is an agreement that the EUCC attestation can only be requested by companies having a valid LPID in their wallet. Therefore, this attestation can only be issued to an EUDI valid organizational wallet.

## 3. EUCC Verification process

In the EWC context, a generic attestation verification process has been described by wallet providers in the pilots. Those controls and generic steps are described in [RFC-002](https://github.com/EWC-consortium/eudi-wallet-rfcs/blob/main/ewc-rfc002-present-verifiable-credentials.md).

EWC participating Business Registries don’t impose any data or attributes specific verification at this stage of the pilot; it is up to the Relying Party needs and requirements in the business or administrative process to decide.

## 4. EUCC attributes

EUCC attributes have been decided together by business registries in the EWC pilot in accordance with the EU Company Law proposal.

This table contains the name of the attribute, its description, and whether the attribute is required or not.

| Property Name | Description | Required |
| ------------- | ----------- | -------- |
| legal_person | Information regarding the legal entity, including name, ID, legal form, etc. | Yes |
| legal_person_name | Official current legal entity name as registered in the business register. | Yes |
| legal_person_id | Unique ID for the legal entity in the EUID structure. | Yes |
| legal_form_type | Legal form of the company. | Yes |
| registration_member_state | The member state where the company is registered (Alpha-2 country code according to ISO 3166-1). | Yes |
| registered_address | The official address of the company. | Yes |
| care_of | Used when the address is at the address of another person or legal entity. | No |
| full_address | Complete address of the company, written as a string, separated by semicolons. | Yes |
| thorough_fare | The name of a passage or way through from one location to another. | No |
| locator_designator | A number or sequence of characters that uniquely identifies the locator within the relevant scope. | No |
| post_code | The code created and maintained for postal purposes to identify a subdivision of addresses and postal delivery points. | No |
| post_name | A name created and maintained for postal purposes to identify a subdivision of addresses and postal delivery points. | No |
| post_office_box | A location designator for a postal delivery point at a post office, usually a number. | No |
| locator_name | Proper noun(s) applied to the real-world entity identified by the locator. | No |
| admin_unit_level_1 | The uppermost administrative unit for the address, almost always a country. | No |
| admin_unit_level_2 | The name of a secondary level/region of the address, typically a county, state, or other area that encompasses localities. | No |
| registration_date | Date of company registration. | Yes |
| share_capital | Amount of the subscribed capital with currency. | No |
| legal_entity_status | Status of the company as defined in national law and recorded in the national register. | Yes |
| legal_entity_activity | Main activity or activities of the company, expressed using the NACE (Statistical Classification of Economic Activities). | Yes |
| legal_entity_duration | Duration of the company, if limited. | No |
| contact_point | Correspondence address of the company, such as email or postal address (full or partial). | No |
| contact_email | Details of the company email address. | No |
| contact_telephone | Details of the company phone number. | No |
| contact_page | Details of the company website. | No |
| legal_representative | Information about the person(s) authorized to represent the company, either individually or jointly. | Yes |
| legal_representative.natural_person | Details about the natural person representing the company. | No |
| full_name | Full name of the natural person representing the company. | Yes |
| date_of_birth | Date of birth of the natural person representing the company. | Yes |
| nationality | Nationality of the natural person representing the company. | No |
| signatory_rule | Information on whether the representative can engage the company alone or jointly. | Yes |
| legal_representative.legal_person | Details about the legal person representing the company. | No |
| legal_person_name | Official current legal person name as registered in the business register. | Yes |
| legal_person_id | Unique ID for the legal entity in the EUID structure. | Yes |
| legal_form_type | Legal form of the legal person representing the company. | Yes |

The EUCC schema is available in the EWC schemas and rulebooks repository: [EUCC data schema](https://github.com/EWC-consortium/eudi-wallet-rulebooks-and-schemas/blob/main/data-schemas/ds001-eu-company-certificate.json).

**Note**: The EUCC attestation metadata are aligned with the LPID. The necessary information about those can be found in the [LPID Rulebook](https://github.com/EWC-consortium/eudi-wallet-rulebooks-and-schemas/blob/main/rulebooks/rb001-legal-person-identification-data.md).

### 4.1 Registered address-related attributes

This document defines the following attributes related to the registered address of the EUCC holder:

- full_address
- care_of
- thorough_fare
- locator_designator
- post_code
- post_name
- post_office_box
- locator_name
- admin_unit_level_1
- admin_unit_level_2

The detailed attributes allow the EUCC attestation to represent the granularity of the elements describing a registered address as defined in the [EU Core Business Vocabulary](https://semiceu.github.io/Core-Business-Vocabulary/releases/2.2.0/#Address).. 

### 4.2 Legal representative-related attributes

This document defines the following attributes related to the legal representation of a company. This list of attributes allows EUCC issuers to use either or both list in order to describe a legal representative who is a natural person or a legal person. 

**Attributes to define a natural person holding a legal representative right:**

- full_name
- date_of_birth
- nationality
- signatory_rule

**Attributes to define a legal person holding a legal representative right:**

- legal_person_name
- legal_person_id
- legal_form_type
- signatory_rule

It is the responsibility of Business Registries to include as many legal_representative objects as present in their registry.

### 4.3 Minimum number of optional attributes

There is no minimum number of optional attributes for the EUCC. Each Business Registry will have the responsibility to fill in the attributes when registered in their national registry.

## 5. Trust infrastructure details

In this chapter, trust requirements and general considerations regarding the EUCC attestation itself are described.

### 5.1 Trust requirements on the EUCC attestation from the perspective of company registration offices as authentic sources for the EUCC

In the ARF 1.4, the following information for Pub-EAAs and QEAAs Providers is given.

Pub-EAAs and QEAAs Providers are trusted entities responsible to:

- Verify the identity of the EUDI Wallet User in compliance with LoA high requirements.
- Issue attestations to the EUDI Wallet in a harmonized common format.
- Make available information for Relying Parties to verify the validity of the attestation.

The EUCC SHALL contain the qualified electronic signature or qualified electronic seal of the issuing body and adhere to the legal requirements defined in Annex VII of the Regulation (EU) 2024/1183.

The EUCC SHALL follow the SD-JWT format.

It SHALL not be possible to log into company registers solely with the EUCC, since procedures legally require an individual person to act.

EUCC Issuers SHALL follow the EUCC requirements and trust mechanisms defined by Authentic Sources on a national level.
Authentic Sources that are company registration offices need to accept each other's PUB-EAA attestations according to the regulation. Therefore, common legal trust mechanisms need to be established ifor the trust ecosystem to be trustworthy: 

- The EUCC unique identifier SHALL be unique and agreed upon on EU and EES level.
- There SHALL be one common schema for the EUCC which is accepted by all company registries offices.
- Only mandatory metadata and attributes SHALL be present in the EUCC attestations.
- The EUCC SHALL be in a machine-readable format defined in the ARF during its whole lifecycle.
- The EUCC SHALL be in a format that can scale to additional/new legal forms.
- The EUCC SHALL apply for all legal persons.
- The issuer of the EUCC SHALL be responsible for its revocation. 

### 5.2 Trust a signature or seal over a EUCC

To trust a signature or seal over an EUCC, the Relying Party needs a mechanism to validate that the public key it uses to verify that signature or seal is trusted. OpenID4VP provides such mechanisms. However, additional details need to be analyzed to fully specify these mechanisms for EUCCs within the EUDI Wallet ecosystem and the trust anchor for it. It is assumed that this will be part of a detailed specification from a standardization authority.

### 5.3 EUCC Provider Trusted List

For authenticating EUCCs, trust anchors will be used that are present in an EUCC issuer Provider Trusted List.

### 5.4 SD-JWT-compliant

EUCC is fully compliant with [OpenID4VP] and [SD-JWT VC].

## 6. References

- RFC-001: [Issue Verifiable Credential](https://github.com/EWC-consortium/eudi-wallet-rfcs/blob/main/ewc-rfc001-issue-verifiable-credential.md)
- RFC-002: [Present Verifiable Credentials](https://github.com/EWC-consortium/eudi-wallet-rfcs/blob/main/ewc-rfc002-present-verifiable-credentials.md)
- SD-JWT VC: [SD-JWT VC Specification](https://datatracker.ietf.org/doc/draft-ietf-oauth-sd-jwt-vc/)
- OpenID4VP: [OpenID4VP Specification](https://openid.net/specs/openid-4-verifiable-presentations-1_0.html)
