# Dashboard Platform

The shared foundation for a family of data-heavy React dashboards served from BigQuery-derived tables on Google Cloud, and the vocabulary for talking about it.

## Language

**Platform**:
The monorepo and everything shared in it: standards, tooling, the API, infrastructure.
_Avoid_: Mother, mother repo, core

**Dashboard**:
One React application for one audience, living in the Platform and following its standards.
_Avoid_: Daughter, app, report

**Source**:
The BigQuery project and datasets that hold the final tables Dashboards are built from.
_Avoid_: Warehouse, upstream

**Serving store**:
The PostgreSQL copy of final tables that Dashboards read from through the API.
_Avoid_: Cache DB, replica, Postgres (when meaning the role rather than the product)

**Table sync**:
One configured copy of a Source table into the Serving store, with its own settings (load strategy, Sync schedule, size).
_Avoid_: Replication, pipeline, load, ETL job

**Sync schedule**:
A named cadence that Table syncs run on; one is the default for any Table sync that names none.
_Avoid_: Trigger, cron, frequency

**Sync stamp**:
The time a table's most recent successful Table sync finished; changes exactly when that table's served data changes.
_Avoid_: Dataset version, table version, data version, snapshot id

**Measure**:
A named business quantity with exactly one definition for the whole Platform, calculated in the Source.
_Avoid_: Metric, KPI, figure, calculated field

**Grant**:
A user's permission to use one Dashboard in one environment.
_Avoid_: Role, permission, entitlement

**Grant event**:
A permanent record that a Grant was given or revoked, by whom and when.
_Avoid_: Audit row, access change, permission log

## Layers

Seven layers that a request or the data passes through, in order from the viewer to the Source.

**Frontend layer**:
What the viewer sees and touches: the Dashboards.
_Avoid_: UI layer, client

**Contract layer**:
The agreed shape of what the Frontend layer may ask for and what comes back, including what each Measure means.
_Avoid_: API schema, interface layer

**Access layer**:
Everything that decides who may open which Dashboard, and remembers how that changed.
_Avoid_: Auth layer, security layer

**Serving layer**:
What answers a Dashboard's questions from the Serving store. It adds no business calculation.
_Avoid_: Infra layer, backend, API layer

**Storage layer**:
Where the served copy of the data lives: the Serving store.
_Avoid_: Database layer, persistence

**Sync layer**:
What carries tables from the Source into the Storage layer: the Table syncs.
_Avoid_: Ingestion, ETL layer, replication layer

**Analytics layer**:
Where the numbers are produced: the Source. Measures are calculated here and nowhere else.
_Avoid_: Warehouse layer, data layer

Four layers that apply to all of the above.

**Standards layer**:
The shared rules and building blocks that make every Dashboard look and behave alike.
_Avoid_: Mother, common, shared code

**Infrastructure layer**:
The cloud resources and environments everything runs in.
_Avoid_: Infra layer (when meaning the Serving layer), platform ops

**Delivery layer**:
How a change travels from a pull request to dev, QA and prod.
_Avoid_: CI/CD, pipeline, deployment

**Observability layer**:
How the Platform's health and its use are recorded, alerted on and kept.
_Avoid_: Monitoring, logging, audit layer
