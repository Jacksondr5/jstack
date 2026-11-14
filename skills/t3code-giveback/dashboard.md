# The giveback dashboard

https://j5-giveback.lakebed.app is the **source of truth** for every giveback item: its stage, what it waits on, its upstream links, its asks of Jackson, and its history. It replaced the prose in `Jacksondr5/j5code#276` on 2026-11-11. The site is public and read-only. Agents write to it over HTTP.

- App repo: (`Jacksondr5/j5-giveback`, public). It is owned by agents in the J5 project **j5-giveback**. Bugs and feature requests for the app go to that project. If you find an issue, raise it to Jackson and he'll have you spawn an agent there to handle it.  This project uses the app and does not change it.
- **Read `j5-giveback/AGENT_API.md` in the j5-giveback repo before your first write.** It is the full contract: the model, stages, enumerations, limits and errors. This page covers only what an agent here gets wrong without it.

## The CLI

```sh
GB=~/[path/to]/j5-giveback/scripts/giveback
$GB show GB-12          # one item: links, asks, events, private fields
$GB export --out <scratchpad>/export.json   # everything
$GB watchlist           # every link on an item that is not closed
$GB agents              # every item that is not closed, with its agent, and the dispatcher's address
$GB upsert item.json    # create or change items; omitted fields keep their value
$GB ask open|close ...  # asks of Jackson
$GB event add ...       # history lines
```

It reads its URL and secrets from `j5-giveback/env` on its own. Never print, copy, or pass that file's values as arguments. If the file is missing, stop and tell Jackson. Do not ask him to paste a secret.

## Rules that bite

- **Read state from the dashboard and never from memory.** Before you act on an item, `show` it. Another agent may have moved it since you last looked.
- **Name an item by its ID and its title**: "GB-30, the Merge back tooltip". The ID alone is not enough, and the slug is not a name for people.
- **The site is public.** `isPublic: false` for anything security- or permission-sensitive until it is filed upstream. In any field, public or private, write no local paths, host names, ports, thread ids, token locations, or commentary about maintainers. The one exception is `private.agentAddress`, which holds the address of the item's agent. A summary is one or two plain sentences that a stranger can read.
- **Date-only times go at `T12:00:00Z`.** Midnight UTC shows as the day before in US browsers. Send `stageEventAt` when you record something that happened earlier.
- **Batch writes.** Lakebed allows 1,000 writes a day across the whole app. Use one upsert for many items, and avoid one request per field.
- **Asks are how Jackson's decisions are tracked.** Open an ask when something waits on him, give it a `private.recommendation`, and close it with a `resolution` when he answers. Set `waitingOn: jackson` only while an ask is open.
- **An item's agent is recorded on it**, in `private.agentName` and `private.agentAddress`. Set both in one upsert when you take an item, and again if the item moves to another agent. `giveback agents` reads them back, and the watcher sends to the address.
- **`held` is a side stop**, for a valid item waiting on someone else's upstream PR. Give it a `covering` or `base` link. It is never a way to park an item that waits on Jackson.
- **The refresh never moves a stage.** When a link merges, closes or starts failing CI, the item gets an "upstream changed" marker. Decide the new stage, and send `clearUpstreamChange: true` with that change.

## Background jobs (systemd user timers)

| Timer | What it does |
|---|---|
| `giveback-refresh.timer` | Every 15 min: `giveback refresh` (GitHub state onto links) |
| `giveback-backup.timer` | Daily backup export of the data |
| `giveback-watch.timer` | Every 15 min: checks the upstream threads of every item that is not closed, and messages the item's agent about new activity with `j5 a2a send`. An item with no recorded agent goes to the dispatcher. |
