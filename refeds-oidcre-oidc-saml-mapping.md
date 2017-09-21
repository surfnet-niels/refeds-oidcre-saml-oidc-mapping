---
title: This is an Internet-draft 
abbrev: I-D
docname: draft-name-wg-topic-00
date: 2017-09-15
category: info
ipr: trust200902

author:
 -
    ins: N. van Dijk
    name: Niels van Dijk
    organization: SURFnet BV
    email: niels.vandijk@surfnet.nl

normative:
  RFC2119:

informative:

--- abstract



--- middle

# Introduction

This document describes a best current practice for implementing mappings between SAML 2.0 [SAML 2] attributes and OpenID Connect [OIDC] claims, in the context of use cases in Higher Education. It describes how to map commonly used attributes, for example from the eduPerson schema [eduPerson], into claims for use with the OIDC protocol and vice versa. 

# Terminology

In this document, the key words "MUST", "MUST NOT", "REQUIRED",
"SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY",
and "OPTIONAL" are to be interpreted as described in BCP 14, RFC 2119
{{RFC2119}}.


# Basic profile
The basic profile implements a attribute to claim mapping in such a way that would allow an unmodified OIDC client to receive claims based on SAML attributes. This would allow existing SAML based Identity federation to add a proxy to onboard OIDC RPs, which seems the most common scenario at the time of writing.

For servcies supporting this profile, the mapping MUST be implemented as is designated below

| OIDC Scope |  OIDC claim  |  eduPerson attribute |
|---|---|---|
| profile | Public sub | eduPersonPrincipalName (non-reassigned), eduPersonTargetedID, eduPersonUniqueID  |
|   | name  | displayName  |
|   | given_name  |  givenName |
|   | family_name | sn (surname)  |
| email  | mail  | mail  |

This profile is equivelent with the Reseach and Scholarship attribute bundle  as described in the Research and Scholarship Entity Category [R&S].

### mail_verified
OIDC supports a claim mail_verified which is part of the Standard claims [Standard claims], but has no equivelent in SAML attributes.

It is RECOMMENDED to issue the mail_verified claim as part of the basic profile. If mail_verified is send, the following condition MUST be met: 
(1) email was provided by the Institutional identity provided as part of the SAML assertion, 
(2)  the domain part of the email address is a (sub) domain of the institution,
(3) the SAMLmetadata for the entity contains a shibmd:Scope element [shibmd Scope] for the given (sub)domain.

In this case it may be assumed the email service being used is under direct administrative control of the Institution and due dilligence was taken by the institution before the email account was issued to the user.

There is no representation of mail_verified in SAML attributes.

# Advanced profile
The advance profile provides a more elaborate profile for mapping SAML attributes from the eduPerson, eduMember [eduMember] and SCHAC [SCHAC] schemas to OIDC Additional Claims [additional claims] and vise versa. 

The advance profile retains the mappings as presented in the basic profile, but adds a direct, literal mapping from commonly used attributes from eduPerson and SCHAC into claims. These claims are represented in the edu scope (which currently is none existing!). In this way any attribute from the SAML world can be represented in OIDC claims without ambiguity.

The OpenID Conect specificication reccommends, all aditional claims are been  

 | OIDC Scope | OIDC name             | eduPerson or SCHAC name | 
 |---|---|---|
|      profile      | sub                   | eduPersonPrincipalName (if non-reassigned) or eduPersonTargetedID | 
|            | name                  | displayName             |
|            | given_name            | givenName               |
|            | family_name           | sn (surname)            |
|            | preferred_username    | displayName             |
|            | locale                | preferredLanguage       |
| email      | email                 | mail                    |
|            | email_verified        |See ¨Using mail_verified"|
|            |   | 
| Additional Claims            |   | 
|            | eduperson_affiliation | eduPersonAffiliation    |
|            | eduperson_entitlement | eduPersonEntitlement    |
|            | eduperson_principal_name | eduPersonPrincipalName |
|            | eduperson_scoped_affiliation | eduPersonScopedAffiliation |
|            | eduperson_targeted_id | eduPersonTargetedID |
|            | eduperson_assurance   | eduPersonAssurance      |
|            | eduperson_unique_id   | eduPersonUniqueId |
|            | eduperson_orcid       | eduPersonOrcid |
|            | edumember_is_member_of| IsMemberOf |
|            | schac_home_organisation | SchacHomeOrganisation |
|            | schac_personal_unique_code |SchacPersonalUniqueCode | 

It is the intent of the the OIDCre workinggroup to register the additional claims with the Registered Claim Names, per the JWT specification [JWT specification]. 

## References
[SAML 2] 
[OIDC] http://openid.net/specs/openid-connect-core-1_0.html
[eduPerson] Internet2 MACE Directory Working Group, “eduPerson Object Class Specification (201602)”, February 2016.
[R&S] https://refeds.org/category/research-and-scholarship
[Standard claims] https://openid.net/specs/openid-connect-core-1_0.html#StandardClaims
[shibmd Scope] https://wiki.shibboleth.net/confluence/display/SC/ShibMetaExt+V1.0
[eduMember] https://spaces.internet2.edu/display/macedir/eduMember+2010xx
[SCHAC] https://wiki.refeds.org/download/attachments/1606048/SCHAC%2B1.5.0.pdf
[additional claims]  https://openid.net/specs/openid-connect-core-1_0.html#AdditionalClaims
[JWT specification] https://tools.ietf.org/html/draft-ietf-oauth-jwt-bearer-12

## Acknowledgement
This work is the result of the REFEDs (https://refeds.org/) OIDCre working group, with support from the AARC project (https://aarc-project.eu/) and the GEANT project (https://www.geant.org/Projects/GEANT_Project_GN4) and numerous individual contributions
