# Security Models & Design Principles

## CIA Triad Attacks

| CIA Property | Attack Type | Description |
|-------------|------------|-------------|
| Confidentiality | Disclosure | Exposing secret data |
| Integrity | Alteration | Modifying data |
| Availability | Destruction/Denial | Destroying or denying access to data |

---

## Security Models

### Bell-LaPadula Model (Confidentiality)

| Rule | Name | Description |
|------|------|-------------|
| Simple Security Property | "No read up" | Lower security subject cannot read higher security objects |
| Star Security Property | "No write down" | Higher security subject cannot write to lower security objects |
| Discretionary-Security Property | Access matrix | Uses a matrix to allow/deny read and write operations |

### Biba Model (Integrity)

| Rule | Name | Description |
|------|------|-------------|
| Simple Integrity Property | "No read down" | Higher integrity subject should not read lower integrity objects |
| Star Integrity Property | "No write up" | Lower integrity subject should not write to higher integrity objects |

### Clark-Wilson Model (Integrity)

| Concept | Description |
|---------|-------------|
| Constrained Data Item (CDI) | Data whose integrity we want to preserve |
| Unconstrained Data Item (UDI) | All data beyond CDI, such as user and system input |
| Transformation Procedures (TPs) | Programmed operations (read/write) that maintain CDI integrity |
| Integrity Verification Procedures (IVPs) | Check and ensure the validity of CDIs |

---

## ISO/IEC 19249 - Five Architectural Principles

| Principle | Description | Example |
|-----------|-------------|---------|
| Domain Separation | Related components grouped as a single entity with shared security attributes | x86 ring 0 (kernel) vs ring 3 (user apps) |
| Layering | System structured into abstract levels, security policies imposed at each layer | OSI model 7 layers - relates to Defence in Depth |
| Encapsulation | Hide low-level implementation, expose only specific methods | OOP clock object with increment() instead of direct variable access |
| Redundancy | Ensures availability and integrity through duplication | RAID 5 - if one drive fails, data still available |
| Virtualization | Sharing hardware across multiple OS, provides sandboxing for security | Cloud services, malware detonation environments |

---

## ISO/IEC 19249 - Five Design Principles

| Principle | Description | Example |
|-----------|-------------|---------|
| Least Privilege | Provide minimum permissions needed for a task, nothing more | Read rights only if user just needs to view a document |
| Attack Surface Minimisation | Reduce vulnerabilities by removing unnecessary services | Disabling unused services when hardening Linux |
| Centralized Parameter Validation | Validate all inputs through one shared library or system | Web app passing all form fields through one validation library |
| Centralized General Security Services | Centralize all security services to avoid duplication | Active Directory checking every login rather than each computer keeping its own user list |
| Preparing for Error and Exception Handling | Design systems to fail safe, avoid leaking sensitive info in error messages | Firewall blocks all traffic if it crashes rather than allowing all |

---

## Zero Trust vs Trust but Verify

| Principle | Approach |
|-----------|---------|
| Trust but Verify | Trust entities but set up logging and review mechanisms to confirm behaviour |
| Zero Trust | Treat every entity as adversarial until proven otherwise - authentication and authorization required for every resource |

**Microsegmentation** - a Zero Trust implementation where network segments can be as small as a single host, with authentication required between all segments.

---

## Cloud Security Responsibility

| Model | Provider Secures | You Secure |
|-------|-----------------|-----------|
| IaaS (e.g. AWS EC2) | Data centres, physical hosts, virtualization layer | OS, applications, data |
| PaaS (e.g. Azure App Service) | Data centres, physical hosts, virtualization, OS, runtime | Application code, settings, data |
| SaaS (e.g. Microsoft 365) | Everything - runs the whole application | Accounts, access settings, your data |
