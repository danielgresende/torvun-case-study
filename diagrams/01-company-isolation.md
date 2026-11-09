# One company per request

Every request is resolved to exactly one company before a service reads a row. The gate checks the identity and the device, and the company comes from that check, never from the request itself.

```mermaid
flowchart TB
  Owners["Signed-in devices<br/>identity · device"] --> Gate["Gate<br/>resolves the company"]
  Gate --> AData
  Gate --> BData
  subgraph Host["Host and runtime"]
    subgraph B["Company B"]
      BData["Company B data<br/>agents · conversations · files<br/>reached only with a company B session"]
    end
    subgraph A["Company A"]
      AData["Company A data<br/>agents · conversations · files<br/>reached only with a company A session"]
    end
    Note["The two companies share no data<br/>no read, no write, no shared cache"]:::note
  end
  classDef note fill:#fff8e1,stroke:#c9a227,stroke-dasharray:5 3,color:#4a3b00
```

The gate is the only place that turns a session into a company. Services receive the resolved company; they do not re-read one from a query string. A cache key, an agent run, a file read and a conversation write all carry the same company, so a forgotten filter in one endpoint cannot cross the boundary.
