# DevOps Forge Gadgets

A single home for **Atlassian Forge** apps — Jira Cloud
dashboard gadgets created by me that power the *DevOps/SRE Team Workload Metrics* dashboard.

Each app lives in **its own repository** and is included here as a **git
submodule**, so this umbrella keeps everything in one place while each app keeps
its own history, issues, and releases.

## Apps

| Submodule | What it does | Repository |
|---|---|---|
| [`devops-cycle-time-gadget`](https://github.com/2victor2/devops-cycle-time-gadget) | Ready→resolved cycle time per assignee/priority/request type, split into Wait (First Response SLA) + Execution (Time to Resolution SLA) | `2victor2/devops-cycle-time-gadget` |
| [`devops-workload-index`](https://github.com/2victor2/devops-workload-index) | Priority- and request-type-weighted workload band per engineer and for the team | `2victor2/devops-workload-index` |
| [`devops-request-type-gadget`](https://github.com/2victor2/devops-request-type-gadget) | Breakdown of work by JSM request type | `2victor2/devops-request-type-gadget` |

All three are Forge `jira:dashboardGadget` apps: auth `asApp`, scope
`read:jira-work` only, no external egress. Site-specific config (filter id, field
ids) is supplied via Forge environment variables — see each app's own README and
`.env.example`.

## Cloning

Clone with submodules in one step:

```bash
git clone --recurse-submodules git@github.com:2victor2/devops-forge-gadgets.git
```

Already cloned without `--recurse-submodules`? Populate them:

```bash
git submodule update --init --recursive
```

## Working on an app

Submodules check out a **specific pinned commit** (detached HEAD). To make
changes, switch the submodule to its branch first:

```bash
cd devops-cycle-time-gadget
git checkout main
# …edit, commit, push as normal — this pushes to the app's own repo…
```

Then record the new pin in the umbrella:

```bash
cd ..
git add devops-cycle-time-gadget
git commit -m "Bump devops-cycle-time-gadget"
git push
```

## Updating pins to the latest of each app

```bash
git submodule update --remote --merge
git commit -am "Bump submodules to latest"
```

## Per-app setup / deploy

Each submodule is a complete Forge app. From inside it:

```bash
npm install
cp .env.example .env            # fill in APP_ID + field/filter ids
source scripts/forge-env.sh --set-variables
forge deploy -e production
```

See the app's own `README.md` for full details.

## License

Each app is licensed under the terms in its own repository
(GNU GPL v3.0). This umbrella simply references them.
