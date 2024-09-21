<script lang="ts">
  export let title: string;
  export let description: string;
  export let url: string;
</script>

<div class="rounded-md border-2 border-gray-100">
  <div class="flex items-start gap-4 p-4 sm:p-6">
    <button type="button" class="block shrink-0 border border-gray-200 rounded-md">
      <svg xmlns="http://www.w3.org/2000/svg" class="size-9 text-gray-500" viewBox="0 0 24 24">
        <path fill="currentColor" d="M8 6.82v10.36c0 .79.87 1.27 1.54.84l8.14-5.18a1 1 0 0 0 0-1.69L9.54 5.98A.998.998 0 0 0 8 6.82" />
      </svg>
    </button>

    <div>
      <h3 class="font-medium sm:text-lg">
        <a href={url} class="hover:underline text-gray-900" target="_blank">
          {title}
        </a>
      </h3>

      <p class="line-clamp-2 text-sm text-gray-700">
        {description}
      </p>
    </div>
  </div>
</div>
