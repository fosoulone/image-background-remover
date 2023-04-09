<script lang="ts">
  import StepUpload from './components/StepUpload.svelte'
  import CloudinaryLogo from './assets/CloudinaryLogo.svelte'
  import Dropzone from 'dropzone'
  import 'dropzone/dist/dropzone.css'
  import { imageStatus } from './store/store'
  import { ImageStatus } from './types.d'
  import StepEdit from './components/StepEdit.svelte'
</script>

<div
  class="max-w-xl m-auto grid grid-cols-1 place-content-center w-full h-screen p-4"
>
  <header class="flex justify-center py-10">
    <h1 class="text-3xl font-bold text-red-400 tracking-tighter">
      imageremover<span class="text-red-700">.bg</span>
    </h1>
  </header>

  <main class="w-full block">
    {#if $imageStatus === ImageStatus.READY || $imageStatus === ImageStatus.UPLOADING}
      <StepUpload />
    {:else if $imageStatus === ImageStatus.DONE}
      <StepEdit />
    {/if}
  </main>

  <footer class="flex justify-center items-center gap-x-2 font-semibold pt-10">
    Made using <a
      href="https://cloudinary.com/"
      target="_blank"
      rel="noreferrer"><CloudinaryLogo /></a
    >
  </footer>
</div>
