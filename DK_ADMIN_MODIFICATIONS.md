# Admin Notebook Modifications

## Overview
All object names in `admin.ipynb` were refactored from cryptic abbreviations to intuitive, memorable names. Narrative markdown cells were added before every code cell so the notebook reads as a complete story even with code collapsed. Every RBAC concept, governance pattern, compute config, caching demo, and cost query remains conceptually identical.

---

## Change Table

| Concept | Original | Refactored | Changed? |
|---------|----------|------------|----------|
| **Database + schemas as containers** | Z2S with DEMO_FINANCE, DEMO_HR, GOVERNANCE | COMPANY with FINANCE, MARKETING, GOVERNANCE | Same concept, clearer names |
| **Three access tiers (Read/Write/Create)** | Z2S_SCH_FIN_R, Z2S_SCH_FIN_W, Z2S_SCH_FIN_C | FINANCE_READ, FINANCE_WRITE, FINANCE_CREATE | Same hierarchy, spelled out |
| **Inheritance: Create -> Write -> Read** | Identical GRANT chain | Identical GRANT chain | No change |
| **DB-level rollup roles** | Z2S_DB_R, Z2S_DB_W, Z2S_DB_C | DB_READ, DB_WRITE, DB_CREATE | Same pattern, clearer |
| **Functional roles assigned to users** | Z2S_FINANCE_ANALYST gets create on finance + read on governance | FINANCE_ANALYST gets create on finance + read on governance | Same privileges |
| **Functional role with limited access** | Z2S_HR_MANAGER gets read-only on HR + governance | MARKETING_MANAGER gets read-only on marketing + governance | Same privilege contrast |
| **Person user vs Service user** | JOHN_ANALYST (person) vs HR_SERVICE_ACCOUNT (service) | GORDON_GEKKO (person) vs MARKETING_AUTOMATION (service) | Same distinction, memorable names |
| **Role testing by switching roles** | USE ROLE + SELECT to prove access works | Identical | No change |
| **Monitoring grants/privileges** | SHOW GRANTS + ACCOUNT_USAGE query | Identical | No change |
| **Tags + tag-based masking** | PII_LEVEL tag -> PII_MASK policy | Identical | No change |
| **Column-level masking policies** | PHONE_MASK, REVENUE_MASK on specific columns | Identical, just updated role names in CASE logic | Same logic |
| **Row access policies with lookup tables** | Region-based filtering per role | Identical, just role name references updated | Same logic |
| **Comparing admin vs restricted role views** | ACCOUNTADMIN sees all, HR_MANAGER sees filtered/masked | ACCOUNTADMIN sees all, MARKETING_MANAGER sees filtered/masked | Same contrast |
| **Warehouse creation + t-shirt sizing** | Z2S_SIZING_WH X-Small with auto-suspend | ANALYTICS_WH X-Small with auto-suspend | Same config |
| **Warehouse resizing + QAS** | ALTER to LARGE + enable QAS | Identical | No change |
| **Multi-cluster warehouses** | Z2S_MULTICLUSTER_STANDARD_WH with STANDARD policy | REPORTING_MULTICLUSTER_WH with STANDARD policy | Same config |
| **Gen2 warehouses** | Z2S_GEN2_WH with STANDARD_GEN_2 | NEXT_GEN_WH with STANDARD_GEN_2 | Same config |
| **Resource monitors** | Z2S_WAREHOUSE_MONITOR at 100 credits | CREDIT_WATCHDOG at 100 credits | Same thresholds |
| **3 caching layers** | Results, metadata, warehouse cache demos | Identical queries and explanations | No change |
| **Cost management queries** | Service type consumption + weekly trends | Identical ACCOUNT_USAGE queries | No change |
| **Data Metric Functions** | NULL_COUNT on REVENUE_DATA.QUARTER | NULL_COUNT on REVENUE.QUARTER | Same DMF logic |

---

## Detailed Renaming Map

### Database
| Original | New |
|----------|-----|
| `Z2S` | `COMPANY` |

### Schemas
| Original | New |
|----------|-----|
| `Z2S.DEMO_FINANCE` | `COMPANY.FINANCE` |
| `Z2S.DEMO_HR` | `COMPANY.MARKETING` |
| `Z2S.GOVERNANCE` | `COMPANY.GOVERNANCE` |

### Database-Level Database Roles
| Original | New |
|----------|-----|
| `Z2S_DB_R` | `DB_READ` |
| `Z2S_DB_W` | `DB_WRITE` |
| `Z2S_DB_C` | `DB_CREATE` |

### Schema-Level Database Roles (Finance)
| Original | New |
|----------|-----|
| `Z2S_SCH_FIN_R` | `FINANCE_READ` |
| `Z2S_SCH_FIN_W` | `FINANCE_WRITE` |
| `Z2S_SCH_FIN_C` | `FINANCE_CREATE` |

### Schema-Level Database Roles (Marketing)
| Original | New |
|----------|-----|
| `Z2S_SCH_HR_R` | `MARKETING_READ` |
| `Z2S_SCH_HR_W` | `MARKETING_WRITE` |
| `Z2S_SCH_HR_C` | `MARKETING_CREATE` |

### Schema-Level Database Roles (Governance)
| Original | New |
|----------|-----|
| `Z2S_SCH_GOVERNANCE_R` | `GOVERNANCE_READ` |
| `Z2S_SCH_GOVERNANCE_W` | `GOVERNANCE_WRITE` |
| `Z2S_SCH_GOVERNANCE_C` | `GOVERNANCE_CREATE` |

### Functional (Account) Roles
| Original | New |
|----------|-----|
| `Z2S_FINANCE_ANALYST` | `FINANCE_ANALYST` |
| `Z2S_HR_MANAGER` | `MARKETING_MANAGER` |

### Warehouse Roles
| Original | New |
|----------|-----|
| `Z2S_SIZING_WH_U` | `ANALYTICS_WH_USER` |
| `Z2S_SIZING_WH_O` | `ANALYTICS_WH_OPERATOR` |

### Users
| Original | New | Type |
|----------|-----|------|
| `JOHN_ANALYST` | `GORDON_GEKKO` | PERSON |
| `HR_SERVICE_ACCOUNT` | `MARKETING_AUTOMATION` | SERVICE |

### Warehouses
| Original | New |
|----------|-----|
| `Z2S_SIZING_WH` | `ANALYTICS_WH` |
| `Z2S_MULTICLUSTER_STANDARD_WH` | `REPORTING_MULTICLUSTER_WH` |
| `Z2S_GEN2_WH` | `NEXT_GEN_WH` |

### Resource Monitor
| Original | New |
|----------|-----|
| `Z2S_WAREHOUSE_MONITOR` | `CREDIT_WATCHDOG` |

### Tables
| Original | New |
|----------|-----|
| `Z2S.DEMO_FINANCE.REVENUE_DATA` | `COMPANY.FINANCE.REVENUE` |
| `Z2S.DEMO_HR.EMPLOYEE_INFO` | `COMPANY.MARKETING.CAMPAIGNS` |
| `Z2S.DEMO_FINANCE.RESULTS_METADATA_CACHE_EXAMPLE` | `COMPANY.FINANCE.CACHE_DEMO_RESULTS` |
| `Z2S.DEMO_FINANCE.WAREHOUSE_CACHE_EXAMPLE` | `COMPANY.FINANCE.CACHE_DEMO_WAREHOUSE` |
| `Z2S.DEMO_HR.EMPLOYEES_PII` | `COMPANY.MARKETING.EMPLOYEES_SENSITIVE` |
| `Z2S.DEMO_FINANCE.CUSTOMERS_PII` | `COMPANY.FINANCE.CUSTOMERS_SENSITIVE` |
| `Z2S.GOVERNANCE.HR_MANAGER_REGIONS` | `COMPANY.GOVERNANCE.MARKETING_REGION_ACCESS` |
| `Z2S.GOVERNANCE.FINANCE_ANALYST_REGIONS` | `COMPANY.GOVERNANCE.FINANCE_REGION_ACCESS` |

### Governance Objects (Unchanged)
| Object Type | Name |
|-------------|------|
| Tag | `COMPANY.GOVERNANCE.PII_LEVEL` |
| Tag | `COMPANY.GOVERNANCE.DEPARTMENT` |
| Masking Policy | `COMPANY.GOVERNANCE.PII_MASK` |
| Masking Policy | `COMPANY.GOVERNANCE.PHONE_MASK` |
| Masking Policy | `COMPANY.GOVERNANCE.REVENUE_MASK` |
| Row Access Policy | `COMPANY.GOVERNANCE.EMPLOYEE_REGION_POLICY` |
| Row Access Policy | `COMPANY.GOVERNANCE.CUSTOMER_REGION_POLICY` |

---

## Narrative Markdown Addition
Every code cell now has a plain-English markdown cell above it explaining what the cell does and why. The notebook reads as a complete story even with all code collapsed.
