<script lang="ts">
  import { fade } from 'svelte/transition'

  let url: string | null = null
  let title: string | null = null

  let visible: boolean = false

  getUrl()

  async function getUrl() {
    chrome.tabs.query(
      { active: true, lastFocusedWindow: true },
      (tabs: chrome.tabs.Tab[]) => {
        const current = tabs[0]
        url = current.url
        title = current.title
      }
    )
  }

  async function copyUrlAndTitle() {
    try {
      await navigator.clipboard.writeText(`[${title}](${url})`)
      visible = true
      setTimeout(() => (visible = false), 1000)
    } catch (err) {
      console.error('Failed to copy: ', err)
    }
  }
  async function copyUrl() {
    try {
      await navigator.clipboard.writeText(url)
      visible = true
      setTimeout(() => (visible = false), 1000)
    } catch (err) {
      console.error('Failed to copy: ', err)
    }
  }
  async function copyTitle() {
    try {
      await navigator.clipboard.writeText(title)
      visible = true
      setTimeout(() => (visible = false), 1000)
    } catch (err) {
      console.error('Failed to copy: ', err)
    }
  }
</script>

<main>
  <section class="title">
    <h2 class="title__label text-l font-bold">Title</h2>
    <div class="title__content">{title}</div>
  </section>
  <section class="url mt-2">
    <h2 class="url__label text-l font-bold">URL</h2>
    <div class="url__content">{url}</div>
  </section>
  <button
    class="button mt-2 py-2 w-full rounded bg-blue-400 text-l text-white"
    on:click={copyUrlAndTitle}>Copy as markdown</button
  >
  <button
    class="button mt-2 py-2 w-full rounded bg-blue-400 text-l text-white"
    on:click={copyTitle}>Copy title</button
  >
  <button
    class="button mt-2 py-2 w-full rounded bg-blue-400 text-l text-white"
    on:click={copyUrl}>Copy URL</button
  >

  {#if visible}
    <p class="msg none" transition:fade>Copied! {msg}</p>
  {/if}
</main>
