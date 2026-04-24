---
created: 2026-01-18
last_edited: 2026-04-24
tags:
  - dbt
  - seeds
  - exposures
  - documentation
  - lineage
  - testing
  - yaml
connections:
  - "[[001 Knowledge Base/Data/_index|Data Index]]"
  - "[[07 Quality Assurance]]"
  - "[[01 Architecture and Layering]]"
ai_generated: true
human_approved: true
category:
  - Knowledge Base
  - Data
  - Data Platform Design
---
## 1. Seeds: Managing Static Data
**Problem**: You have small mapping tables (e.g., "Country Code to Country Name", "Wall Status IDs") that rarely change. You don't want to keep them in a separate database or hardcode `CASE WHEN` statements.

**Solution: dbt Seeds**
* **Mechanism**: CSV files stored in the `seeds/` directory.
* **Action**: `dbt seed` uploads them to the warehouse as tables.
* **Best Practice**: This project applies **Tests** to seeds!
    *   *Example*: `import_seed_country.yml` checks that `iso2` code is `not_null` and `string`. This ensures even your static manual data is validated.

## 2. Exposures: Downstream Lineage
**Problem**: You change the `core_fact_orders` model. You don't know which Looker dashboard might break.

**Solution: Exposures**
Located in `models/exposures/exposures.yml`.
Exposures allow us to define "External Consumers" of our dbt models.

```yaml
exposures:
  - name: daily_dashboard
    type: dashboard
    url: https://looker.com/dashboards/165
    depends_on:
      - ref('core_fact_orders')
      - ref('core_dim_page')
```

* **Benefit**: When you build the dbt documentation (`dbt docs generate`), the Lineage Graph extends *beyond* the warehouse tables out to the Dashboards. You can visualize exactly what business asset depends on your code.

## 3. Documentation
This project uses a standard documentation structure.
* **YAML Descriptions**: Every model and column is described in `.yml` files.
* **Doc Blocks**: Reusable standard descriptions (e.g., `{{ doc('view_id') }}`) are defined in `docs/` and referenced in YAML. This ensures that the definition of "View ID" is identical across 50 different tables.

