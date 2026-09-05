# Houston

Houston is a Git-controlled Markdown administrative layer for orchestrating
tasks across existing systems of record.

## Solution

### Vocabulary

* **Task:** a unit of work.
  * Alternates: actionable, pender, DoBit
* **Task source:** a system of record for pending pieces of work
* **Principal:** an identity / permission bundle that owns permissions.
* **Domain:** an area of concern governed by one or more principals.


### Principles

* Task sources belong to domains and have access rules.
* The source buckets below are organizational groupings, not domains; each
  source is assigned to one or more domains.
* Primary task sources are the sources of truth for actual tasks.
* Human controls admin via markdown edits (e.g via voice interaction)
* Admin controls human by operating on task sources
* Admin has no state other than git-controlled markdown artifacts

### Domains

* Personal
* Family / HoltzLusin
* Walmart
* Purissima
* Libertarian

### Artifacts

* Ledger/log/history: read-only except for reformatting
* Values
* Goals
* Projects

## Task Sources

### Professional

#### Apple Notes

See [Notes.md](Notes.md) for integration details.

#### Outlook Tasks

See [Notes.md](Notes.md) for integration details.

#### Outlook Calendar

#### Outlook Email

#### Slack: Later

#### Pull Request Comments

#### Jira

#### Task Lists in Current Project Docs

### Personal

#### Google Tasks

#### Gmail

#### Google Calendar

#### Google Keep

#### Google Chat

#### SMS

#### Discord

#### LinkedIn Messages

#### YouTube Playlists

#### Twitter Bookmarks

### Both (Work & Personal)

#### Browser Tabs

#### Downloads Folder

#### Desktop Files

## Existing Solutions

### Existing Personal OS Landscape

* **Akiflow MCP:** closest current agent-facing product; exposes task and
  calendar operations to Claude, ChatGPT, Codex, and other MCP clients, but
  Akiflow remains an operational task cockpit rather than a neutral
  Git/Markdown administrative layer.
* **Morgen:** closest source-preserving planner pattern; imports tasks from
  external tools such as Google Tasks and Microsoft To Do, and completion can
  sync back to the source, but it is not an agentic Houston-style control
  plane.
* **Routine:** emerging AI/MCP control surface; its local MCP server connects
  desktop AI clients such as ChatGPT, Claude, Codex, and Cursor, but it
  remains centered on Routine's own workspace.
* **LifeOS / Obsidian-style personal OS projects:** valuable design
  references, but mismatched because they tend to make the vault the
  operational center; LifeOS explicitly describes Obsidian-backed tasks.
* **Conclusion:** no existing product matches Houston cleanly. The right
  near-term artifact is a Git-controlled admin layer plus playbooks and
  source-specific control surfaces.

### Control Surfaces

Preferred order:

* Official APIs and built-in connectors.
* Hosted MCP/plugin connectors.
* Local OS automation.
* Supervised browser/app-driving agents.

UI automation is allowed as a human-equivalent control surface, but not as
unattended infrastructure.
