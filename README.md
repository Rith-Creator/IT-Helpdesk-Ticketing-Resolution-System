# IT Helpdesk Ticketing & Resolution System

A self-built helpdesk ticketing system with randomized ticket generation and a multiple-choice resolution workflow, built to practice IT support triage and troubleshooting reasoning across common end-user issues.

## Overview

This project simulates a small company's helpdesk queue — tickets are generated automatically with realistic symptom descriptions across networking, hardware, and account-access issues, weighted toward network problems (Wi-Fi, VPN, slow internet). Each ticket is resolved by selecting the correct fix from four multiple-choice options, and the system automatically grades the answer and tracks the outcome.

The same backend (SQLite database + ticket generator) powers two separate interfaces: a command-line tool and a Flask web app, to practice building one shared data layer with multiple front-ends on top of it.

**Environment:**
- Language: Python 3
- Web framework: Flask
- Database: SQLite
- Frontend: HTML, Jinja2 templates, custom CSS

## What's included

### Ticket generation
- Randomized tickets pulled from a weighted category pool: **Network (50%)**, **Hardware (25%)**, **Account-Access (25%)**
- Each ticket's symptom description is built from a template with randomized details (device type, OS, department) so no two tickets read identically
- Severity (Low/Medium/High/Critical) assigned at random per ticket

### Multiple-choice resolution
- Every ticket comes with 4 possible fixes: the correct resolution plus 3 distractors pulled from other real fixes in the same category, so wrong answers still read as plausible IT troubleshooting steps
- Submitting an answer automatically grades it against the correct fix and records whether it was right or wrong
- Resolved tickets show the selected answer and correct answer side by side for review

### Two interfaces, one backend

| Interface | File | Description |
|---|---|---|
| CLI | `cli.py` | Menu-driven terminal tool: view open tickets, generate new ones, work a ticket, view resolution history |
| Web app | `app.py` | Flask app serving the same ticket queue in a browser at `localhost:5000`, with a clickable ticket list and radio-button resolution form |

Both interfaces call the same two backend modules and never touch the database or ticket-generation logic directly:

| Module | Purpose |
|---|---|
| `db.py` | Only file that talks to SQLite — handles all table creation, inserts, queries, and grading logic |
| `generator.py` | Builds randomized ticket content: category, symptom description, severity, and the 4 answer choices |

### Verified working end-to-end
Tested through both interfaces: generating tickets, viewing the open queue, selecting a resolution, and confirming the system correctly marks answers as Correct or Incorrect and persists that result back to the database — visible from either the CLI or the web app, since both read from the same `tickets.db` file.

## Skills demonstrated

- Python application structure: separating data layer, business logic, and interface into distinct modules
- SQLite database design and querying (schema design, CRUD operations)
- Flask web development: routing, Jinja2 templating, form handling, redirects
- Randomized content generation with weighted probability
- Basic frontend development: semantic HTML, custom CSS, responsive layout
- Designing a self-grading assessment/workflow tool

## Demo

Short walkthrough of the app in action — generating a batch of tickets, opening one, selecting a resolution, and seeing it graded.

![Demo](screenshots/demo.gif)

*(Or, if hosted elsewhere: [Watch the demo video](your-video-link-here))*

## Screenshots

### Ticket Queue
![Ticket Queue](screenshots/ticket-queue.png)

### Ticket Detail & Resolution
![Ticket Detail](screenshots/ticket-detail.png)

### Resolved Ticket (Graded)
![Resolved Ticket](screenshots/resolved-ticket.png)

## Notes

This is a learning/portfolio project, not a production ticketing platform — there's no authentication, multi-user support, or persistent hosting, and the resolutions are pre-written multiple-choice options rather than open-ended troubleshooting against a real broken system.
