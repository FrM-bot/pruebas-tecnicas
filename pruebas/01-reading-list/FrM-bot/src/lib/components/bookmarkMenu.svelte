<script>
  import Bookmark from '$lib/icons/bookmark.svelte'
  import Cross from '$lib/icons/cross.svelte'
  import { BookmarkStore, bookmarks } from '$lib/store/bookmark'
  import { LocalRotes } from '$lib/utils/routes'
  import { slide } from 'svelte/transition'

  let isOpenMenu = false
  function openeCloseMenu() {
    isOpenMenu = !isOpenMenu
  }
  function closeMenu() {
    isOpenMenu = false
  }
  $: bookmark = $bookmarks
</script>

<div class="relative flex">
  <button
    on:click={openeCloseMenu}
    class="relative rounded hover:text-white text-transparent duration-200 hover:bg-tertiary/50"
    aria-label="Bookmarks"
  >
    <span
      class="bg-white absolute font-bold top-0 p-1 text-sm grid place-content-center max-h-4 text-dark-v1 right-0 rounded-sm"
      aria-label="Books in bookmark"
    >
      {bookmark.length}
    </span>
    <Bookmark props={{ width: 35, height: 35 }} />
  </button>
  {#if isOpenMenu}
    <div
      transition:slide={{ duration: 200 }}
      on:mouseleave={() => closeMenu()}
      role="dialog"
      class="absolute shadow-lg p-1 top-[120%] max-w-xl sm:w-[470px] w-80 bg-dark-v2 rounded-md rounded-tr-none right-[1px] before:absolute before:z-[1] before:w-4 before:h-4 before:bg-dark-v2 before:right-[9px] before:-top-4 before:[clip-path:polygon(50%_10%,_0%_100%,_100%_100%)]"
    >
      <div
        class="w-full flex flex-col gap-2 p-1 bg-dark-v1 rounded max-h-[520px] overflow-y-scroll"
      >
        {#if bookmark.length === 0}
          <p class="mx-auto">
            Haz click en <span class="bg-dark-v2 p-1 text-transparent"><Bookmark /></span> para agregar
            a marcadores
          </p>
        {/if}
        {#each bookmark as book}
          <div class="relative grid grid-cols-[80px_1fr] gap-2 hover:bg-dark-v2 rounded-sm">
            <a href={LocalRotes.book.details(book.id)}>
              <picture class="rounded rounded-r-none overflow-hidden flex cursor-pointer">
                <img
                  class="object-contain duration-200"
                  src={book.cover}
                  alt={book.title}
                  loading="lazy"
                />
              </picture>
            </a>
            <div class="flex flex-col">
              <a
                class="sm:max-w-[30ch] max-w-[17ch] text-sm overflow-hidden whitespace-nowrap text-ellipsis"
                href={LocalRotes.book.details(book.id)}>{book.title}</a
              >
              <span class="text-xs opacity-80">{book.author}</span>
              <span class="text-xs opacity-80">{book.genre}</span>
              <span class="text-xs opacity-80">Páginas: {book.pages}</span>
            </div>
            <div class="absolute top-1 right-1 z-[1]">
              <button
                aria-label="Remove book"
                on:click={() =>
                  BookmarkStore.toggle({
                    id: book.id,
                    title: book.title,
                    author: book.author,
                    cover: book.cover,
                    genre: book.genre,
                    pages: book.pages
                  })}
                class="bg-dark-v2 p-1 hover:bg-dark-v1 rounded duration-200"
              >
                <Cross />
              </button>
            </div>
          </div>
        {/each}
      </div>
    </div>
  {/if}
</div>
