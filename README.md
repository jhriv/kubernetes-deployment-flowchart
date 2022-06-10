# Kubernetes Flowchart

A flow chart outlining Kubernetes deployment for UCLA Library

```mermaid
flowchart TD
  classDef terminator fill:#add8e6;
  classDef magic fill:#B4CE5F;
  S((START)) --> LC(local coding)
  class S terminator;
  LC --> CC(code commit)
  CC --> GHE{is running local git hooks required?}
  GHE --> |YES| LGH(run local git hooks)
  GHE --> |NO| PC["push commit(s)"]
  LGH --> GHP{is the status of local git hooks passing?}
  GHP --> |YES| PC
  GHP --> |NO| MWR{is more work required?}
  MWR --> |YES| LC
  MWR --> |NO| DBr[delete branch]
  DBr --> H((END))
  class H terminator;
  PC --> RT[run tests]
  RT --> TSP{is test suite passing?}
  TSP --> |YES| DB{is development branch?}
  TSP --> |NO| MWR
  DB --> |YES| DE[set enviromnent to development]
  DB --> |NO| TB{is test branch?}
  TB --> |YES| TE[set environment to test]
  TB --> |NO| SE[set environment to stage]
  DE --> HC{"is commit affecting helm chart(s)?"}
  TE --> HC
  SE ---> HC
  HC --> |YES| PM["deploy helm chart(s) to museum"]
  HC --> |NO| PH["pull current helm chart(s) from museum"]
  PM --> PH
  PH --> RC["run chart(s) with selected environment"]
  RC --> KM[[KUBERNETES MAGIC]]
  class KM magic;
  KM --> MM{is environment set to production?}
  MM --> |YES| H
  MM --> |NO| CR{is commit ready for mergin to main?}
  CR --> |YES| PR[create pull request for merge to main]
  CR --> |NO| MWR
  PR --> PRA{is pull request approved?}
  PRA --> |YES| MPR[merge pull request]
  MPR --> PE[set environment to production]
  PRA --> |NO| MWR
  PE --> PH
```
