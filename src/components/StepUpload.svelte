<script lang="ts">
  import Dropzone from 'dropzone'
  import 'dropzone/dist/dropzone.css'
  import { onMount } from 'svelte'
  import { ImageStatus } from '../types.d'
  import { imageStatus, modifiedImage, originalImage } from '../store/store'
  import { Cloudinary } from '@cloudinary/url-gen'
  import { backgroundRemoval } from '@cloudinary/url-gen/actions/effect'

  const cloudinary = new Cloudinary({
    cloud: {
      cloudName: 'fosouldev'
    },
    url: {
      secure: true
    }
  })

  onMount(() => {
    const dropzone = new Dropzone('#dropzone', {
      uploadMultiple: false,
      acceptedFiles: 'image/*',
      maxFiles: 1
    })

    dropzone.on('sending', (file, xhr, formData) => {
      imageStatus.set(ImageStatus.UPLOADING)
      formData.append('upload_preset', 'zvxxtg5b')
      formData.append('timestapm', Date.now() / 1000)
      formData.append('api_key', 269885839669855)
    })

    dropzone.on('success', (file, response) => {
      const { public_id, secure_url: url } = response
      const imageWithoutBackground = cloudinary
        .image(public_id)
        .effect(backgroundRemoval())
      imageStatus.set(ImageStatus.DONE)
      originalImage.set(url)
      modifiedImage.set(imageWithoutBackground.toURL())
      console.log(response)
    })

    dropzone.on('error', (file, response) => {
      console.log('BAD')
      console.log(response)
    })
  })
</script>

<form
  action="https://api.cloudinary.com/v1_1/fosouldev/image/upload"
  id="dropzone"
  class=" flex shadow-2xl border-dashed border-2 border-gray-300 rounded-lg aspect-video w-full items-center justify-center flex-col"
>
  {#if $imageStatus === ImageStatus.READY}
    <button
      class="pointer-event-nonte bg-red-400 rounded-full text-bold text-white text-xl px-6 py-4"
    >
      Upload Files
    </button>
    <strong class="text-lg mt-4 text-gray-800">or drop a file here</strong>
  {/if}
  {#if $imageStatus === ImageStatus.UPLOADING}
    <strong class="text-lg mt-4 text-gray-800">Uploading file...</strong>
  {/if}
</form>
