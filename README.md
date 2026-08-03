# Automated Sorting Systems (automated-sorting-systems)

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

Automated Sorting Systems covers the technology landscape of automated conveyance, sortation, and parcel routing systems used in logistics, warehousing, e-commerce fulfillment, and postal distribution. Key vendors include Dematic, Vanderlande, BEUMER Group, Swisslog, Honeywell Intelligrated, and Solystic. These systems integrate with warehouse management systems (WMS), warehouse control systems (WCS), and ERP platforms via APIs and EDI to orchestrate high-speed package sorting.

**URL:** [https://raw.githubusercontent.com/api-evangelist/automated-sorting-systems/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/automated-sorting-systems/refs/heads/main/apis.yml)

**Run:** [Capabilities Using Naftiko](https://github.com/naftiko/fleet?utm_source=api-evangelist&utm_medium=readme&utm_campaign=company-api-evangelist&utm_content=repo)

## Tags

 - Automation, Conveyor Systems, Distribution, E-Commerce Fulfillment, Logistics, Package Tracking, Parcel Sorting, Sorting, Warehouse, Warehouse Automation

## Timestamps

- **Created:** 2024-01-15
- **Modified:** 2026-04-19

## APIs

### Dematic Warehouse Management API

Dematic provides warehouse management and sortation control software with REST APIs for integrating automated sorting systems with ERP, TMS, and order management systems. Their iQ software platform exposes real-time order, inventory, and conveyor status data.

**Human URL:** [https://www.dematic.com/en-us/products/technologies/software/](https://www.dematic.com/en-us/products/technologies/software/)

#### Tags

 - Warehouse Management, Sortation Control, Real-Time Tracking, ERP Integration

#### Properties

- [Website](https://www.dematic.com/en-us/products/technologies/software/)
- [Documentation](https://www.dematic.com/en-us/products/technologies/software/warehouse-management-systems/)

### Honeywell Intelligrated Momentum WMS API

Honeywell Intelligrated offers the Momentum warehouse management system with APIs for real-time conveyor and sortation control, order fulfillment orchestration, labor management, and integration with ERP systems.

**Human URL:** [https://sps.honeywell.com/us/en/products/productivity/warehouse-automation](https://sps.honeywell.com/us/en/products/productivity/warehouse-automation)

#### Tags

 - Warehouse Control, Sortation, WMS, WCS

#### Properties

- [Website](https://sps.honeywell.com/us/en/products/productivity/warehouse-automation)

### Vanderlande FLEET WMS API

Vanderlande's FLEET warehouse management software provides APIs and interfaces for controlling automated sorting systems in baggage handling, parcel logistics, and warehouse automation.

**Human URL:** [https://www.vanderlande.com/software-and-services/fleet/](https://www.vanderlande.com/software-and-services/fleet/)

#### Tags

 - Baggage Handling, Parcel Logistics, WMS, Sortation

#### Properties

- [Website](https://www.vanderlande.com/software-and-services/fleet/)

### BEUMER Group Sortation API

BEUMER Group provides sortation systems for airports, parcel, and distribution centers with software integration capabilities via standard industrial protocols and higher-level WMS/WCS REST APIs.

**Human URL:** [https://www.beumer.com/en/industries/logistics/](https://www.beumer.com/en/industries/logistics/)

#### Tags

 - Airport Logistics, Parcel Sorting, Industrial Integration, OPC-UA

#### Properties

- [Website](https://www.beumer.com/en/industries/logistics/)

## Features

| Name | Description |
|------|-------------|
| Real-Time Package Tracking | APIs that expose real-time location and status of packages moving through sorter lanes, divert points, and conveyor segments. |
| Sortation Configuration | Programmatic configuration of sort destinations, divert rules, and routing logic for automated sorters via WCS APIs. |
| Throughput Monitoring | Real-time and historical throughput metrics from sortation lines including items per hour, jam rates, and divert accuracy. |
| Order Management Integration | Integration with order management systems to trigger sortation tasks based on pick-and-pass workflows and order release events. |
| Exception Handling | Automated exception reporting for unknown barcodes, mis-sorts, and conveyor faults via event-driven API callbacks. |

## Use Cases

| Name | Description |
|------|-------------|
| E-Commerce Fulfillment | High-speed sortation of outbound orders in e-commerce fulfillment centers with API-driven carrier assignment and label printing. |
| Parcel Distribution | Cross-dock parcel sortation at postal and carrier distribution hubs with real-time barcode scanning and routing. |
| Airport Baggage Handling | Automated baggage sortation in airports with flight-based routing rules and late-bag divert capabilities via WCS APIs. |
| Intralogistics Automation | Internal warehouse sortation for order picking, replenishment, and returns processing integrated with WMS systems. |

## Integrations

| Name | Description |
|------|-------------|
| Warehouse Management Systems | Standard integrations between sortation WCS and leading WMS platforms including SAP EWM, Manhattan Associates, Blue Yonder, and Oracle WMS. |
| ERP Systems | Order and inventory data integration with SAP, Oracle, and Microsoft Dynamics ERP systems to drive sortation rules. |
| Parcel Carrier APIs | Integration with UPS, FedEx, DHL, and USPS carrier APIs for label generation and manifest creation at sortation endpoints. |
| Barcode and RFID Scanning | Integration with Zebra, Honeywell, and SICK scanning hardware for real-time package identification feeding sortation routing decisions. |

## Maintainers

**FN:** Kin Lane

**Email:** kin@apievangelist.com
