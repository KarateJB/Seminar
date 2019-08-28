# Progressive Deployment & NoDeploy

## History

**堅固式設計** (ITIL) -> **反脆弱式設計** (DevOps, Agile) -> **SRE**

**CI** -> **Continuous Delivery** -> **Continuous Deploy** -> **DevOps/SRE** -> **Dev(Ops)**

## Original Continuous Deploy stages

Commit -> Dev -> Test -> Staging -> Test -> Deploy -> Production -> Chaos

## Are we chasing tookits, not culture?

- NO Acceptence, Only commit
- NO Deploy, Only Feature flag
- NO Staging, Only Progressive Deployment
- NO QA/CI, Only Observability(觀測性)
- NO Visible Container  , Test in Production
- NO Test locally

> No means 無意識，not really NO

## Why we use facade|adapter|middleware when we need a fast deployment?

> STAGING: it must be as same as Production, or it's no meaning!

## Ideal Continuous Deploy stages

Local -> Production

## Feature Flag

Crons:

1. All codes will be deployed with a turn ON/OFF
2. By small segment
3. No need to deploy

Profs:

1. Might affect other codes
2. Codes not completed will be committed


## Test in Production


- Sometimes we can not reproduce the Production's architecture for E2E testing, so it is possible to test on Production environment. ([Chaos Engineering(Netflix)])


## Progressive Deployment

- No Deploy == No conscious Deploy(ment)
- Your aim wont be perfect, control the blast redius (你的瞄準並不完美，請控制好爆炸半徑)
- Deployment a service is not as same as activating for all users
- Observability (WHY) != Monitoring (WHAT)
- Observability = Logs + Traces + Matrics
- Resource, ability, time and cost

1. User segment
2. Traffic management
3. Observability
4. Automation

* Feature flag
* AB Test
* Blue-green deployment
* Canary(金絲雀) deployment
* Service mesh


## 

- [How Dark deploys code in 50ms](https://medium.com/darklang/how-dark-deploys-code-in-50ms-771c6dd60671)