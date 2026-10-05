# Team Cluster Definitions

This directory contains declarative self-service EKS cluster definitions for teams.

## Structure

```
teams/
  <team>/
    clusters/
      <name>-<env>.yaml
```

Each team maintains its cluster requests in its own directory:
- `<team>`: The lowercase DNS name of your squad (e.g. `team-data`).
- `<name>`: The workload cluster name prefix (e.g. `analytics`).
- `<env>`: The environment (`dev`, `test`, or `prod`).

## File Format

Example: `teams/team-data/clusters/analytics-dev.yaml`
```yaml
team: team-data            # must match the directory name teams/team-data/
name: analytics            # must match filename prefix <name>-<env>.yaml
env: dev                   # dev | test | prod
kubernetesVersion: "1.33"  # allowed: 1.32, 1.33, 1.34
network: platform-default  # tier-1 network owned and managed by the platform
nodeGroup:
  instanceType: t3.medium  # allowed: t3.medium, t3.large, m5.large
  minSize: 1
  desiredSize: 2
  maxSize: 3               # <= 3 for dev/test, <= 5 for prod
```

## Guardrails & Governance
- **Filename convention**: The file must be named `<name>-<env>.yaml`.
- **Team isolation**: A team cannot claim clusters on behalf of another team.
- **Relational sizing**: Sizing must satisfy `1 <= minSize <= desiredSize <= maxSize`.
- **Production governance**: Claims targeting `prod` (`*-prod.yaml`) request review from platform owner `@brunobml` via CODEOWNERS.
