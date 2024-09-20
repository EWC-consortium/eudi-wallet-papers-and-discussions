# Wallet eAddress

## Need for wallet eAddress

To initiate a wallet interaction, the current EUDI Wallet designs assume the wallet is equipped with a camera for scanning a QR code presented by an issuer or a relying party. Alternatively, the wallet user can invoke their wallet by clicking a deep link that the issuer or relying party presents. This method enables the user to initiate the credential exchange by “pulling” the issuer or relying party to interact with the wallet without exposing a public endpoint.

Server-based wallets in general have no camera or support for deep links, so an alternative approach is needed to enable secure initiation between a server-based wallet and an issuer or relying party. The solution would require that the issuer or relying party is provided with information that enables them to initiate the wallet interaction by “pushing” a request to a server-based wallet.

This document proposes that server-based wallets should have a resolvable address (eAddress), which can be shared by authorized natural persons, and which can be used by the issuer or relying party to resolve an endpoint to which credential proposals and presentation requests can be sent.

Server-based wallets are seen useful, in particular for organization (legal person) wallets that need to be used by multiple persons (or IT systems) in the organization.
![eAddress overview](images/eaddress-overview.png)

Without a harmonized eAddress, server-based wallet endpoints are challenging to discover. Additionally, eAddress provides organizations with tools for better management and monitoring of wallet endpoint usage. This is because a large organization may have multiple users (up to hundreds) utilizing the wallet capabilities simultaneously, depending on the type of business. The use of eAddresses harmonize how server-based wallet endpoints are exposed; to which users or use cases they belong to; and ease endpoint versioning and access control.

## Benefits of eAddress

Use of eAddress provides many benefits for use with server-based wallets. Especially it aids organizations to better integrate wallets with their Identity and Access Management architecture.

Some identified benefits of eAddresses with server-based wallets are:
- Flexible presentation and discovery of server-based wallet endpoints.
- Additional security measures that can be fine-grained according to organization’s architecture and use case needs.
- Helps integrate wallet interactions with specific business use cases and systems.
- Easier management of wallet endpoint use, including endpoint access control, API version upgrades, rotation of keys and endpoint access revocation.

## eAddress use cases

### LPID issuance

In the LPID issuance process, after authenticating the legal person representative and validating their authorization to represent the legal person, the PID Provider asks the natural person for a wallet id or endpoint for the wallet of the legal person and the natural person submits them. The PID Provider then issues the LPID to the wallet instance.
![LPID Issuance use case using eAddress](images/lpid-issuance.png)
In this use case, the natural person representing the legal person can provide the eAddress that points to the LPID issuance endpoint of the Legal Person Wallet.

### Presentation of attestations to relying party

In a typical interaction with a relying party, the relying party needs to discover the wallet which can present the necessary attestations to the relying party. This can be done by providing the wallet’s eAddress to the relying party, which uses the address to send a subsequent presentation request to the server-based wallet.

In EWC, this applies at least to Task 3.3. pilots on:
- Opening a business account in a bank (KYC)
- Public procurement
- Create branch

### vReceipt delivery to a wallet

In EWC Task 3.3 pilot on vReceipt, a traveler has bought a travel ticket and wants the merchant to send a verifiable eReceipt (vReceipt) directly to their employer for accounting. The traveler presents the eAddress of their employer’s wallet to let the merchant start the issuance flow of the vReceipt to it.

## Solution outline

### eAddress solution overview

The eAddress is implemented as a Decentralized Identifier, based on W3C Decentralized Identifiers (DIDs) v1.0 recommendation. Conforming to the W3C DID specification and the architecture.
![W3C DID Architecture](images/did-architecture.png)
Use of the eAddress requires that the organization wallet has access to a Verifiable Data Registry (VDR), used to resolve the eAddress to a wallet endpoint. VDR’s are DID-method specific, and this specification defines which DID-methods and thus VDR’s are supported.

The DID subject of the eAddress can be the entity that controls the server-based wallet, or any other system subject it. Examples of a system are an automated business process, or an expense management system. The DID controller is the wallet administrator, or an authorized user with privileges to control the DID Document.

In addition to the W3C DID recommendation, eAddress requires that it MUST have at least one service endpoint defined in the DID Document.

### eAddress presentation

The eAddress can be presented using any mechanism either in-band using EUDI Wallets, or out-of-band using other methods.

For example, the eAddress can be presented in the following ways:
- as an attestation from the natural person wallet
- using proximity presentation (QR-code / NFC) read by the RP/issuer (e.g. cash registry)
- By writing or copying the address to a web form
- presenting it using a smart card or a virtual card
- providing it via API
- presenting a QR-code via physical medium (e.g. sticker)

### DID methods

Once the eAddress has been provided to the RP / issuer, it must be resolved to a DID Document, which contains the wallet endpoint address and any access information.

According to the DID Core specification, the DID methods define how the DID is resolved to the DID Document and also how the DID and accompanying DID Document is created, updated and deactivated.

This RFC defines the supported DID methods.

### Endpoint types

The eAddress can technically refer to any type of endpoint. However, for security, control, and enterprise system integration purposes, it is useful to define the types of endpoints the eAddress can point to. The endpoint type can be used to define additional control checks by the wallet prior to proceeding with the interaction.

For example, the eAddress can point to one of the following types of endpoints:
- General wallet interaction endpoint. Allows generally supported EUDI wallet interactions, such as credential presentation request or credential proposal.
- Use case specific endpoint. Endpoint accepts only specific wallet interactions or attestation types. E.g. endpoint to receive only eReceipt EAA’s, or LPID issuance. Others are denied.
- System or user verifiable endpoint. Requests made to the endpoint require system or user verification (note: consider including this as a parameter in the eAddress)

### eAddress distribution profiles

The eAddress may be distributed in alternative profiles, depending on how it needs to be used, and what level of authentications or authorizations are required by organization or the use case.

Three types of distribution profiles are identified:
- Private. The eAddress is bound to the receiving entity. eAddress DID Document includes RP / Issuer public key, used by the API Access Control for authentication before allowing additional requests
- Protected. Additional authorization token is provided to the presenter (e.g. natural person). The token can for example be a cryptographic token (e.g. OAuth access token) or human-readable (e.g. PIN-code). API Access Control authorizes the token of the RP / Issuer before allowing additional requests.
- Public. API Access Control does not verify authentication or authorization the RP / Issuer. Wallet performs required authorization and identification checks using attestation request, or other means.

![eAddress endpoint types and distribution profiles](images/eaddress-endpoints-distribution-types.png)

## Conformance criteria for wallets

A wallet conforms to the eAddress specification if it:
- supports creation of eAddresses with at least one of the supported did methods
- supports describing at least one wallet endpoint using an eAddress
- supports distributing eAddresses using at least one distribution profile
- supports initiation of a wallet interaction using at least one of the endpoint types

## Security considerations

Using public distribution type and thus a public endpoint should be limited to mitigate any potential spam or DDoS attempts.

Wallets should consider providing integration methods to enterprise IAM systems to enable authorized natural persons to verify incoming requests. This is useful for example when a natural person initiates an incoming connection and needs to authorize it.

## Appendix A: Draft RFC text

RFC005, Issue Legal Person Identification Data (LPID) - v0.4
Let me know if you need any further assistance!
