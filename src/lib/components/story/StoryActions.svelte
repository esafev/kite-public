<script lang="ts">
import { s } from '$lib/client/localization.svelte';
import { page } from '$app/state';
import ShareButton from '../ShareButton.svelte';
import { UrlNavigationService } from '$lib/services/urlNavigationService';
import { dataLanguage } from '$lib/stores/dataLanguage.svelte';

// Props
interface Props {
	story: any;
	onClose?: () => void;
	batchId?: string;
	categoryId?: string;
	storyIndex?: number;
}

let { story, onClose, batchId, categoryId, storyIndex }: Props = $props();

// Get current navigation params from URL if not provided
const navigationParams = $derived.by(() => {
	if (batchId && categoryId && storyIndex !== undefined) {
		return { batchId, categoryId, storyIndex, dataLang: dataLanguage.current };
	}
	
	// Fall back to parsing from current URL
	const params = UrlNavigationService.parseUrl(page.url);
	return {
		batchId: params.batchId || batchId,
		categoryId: params.categoryId || categoryId,
		storyIndex: params.storyIndex ?? storyIndex,
		dataLang: params.dataLang || dataLanguage.current
	};
});
</script>

<div class="order-last mt-6 flex w-full items-center justify-center md:px-0">
	<!-- Left side: Share Button -->
	<div class="flex-1 flex justify-start">
		<ShareButton
			title={story.title}
			description={story.short_summary}
			batchId={navigationParams.batchId}
			categoryId={navigationParams.categoryId}
			storyIndex={navigationParams.storyIndex}
			dataLang={navigationParams.dataLang}
			class="text-gray-600 transition-all duration-200 hover:bg-gray-100 dark:text-gray-400 dark:hover:bg-gray-800 border border-gray-200 dark:border-gray-700"
		/>
	</div>

	<!-- Center: Close Button -->
	<button
		onclick={onClose}
		class="focus:ring-opacity-75 rounded-lg bg-black px-6 py-3 font-semibold text-white transition-colors duration-200 ease-in-out hover:bg-gray-800 focus:ring-2 focus:ring-gray-400 focus:outline-none"
	>
		{s('article.closeStory') || 'Close'}
	</button>

	<!-- Right side: Empty spacer for balance -->
	<div class="flex-1"></div>
</div> 