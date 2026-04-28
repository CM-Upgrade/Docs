---
title: Getting Started
weight: 1
next: /configuration
prev: /
---

Install and run **{{< param "productName" >}}** in just a few minutes.

## Prerequisites

- A supported package manager (npm, pip, go modules, or cargo)
- Git ≥ 2.30
- Read/write access to your repository

## Install

{{< tabs items="macOS,Linux,Windows" >}}

  {{< tab >}}
  ```bash
  brew install upgrademate
  ```
  {{< /tab >}}

  {{< tab >}}
  ```bash
  curl -sSL https://get.upgrademate.example | bash
  ```
  {{< /tab >}}

  {{< tab >}}
  ```powershell
  winget install UpgradeMate.UpgradeMate
  ```
  {{< /tab >}}

{{< /tabs >}}

## First run

```bash
upgrademate init
upgrademate scan
upgrademate upgrade --dry-run
```

{{< callout type="info" >}}
  The `--dry-run` flag shows what *would* change without modifying any files —
  perfect for your first run.
{{< /callout >}}

## Next steps

- Configure {{< param "productName" >}} via [`upgrademate.yaml`](/configuration)
