<!--
SPDX-FileCopyrightText: 2025 Forkbomb BV

SPDX-License-Identifier: AGPL-3.0-or-later
-->

<script lang="ts">
	import { currentUser } from '@/pocketbase';
	import { getUserRole, type OrgRole } from '@/organizations';
	import { createOrganizationLinks } from '@/organizations';
	import NavigationTabs from '@/components/ui-custom/navigationTabs.svelte';

	//

	interface Props {
		organizationId: string;
	}

	let { organizationId }: Props = $props();

	//

	let userRole: OrgRole = $state('member');

	$effect(() => {
		getUserRole(organizationId, $currentUser?.id ?? '').then((res) => {
			userRole = res;
		});
	});

	let tabs = $derived(createOrganizationLinks(organizationId, userRole));
</script>

<NavigationTabs {tabs} />
