# Airport Simulation

## Existing System

### Passenger Flow

Arrival → Passenger Classification → Check-in → Security Screening → Boarding Gate → Exit

### Passenger Classification

- Economy: 70% (Standard check-in and regular security queue).
- Priority: 30% (Priority check-in and dedicated security queue).

### Given Parameters

| **Component** | **Configuration** |
|---|---|
| Passenger Arrival | Exponential (mean 12 seconds) |
| Economy Check-in | 1 desk, Triangular (1.5, 2.5, 4.5) minutes |
| Priority Check-in | 1 desk, Triangular (1.0, 1.5, 2.5) minutes |
| Economy Security | 2 lanes, Triangular (0.8, 1.2, 2.0) minutes |
| Priority Security | 2 lanes, Triangular (0.8, 1.2, 2.0) minutes |
| Boarding Gates | 2 gates, Exponential (mean 0.4 minutes) |

### Additional Parameters

Passenger walking time is modeled using `EntityConveyor`.

Assumed walking speed: **1.2 m/s**

| **Route** | **Distance** | **Travel Time** |
|---|---:|---:|
| Lobby → Check-in | 30 m | 25 seconds |
| Check-in → Security | 60 m | 50 seconds |
| Security → Boarding Gate | 150 m | 125 seconds |

### Implemented JaamSim Objects

- **Passenger Generation:** `Passenger`, `PassengerArrivalDist`, `PassengerGenerator`
- **Passenger Classification:** `PassengerClassDist`, `PassengerClassRouter`
- **Economy Process:** `EconomyCheckInQueue`, `EconomyCheckIn`, `EconomySecurityQueue`, `EconomySecurity`
- **Priority Process:** `PriorityCheckInQueue`, `PriorityCheckIn`, `PrioritySecurityQueue`, `PrioritySecurity`
- **Boarding:** `BoardingGateQueue`, `BoardingGates`, `PassengerExit`
- **Conveyors:** `EconomyToCheckIn`, `PriorityToCheckIn`, `EconomyToSecurity`, `PriorityToSecurity`, `EconomyToBoarding`, `PriorityToBoarding`

---

## Proposed System 1

### System Concept

Increase the number of service units while maintaining separate Economy and Priority processes. The service-time distributions and passenger classification remain unchanged from the Existing System.

### Passenger Flow

Arrival → Passenger Classification → Dedicated Check-in → Dedicated Security Screening → Shared Boarding Gates → Exit

### Proposed Capacity

| **Component** | **Existing System** | **Proposed System 1** |
|---|---:|---:|
| Economy Check-in | 1 desk | 12 units |
| Priority Check-in | 1 desk | 3 units |
| Economy Security | 2 lanes | 6 lanes |
| Priority Security | 2 lanes | 3 lanes |
| Boarding Gates | 2 gates | 3 units |
| **Total Service Units** | **8** | **27** |

### Service-Time Parameters

All service-time distributions remain unchanged from the Existing System.

| **Component** | **Service-Time Distribution** |
|---|---|
| Economy Check-in | Triangular (1.5, 2.5, 4.5) minutes |
| Priority Check-in | Triangular (1.0, 1.5, 2.5) minutes |
| Economy Security | Triangular (0.8, 1.2, 2.0) minutes |
| Priority Security | Triangular (0.8, 1.2, 2.0) minutes |
| Boarding Gates | Exponential (mean 0.4 minutes) |

### Implemented Changes

- Economy Check-in capacity increased from 1 to 12 using `EntityProcessor`.
- Priority Check-in capacity increased from 1 to 3 using `EntityProcessor`.
- Economy Security capacity increased from 2 to 6.
- Priority Security capacity increased from 2 to 3.
- Boarding Gates capacity increased from 2 to 3.

### Design Approach

- Maintain separate Economy and Priority check-in processes.
- Maintain separate Economy and Priority security queues.
- Merge both passenger classes into a shared boarding queue.
- Increase service capacity without changing service-time distributions.
- Retain the same passenger arrival, classification, and walking-time assumptions as the Existing System.