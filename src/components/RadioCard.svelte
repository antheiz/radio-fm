<script lang="ts">
  import { onMount, onDestroy } from 'svelte';
  import { Howl } from 'howler';

  export let title: string;
  export let description: string;
  export let url: string;

  let sound: Howl | null = null;
  let isPlaying = false;
  let isLoading = false;
  let errorMessage = '';

  onMount(() => {
    console.log(`Mounting component for ${title}`);
    initializeHowl();
  });

  onDestroy(() => {
    if (sound) {
      console.log(`Unloading sound for ${title}`);
      sound.unload();
    }
  });

  function initializeHowl() {
    console.log(`Initializing Howl for ${title} with URL: ${url}`);
    sound = new Howl({
      src: [url],
      html5: true,
      format: ['mp3', 'aac'],
      onload: () => {
        console.log(`Audio loaded successfully for ${title}`);
        isLoading = false;
      },
      onloaderror: (id: number, error: unknown) => {
        console.error(`Error loading audio for ${title}:`, error);
        errorMessage = 'Error loading audio. Please check your connection and try again.';
        isLoading = false;
      },
      onplay: () => {
        console.log(`Audio started playing for ${title}`);
        isPlaying = true;
        isLoading = false;
      },
      onpause: () => {
        console.log(`Audio paused for ${title}`);
        isPlaying = false;
      },
      onstop: () => {
        console.log(`Audio stopped for ${title}`);
        isPlaying = false;
      },
      onplayerror: (id: number, error: unknown) => {
        console.error(`Error playing audio for ${title}:`, error);
        errorMessage = 'Error playing audio. Please try again later.';
        isPlaying = false;
        isLoading = false;
      }
    });
  }

  function togglePlay() {
    console.log(`Toggle play clicked for ${title}`);
    if (!sound) {
      console.log('Howl instance not created, initializing...');
      initializeHowl();
    }

    errorMessage = ''; // Clear any previous error messages

    if (isPlaying) {
      console.log(`Pausing audio for ${title}`);
      sound?.pause();
    } else {
      console.log(`Attempting to play audio for ${title}`);
      isLoading = true;
      sound?.play();
    }
  }
</script>

<div class="rounded-md border-2 border-gray-100 hover:border-gray-200 transition-colors duration-200">
  <div class="flex items-start gap-4 p-4 sm:p-6">
    <button
      type="button"
      class="block shrink-0 border border-gray-200 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500 transition-colors duration-200 hover:bg-gray-100"
      on:click={togglePlay}
      disabled={isLoading}
      aria-label={isPlaying ? "Pause" : "Play"}
    >
      {#if isLoading}
        <svg class="animate-spin h-9 w-9 text-gray-500" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24">
          <circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle>
          <path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"></path>
        </svg>
      {:else}
        <svg xmlns="http://www.w3.org/2000/svg" class="size-9 text-gray-500" viewBox="0 0 24 24">
          {#if isPlaying}
            <path fill="currentColor" d="M14,19H18V5H14M6,19H10V5H6V19Z" />
          {:else}
            <path fill="currentColor" d="M8 6.82v10.36c0 .79.87 1.27 1.54.84l8.14-5.18a1 1 0 0 0 0-1.69L9.54 5.98A.998.998 0 0 0 8 6.82" />
          {/if}
        </svg>
      {/if}
    </button>

    <div class="flex-1">
      <h3 class="font-medium sm:text-lg">
        <a href={url} class="hover:underline text-gray-900" target="_blank" rel="noopener noreferrer">
          {title}
        </a>
      </h3>

      <p class="line-clamp-2 text-sm text-gray-700 mt-1">
        {description}
      </p>
      {#if errorMessage}
        <p class="text-red-500 text-sm mt-2">{errorMessage}</p>
      {/if}
      {#if isPlaying}
        <p class="text-gray-500 underline text-sm mt-2">Now playing</p>
      {/if}
    </div>
  </div>
</div>
