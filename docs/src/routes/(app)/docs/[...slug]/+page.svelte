<script lang="ts">
	import { Badge } from "$lib/registry/ui/badge/index.js";
	import { Button } from "$lib/registry/ui/button/index.js";
	import ExternalLinkIcon from "@lucide/svelte/icons/external-link";
	import ArrowLeftIcon from "@lucide/svelte/icons/arrow-left";
	import ArrowRightIcon from "@lucide/svelte/icons/arrow-right";
	import DocsToc from "$lib/components/docs-toc.svelte";
	import { findNeighbors } from "$lib/navigation.js";
	import { page } from "$app/state";
	import Cta from "$lib/components/cta.svelte";
	import ComponentCodeViewer from "$lib/components/component-code-viewer/component-code-viewer.svelte";
	import Metadata from "$lib/components/metadata.svelte";
	import Ethical from "$lib/components/ethical.svelte";

	let { data } = $props();

	const Markdown = $derived(data.component);
	const doc = $derived(data.metadata);
	const apiLink = $derived(doc.links?.api);
	const docLink = $derived(doc.links?.doc);
	const source = $derived(data.viewerData);

	const neighbors = $derived(findNeighbors(page.url.pathname));
</script>

<Metadata
	title={doc.title}
	description={doc.description}
	ogImage={{
		url: `/og?title=${encodeURIComponent(doc.title)}&description=${encodeURIComponent(doc.description)}`,
	}}
	ogType="article"
/>

<!--
NOTE: The TOC needs to come first in the DOM order to prevent CLS, so we flex-row-reverse
the docs container. The issue this resolves is prominent on slow connections (3G).
  -->
<div
	data-slot="docs"
	class="flex flex-row-reverse items-stretch text-[1.05rem] sm:text-[15px] xl:w-full"
>
	<div
		class="sticky top-[calc(var(--header-height)+1px)] z-30 ml-auto hidden h-[calc(100svh-var(--header-height)-var(--footer-height))] w-72 flex-col gap-4 overflow-hidden overscroll-none pb-8 xl:flex"
	>
		<div class="h-(--top-spacing) shrink-0"></div>
		{#if doc.toc.length}
			<div class="no-scrollbar overflow-y-auto px-8">
				<DocsToc toc={{ items: doc.toc }} />
				<div class="h-12"></div>
			</div>
		{/if}
		<div class="flex flex-1 flex-col gap-12 px-6">
			<Cta />
		</div>
		<div class="flex flex-col gap-12 px-6">
			<Ethical />
		</div>
	</div>
	<div class="flex min-w-0 flex-1 flex-col">
		<div class="h-(--top-spacing) shrink-0"></div>
		<div
			class="mx-auto flex w-full min-w-0 max-w-2xl flex-1 flex-col gap-8 px-4 py-6 text-neutral-800 md:px-0 lg:py-8 dark:text-neutral-300"
		>
			<div class="flex flex-col gap-2">
				<div class="flex flex-col gap-2">
					<div class="flex items-start justify-between">
						<h1
							class="scroll-m-20 text-4xl font-semibold tracking-tight sm:text-3xl xl:text-4xl"
						>
							{doc.title}
						</h1>
						<div class="flex items-center gap-2 pt-1.5">
							{#if neighbors.previous}
								<Button
									variant="secondary"
									size="icon"
									class="extend-touch-target size-8 shadow-none md:size-7"
									href={neighbors.previous.href}
								>
									<ArrowLeftIcon />
									<span class="sr-only">Previous</span>
								</Button>
							{/if}
							{#if neighbors.next}
								<Button
									variant="secondary"
									size="icon"
									class="extend-touch-target size-8 shadow-none md:size-7"
									href={neighbors.next.href}
								>
									<span class="sr-only">Next</span>
									<ArrowRightIcon />
								</Button>
							{/if}
						</div>
					</div>
					{#if data.metadata.description}
						<p class="text-muted-foreground text-balance text-[1.05rem] sm:text-base">
							{doc.description}
						</p>
					{/if}
				</div>
				{#if apiLink || source || docLink}
					<div class="flex items-center space-x-2 pt-4">
						{#if docLink}
							<Badge
								href={docLink}
								variant="secondary"
								target="_blank"
								rel="noreferrer"
							>
								Docs <ExternalLinkIcon />
							</Badge>
						{/if}
						{#if apiLink}
							<Badge
								href={apiLink}
								variant="secondary"
								target="_blank"
								rel="noreferrer"
							>
								API Reference <ExternalLinkIcon />
							</Badge>
						{/if}
						{#if source}
							{#key page.url.pathname}
								<ComponentCodeViewer item={source} />
							{/key}
						{/if}
					</div>
				{/if}
			</div>
			<div class="w-full flex-1 *:data-[slot=alert]:first:mt-0">
				<Markdown viewerData={data.viewerData} />
			</div>
		</div>
		<div class="mx-auto flex h-16 w-full max-w-2xl items-center gap-2 px-4 md:px-0">
			{#if neighbors.previous}
				<Button
					variant="secondary"
					size="sm"
					class="shadow-none"
					href={neighbors.previous.href}
				>
					<ArrowLeftIcon />
					{neighbors.previous.title}
				</Button>
			{/if}
			{#if neighbors.next}
				<Button
					variant="secondary"
					size="sm"
					class="ml-auto shadow-none"
					href={neighbors.next.href}
				>
					{neighbors.next.title}
					<ArrowRightIcon />
				</Button>
			{/if}
		</div>
	</div>
</div>
