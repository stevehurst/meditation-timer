<script lang="ts">
	import { X } from 'lucide-svelte';
	import { timerSettings } from '$lib/stores/timerSettings';
	import ToggleSwitch from './ToggleSwitch.svelte';
	import VolumeSlider from './VolumeSlider.svelte';
	import IntervalTimeInput from './IntervalTimeInput.svelte';
	import StoreDebugInfo from './StoreDebugInfo.svelte';

	let { isOpen } = $props();

	function handleDebugModeChange(event: Event) {
		const value = (event.target as HTMLInputElement).checked;
		timerSettings.update((settings) => ({
			...settings,
			isDebugMode: value
		}));
	}

	function handleIntervalTimeChange(event: Event) {
		const value = parseInt((event.target as HTMLInputElement).value);
		timerSettings.update((settings) => ({
			...settings,
			intervalTime: value
		}));
	}

	function handleStartStopBellChange(event: Event) {
		const value = (event.target as HTMLInputElement).checked;
		timerSettings.update((settings) => ({
			...settings,
			startStopBellEnabled: value
		}));
	}

	function handleIntervalBellChange(event: Event) {
		const value = (event.target as HTMLInputElement).checked;
		timerSettings.update((settings) => ({
			...settings,
			intervalBellEnabled: value
		}));
	}

	function handleStartStopVolumeChange(event: Event) {
		const value = parseFloat((event.target as HTMLInputElement).value);
		timerSettings.update((settings) => ({
			...settings,
			startStopBellVolume: value
		}));
	}

	function handleIntervalVolumeChange(event: Event) {
		const value = parseFloat((event.target as HTMLInputElement).value);
		timerSettings.update((settings) => ({
			...settings,
			intervalBellVolume: value
		}));
	}
</script>

{#if isOpen}
	<div
		class="fixed top-0 right-0 z-50 h-full w-80 transform bg-white shadow-lg transition-transform duration-300 ease-in-out dark:bg-slate-800"
	>
		<div class="relative p-6">
			<button
				onclick={() => (isOpen = false)}
				class="absolute top-4 right-4 rounded-full p-2 transition-colors hover:bg-gray-100 dark:hover:bg-slate-700"
			>
				<X class="h-5 w-5 text-gray-600 dark:text-gray-300" />
			</button>
			<h2 class="mb-6 text-2xl font-semibold dark:text-white">Settings</h2>

			<div class="space-y-6">
				<IntervalTimeInput
					value={$timerSettings.intervalTime}
					onChange={handleIntervalTimeChange}
				/>

				<div class="space-y-4">
					<div class="space-y-2">
						<ToggleSwitch
							id="startStopBell"
							checked={$timerSettings.startStopBellEnabled}
							onChange={handleStartStopBellChange}
							label="Start/Stop Bell"
						/>
						{#if $timerSettings.startStopBellEnabled}
							<VolumeSlider
								id="startStopVolume"
								value={$timerSettings.startStopBellVolume}
								changeVolume={handleStartStopVolumeChange}
							/>
						{/if}
					</div>

					<div class="space-y-2">
						<ToggleSwitch
							id="intervalBell"
							checked={$timerSettings.intervalBellEnabled}
							onChange={handleIntervalBellChange}
							label="Interval Bell"
						/>
						{#if $timerSettings.intervalBellEnabled}
							<VolumeSlider
								id="intervalVolume"
								value={$timerSettings.intervalBellVolume}
								changeVolume={handleIntervalVolumeChange}
							/>
						{/if}
					</div>
				</div>

				{#if import.meta.env.DEV}
					<div class="mt-8 border-t border-gray-200 pt-4 dark:border-gray-700">
						<ToggleSwitch
							id="debugMode"
							checked={$timerSettings.isDebugMode}
							onChange={handleDebugModeChange}
							label="Debug Mode (10x faster)"
						/>
					</div>
					<StoreDebugInfo />
				{/if}
			</div>
		</div>
	</div>
{/if}
