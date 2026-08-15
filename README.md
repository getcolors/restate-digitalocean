# restate-digitalocean

Desired state for [https://bigconfig.website](https://bigconfig.website):
Restate 1.7.3 and a TypeScript SDK 1.16.6 workflow application on one Amsterdam
DigitalOcean Droplet.

```sh
./green build
./green create --dry-run
./green create
```

Credentials listed in `colors.yml` live only in ignored `.envrc.private`.
Never set `COLORS_PAR_PROFILE`. OpenTofu discovers the `ams3` default VPC during
apply and does not create or configure a VPC. `compute-prevent-destroy: true`
protects the Droplet and persistent state.

## Operations

```sh
ssh root@SERVER 'cd /opt/restate && docker compose ps'
ssh root@SERVER 'cd /opt/restate && docker compose logs --tail=200 restate app caddy'
ssh root@SERVER 'systemctl list-timers restate-backup.timer'
curl https://bigconfig.website/health
```

Start and inspect a deterministic workflow:

```sh
curl -X POST -H 'content-type: application/json' -d '{"value":7}' \
  https://bigconfig.website/workflows/example-1
curl https://bigconfig.website/workflows/example-1
```

The service durably sleeps, intentionally fails twice, succeeds on activity
attempt three, and returns 49 with a deterministic verification hash. Repeating
the POST safely addresses the same Restate workflow ID.

## Backup, restore and upgrades

The daily timer briefly stops app and Restate, archives both persistent state
directories and uploads the archive to R2. Check timer and service logs; silence
does not prove backup success. To restore, provision an isolated replacement,
stop Compose, extract one archive under `/var/lib`, preserve node name
`restate-1`, and start Compose. Validate workflows before changing DNS.

Before an upgrade, verify a current backup, update exact server/SDK/image pins
upstream, inspect golden output, run all tests/build/dry-run, and converge.
Single-node operation provides restart durability but no high availability.
Droplet or regional failure causes downtime; backup RPO is up to one day and
restoration is manual.
