# Kubernetes Flowchart

A flow chart outlining Kubernetes deployment for UCLA Library

```mermaid
flowchart TD
  LC(local coding) --> CC(code commit)
  CC --> LGH(run local git hoks)
  LGH --> GHP{do local git hooks pass?}
  GHP --> |YES| PC["push commit(s)"]
  GHP --> |NO| MWR[is more work required?]
  MWR --> |YES| LC
  MWR --> |NO| DBr[delete branch]
  DBr --> H((HALT))
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
  HC --> |YES| PM[push to museum]
  HC --> |NO| PH[pull current helm chart from museum]
  PM --> PH
  PH --> RC[run chart with selected environment]
  RC --> KM[[KUBERNETES MAGIC]]
  KM --> MM{is environment set to production?}
  MM --> |YES| H
  MM --> |NO| CR[is commit ready for mergin to main?]
  CR --> |YES| PR[create pull request for merge to main]
  CR --> |NO| MWR
  PR --> PRA{is pull request approved?}
  PRA --> |YES| PE[set environment to production]
  PRA --> |NO| MWR
  PE --> PH
```

- local coding

- code commit

- local git hooks

- do local hooks pass?
  - Y skip to is more work required?
  - N continue

- push commit(s)

- run tests

- is test suite passing?
  - Y continue
  - N skip to is more work required?

- is development branch?
  - Y environment is development
      skip to does commit affect helm charts
  - N continue

- is test branch?
  - Y environment is test
      continue
  - N environment is stage
      continue

- is commit affecting helm charts?
  - Y push to museum
      continue
  - N continue

- pull current chart from museum

- run chart with selected environment

- `[[ KUBERNETES MAGIC ]]`

- is merge to main?
  - Y HALT
  - N continue

- is commit ready for merging to main?
  - Y continue
  - N skip to is more work required?

- create PR for merge to main

- is PR approved?
  - Y environment is production
      skip to pull current chart from museum
  - N continue

- is more work required?
  - Y skip to local coding
  - N delete branch
      HALT
