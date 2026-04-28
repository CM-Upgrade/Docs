---
linkTitle: "Documentation"
title: Introduction
next: /getting-started
# Apply Hextra's `docs` layout (sidebar + TOC) to every page in the site.
cascade:
  type: docs
---

Welcome to the **{{< param "productName" >}}** documentation!

{{< param "productTagline" >}}

<!-- Branded callout (uses .brand-callout class from assets/css/custom.css) -->
<div class="brand-callout">
  💡 <strong>Tip:</strong> Every reference to <em>{{< param "productName" >}}</em> on this
  site comes from <code>hugo.yaml → params.productName</code>. Change it once,
  rebuild, and the entire site updates.
</div>

## What is UpgradeMate?

{{< param "productName" >}} is a tool that automates dependency upgrades for
modern applications. It opens reviewable pull requests, runs your test suite,
and gives you a single dashboard to track upgrade health across all your
services.

## Next

{{< cards >}}
  {{< card link="getting-started" title="Getting Started" icon="document-text" subtitle="Install and run UpgradeMate in 5 minutes." >}}
  {{< card link="configuration"   title="Configuration"   icon="cog"           subtitle="Tune behavior with upgrademate.yaml." >}}
{{< /cards >}}

## Questions or feedback?

{{< callout emoji="❓" >}}
  {{< param "productName" >}} is under active development.
  Have feedback? [File an issue](https://github.com/upgrademate/docs/issues).
{{< /callout >}}
