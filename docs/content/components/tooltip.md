---
title: Tooltip
description: A popup that displays information related to an element when the element receives keyboard focus or the mouse hovers over it.
component: true
links:
  source: https://github.com/huntabyte/shadcn-svelte/tree/next/sites/docs/src/lib/registry/ui/tooltip
  doc: https://bits-ui.com/docs/components/tooltip
  api: https://bits-ui.com/docs/components/tooltip#api-reference
---

<script>
	import ComponentPreview from "$lib/components/component-preview.svelte";
	import PMAddComp from "$lib/components/pm-add-comp.svelte";
	import PMInstall from "$lib/components/pm-install.svelte";
	import Steps from "$lib/components/steps.svelte";
	import Step from "$lib/components/step.svelte";
	import InstallTabs from "$lib/components/install-tabs.svelte";
</script>

<ComponentPreview name="tooltip-demo">

<div></div>

</ComponentPreview>

## Installation

<InstallTabs>
{#snippet cli()}
<PMAddComp name="tooltip" />
{/snippet}
{#snippet manual()}
<Steps>

<Step>

Install `bits-ui`:

</Step>

<PMInstall command="bits-ui -D" />

<Step>

Copy and paste the component source files linked at the top of this page into your project.

</Step>

</Steps>
{/snippet}
</InstallTabs>

## Usage

```svelte
<script lang="ts">
  import * as Tooltip from "$lib/components/ui/tooltip/index.js";
</script>

<Tooltip.Provider>
  <Tooltip.Root>
    <Tooltip.Trigger>Hover</Tooltip.Trigger>
    <Tooltip.Content>
      <p>Add to library</p>
    </Tooltip.Content>
  </Tooltip.Root>
</Tooltip.Provider>
```
