# Food Pantry Inventory Tracker: Requirements Backlog

Term project for a software engineering course. This repository contains the initial requirements backlog for a web tool that helps small food pantries track donations, flag low-stock and expiring items, and generate a weekly needs list for donors.

- **Backlog:** [BACKLOG.md](BACKLOG.md) (22 functional and 10 non-functional requirements, tagged with priority, release, source, and dependencies)
- **Process:** Kanban, with a prioritized pull-based backlog.

## How this was produced

The backlog was generated with a large language model (Claude, by Anthropic) as an initial pass of requirements elicitation. The model was given the problem statement from Assignment 1 and asked to produce requirements with metadata and dependencies.

### Prompt used

> I am building a web-based inventory tracker for small food pantries. Problem: pantries track inventory in a shared spreadsheet, so volunteers don't know what is running low, food expires on shelves, and coordinators aren't on the same page. Core needs: volunteers log incoming donations (item, quantity, etc.); coordinators see current stock with low-stock and expiring-soon tags; the system generates a weekly "needs" list to share with donors. Version 1 serves one pantry through a simple web interface. Barcode scanning, multi-site support, and donor accounts come later, if at all.
>
> Act as a requirements analyst performing an initial elicitation pass. Produce a backlog of functional and non-functional requirements. For each, include a unique ID, user story, MoSCoW priority, target release, source/rationale, dependencies on other requirement IDs, and acceptance criteria. List stakeholders, a dependency summary, and open questions for the next elicitation round.

The output was reviewed and edited for relevance to the project scope.
