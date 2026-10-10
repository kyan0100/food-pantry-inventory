# Requirements Backlog: Food Pantry Inventory Tracker

Initial pass of requirements elicitation. Product: a simple web tool that lets small food pantries log donations, see current stock, flag low-stock and expiring items, and produce a weekly "needs" list for donors.

**Workflow:** Kanban. The backlog is a prioritized pull queue with no sprints. Priority uses MoSCoW (Must / Should / Could / Won't for v1). Release tags: `v1` is a single-pantry web app; `later` covers barcode scanning, multi-site, and donor accounts.

## Stakeholders (elicited)

| Stakeholder | Interest |
|---|---|
| Volunteers | Quickly log what arrives and what goes out. Often non-technical, on phones, working short shifts. |
| Pantry coordinators | See stock at a glance, catch expiring food, plan what to request. Stretched thin. |
| Donors | Know what the pantry actually needs. |
| Pantry clients (indirect) | Receive food that is available, safe and not expired. |
| Developer / course instructor | Scope stays achievable for a term project. |

## Functional Requirements

| ID | Title / User Story | Priority | Release | Source | Depends On | Acceptance Criteria |
|---|---|---|---|---|---|---|
| FR-01 | **Sign in.** As a user, I sign in so only authorized people change inventory. | Must | v1 | Shared spreadsheet has no access control | none | Valid credentials grant access; invalid are rejected; sessions expire. |
| FR-02 | **Roles.** As a pantry, I want volunteer and coordinator roles so coordinators control settings and volunteers do data entry. | Must | v1 | Coordinators and volunteers have different needs | FR-01 | Volunteers cannot edit thresholds or manage users; coordinators can. |
| FR-03 | **Item catalog.** As a coordinator, I maintain a list of standard items and categories so entries are consistent. | Must | v1 | Free-text spreadsheet entries are inconsistent | FR-02 | Items have name, category, and unit; duplicates are warned against. |
| FR-04 | **Log donation.** As a volunteer, I log incoming items (item, quantity, unit, expiration date, date received) so stock stays accurate. | Must | v1 | Core problem: no reliable record of what is on hand | FR-01, FR-03 | Entry is saved and stock increases; required fields validated; expiration date optional for non-perishables. |
| FR-05 | **Log distribution.** As a volunteer, I record items going out so stock decreases. | Must | v1 | Stock must reflect reality, not just intake | FR-04 | Quantity cannot go below zero; stock updates immediately. |
| FR-06 | **View current stock.** As a coordinator, I see current quantities per item. | Must | v1 | Volunteers don't know what's running low | FR-04, FR-05 | List shows item, quantity, nearest expiration; reflects latest entries. |
| FR-07 | **Low-stock thresholds.** As a coordinator, I set a minimum quantity per item or category. | Must | v1 | Need a definition of "running low" | FR-03, FR-02 | Threshold is saved per item; default value exists. |
| FR-08 | **Low-stock tag.** As a coordinator, I see items below threshold tagged "low stock." | Must | v1 | Stated need: tags for low-stock items | FR-06, FR-07 | Tag appears when quantity < threshold and clears when restocked. |
| FR-09 | **Expiring-soon tag.** As a coordinator, I see items nearing expiration tagged, with a configurable window (default 14 days). | Must | v1 | Food expires on shelves | FR-04, FR-06 | Items within the window are tagged; expired items are flagged separately. |
| FR-10 | **Weekly needs list.** As a coordinator, I generate a list of items to request from donors, based on low-stock items and target levels. | Must | v1 | Stated need: weekly "needs" list for donors | FR-08 | List includes low-stock items with suggested quantity; coordinator can edit before finalizing. |
| FR-11 | **Share needs list.** As a coordinator, I share the needs list via link, print, or PDF. | Must | v1 | Needs list must reach donors | FR-10 | Output is readable without logging in and does not expose internal data. |
| FR-12 | **Edit / correct entries.** As a volunteer, I fix a mistaken entry. | Should | v1 | Data-entry errors are inevitable | FR-04, FR-05 | Edits update stock correctly; original value retained in history. |
| FR-13 | **Audit log.** As a coordinator, I see who changed what and when. | Should | v1 | Multiple volunteers, accountability | FR-12 | Each create/edit/delete records user and timestamp. |
| FR-14 | **Write off expired/damaged items.** As a coordinator, I remove spoiled stock with a reason. | Should | v1 | Food expires on shelves | FR-09, FR-05 | Write-off reduces stock and is recorded with a reason. |
| FR-15 | **Search and filter stock.** As a user, I filter by category, tag, or text. | Should | v1 | Faster use during short shifts | FR-06 | Filters combine; results update without a full page reload. |
| FR-16 | **Coordinator dashboard.** As a coordinator, I see a summary of low-stock count, expiring count, and recent activity. | Should | v1 | Coordinators are spread thin | FR-08, FR-09 | Dashboard loads with current counts matching the stock list. |
| FR-17 | **Import from spreadsheet.** As a coordinator, I import existing inventory from the current shared spreadsheet (CSV). | Should | v1 | Pantry already tracks everything in a spreadsheet | FR-03, FR-04 | Valid rows import; invalid rows are reported with reasons. |
| FR-18 | **Export to CSV.** As a coordinator, I export stock and history. | Could | v1 | Reporting to the pantry's parent organization | FR-06 | CSV opens correctly in common spreadsheet tools. |
| FR-19 | **Low-stock / expiring email alerts.** As a coordinator, I receive a periodic email digest. | Could | later | Reduces need to check the app | FR-08, FR-09 | Digest sent on a configurable schedule. |
| FR-20 | **Barcode scanning.** As a volunteer, I scan items to speed up intake. | Won't (v1) | later | Deferred in A1 scope | FR-04, FR-03 | Out of scope for v1. |
| FR-21 | **Multi-site support.** As an organization, I manage several pantries. | Won't (v1) | later | Deferred in A1 scope | FR-02, FR-06 | Out of scope for v1. |
| FR-22 | **Donor accounts.** As a donor, I view needs and pledge donations. | Won't (v1) | later | Deferred in A1 scope | FR-10, FR-11 | Out of scope for v1. |

## Non-Functional Requirements

| ID | Category | Requirement | Priority | Release | Depends On |
|---|---|---|---|---|---|
| NFR-01 | Usability | A new volunteer can log a donation within 5 minutes of first use and without training. Logging one item takes under 60 seconds. | Must | v1 | FR-04 |
| NFR-02 | Usability | Works on phone, tablet, and desktop browsers (responsive design). | Must | v1 | none |
| NFR-03 | Accessibility | Meets WCAG 2.1 AA for core flows. | Should | v1 | NFR-02 |
| NFR-04 | Reliability | Concurrent volunteers editing at once do not overwrite each other. | Must | v1 | FR-04, FR-05 |
| NFR-05 | Reliability | Automated daily backups; data restorable. | Must | v1 | none |
| NFR-06 | Security | Passwords are hashed; all traffic over HTTPS; role checks enforced server-side. | Must | v1 | FR-01, FR-02 |
| NFR-07 | Privacy | The system stores inventory only. No personal data about pantry clients is collected. | Must | v1 | none |
| NFR-08 | Performance | Stock list loads in under 2 seconds for up to 5,000 item records. | Should | v1 | FR-06 |
| NFR-09 | Cost | Runs on free or low-cost hosting, since pantries have minimal budgets. | Must | v1 | none |
| NFR-10 | Maintainability | Documented setup so a future volunteer-developer can maintain it. | Should | v1 | none |

## Dependency Summary

- **Foundation:** FR-01 → FR-02 → FR-03 → FR-04 → FR-05 → FR-06
- **Alerts:** FR-06 + FR-07 → FR-08; FR-06 → FR-09
- **Donor-facing:** FR-08 → FR-10 → FR-11
- **Quality-of-life:** FR-12 → FR-13; FR-09 → FR-14; FR-08 + FR-09 → FR-16
- **Critical path for first usable release:** FR-01, 02, 03, 04, 05, 06, 07, 08, 09, 10, 11

## Assumptions and Open Questions (for next elicitation round)

1. What unit conventions do pantries use (cases, pounds, individual items)?
2. Do pantries need lot-level tracking (several expiration dates for the same item)? Current design assumes entries by batch.
3. Who decides target stock levels: coordinators only, or in consultation with donors?
4. How do volunteers currently access the spreadsheet (personal devices, shared computer)?
5. Does the pantry's parent organization have reporting requirements?
