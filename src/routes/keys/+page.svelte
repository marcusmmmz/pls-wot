<script lang="ts">
	import { goto } from "$app/navigation";
	import { nostrAuth } from "$lib/nostr";
	import { Button } from "flowbite-svelte";
	import { nip19 } from "nostr-tools";
	import { onMount } from "svelte";

	onMount(() => {
		if (window.nostr) nostrAuth.tryLogin();
	})
</script>

<div class="flex flex-col items-center justify-center h-screen w-full gap-4">
	{#if !$nostrAuth?.privkey && $nostrAuth?.pubkey}
		<h1 class="text-3xl text-center">Logged in with nostr browser extension</h1>
	{/if}

	{#if $nostrAuth?.pubkey}
		<p class="text-center break-all px-2">
			Your public ID: <br />
			{nip19.npubEncode($nostrAuth?.pubkey)}
		</p>

		<Button
			on:click={() => {
				nostrAuth.signOut();
				goto('/');
			}}
		>
			Sign out
		</Button>
	{/if}
</div>
