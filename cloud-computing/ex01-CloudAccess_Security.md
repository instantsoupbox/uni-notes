# Ex01 Cloud Access and Security

## Cloud Security
* The set of `{ policies, tech, controls }` deployed to protect `{ data, applications, associated infrastructure }` of cloud computing.

### Security Issue Categories
* Categories of security issues:
    * `c`: faced by customers (companies, organizations who host applications or store data on the cloud)
    * `p`: faced by cloud providers

### Seven Responsibilities
* **Seven responsibilities** based on service models `{ On-premises op, IaaS is, PaaS ps, SaaS ss }`:
    0. `op is ps ss` Responsibility
    1. `cc cc cc cc` Data classification & accountability
    2. `cc cc cc cp` Client & end-point protection
    3. `cc cc cp cp` Identity & access management
    4. `cc cc cp pp` Application level controls
    5. `cc cp pp pp` Network controls
    6. `cc cp pp pp` Host infrastructure
    7. `cc pp pp pp` Physical security

## Authentication
* The process of determining the identity of a client and what permissions an authenticated identity has on a set of resources.
* **VM / Cloud level authentication**
    * **User level authentication**
    * **Group level authentication**
* **Service level authentication**
    * **Global authentication**
    * **Single-route authentication**

## NodeJS
* Principle: Event-driven programming.
    * Main loop that listens for events.
    * Import `events` module
* Development process comprises writing processing functions for specific kinds of events.
* Components:
    * Module imports
    * Server (listener)
    * Reading requests, returning response