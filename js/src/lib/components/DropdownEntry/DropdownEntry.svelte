<script lang="ts">
	import type { SvelteComponent } from "svelte";

	export let classNames = "";
	export let href: string | undefined = undefined;
	export let icon: typeof SvelteComponent | undefined = undefined;
	export let iconClassNames = "";
	export let label = "";
	export let noFollow = false;
	export let underline = false;
	export let onClick: (e: MouseEvent) => void = () => {};
	export let targetBlank = false;
	export let type: "link" | "button" | "submit" | undefined = undefined;

	const element = type === undefined ? (href !== undefined ? "a" : "button") : type === "link" ? "a" : "button";
</script>

<li>
	<svelte:element
		this={element}
		class="flex w-full cursor-pointer items-center whitespace-nowrap px-3 py-1.5 text-left hover:bg-gray-50 dark:hover:bg-gray-800
		{classNames} {underline ? 'hover:underline' : ''}"
		{href}
		rel={element === "a" && noFollow ? "nofollow" : undefined}
		target={element === "a" && targetBlank ? "_blank" : undefined}
		on:click={e => {
			if (type === "submit") {
				e.stopPropagation();
			}
			onClick(e);
		}}
		type={element === "button" ? type : undefined}
	>
		<!-- Adding children to the DropdownEntry element overwrite the default label/icon stuff -->
		{#if $$slots.default}
			<slot />
		{:else}
			{#if icon}
				<svelte:component this={icon} classNames="mr-1.5 flex-none {iconClassNames}" />
			{/if}
			<span class="truncate">{label}</span>
		{/if}
	</svelte:element>
</li>
