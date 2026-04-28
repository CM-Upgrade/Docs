---
title: Configuration
weight: 2
prev: /getting-started
---

Configure **{{< param "productName" >}}** with a single `upgrademate.yaml`
file at the root of your repository.

## Example

```yaml
# upgrademate.yaml
schedule: "weekly"
strategy: "minor"          # patch | minor | major
ignore:
  - "react@18"             # pin to current major
reviewers:
  - "@platform-team"
notifications:
  slack: "#deploys"
```

## Options

| Key             | Type     | Default   | Description                              |
| --------------- | -------- | --------- | ---------------------------------------- |
| `schedule`      | string   | `weekly`  | How often to scan for upgrades.          |
| `strategy`      | string   | `minor`   | Maximum semver bump to apply.            |
| `ignore`        | string[] | `[]`      | Packages or version ranges to skip.      |
| `reviewers`     | string[] | `[]`      | GitHub handles to request review from.   |
| `notifications` | object   | `{}`      | Where to post upgrade summaries.         |

{{< callout type="warning" >}}
  Setting `strategy: major` can introduce breaking changes. Pair it with a
  strong CI suite and the `--dry-run` flag.
{{< /callout >}}
