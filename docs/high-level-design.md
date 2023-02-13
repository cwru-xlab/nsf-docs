# High-level design (WIP)

## Problem context
Many research projects depend directly on the quality and availability of data collected from participants. Unfortunately, great complexity comes with this process of data collection due to the privacy concerns with assimilating data from the various different isolated sources that researchers may want to tap into. For example, researchers may want participants to share data from a wide range of sources such as health records, smartphones, or wearable technology. Participants however might lack the ability to control precisely which data from these sources gets shared at what times, and to which parties that data gets consumed by. Traditional centralized solutions may be not preferable, as they can be vulnerable to a single-point-of-failure, and may ultimately aggregate either too little or too much data to match both the needs of the researchers and the privacy concerns of the participants. This design aims solve this technical challenge by providing participants, or "data donors", with a simple privacy-preserving way to control and monitor their data for sharing with a potential variety of different researcher targets.

## Terminology
- **Service Provider:** An Agent that provides a service and communicates with *universal data agents* to access personal data.
- **Universal Data Agent:** An Agent that an individual client user has control of, which mediates data sharing between *personal data stores* and *service providers*, providing the user with self-sovereign access control over their data.
- **Personal Data Store:** A source/store of personal data that is accessible to the *universal data agent* via *data store drivers*. Note that the *universal data agent* does not assume that it has exclusive access to *personal data stores*, as they may be read and modified outside of this system.
- **Verifiable Credential (VC):** (See [W3C Verifiable Credentials Definition](https://www.w3.org/TR/vc-data-model/)).
- **Decentralized Identifier (DID):** (See [W3C DIDs Definition](https://www.w3.org/TR/did-core/)).

## Requirements
What this solution should achieve is a privacy-preserving way for people (users) to share their personal data with services (service providers) within the decentralized ecosystem. Users, or "data donors", must be guaranteed control over the data in their "personal clouds". Specifically, users should be able to control and monitor what specific data is shared from their personal clouds. Furthermore, they should have control over which specific target services will have access to their data, and must be able to first review and consent to the respective Terms of Service before data sharing occurs. 

## Scope

## Proposed Solution
This design addresses the simple scenario where agents want to be provided with data from other the personal stores of other agents. For example, where a service agent wants to be provided with personal data from a user agent. To rephrase this example in the terms used by this design: the service agent is the "service provider", the user agent is the "universal data agent", and the user's personal data is kept in "personal data stores".

This design provides a modular architecture for how the service provider can access personal data via universal data agents. This design allows the service provider to be decoupled from users' personal data by forcing data transfer to be mediated by the universal data agent. Thereby, the party in control of the universal data agent has definitive control over what data the service provider can access, and the universal data agent can pull from as few or as many personal data stores as desired to accommodate service providers, even if those personal data stores wouldn't otherwise have the necessary access control rules to do so on their own.

### Architecture

```mermaid
flowchart
  subgraph Universal data agent
  direction BT
  dac[Data access control]
  dm --> dac
  dm[Data mapping]
  dsd --> dm
  dsd[Data store drivers]
  sp[(Solid pod)] --> dsd
  dwn[(Decentralized\nweb node)] --> dsd
  agent[(DIDComm\nagent)] --> dsd
  end
```

## Use cases

### NSF Healthcare
This architecture supports the use case where a researcher party maintains service providers to collect health data from many people, where each person is considered an "end user", and is represented by a respective universal data agent. Users have their health data stored inside of what is called a "Personal Data Account" (PDA), which is a remote store for JSON data provided by the company "Dataswift". Users' PDAs are assumed to be populated with health data outside of this architecture. The end user can control their universal data agents via a mobile app, in which they have the option to transfer data from their personal PDA to service providers.

In a typical example case of data transfer, the user is assumed to have a QR code from a service provider that they want to send to. How the user obtains this QR code is outside of this architecture. The user is also assumed to have set data access rules for what specific data they want to send to the service provider. Once the user scans the QR code in the mobile app, the app makes a request to the universal data agent, saying that they want to transfer specific data to the service provider specified by the QR code's. Once the data agent receives this request, it parses the QR code as contact information for where it can communicate with the service provider. With this information, the data agent sends an Out-of-Band message to the service provider... ***TODO*** The data agent then pulls the personal data as previously specified, and sends it to the service provider via ***TODO protocol***. At this point, the transfer is completed, and the end user is notified of the successful transfer in their app.

Docs for this use case implementation can be seen here: [TODO]().


## Delivery plan
