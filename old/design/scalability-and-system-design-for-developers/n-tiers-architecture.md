# N-Tiers architecture

Tier - is a logical/physical separation of components.

Components:

* Databases
* Backend application
* UI
* Messaging
* Caching
* Load balancers

### Single tier application

![](<../../.gitbook/assets/image (45).png>)

* no network latency
* performance depends on client hardware
* no personal data transfer over the network
* no control of updating - user should do it manually
* vulnerability to reverse engineering



### Two-tier application

![](<../../.gitbook/assets/image (1).png>)

* **client-server**
* client contains the UI and the business logic
* database hosted and control by business
* application code vulnerable to being accessed by a third person

Typical examples are:

* to-do list applications, planners, and other
* online browser
* online games

When BL has no significant harm, this approach could reduce network calls (backend server keeps the latency of the application low) and improve UX.

Application makes calls to the server only for update state.



### Three-tier application

![](<../../.gitbook/assets/image (19).png>)

{% hint style="info" %}
UI, application logic and database lie on different machines
{% endhint %}



### N-tier application

{% hint style="info" %}
More than 3 components are involved

* Caches
* Queues for asynchronous behavior
* Load balancer
* Search servers
* web services
{% endhint %}

Distributed applications

#### Why so many tiers?

* **Single responsibility principle** means giving only one job to a component, be it saving data, running business logic, or delivering messages throughout the system.
* **Separation Of Concerns** means components shouldn't worry about other components. Components should be reusable and not tightly coupled with each other.

<details>

<summary>Tiers isn't the same with layers</summary>

<img src="../../.gitbook/assets/image (54).png" alt="" data-size="original">

The layers is the code organizations when tier is physical component separation.

</details>

