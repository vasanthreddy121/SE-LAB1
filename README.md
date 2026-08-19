# SE-LAB1
# Problem Statement #64: Disaster Relief Supply & Volunteer Coordinator
**Track:** Sustainability & Green Tech  
**Course:** Software Engineering Lab 1 – Requirements Engineering & UML Use-Case Modelling  

---

## Deliverable 1: Complete Requirements Table

### 1.1 Functional Requirements (FR)

| Requirement ID | Type | Description | Priority | Acceptance Criteria | Rationale |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **FR-001** | Supply Allocation | The system shall match incoming field shelter supply requisitions against central warehouse inventory and assign delivery priority based on triage criticality. | High | **Pass:** Critical supplies (water, medical kits) are prioritized first in the manifest.<br>**Fail:** Low-priority supplies dispatched ahead of urgent needs. | Prevents life-threatening shortages by automating priority-based resource distribution. |
| **FR-002** | Volunteer Dispatch | The system shall register volunteer skill sets and match volunteers to shelter tasks based on required domain expertise. | High | **Pass:** Certified medical volunteers are matched to triage zones.<br>**Fail:** Unqualified personnel are assigned to specialized tasks. | Ensures effective on-ground emergency response by utilizing proper volunteer capabilities. |
| **FR-003** | Fleet Tracking | The system shall track relief vehicle coordinates and distribution status in real-time via GPS. | Medium | **Pass:** Vehicle location updates on the coordinator dashboard within 30 seconds.<br>**Fail:** Transit location data is stale by >5 minutes. | Provides logistics transparency and ensures accountability of relief supplies in transit. |
| **FR-004** | Shelter Mapping | The system shall provide an interactive GIS map displaying shelter capacities, current resource levels, and shortage alerts. | High | **Pass:** Shortage indicators dynamically trigger when inventory crosses lower threshold.<br>**Fail:** Critical shelter stockout alerts fail to display. | Provides coordinators with actionable situational awareness across geographic zones. |
| **FR-005** | Offline Sync | The mobile client shall allow relief volunteers to record distribution logs offline and automatically sync when connectivity is restored. | Medium | **Pass:** Offline records upload correctly to the central server without data corruption upon reconnect.<br>**Fail:** Cached offline logs are lost or overwritten. | Maintains continuous record-keeping during communication and power grid blackouts. |

| Requirement ID | Type | Description | Priority | Acceptance Criteria | Rationale |
| --- | --- | --- | --- | --- | --- |
| **NFR-001** | Performance & Offline Resilience | The disaster mapping dashboard must operate under low network bandwidth conditions and support offline GIS tile caching.

 | High | **Pass:** Map renders cached layers within 2 seconds on 2G/low-bandwidth connections.

<br>

<br>**Fail:** Dashboard stalls or fails to load without high-speed internet. | Critical disaster response operations must continue without disruption in areas with damaged infrastructure.

 |
| **NFR-002** | Security & Data Protection | The system must enforce Role-Based Access Control (RBAC) and encrypt all shelter and volunteer data at rest and in transit using TLS 1.3 and AES-256. | High | **Pass:** Unauthorized users are blocked from administrative controls and all intercepted traffic is encrypted.<br>

<br>**Fail:** Sensitive volunteer PII or supply logistics data is accessible in plaintext. | Protects sensitive identity data and prevents malicious disruption of relief supply routes. |
| **NFR-003** | Availability & Reliability | The central management platform must maintain an uptime of 99.9% during active disaster response operations, with automatic failover to redundant cloud servers. | High | **Pass:** Unscheduled downtime does not exceed 43 minutes per month, and failover completes in under 30 seconds.<br>

<br>**Fail:** Platform crashes during an active emergency and requires manual restart. | System unavailability during an ongoing crisis directly delays critical life-saving relief dispatches. |
| **NFR-004** | Scalability & Concurrency | The system shall handle up to 10,000 concurrent active users (volunteers and coordinators) and process up to 1,000 requisitions per minute without service degradation. | Medium | **Pass:** Response time remains under 1.5 seconds under peak simulated load.<br>

<br>**Fail:** Requests time out or throw 5xx errors during sudden mass-casualty event surges. | Rapid influxes of volunteers and shelter requests during large-scale disasters must not crash the backend. |
| **NFR-005** | Usability & Accessibility | The user interface for field volunteers must adhere to WCAG 2.1 Level AA standards, featuring high-contrast modes and support for single-handed mobile operation. | Medium | **Pass:** 90% of novice volunteers complete an intake/distribution report in under 60 seconds during field trials.<br>

<br>**Fail:** Form fields are unreadable in direct sunlight or require complex multi-step navigation. | Volunteers frequently work in harsh field conditions (e.g., bright sunlight, extreme weather) and need rapid, frictionless data entry. |
## Deliverable 2: UML Use-Case Diagram
Supply & Volunteer Coordinator
### 2.1 Visual Diagram
<img width="747" height="761" alt="umldiagram" src="https://github.com/user-attachments/assets/b7cecc12-387e-4b1f-a2ef-3d45668729a9" />




## Deliverable 3: Use-Case Flow Specification

Use Case: Match and Allocate Shelter Supplies (UC-001)

* **Primary Actor:** Disaster Manager
* **Supporting Actors:** Central Warehouse Inventory System, Field Shelter Coordinator

#### 1. Brief Description

The Disaster Manager reviews incoming shelter supply requests (e.g., water, medical kits, blankets), matches them against available warehouse stock, prioritizes dispatch orders based on triage urgency, and generates a delivery manifest for logistics routing.

2. Preconditions

* The Disaster Manager is authenticated into the system.
* At least one shelter requisition exists in the system queue.
* Warehouse inventory records are accessible.

3. Postconditions

* Resource allocations are committed to the central database.
* Warehouse stock is decremented accordingly.
* A prioritized dispatch manifest is generated and assigned to a delivery vehicle.

4. Main Success Scenario (MSS)

1. The Disaster Manager opens the **Supply Allocation** module.
2. The system retrieves and displays all pending shelter requisitions sorted by urgency rating (`Critical`, `High`, `Medium`, `Low`).
3. The Disaster Manager selects a critical shelter requisition for processing.
4. The system queries warehouse inventory and matches requested items against real-time stock levels.
5. The system automatically assigns highest dispatch priority to life-critical items (water, medical kits).
6. The Disaster Manager confirms the allocation and selects an available relief delivery vehicle.
7. The system updates the warehouse stock count, generates the dispatch manifest, and sets the requisition status to `Dispatched`.
8. The system displays a confirmation banner and routes the manifest to the vehicle tracking dashboard.

5. Alternate Flow

* **AF-1: Insufficient Warehouse Stock (Partial Allocation)**
* **Trigger:** Step 4 indicates available warehouse stock is less than the requested quantity.
* **Flow:**
1. The system flags the item as `Understocked` and calculates the maximum allocatable quantity.
2. The system prompts the Disaster Manager to choose: *(a) Partial Dispatch* or *(b) Re-route from Secondary Warehouse*.
3. The Disaster Manager selects *Partial Dispatch*.
4. The system allocates all available units to the current manifest and creates a split backorder requisition for the remaining deficit.
5. The system resumes from **Step 6** of the Main Success Scenario.

