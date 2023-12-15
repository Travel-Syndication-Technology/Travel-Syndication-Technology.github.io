# CRM Integration

```mermaid
sequenceDiagram
    box User
    participant CRM
    participant Agent Browser
    end
    box TST
    participant Admin
    participant Trip
    end
    CRM->>Admin: Delivers Customer Data (JSON)
    Admin-->>Agent Browser: Saves CRM Key 
    Admin->>Agent Browser: Redirects to Login
    Agent Browser->>Admin: Login
    Admin->>Trip: Proceed to Checkout
    Trip->>Agent Browser: Checks for CRM Key
    Trip-->>Admin: Requests CRM Payload
    Admin->>Trip: Populates Traveler in Checkout
```