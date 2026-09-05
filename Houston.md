# Houston

Houston is a Git-controlled Markdown administrative layer for orchestrating
tasks across existing systems of record.

## Solution

### Principles

* Principals own permissions. A principal is an identity / permission bundle.
* Domains are areas of concern governed by one or more principals.
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

### Vocabulary

* **Task:** a unit of work.
  * Alternates: actionable, pender, DoBit
* **Task source:** a system of record for pending pieces of work

### Artifacts

* Ledger/log/history: read-only except for reformatting
* Values
* Goals
* Projects

## Task Sources

### Professional

#### Apple Notes ✅ Full read/write confirmed — Priority

**Capabilities:**

- **Read:** find any note by title, search by keyword, list all note titles
- **Read:** read full text content of any note
- **Read:** identify checklist items and their checked/unchecked state
- **Edit:** reorder checklist items (e.g. move completed items to bottom)
- **Edit:** mark items done or undone
- **Edit:** create new notes, append or rewrite note body

**Access method:** Direct SQLite + protobuf — requires VS Code Full Disk Access.

**Setup required (one-time):** System Settings → Privacy & Security → Full Disk Access → add **Visual Studio Code**. Without this, all reads/writes fail with `authorization denied`.

**Implementation notes:**

- DB path: `~/Library/Group Containers/group.com.apple.notes/NoteStore.sqlite`
- Note record: `ZICCLOUDSYNCINGOBJECT` — `ZISPINNED`, `ZTITLE`, `ZHASCHECKLIST`, `ZNOTEDATA` (FK)
- Note data: `ZICNOTEDATA.ZDATA` — gzip-compressed protobuf
- Protobuf structure: outer → field 2 (NoteData) → field 3 (TextData) → field 2 (text string UTF-8), field 5 repeated (attribute runs)
- Checklist items: attribute run field 2 (paragraph style) → field 1 = 103 means checklist item; field 5 (ChecklistItem) → field 2 = `isDone` (0/1), field 1 = UUID bytes
- All attribute runs use **Unicode code point counts** for length (not UTF-8 bytes)
- To edit: modify `ZDATA` + bump `ZMODIFICATIONDATE` on `ZICCLOUDSYNCINGOBJECT` to Apple epoch time (`time.time() - 978307200`)
- Must quit + reopen Notes for DB changes to take effect (`osascript -e 'tell application "Notes" to quit'`)
- Always backup DB before writes: `shutil.copy2(DB, "/tmp/NoteStore_backup_YYYYMMDD.sqlite")`
- AppleScript `body` property: readable without FDA, but **strips all checklist state** — useless for checked/unchecked

**Known limitations:**

- Encrypted notes (`ZISPASSWORDPROTECTED=1`): content is not readable
- Notes stored only in iCloud (`ZNEEDSTOBEFETCHEDFROMCLOUD=1`): ZDATA may be empty until synced locally
- Rich content (images, drawings, handwriting): present as attachment references, not modifiable via this method

---

#### Outlook Tasks (Microsoft To Do) — Priority

**Capabilities:**

- **Read:** list all task lists and every task (title, due date, status, notes)
- **Read:** filter by status (not started, in progress, completed), due date, importance
- **Edit:** create tasks with title, due date, body, reminder
- **Edit:** mark complete, update due date, modify body
- **Edit:** move between lists, delete tasks

**Access method:** `msgraph` skill via MS Graph API — requires valid Microsoft 365 session.

---

#### Outlook Calendar — Priority

#### Outlook Email — Priority

#### Slack: Later

#### Pull Request Comments

#### Jira

#### Task Lists in Current Project Docs

---

### Personal

#### Google Tasks — Priority

#### Gmail — Priority

#### Google Calendar — Priority

#### Google Keep — Priority

#### Google Chat

#### SMS

#### Discord

#### LinkedIn Messages

#### YouTube Playlists

#### Twitter Bookmarks

---

### Both (Work & Personal)

#### Browser Tabs — Priority

#### Downloads Folder — Priority

#### Desktop Files — Priority

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

### Notable AI-Powered Personal Admin Solutions
