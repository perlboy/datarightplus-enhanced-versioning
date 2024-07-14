%%%
title = "DataRight+: Enhanced Endpoint Versioning"
area = "Internet"
workgroup = "datarightplus"
submissionType = "independent"

[seriesInfo]
name = "Internet-Draft"
value = "draft-authors-datarightplus-enhanced-versioning-latest"
stream = "independent"
status = "experimental"

date = 2024-07-14T00:00:00Z

[[author]]
initials="S."
surname="Low"
fullname="Stuart Low"
organization="Biza.io"
  [author.address]
  email = "stuart@biza.io"

[[author]]
initials="B."
surname="Kolera"
fullname="Ben Kolera"
organization="Biza.io"
  [author.address]
  email = "bkolera@biza.io"

%%%

.# Abstract

This specification outlines the technical components associated with the DataRight+ Enhanced Endpoint Versioning. This is intended to be an alternate endpoint versioning scheme to that outlined with [CDS] which facilitates a more developer friendly response processing mechanism (by way of `oneOf`) and a deterministic request payload versioning mechanism.

.# Notational Conventions

The keywords  "**REQUIRED**", "**SHALL**", "**SHALL NOT**", "**SHOULD**", "**SHOULD NOT**", "**RECOMMENDED**",  "**MAY**", and "**OPTIONAL**" in this document are to be interpreted as described in [@!RFC2119].

{mainmatter}

# Scope

The scope of this specification is specifically with regard to a standardised request and response versioning pattern for resource server endpoint paths as an alternate to that prescribed by [CDS].

# Terms & Definitions

This specification uses the terms "Provider" and "Initiator" as defined by [@!DATARIGHTPLUS-ROSETTA].

# Introduction

The [@!CDS] introduced an endpoint versioning scheme based on a negotiation pattern contained within the `x-v` and `x-min-v` headers. This effectively allows for dynamic response negotiation by the resource server between a range of available versions. The challenges with this approach though is that the payloads themselves are not explicitly versioned, something `oneOf` in OpenAPI 3 lends itself to, and the payloads contained within `POST` requests are essentially unversioned.

This specification seeks to solve for these two challenges by introducing an explicit `version` in both request and response payloads. Further it seeks to retire the `x-min-v` header on the basis that such a negotiation pattern is unable to be applied to potentially multiple versions of a request payload. Much of this specification assumes the adoption of the related [@!DATARIGHTPLUS-DISCOVERY] specification.

# Provider

The following provisions apply to Provider Resource Server endpoints supporting this specification:
1. Response payloads **SHALL** be determined based on the `x-v` header specified in the request;
2. Response payloads **SHALL** include the `version` attribute in the response payload;
3. Response headers **MAY** include an `x-v` value matching the `version` attribute within the payload
4. Request payloads received using the HTTP POST method **SHALL** be processed by matching the `version` attribute to the appropriate OpenAPI specification
5. If a request is received containing a `x-v` value which is unsupported the server shall respond with a `urn:au-cds:error:cds-all:Header/UnsupportedVersion` error as outlined in [@!CDS]

# Initiator

The following provisions apply to participants operating individual Initiators accessing endpoints supporting this specification:
1. Requests shall include an `x-v` header representing the requested response payload
2. Request payloads **SHALL** be constructed to contain the `version` attribute contained within the relevant OpenAPI specification
3. Response payloads **SHALL** be processed in accordance with the `version` attribute and the `oneOf` specification within the relevant OpenAPI specification
4. Responses containing an `x-v` header **SHALL** be validated to ensure the `x-v` and `version` values are the same

# Implementation Considerations

For endpoints which are currently compliant to the [@!CDS] versioning schema the endpoint can be transitioned by incrementing the endpoint version while introducing the `version` attribute into the response and, if necessary, the request payloads. In order to retire the use of `x-min-v` it is necessary to introduce a discovery document, as outlined in [@!DATARIGHTPLUS-DISCOVERY].

{backmatter}

<reference anchor="CDS" target="https://consumerdatastandardsaustralia.github.io/standards"><front><title>Consumer Data Standards (CDS)</title><author><organization>Data Standards Body (Treasury)</organization></author></front> </reference>

<reference anchor="DATARIGHTPLUS-ROSETTA" target="https://datarightplus.github.io/datarightplus-rosetta/draft-authors-datarightplus-rosetta.html"> <front><title>DataRight+ Rosetta Stone</title><author initials="S." surname="Low" fullname="Stuart Low"><organization>Biza.io</organization></author></front> </reference>

<reference anchor="DATARIGHTPLUS-INFOSEC-BASELINE" target="https://datarightplus.github.io/datarightplus-infosec-baseline/draft-authors-datarightplus-infosec-baseline.html"> <front><title>DataRight+ Security Profile: Baseline</title><author initials="S." surname="Low" fullname="Stuart Low"><organization>Biza.io</organization></author></front> </reference>

<reference anchor="DATARIGHTPLUS-DISCOVERY" target="https://datarightplus.github.io/datarightplus-discovery/draft-authors-datarightplus-discovery.html"> <front><title>DataRight+: Discovery</title><author initials="S." surname="Low" fullname="Stuart Low"><organization>Biza.io</organization></author></front> </reference>

<reference anchor="OIDC-Discovery" target="https://openid.net/specs/openid-connect-discovery-1_0.html"> <front> <title>OpenID Connect Discovery 1.0 incorporating errata set 1</title> <author initials="N." surname="Sakimura" fullname="Nat Sakimura"> <organization>NRI</organization> </author> <author initials="J." surname="Bradley" fullname="John Bradley"> <organization>Ping Identity</organization> </author> <author initials="M." surname="Jones" fullname="Mike Jones"> <organization>Microsoft</organization> </author> <author initials="E." surname="Jay"> <organization>Illumila</organization> </author><date day="8" month="Nov" year="2014"/> </front> </reference>

<reference anchor="JWT" target="https://datatracker.ietf.org/doc/html/rfc7519"> <front> <title>JSON Web Token (JWT)</title> <author fullname="M. Jones"> <organization>Microsoft</organization> </author> <author initials="J." surname="Bradley" fullname="John Bradley"> <organization>Ping Identity</organization> </author><author fullname="N. Sakimura"> <organization>Nomura Research Institute</organization> </author> <date month="May" year="2015"/></front> </reference>


