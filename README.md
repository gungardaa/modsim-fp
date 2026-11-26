# Airport Simulation – Progress Log

## Existing System

### Passenger Flow
Arrival → Passenger Classification → Check-in → Security Screening → Boarding Gate → Exit

### Passenger Classification
- Economy: 70% (Standard check-in and regular security queue).
- Priority: 30% (Priority check-in and dedicated security queue)

### Given Parameters

| Component | Configuration |
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

| Route | Distance | Travel Time |
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