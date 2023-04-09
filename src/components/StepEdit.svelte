<script lang="ts">
  import { modifiedImage } from './../store/store'
  import { originalImage } from '../store/store'
  import 'two-up-element'

  let processingImage = true
  let tries = 0
  let intervalId: number

  $: {
    if (processingImage) {
      clearInterval(intervalId)
      intervalId = setInterval(() => {
        tries++
        const img = new Image()
        img.src = $modifiedImage
        img.onload = () => {
          processingImage = false
          clearInterval(intervalId)
        }
      }, 500)
    }
  }
</script>

<two-up>
  <!-- svelte-ignore a11y-img-redundant-alt -->
  <img src={$originalImage} alt="Original image uploaded by the user" />
  <!-- svelte-ignore a11y-img-redundant-alt -->
  {#if processingImage}
    <div class="flex justify-center items-center">
      <div
        class="animate-spin rounded-full h-32 w-32 border-b-2 border-gray-900"
      />
    </div>
  {:else}
    <img
      src={$modifiedImage}
      alt="Image without background uploaded by the user"
    />
  {/if}
</two-up>
<a
  download
  href={$modifiedImage}
  class="block mt-10 bg-red-400 hover:bg-red-900 text-xl text-center w-full font-bold text-white rounded-full px-4 py-2"
  >Download image without background
</a>
