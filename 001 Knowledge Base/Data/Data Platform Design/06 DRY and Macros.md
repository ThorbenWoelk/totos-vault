---
created: 2026-01-18
last_edited: 2026-04-24
tags:
  - dbt
  - macros
  - dry
  - business-logic
  - best-practices
  - sql
connections:
  - "[[001 Knowledge Base/Data/_index|Data Index]]"
  - "[[02 Dimensional Modeling Kimball]]"
  - "[[01 Architecture and Layering]]"
ai_generated: true
human_approved: true
category:
  - Knowledge Base
  - Data
  - Data Platform Design
---
## The dbt Solution: Jinja Macros
Macros are functions that write SQL for you.

### 1. Business Logic Macros
This repository uses macros to centralize business rules.

**Example: `clean_ressort`**
Located in `macros/business_logic/clean_ressort.sql`.
* **Goal**: Standardize the "Ressort" (Department) field from messy source data (typos, variations).
* **Implementation**:
    ```sql
    {% macro clean_ressort(column_name) %}
    case
        when lower({{column_name}}) like '%leo%' then '<client-product>'
        when lower({{column_name}}) in ('politik schweiz', 'politik ch') then 'politik schweiz'
        else lower({{column_name}})
    end
    {% endmacro %}
    ```
* **Usage**: Called in `core_dim_page_changelog` and any other model needing this field.
    ```sql
    select
        {{ clean_ressort('source.department_raw') }} as ressort
    from source
    ```
* **Impact**: Change the rule in one file, update the entire warehouse.

### 2. Utility Macros
Technical logic that is repetitive but not business-specific.

**Example: `calculate_sk_for_id`**
* **Goal**: Safe hashing for Surrogate Keys.
* **Logic**: Handle NULLs -> Default Value -> Hash.
* **Usage**: Used in every single Fact table to generate join keys.

### 3. Package Macros
We also leverage macros from external packages (`dbt_utils`, `snowplow_utils`).
* **Example**: `snowplow_utils.combine_column_versions`
* **Problem**: Snowplow schema evolves. You might have `context_v1`, `context_v2`, `context_v3`.
* **Macro Solution**: This macro coalesces them automatically into a single column, extracting the data from whichever version is present.

## Best Practices for Macros
1.  **Arguments**: Make column names arguments, so the macro can be applied to any table.
2.  **Modularity**: One macro, one specific job.
3.  **Documentation**: Macros should be self-explanatory or commented, as they abstract away the underlying SQL.

