<!-- <script>
	import Tag from '$lib/components/Tag.svelte';
	import Title from '$lib/components/Title.svelte';
	import Author from '$lib/components/Author.svelte';
	import SearchBox from '$lib/components/SearchBox.svelte';
	import { page } from '$app/stores';
	import fuzzySearch from '$utils/search.js';
	import { onMount } from 'svelte';

	export let title = '';
	export let subtitle = '';
	export let posts = [];
	export let tags = [];
	export let more = true;
	export let search = true;
	export let h2 = false;
	export let count = 0;

	if (count) {
		posts = posts.slice(0, count);
	}

	let filter = '';
	let currentPosts = posts;

	onMount(() => {
		filter = $page.url.searchParams.get('query') || '';
		currentPosts = filter ? fuzzySearch(posts, filter) : posts;
	});

	function handleSearchUpdate() {
		// Handle search updates here if needed
		currentPosts = filter ? fuzzySearch(posts, filter) : posts;
	}
</script> -->

<script>
	import Tag from '$lib/components/Tag.svelte';
	import Title from '$lib/components/Title.svelte';
	import Author from '$lib/components/Author.svelte';
	import SearchBox from '$lib/components/SearchBox.svelte';
	import { page } from '$app/stores';
	import fuzzySearch from '$utils/search.js';

	export let title = '';
	export let subtitle = '';
	export let posts = [];
	export let tags = [];
	export let more = true;
	export let search = true;
	export let h2 = false;
	export let count = 0;

	if (count) {
		posts = posts.slice(0, count);
	}

	$: filter = $page.url.searchParams.get('query');
	$: currentPosts = filter ? fuzzySearch(posts, filter) : posts;
</script>

<div class="divide-y divide-gray-200 dark:divide-gray-700">
	<div class="space-y-2 pt-6 pb-8 md:space-y-5">
		<div class="grid lg:grid-cols-2 gap-4">
			<div>
				<Title {title} {subtitle} {h2} />
			</div>

			<div class="pl-4" class:border-l-2={search}>
				{#if search}
					<SearchBox />
				{/if}

				{#if tags.length}
					<div class="flex flex-wrap">
						{#each tags as tag}
							<div class="mr-5">
								<Tag text={tag.text} size="text-xs" />
								<a
									href={`/tags/${tag.slug}`}
									class="-ml-2 text-xs font-semibold uppercase text-gray-600 dark:text-gray-300"
								>
									{` (${tag.count})`}
								</a>
							</div>
						{/each}
					</div>
				{/if}
			</div>
		</div>
	</div>
	{#if !currentPosts.length}
		No post found.
	{:else}
		<ul class="grid grid-cols-1 gap-6 sm:grid-cols-2 lg:grid-cols-3">
			{#each currentPosts as post}
				<li class="py-12">
					<article class="h-full border p-4 rounded-lg shadow-md bg-white dark:bg-gray-800">
						<div class="space-y-2">
							<Author author={post.author} postDate={post.date} />
							<div class="space-y-5">
								<div class="space-y-6">
									<div>
										<h2 class="text-2xl font-bold leading-8 tracking-tight">
											<a href={`/${post.slug}`} class="text-gray-900 dark:text-gray-100">
												{post.title}
											</a>
										</h2>
										<div class="flex flex-wrap">
											{#each post.tags as tag}
												<Tag text={tag} />
											{/each}
										</div>
									</div>
									<div class="prose max-w-none text-gray-500 dark:text-gray-400">
										{post.summary}
									</div>
								</div>
								{#if more}
									<div class="text-base font-medium leading-6">
										<a
											href={`/${post.slug}`}
											class="text-primary-500 hover:text-primary-600 dark:hover:text-primary-400"
											aria-label={`Read "${post.title}"`}
										>
											Read more &rarr;
										</a>
									</div>
								{/if}
							</div>
						</div>
					</article>
				</li>
			{/each}
		</ul>
	{/if}
</div>
