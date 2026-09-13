# TRANSITBOARD

## A Smart Bus Tracking System

### Project Report

**Project Better Tomorrow**
**Methodology:** Empathize → Define → Ideate with AI → Prototype → Validate

**Submitted by:**
Your Name
Course / Roll Number
Department
Institution

**Academic Year:** 2026

---

# ABSTRACT

Public transportation is an important part of everyday mobility for students, employees, and other commuters. However, one of the common difficulties experienced by bus users is not necessarily the journey itself, but the uncertainty associated with waiting. A commuter may know the destination but still not know where the bus currently is, when it will arrive, whether it has already departed, or whether there is enough time to reach the stop. This uncertainty can make waiting feel inefficient and stressful.

This project presents **TransitBoard**, a browser-based prototype of a Smart Bus Tracking System designed to address this problem. The project follows a user-centred design approach based on the stages of **Empathize, Define, and Ideate with AI**, followed by prototype development. Instead of attempting to build a complete city-scale transportation platform, the project deliberately focuses on demonstrating the core functionality required for a smart bus tracking experience.

TransitBoard provides three simulated routes: **R7 – Campus Loop, R12 – Highway Express, and R4 – Old Town Connector**. The prototype displays buses moving along their routes, provides estimated arrival information for individual stops, and includes a trip planner that calculates waiting time, ride duration, and estimated arrival time. The system uses simulated schedule data and a time-based position engine rather than physical GPS hardware. Each route is represented as a continuous loop, with stops assigned fractional positions along the route and buses distributed using different offsets.

The project demonstrates how transportation information can be converted into useful passenger decisions. Rather than simply showing a bus location, the system attempts to answer the practical question: **“Should I leave now?”**

The current implementation is intentionally a prototype and does not claim to provide real-world GPS accuracy. Its purpose is to demonstrate the underlying interaction, tracking logic, ETA calculation, route planning, and user experience that could form the basis of a future real-time transportation system. Future development could integrate GPS devices, live traffic information, passenger occupancy data, delay prediction, driver applications, notifications, and accessibility features.

**Keywords:** Smart Bus Tracking, TransitBoard, Public Transportation, ETA, Route Planning, Simulated GPS, User-Centred Design, Prototype, Artificial Intelligence

---

# 1. INTRODUCTION

## 1.1 Background

Public transportation systems depend on the ability of passengers to understand where vehicles are, which routes they follow, and when they are expected to arrive. In a traditional bus system, passengers may have access to a printed timetable or a fixed schedule, but a timetable alone cannot always represent the actual state of a bus. Traffic, delays, early departures, operational changes, and other factors can cause the expected arrival time to differ from the scheduled time.

For passengers, this creates a simple but important problem: **uncertainty**.

A commuter waiting at a bus stop may have several questions:

* Where is the bus right now?
* How long will I have to wait?
* Has the bus already passed?
* Which bus should I take?
* How long will my complete journey take?
* Should I leave my current location now or wait a few more minutes?

These questions are particularly relevant for students travelling around a campus, employees commuting between locations, and passengers who need to transfer between routes.

The TransitBoard project was developed around this everyday friction. The central idea is that transportation information becomes more useful when it helps passengers make decisions instead of simply displaying raw data.

---

## 1.2 Project Context

TransitBoard was developed as part of **Project Better Tomorrow**. The project uses a design-thinking-oriented process:

**Empathize → Define → Ideate with AI → Prototype → Validate**

The initial problem exploration focused on commuter experiences and the uncertainty surrounding bus waiting. The project presentation describes this as “waiting is the friction — not the ride.”

The goal was therefore not to build a complete commercial transportation infrastructure. Instead, the objective was to create a meaningful working prototype that demonstrates how a smart bus tracking interface could reduce uncertainty.

This distinction is important. A production transportation system would require GPS hardware, communication infrastructure, backend services, databases, authentication, monitoring, fault handling, and potentially mobile applications. Implementing all of these components would significantly increase the project scope.

Therefore, TransitBoard concentrates on the **core user experience and algorithmic concept** of real-time tracking.

---

# 2. PROBLEM STATEMENT

## 2.1 Problem Identification

The primary problem identified in this project is the lack of visibility into bus movement and arrival timing.

A commuter may be physically close to a bus stop but still be unable to determine whether the bus will arrive in one minute, ten minutes, or twenty minutes. A static timetable may provide an expected schedule, but it does not provide an intuitive representation of the current position of buses.

The problem can therefore be expressed as:

> **Daily commuters need visibility into a bus's current location and estimated arrival time because uncertainty, rather than distance alone, makes waiting feel wasted.**

The project presentation further identifies that riders need the combination of **route, timing, and destination information** to decide whether they should leave their current location.

---

## 2.2 User Questions

The project focuses on three fundamental questions:

1. **Where is the bus?**
2. **When will it arrive?**
3. **Will I make it to my destination in time?**

These questions were converted into functional requirements for the prototype.

| User Question                        | TransitBoard Feature                     |
| ------------------------------------ | ---------------------------------------- |
| Where is the bus?                    | Live simulated bus position              |
| When will it arrive?                 | Per-stop ETA board                       |
| Which bus should I take?             | Multiple buses shown simultaneously      |
| How long will my journey take?       | Trip planner                             |
| Which route should I use?            | Route selection                          |
| Can I understand the system quickly? | Simple dashboard and colour-coded routes |

---

# 3. OBJECTIVES

The main objective of TransitBoard is to develop a working prototype that demonstrates the core functionality of a smart bus tracking system.

The specific objectives are:

### 3.1 Provide Live-Looking Bus Tracking

The system should visually represent buses moving continuously along predefined routes.

### 3.2 Provide Estimated Arrival Times

Passengers should be able to view the next expected bus arrivals for each stop.

### 3.3 Support Route Planning

Users should be able to select a route, boarding stop, and destination stop and receive an estimated journey result.

### 3.4 Reduce Waiting Uncertainty

The interface should provide enough information for a passenger to make a practical decision about when to leave.

### 3.5 Demonstrate Real-Time System Logic

The prototype should update bus positions and arrival information continuously rather than displaying static information.

### 3.6 Maintain Appropriate Prototype Scope

The system should demonstrate the core concept without unnecessarily implementing hardware, authentication, multi-city infrastructure, or other production-level features.

### 3.7 Use AI During Ideation and Development

Artificial intelligence was used as a brainstorming and development partner to explore possible features, structure the tracking logic, and evaluate which features were appropriate for the intended prototype scope.

---

# 4. DESIGN THINKING AND CASE STUDY

## 4.1 Empathize

The first stage of the project was understanding the commuter problem from a user's perspective.

The case study identifies three representative user types:

### Campus Regular

A campus regular uses the same route frequently. Their primary concern is knowing whether the bus is close enough that it is worth leaving their room, classroom, or other location.

For this user, even a small improvement in arrival visibility can make the journey more convenient.

### Working Commuter

A working commuter may travel between multiple locations or transfer between routes. Missing a connection can create additional waiting time and affect the overall journey.

For this user, ETA information is important because the arrival of one bus can affect the feasibility of the next part of the journey.

### First-Time Rider

A first-time rider may not understand route names, stop names, or the structure of the transportation network.

For this user, the system should be understandable without requiring detailed knowledge of the route network.

The presentation describes the empathize stage as involving observation of stops, short-form interviews, and observation logs across a week of commutes.

---

## 4.2 Define

The observations were converted into a central problem statement:

> **Daily commuters need real-time visibility into a bus's location and arrival time because uncertainty, not distance, is what makes waiting feel wasted.**

This definition helped establish an important design principle.

The objective was not simply:

> “Show buses on a screen.”

Instead, the objective was:

> **“Give passengers enough information to confidently decide when and where to travel.”**

This distinction influenced the design of the trip planner and ETA board.

---

# 5. IDEATION WITH AI

## 5.1 Role of AI

Artificial intelligence was used during the ideation stage as a **divergence and development partner**.

The role of AI was not to replace the project decision-making process. Instead, it helped explore alternatives and structure the problem into implementable components.

The project presentation specifically describes the use of Claude for:

* Exploring solution options
* Structuring tracking logic
* Pressure-testing the appropriate prototype scope
* Considering which features should be adopted, modified, or rejected

---

## 5.2 Features Adopted

Two major ideas were adopted.

### Loop-Based Position Engine

A route is represented as a continuous loop. Each stop has a position between 0 and 1 along that route.

The current bus position is calculated from time and route duration.

This approach produces continuously changing positions without requiring GPS hardware.

### Trip Planner

The trip planner combines:

* Waiting time
* Ride duration
* Estimated arrival time

This turns tracking information into a practical passenger decision.

---

## 5.3 Features Modified

The prototype originally could have been expanded into a much larger transportation network. Instead, the project deliberately uses three routes:

* R7 – Campus Loop
* R12 – Highway Express
* R4 – Old Town Connector

Colour coding was also used to make route identification faster.

---

## 5.4 Features Rejected

Several possible features were intentionally excluded from the prototype:

### Real GPS Hardware

Real GPS integration would require physical devices, a communication mechanism, backend processing, and live data management.

Because the project is a prototype, this was considered outside the intended scope.

### Login System

Authentication does not contribute directly to demonstrating bus tracking and therefore would add complexity without improving the core prototype.

### Multi-City Support

A full multi-city transportation system would require substantially more data and infrastructure. It was therefore considered unnecessary for this project.

This demonstrates an important aspect of the development process: **features were selected according to the problem and project scope rather than simply maximizing the number of features.**

---

# 6. SYSTEM OVERVIEW

## 6.1 System Description

TransitBoard is implemented as a browser-based dashboard.

The interface consists primarily of:

1. Application header
2. Live clock
3. Route selection tabs
4. Route tracking panel
5. Stop-by-stop ETA information
6. Trip planning panel
7. Prototype information footer

The system does not require a backend server for its current prototype implementation. The schedule and route data are embedded directly in the JavaScript application.

The prototype identifies itself explicitly as a simulated tracking system rather than claiming to use physical GPS.

---

# 7. SYSTEM ARCHITECTURE

The current prototype can be understood as three logical layers.

## 7.1 Data Layer

The data layer contains route information.

Each route stores:

* Route ID
* Route name
* Route duration
* Number of buses
* Stop names
* Stop positions
* Route colour

For example, R7 is defined as the Campus Loop with a period of 18 minutes and three buses. Its stops include Main Gate, Library Circle, Hostel Block C, Sports Complex, Food Court, and Admin Building.

R12 represents the Highway Express with a 40-minute loop and two buses, while R4 represents the Old Town Connector with a 26-minute loop and two buses.

---

## 7.2 Processing Layer

The processing layer calculates the current position of each bus and its ETA.

The main mathematical concept is fractional route position.

If:

* \(P\) = route period in seconds
* \(B\) = number of buses
* \(H = P/B\) = bus headway
* \(t\) = current time in seconds
* \(i\) = bus index

then the prototype calculates the bus's position using a time offset:

$$
position_i =
\frac{(t+iH)\bmod P}{P}
$$

This produces a value between 0 and 1.

A value of:

* 0.00 represents the beginning of the route
* 0.25 represents approximately one quarter of the route
* 0.50 represents approximately halfway
* 0.75 represents approximately three quarters
* 1.00 returns to the start of the loop

The implementation uses the current time and a different offset for each bus.

---

# 8. ETA CALCULATION

## 8.1 Basic ETA Logic

Each stop also has a fractional position.

Suppose a bus is currently at:

$$
B_f = 0.30
$$

and the target stop is at:

$$
S_f = 0.50
$$

The difference is:

$$
0.50 - 0.30 = 0.20
$$

If the route takes 1,080 seconds for a complete loop, the estimated travel time is:

$$
0.20 \times 1080 = 216\ seconds
$$

or approximately 3.6 minutes.

The prototype uses this general approach through its `etaSeconds()` function.

If the target stop is behind the current bus position, the system adds one complete loop to represent the circular nature of the route.

---

## 8.2 Multiple Bus Handling

The system calculates ETA values for every bus operating on the selected route.

The values are then sorted from the shortest waiting time to the longest waiting time.

The interface displays the **next two arrivals** for each stop.

This is useful because passengers do not necessarily need to know only the first arriving bus. Seeing multiple buses gives them a better understanding of the service frequency.

The implementation explicitly calculates ETA for every active bus and displays the first two after sorting.

---

# 9. TRIP PLANNER

The trip planner is one of the most important passenger-facing features.

A user selects:

1. Route
2. Boarding stop
3. Destination stop

The system then identifies the bus that will reach the boarding stop soonest.

The waiting time is calculated by checking each bus's current position against the boarding stop.

The system then calculates ride duration based on the difference between the boarding stop and destination stop.

The overall estimated journey time is:

$$
Total\ Time = Waiting\ Time + Ride\ Duration
$$

The estimated arrival time is then calculated by adding this total duration to the current system time.

The prototype implements this process by checking all buses, selecting the one with the smallest waiting time, calculating the ride distance along the loop, and adding the two durations.

---

# 10. USER INTERFACE DESIGN

## 10.1 Dashboard

The interface uses a dark dashboard design with clear visual hierarchy.

The major interface components are separated into panels so that users can distinguish:

* Route information
* Bus movement
* ETA information
* Trip planning

The header displays the name **TransitBoard** and describes the system as a smart bus tracking prototype using a simulated campus and city schedule.

---

## 10.2 Route Selection

Three route tabs allow the user to switch between:

* **R7 – Campus Loop**
* **R12 – Highway Express**
* **R4 – Old Town Connector**

Each route has a distinct visual colour.

This provides an immediate visual distinction between routes without requiring users to repeatedly read route descriptions.

---

## 10.3 Route Tracking

The route panel displays each stop vertically.

Every stop contains:

* Stop name
* Approximate position along the route
* Next bus arrival information

Bus markers are positioned dynamically along the route line.

The bus markers are updated continuously, creating a live-tracking effect.

---

## 10.4 Live Clock

The dashboard contains a live clock and date.

The clock updates every second and reinforces the real-time nature of the interface.

---

# 11. IMPLEMENTATION

## 11.1 Technologies Used

The current prototype is implemented using standard web technologies:

* **HTML** – page structure
* **CSS** – layout, styling, responsive design
* **JavaScript** – simulation, calculations, dynamic rendering
* **SVG/visual browser elements** – visual representation where applicable

The prototype is designed to operate as a standalone browser application.

---

## 11.2 Route Data

The route data is stored in JavaScript objects.

Each route contains a list of stops represented using a normalized position.

For example:

```text
Main Gate             0.00
Library Circle        0.16
Hostel Block C        0.34
Sports Complex        0.52
Food Court            0.70
Admin Building        0.88
```

This representation allows the same ETA algorithm to operate across all routes.

---

## 11.3 Continuous Updating

The live interface is refreshed once every second.

The prototype calls the update function through:

```text
setInterval(updateLiveData, 1000)
```

During each update cycle, the system:

1. Reads the current time.
2. Calculates the position of each bus.
3. Calculates ETA values for each stop.
4. Sorts bus arrivals.
5. Updates the arrival chips.
6. Repositions bus markers.
7. Updates the live clock.

The source implementation explicitly performs these operations during its live update cycle.

---

# 12. FUNCTIONAL REQUIREMENTS

The system can be evaluated against the following functional requirements.

| ID   | Requirement                             | Status          |
| ---- | --------------------------------------- | --------------- |
| FR1  | Display available routes                | Implemented     |
| FR2  | Select a route                          | Implemented     |
| FR3  | Display route stops                     | Implemented     |
| FR4  | Simulate bus movement                   | Implemented     |
| FR5  | Calculate ETA                           | Implemented     |
| FR6  | Display next two arrivals               | Implemented     |
| FR7  | Plan a journey                          | Implemented     |
| FR8  | Calculate waiting time                  | Implemented     |
| FR9  | Calculate ride duration                 | Implemented     |
| FR10 | Calculate estimated arrival             | Implemented     |
| FR11 | Continuously refresh live information   | Implemented     |
| FR12 | Integrate physical GPS                  | Not implemented |
| FR13 | Store data in a backend database        | Not implemented |
| FR14 | Provide real-time external traffic data | Not implemented |

The distinction between implemented and non-implemented functionality is important because the project is a prototype rather than a production transportation system.

---

# 13. NON-FUNCTIONAL REQUIREMENTS

## 13.1 Usability

The interface should allow users to understand the route and ETA information without extensive instruction.

## 13.2 Responsiveness

The dashboard uses responsive CSS so that the main layout can adapt to smaller screen sizes.

## 13.3 Performance

Because the current prototype operates primarily on local JavaScript data, it does not require repeated network requests for route calculations.

## 13.4 Maintainability

Route information is separated into structured JavaScript data objects, making it relatively straightforward to add or modify routes.

## 13.5 Transparency

The prototype explicitly states that its bus positions are generated from a simulated schedule rather than live GPS. This prevents the demonstration from being mistaken for a production GPS tracking service.

---

# 14. TESTING AND EVALUATION

## 14.1 Functional Testing

The prototype can be evaluated through scenario-based testing.

### Test Case 1: Route Selection

**Input:** Select R7.

**Expected Result:**
The dashboard changes to the Campus Loop and displays its associated stops and buses.

**Result:**
Implemented successfully.

---

### Test Case 2: Bus Position

**Input:** Observe the dashboard over time.

**Expected Result:**
Bus markers change position continuously.

**Result:**
Implemented through the time-based position engine.

---

### Test Case 3: ETA Display

**Input:** Select a route and inspect a stop.

**Expected Result:**
The system displays estimated arrival times for the next two buses.

**Result:**
Implemented.

---

### Test Case 4: Trip Planning

**Input:** Select a route, boarding stop, and destination stop.

**Expected Result:**
The system displays the next bus, waiting time, ride duration, and estimated arrival time.

**Result:**
Implemented.

---

### Test Case 5: Same Boarding and Destination Stop

**Input:** Select the same stop as boarding and destination.

**Expected Result:**
The system should prevent an invalid journey.

**Result:**
The prototype displays an explanatory message requiring different stops.

---

# 15. VALIDATION APPROACH

The project presentation contains a validation section describing three sample tester scenarios. However, the presentation explicitly states:

> “Sample feedback — replace with notes from your own testers before submission.”

Therefore, these statements should not be presented as verified empirical results unless actual user testing was conducted.

A valid future validation process would involve asking students or commuters to use the prototype without an initial walkthrough and observing whether they can:

1. Identify their route.
2. Find their boarding stop.
3. Determine the next bus.
4. Understand the ETA.
5. Use the trip planner.
6. Determine whether they should leave immediately or wait.
7. Understand route names and colours.

The validation should record both successful interactions and points of confusion.

---

# 16. LIMITATIONS

TransitBoard demonstrates the core concept successfully as a prototype, but it has several important limitations.

## 16.1 Simulated GPS

The system does not receive GPS coordinates from physical buses.

Instead, bus positions are mathematically generated using the current time, route duration, and bus offsets.

Consequently, the displayed position should not be interpreted as the actual position of a real bus.

---

## 16.2 Fixed Route Model

Each route is represented as a simple loop.

Real transportation networks may contain:

* Branches
* Intersections
* One-way sections
* Route variations
* Temporary diversions
* Different travel times between stops

The current normalized-position model simplifies these complexities.

---

## 16.3 Fixed Travel Speed

The ETA calculation assumes that movement along the route is consistent.

Real buses are affected by:

* Traffic
* Weather
* Road conditions
* Passenger boarding time
* Signals
* Road closures
* Driver behaviour

The current prototype does not model these variables.

---

## 16.4 No Backend

The route and schedule information exists inside the client-side application.

A real system would require centralized data management so that multiple users could receive the same updated information.

---

## 16.5 No Physical Bus Integration

The prototype does not communicate with actual buses or GPS devices.

This was a deliberate scope decision rather than an accidental omission.

---

## 16.6 Limited Route Network

Only three sample routes are provided.

The prototype is intended to demonstrate the concept rather than represent a complete city transportation network.

---

# 17. SECURITY AND PRIVACY CONSIDERATIONS

Although the current prototype does not process sensitive personal information, a production version would require security and privacy considerations.

A real implementation could potentially process:

* User accounts
* Travel preferences
* Location information
* Trip histories
* Device information
* Notification settings

Such information should be protected through appropriate authentication, authorization, encryption, access control, and data-retention policies.

The prototype avoids many of these concerns because it does not require user accounts or personal location tracking.

This is another example of how limiting the project scope makes the prototype easier to demonstrate while avoiding unnecessary infrastructure.

---

# 18. PROJECT SCOPE AND DECISION-MAKING

One of the most important design decisions in this project was to prioritize **depth of demonstration over breadth of features**.

A transportation platform can quickly become extremely complex. Possible features include:

* Real GPS
* Traffic prediction
* Passenger counting
* Driver applications
* Ticketing
* Payments
* User accounts
* Notifications
* Route optimization
* Fleet management
* Analytics
* Accessibility systems

Implementing all these components would not necessarily produce a better academic prototype.

Instead, TransitBoard focuses on a core loop:

> **Track → Estimate → Plan**

This means the user can see a bus, understand when it will arrive, and estimate the resulting journey.

The presentation describes the prototype as a 50%-scope implementation designed around this core loop.

---

# 19. FUTURE ENHANCEMENTS

## 19.1 Real GPS Integration

The most important future enhancement is replacing the simulated position engine with real GPS data.

A physical bus could use a GPS-enabled device to periodically send:

* Latitude
* Longitude
* Speed
* Direction
* Timestamp
* Vehicle identifier

A backend service could process this information and provide current vehicle positions to passengers.

---

## 19.2 Backend Architecture

A production system could introduce:

**Bus GPS → Communication Network → Backend → Database → API → Passenger Application**

The backend could manage:

* Vehicle locations
* Routes
* Stops
* Schedules
* Historical trips
* Service alerts
* User requests

---

## 19.3 Traffic-Aware ETA

Instead of assuming a fixed route speed, ETA could be calculated using historical and real-time traffic conditions.

A future model could consider:

$$
ETA = f(distance, speed, traffic, historical\ travel\ time, delays)
$$

This would make arrival estimates more realistic.

---

## 19.4 Crowding Information

Passenger occupancy could be added to help users choose between buses.

Possible states could include:

* Low occupancy
* Moderate occupancy
* High occupancy
* Full

This would extend the system beyond simple location tracking and help improve passenger comfort.

---

## 19.5 Delay Alerts

The system could notify passengers when a bus is delayed.

Examples include:

* “R7 Bus 2 is delayed by 5 minutes.”
* “R12 service is experiencing heavy traffic.”
* “Your selected bus is one stop away.”

---

## 19.6 Driver Companion Application

A lightweight driver application could allow drivers or operators to report:

* Vehicle status
* Breakdown
* Route changes
* Service interruptions
* Emergency conditions

This information could then be reflected in the passenger dashboard.

---

## 19.7 Accessibility

Future versions could include:

* Audio arrival announcements
* Larger text mode
* High-contrast mode
* Screen-reader-friendly components
* Simplified navigation
* Accessible route descriptions

The project presentation specifically identifies audio arrival announcements and larger-text modes as possible future improvements.

---

# 20. EXPECTED IMPACT

The expected impact of TransitBoard is not simply faster transportation. Instead, the system aims to improve **information quality and passenger confidence**.

A passenger who knows that a bus will arrive in two minutes can prepare to leave immediately.

A passenger who knows that the next bus will arrive in twelve minutes can choose to continue studying, working, or completing another activity rather than waiting unnecessarily at the stop.

A passenger travelling to a destination can estimate the complete journey rather than considering only the arrival time of the first bus.

Therefore, the system attempts to convert transportation uncertainty into actionable information.

---

# 21. SOCIAL AND PRACTICAL RELEVANCE

Smart transportation systems can contribute to better public transport experiences by improving access to information.

For students, reliable bus information can make movement around a campus more convenient.

For employees and daily commuters, ETA information can help with time management.

For first-time riders, route information can reduce confusion.

For transport operators, future versions could provide historical information that helps identify demand patterns and service issues.

The prototype therefore represents a small-scale example of how digital systems can be applied to an everyday infrastructure problem.

---

# 22. LEARNING OUTCOMES

The development of TransitBoard provided experience in several areas.

## 22.1 Problem Definition

The project demonstrated that effective software development begins by identifying the actual user problem rather than immediately choosing a technology.

## 22.2 User-Centred Design

The Empathize and Define stages helped translate a general idea of “bus tracking” into a more specific problem involving uncertainty and decision-making.

## 22.3 Algorithmic Thinking

The route-position model demonstrated how a real-world movement problem can be represented mathematically.

## 22.4 Front-End Development

The project required the integration of HTML, CSS, and JavaScript to create a responsive and interactive interface.

## 22.5 Scope Management

The project demonstrated the importance of distinguishing between an academic prototype and a production system.

## 22.6 AI-Assisted Ideation

AI was used to explore alternatives and structure the implementation while the final project scope remained based on the identified problem and prototype requirements.

---

# 23. CONCLUSION

TransitBoard was developed as a prototype Smart Bus Tracking System focused on a simple but important commuter problem: **uncertainty while waiting for a bus**.

The project began by examining the experience of different types of commuters and identifying the questions that matter most to them: where the bus is, when it will arrive, and whether they have enough time to reach their destination.

These observations led to a focused problem statement in which uncertainty, rather than distance alone, was identified as a major source of inconvenience.

The project then used AI during the ideation stage to explore possible solutions and determine an appropriate prototype scope. Rather than attempting to build a complete real-world transportation infrastructure, the project adopted a loop-based position engine, multiple simulated buses, a per-stop ETA board, and a trip planner.

The resulting TransitBoard prototype provides three sample routes, continuously changing simulated bus positions, estimated arrival times, and journey calculations. Its core algorithm uses route positions, loop durations, bus offsets, and current time to create a live-looking tracking experience. The trip planner extends this information by calculating waiting time, ride duration, and estimated arrival.

An important limitation is that TransitBoard is not connected to real GPS hardware or live transportation infrastructure. Its bus positions are generated from simulated schedule data. This is intentional and reflects the prototype scope of the project.

The project can therefore be viewed as a demonstration of the **core interaction model** of a smart transportation system rather than a finished transportation platform.

The central concept of the project can be summarized as:

> **Track → Estimate → Plan**

Tracking shows where the bus is.
Estimation tells the passenger when it will arrive.
Planning helps the passenger decide what to do next.

Future versions could replace the simulation with real GPS feeds and introduce traffic-aware ETAs, occupancy information, delay notifications, driver applications, backend infrastructure, and accessibility features.

Ultimately, the value of a smart bus tracking system is not simply in displaying the location of a vehicle. Its greater purpose is to transform transportation data into information that helps people make better decisions.

TransitBoard demonstrates this concept at prototype scale and provides a foundation from which a more complete real-time public transportation system could be developed.

---

# REFERENCES

1. TransitBoard Smart Bus Tracking Prototype — project HTML implementation.

2. TransitBoard Presentation — Project Better Tomorrow case study and prototype overview.

3. Project methodology: Empathize → Define → Ideate with AI → Prototype → Validate.

4. TransitBoard source implementation — route data, bus position calculation, ETA calculation, route rendering, and trip planner logic.

---

# APPENDIX A — SYSTEM FEATURES

### Current Prototype Features

* Browser-based dashboard
* Three simulated bus routes
* Multiple buses per route
* Live clock
* Route selection
* Stop-by-stop route visualization
* Simulated bus movement
* Per-stop ETA
* Next two bus arrivals
* Trip planner
* Waiting-time calculation
* Ride-duration calculation
* Estimated arrival calculation
* Responsive interface

### Current Routes

**R7 – Campus Loop**

* Main Gate
* Library Circle
* Hostel Block C
* Sports Complex
* Food Court
* Admin Building

**R12 – Highway Express**

* City Junction
* Toll Plaza
* Tech Park
* Riverside
* Central Mall
* Railway Station

**R4 – Old Town Connector**

* Clock Tower
* Market Street
* Old Bridge
* Hospital
* Stadium
* Bus Depot

---

# APPENDIX B — CORE ALGORITHM

The simplified tracking model follows these steps:

### Step 1: Store Route Data

Each route stores a loop duration and stop positions.

### Step 2: Determine Bus Headway

$$
Headway = \frac{Route\ Period}{Number\ of\ Buses}
$$

### Step 3: Calculate Current Bus Position

$$
Bus\ Position =
\frac{(Current\ Time + Bus\ Offset)\bmod Route\ Period}
{Route\ Period}
$$

### Step 4: Calculate ETA

$$
ETA = Route\ Position\ Difference \times Route\ Period
$$

### Step 5: Select Earliest Bus

The ETA of every active bus is compared and the smallest value is selected.

### Step 6: Calculate Complete Journey

$$
Journey\ Time = Waiting\ Time + Ride\ Duration
$$

### Step 7: Calculate Arrival

$$
Estimated\ Arrival =
Current\ Time + Journey\ Time
$$

This model is intentionally simplified but demonstrates the fundamental relationship between vehicle position, route position, time, and estimated arrival.

---

# APPENDIX C — PROTOTYPE LIMITATION STATEMENT

TransitBoard is an academic prototype developed to demonstrate the concept of a Smart Bus Tracking System. Bus positions and arrival estimates are generated using simulated route and schedule data rather than live GPS hardware. The prototype demonstrates the core tracking, ETA, and journey-planning logic that could be extended into a real deployment using GPS feeds, backend infrastructure, traffic information, and additional transportation data.
