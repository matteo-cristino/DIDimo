<!--
SPDX-FileCopyrightText: 2025 Forkbomb BV

SPDX-License-Identifier: AGPL-3.0-or-later
-->

<script lang="ts">
	import { run } from 'svelte/legacy';

	import { invalidateAll } from '$app/navigation';
	import { page } from '$app/stores';
	import { goto } from '@/i18n';

	import * as Pagination from '@/components/ui/pagination';
	import type { Page } from '@sveltejs/kit';

	function handlePageChange(number: number, page: Page) {
		const url = page.url;
		url.searchParams.set('page', number.toString());
		goto(url.pathname + '?' + url.searchParams.toString());
	}
</script>

<Pagination.Root count={100} perPage={8} onPageChange={(n) => handlePageChange(n, $page)}>
	{#snippet children({ pages, currentPage })}
		<Pagination.Content>
			<Pagination.Item>
				<Pagination.PrevButton />
			</Pagination.Item>
			{#each pages as page (page.key)}
				{#if page.type === 'ellipsis'}
					<Pagination.Item>
						<Pagination.Ellipsis />
					</Pagination.Item>
				{:else}
					<!-- <Pagination.Item isVisible={currentPage == page.value}> -->
					<Pagination.Item>
						<Pagination.Link {page} isActive={currentPage == page.value}>
							{page.value}
						</Pagination.Link>
					</Pagination.Item>
				{/if}
			{/each}
			<Pagination.Item>
				<Pagination.NextButton />
			</Pagination.Item>
		</Pagination.Content>
	{/snippet}
</Pagination.Root>
