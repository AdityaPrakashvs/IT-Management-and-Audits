# Agile Sprint Simulator

**A hands-on Python toolkit for simulating Scrum sprints from planning to retrospective.**

Built by Dr Amit Dua, to help teams internalize Scrum through practice rather than theory.

---

## What Is This?

The Agile Sprint Simulator lets you experience a complete Scrum sprint cycle without needing a live project. It models every key ceremony and artifact:

- **Sprint Planning** -- Select stories, estimate effort, assign tasks to a cross-functional team
- **Daily Standups** -- Simulated progress reports with realistic blockers and productivity variance
- **Burndown Charts** -- Real-time visualization of remaining work vs. the ideal trajectory
- **Kanban Board** -- Terminal-based board showing stories flowing through To Do, In Progress, In Review, Done, and Blocked columns
- **Sprint Retrospective** -- Automated analysis of what went well, what to improve, and concrete action items
- **Reports** -- Generate polished Markdown and HTML sprint summaries

The simulator uses a **mobile banking app** as its sample domain, with 20 realistic user stories covering authentication, payments, analytics, accessibility, and more.

---

## Why Agile / Scrum?

Scrum is the most widely adopted Agile framework in software development. At its core:

- **Iterative delivery** -- Work is done in short, time-boxed sprints (typically 2 weeks)
- **Cross-functional teams** -- Small teams with all the skills needed to deliver working software
- **Empirical process control** -- Inspect and adapt based on real data, not guesses
- **Customer collaboration** -- Continuous feedback from stakeholders shapes the product

### The Scrum Framework at a Glance

| Element | Description |
|---------|-------------|
| **Product Backlog** | Prioritized list of everything the product needs |
| **Sprint Backlog** | Subset of stories the team commits to for one sprint |
| **Sprint Planning** | Ceremony where the team selects and plans work |
| **Daily Scrum** | 15-minute daily sync on progress and blockers |
| **Sprint Review** | Demo of completed work to stakeholders |
| **Sprint Retrospective** | Team reflects on process and identifies improvements |
| **Increment** | The working product delivered at sprint end |

---

## Features

| Feature | Description |
|---------|-------------|
| Full Sprint Simulation | Runs a complete sprint from planning through retrospective |
| CLI Interface | Click-based commands: `init`, `plan`, `run`, `retro`, `report`, `kanban`, `burndown` |
| Realistic Backlog | 20 user stories for a mobile banking app with tasks and acceptance criteria |
| Configurable Team | 5-member team with roles, skills, and experience levels |
| Multiple Prioritization | MoSCoW, value-based, WSJF, or random backlog ordering |
| Task Auto-Generation | Automatic task breakdown based on story points |
| Skill-Based Assignment | Tasks assigned to team members based on skill match and capacity |
| Daily Standup Simulation | Random progress, blockers, and natural language standup messages |
| Burndown Charts | Matplotlib-rendered burndown with ideal vs. actual lines and trend analysis |
| Terminal Kanban Board | Rich-powered Kanban board with color-coded columns |
| Sprint Retrospective | Automated metrics, insights, health score, and happiness index |
| Report Generation | Markdown and styled HTML reports with all sprint data |
| Reproducible Runs | Optional random seed for deterministic simulations |
| Comprehensive Tests | pytest test suite for sprint planning and burndown calculations |

---

## Installation

### Prerequisites

- Python 3.9 or higher
- pip package manager

### Steps

```bash
# Clone or download the project
cd agile-sprint-simulator

# Create a virtual environment (recommended)
python -m venv venv
source venv/bin/activate      # Linux/Mac
venv\Scripts\activate         # Windows

# Install dependencies
pip install -r requirements.txt
```

### Verify Installation

```bash
# Run the test suite
python -m pytest tests/ -v

# Check the CLI
python -m src.cli --version
```

---

## Quick Start

### Run a Full Simulation

```bash
python -m src.cli run --backlog data/sample_backlog.json --team data/team.json
```

This executes the complete cycle: loads data, runs sprint planning, simulates 10 days of standups, generates a burndown chart, runs the retrospective, and produces reports in the `output/` directory.

### Individual Commands

```bash
# Sprint Planning only
python -m src.cli plan --backlog data/sample_backlog.json --team data/team.json

# Run with custom settings
python -m src.cli run --sprint-days 14 --sprint-id "Sprint-3" --goal "Ship checkout flow" --seed 42

# View Kanban board (after running a simulation)
python -m src.cli kanban --sprint-data output/sprint_data.json

# Generate burndown chart
python -m src.cli burndown --sprint-data output/sprint_data.json --metric hours

# Run retrospective
python -m src.cli retro --sprint-data output/sprint_data.json

# Generate reports
python -m src.cli report --sprint-data output/sprint_data.json --format both
```

### Run as a Python Module

```python
from src.simulator import SprintSimulator

sim = SprintSimulator(
    backlog_path="data/sample_backlog.json",
    team_path="data/team.json",
    sprint_days=10,
    sprint_id="Sprint-1",
    sprint_goal="Launch core banking features for beta",
    seed=42,  # reproducible results
)

results = sim.run()
# Output files are saved to output/
```

---

## Sample Walkthrough

Here is what happens when you run a full simulation:

### 1. Data Loading
The simulator loads 20 user stories from `data/sample_backlog.json` and a 5-member team from `data/team.json`. The team is called "Phoenix Squad" and includes a Tech Lead, two Developers, a QA Engineer, and a Designer.

### 2. Sprint Planning
The backlog is prioritized using MoSCoW ordering. The team's velocity is calculated based on member capacity (approximately 40 story points for a 10-day sprint). Stories are selected from the top of the backlog until the velocity budget is exhausted. Each story gets an automatic task breakdown, and tasks are assigned to team members based on skill overlap and available capacity.

### 3. Daily Standups (Days 1-10)
Each day, every team member reports:
- What they worked on yesterday
- What they plan to work on today
- Whether they have any blockers

Progress is applied to tasks based on the member's capacity and experience level. Blockers occur randomly (about 12% chance per member per day) and last 1-3 days, during which productivity drops to 30%.

### 4. Burndown Chart
A burndown chart is generated showing the ideal line (linear from total hours to zero) vs. the actual remaining work. The chart highlights where the team is ahead or behind schedule.

### 5. Retrospective
The retrospective analyzes:
- **Velocity**: Planned vs. actual story points delivered
- **Completion rate**: Percentage of committed work finished
- **Blocker impact**: Number and duration of impediments
- **Member performance**: Individual contribution metrics
- **What went well / What to improve / Action items**: Generated insights based on sprint data
- **Sprint health score**: Composite score out of 100
- **Happiness index**: Simulated per-member satisfaction scores

### 6. Reports
Two reports are generated:
- `output/sprint_report.md` -- Full Markdown report
- `output/sprint_report.html` -- Styled HTML report with charts and tables

---

## Project Structure

```
agile-sprint-simulator/
    src/
        __init__.py           # Package metadata
        models.py             # Core data classes (UserStory, Sprint, Team, Task)
        backlog.py            # Product backlog management and prioritization
        sprint_planner.py     # Sprint planning ceremony simulation
        daily_standup.py      # Daily standup simulation with progress and blockers
        burndown.py           # Burndown chart calculation and matplotlib plotting
        kanban.py             # Terminal Kanban board using Rich
        retrospective.py      # Sprint retrospective analysis and insights
        reporter.py           # Markdown and HTML report generation
        simulator.py          # Main orchestrator for end-to-end simulation
        cli.py                # Click CLI interface
    data/
        sample_backlog.json   # 20 user stories for a mobile banking app
        team.json             # 5-member team definition
    tests/
        __init__.py
        test_planner.py       # Sprint planning tests
        test_burndown.py      # Burndown chart calculation tests
    docs/
        AGILE_GUIDE.md        # Comprehensive Scrum reference
        SIMULATION_WALKTHROUGH.md  # Step-by-step simulation guide
        screenshots/
            SCREENSHOTS.md    # Placeholder for screenshots
    output/                   # Generated reports and charts (gitignored)
    requirements.txt
    .gitignore
    LICENSE
    README.md
```

---

## Glossary

| Term | Definition |
|------|------------|
| **Acceptance Criteria** | Conditions that must be met for a user story to be considered "Done" |
| **Backlog Refinement** | Ongoing activity of adding detail, estimates, and order to product backlog items |
| **Blocker** | An impediment that prevents a team member from making progress |
| **Burndown Chart** | A graph showing remaining work over time during a sprint |
| **Capacity** | The amount of work a team can handle in a sprint, measured in hours |
| **Daily Scrum (Standup)** | A 15-minute daily meeting for the development team to synchronize |
| **Definition of Done** | A shared understanding of what it means for work to be complete |
| **Epic** | A large body of work that can be broken into multiple user stories |
| **Fibonacci Sequence** | The number series (1, 2, 3, 5, 8, 13, 21) used for story point estimation |
| **Focus Factor** | The percentage of theoretical capacity actually available for productive work |
| **Increment** | The sum of all completed product backlog items during a sprint |
| **Kanban Board** | A visual workflow tool showing work items in columns by status |
| **MoSCoW** | Prioritization technique: Must Have, Should Have, Could Have, Won't Have |
| **Product Backlog** | The ordered list of everything known to be needed in the product |
| **Product Owner** | The person responsible for maximizing the value of the product |
| **Scrum Master** | The servant-leader who facilitates Scrum events and removes impediments |
| **Sprint** | A time-boxed iteration (typically 1-4 weeks) in which a product increment is created |
| **Sprint Backlog** | The set of product backlog items selected for the sprint |
| **Sprint Goal** | A short objective for the sprint that provides guidance to the team |
| **Sprint Planning** | The ceremony where the team decides what to work on in the upcoming sprint |
| **Sprint Retrospective** | A meeting where the team inspects itself and creates a plan for improvements |
| **Sprint Review** | A meeting to inspect the increment and adapt the product backlog |
| **Story Points** | A relative measure of the effort required to implement a user story |
| **User Story** | A short description of a feature from the end-user perspective |
| **Velocity** | The amount of work (in story points) a team completes per sprint |
| **WSJF** | Weighted Shortest Job First -- a prioritization technique dividing value by effort |

---

## License

This project is licensed under the MIT License. See [LICENSE](LICENSE) for details.

---

## Author

**Dr Amit Dua**

With over a decade of experience helping software teams adopt Agile practices, I built this simulator as a practical training tool. The best way to learn Scrum is by doing it -- even in a simulated environment.

Feedback and contributions are welcome. Open an issue or submit a pull request.
