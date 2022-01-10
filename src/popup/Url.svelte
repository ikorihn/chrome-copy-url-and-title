<script lang="ts">
  let url: string | null = null
  let title: string | null = null

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
    } catch (err) {
      console.error('Failed to copy: ', err)
    }
  }
</script>

<main>
  <section class="title">
    <h2 class="title__label text-xl font-bold">Title</h2>
    <div class="title__content">{title}</div>
  </section>
  <section class="url">
    <h2 class="url__label text-xl font-bold">Url</h2>
    <div class="url__content">{url}</div>
  </section>
  <button class="button w-full rounded bg-blue-400 text-white" on:click={copyUrlAndTitle}>Copy</button>
</main>
