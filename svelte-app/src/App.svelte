<script>
  import {URLPattern} from "urlpattern-polyfill";
  import {onMount} from "svelte";

  /**
   * @var HTMLElement list
   **/
  let list;

  onMount(async () => {
    const {default: ld} = await import('./'+'ld.js');

    const pattern = new URLPattern("/authors/:id", window.origin);
    const list = document.getElementById('list')

    function onUpdate(books) {
      const l = []
      books['hydra:member'].forEach((book) => {
        const li = document.createElement('li')
        li.dataset.testid = 'book'
        li.innerText = `${book.title} - ${book.author?.name}`
        l.push(li)
      });
      list.replaceChildren(...l)
    }

    ld('/books', {urlPattern: pattern, onUpdate})
      .then((books) => {
        books['hydra:member'].forEach((book) => {
          const li = document.createElement('li')
          li.dataset.testid = 'book'
          li.innerText = `${book.title} - ${book.author?.name}`
          list.appendChild(li)
        });
      })
  });
</script>

<ul id="list" bind:this={list}>
</ul>
