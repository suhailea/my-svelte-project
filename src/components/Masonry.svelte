<!-- 
	An almost direct copy and paste of: https://css-tricks.com/a-lightweight-masonry-solution
	Usage:
		- stretchFirst stretches the first item across the top
  <Masonry stretchFirst={true} >
    {#each data as o}
      <div class="_card _padding">
        Here's some stuff {o.name}
        <header>
          <h3>{o.name}</h3>
        </header>
        <section>
          <p>{o.text}</p> 
        </section>
      </div>
    {/each}
  </Masonry>
 -->



 <div bind:this={masonryElement} 
 class={`__grid--masonry ${stretchFirst ? '__stretch-first' : ''}`}
 style={`--grid-gap: ${gridGap}; --col-width: ${colWidth};`}
 >
<slot></slot>
</div>


<script lang="ts">
  import { onMount, onDestroy, tick } from 'svelte';

  export let stretchFirst: boolean = false;
  export let gridGap: string = '0.5em';
  export let colWidth: string = 'minmax(Min(20em, 100%), 1fr)';
  export let items: any[] = []; // Adjust the type of items if you have a specific type

  interface Grid {
    _el: HTMLElement;
    gap: number;
    items: HTMLElement[];
    ncol: number;
    mod: number;
  }

  let grids: Grid[] = [];
  let masonryElement: HTMLElement | null = null;

  const refreshLayout = async () => {
    grids.forEach(async grid => {
      let ncol = getComputedStyle(grid._el).gridTemplateColumns.split(' ').length;

      grid.items.forEach(c => {
        let new_h = c.getBoundingClientRect().height;
        if (new_h !== +c.dataset.h!) {
          c.dataset.h = new_h.toString();
          grid.mod++;
        }
      });

      if (grid.ncol !== ncol || grid.mod) {
        grid.ncol = ncol;
        grid.items.forEach(c => c.style.removeProperty('margin-top'));

        if (grid.ncol > 1) {
          grid.items.slice(ncol).forEach((c, i) => {
            let prev_fin = grid.items[i].getBoundingClientRect().bottom;
            let curr_ini = c.getBoundingClientRect().top;
            c.style.marginTop = `${prev_fin + grid.gap - curr_ini}px`;
          });
        }
        grid.mod = 0;
      }
    });
  };

  const calcGrid = async (_masonryArr: HTMLElement[]) => {
    await tick();
    if (_masonryArr.length && getComputedStyle(_masonryArr[0]).gridTemplateRows !== 'masonry') {
      grids = _masonryArr.map(grid => ({
        _el: grid,
        gap: parseFloat(getComputedStyle(grid).gridRowGap),
        items: Array.from(grid.childNodes).filter(
          c => c.nodeType === 1 && +getComputedStyle(c as Element).gridColumnEnd !== -1
        ) as HTMLElement[],
        ncol: 0,
        mod: 0
      }));
      refreshLayout();
    }
  };

  let _window: Window | null = null;

  onMount(() => {
    _window = window;
    _window.addEventListener('resize', refreshLayout, false);
  });

  onDestroy(() => {
    if (_window) {
      _window.removeEventListener('resize', refreshLayout, false);
    }
  });

  $: if (masonryElement) {
    calcGrid([masonryElement]);
  }

  $: if (items) {
    masonryElement = masonryElement;
  }
</script>

<style>
  :global(.__grid--masonry) {
    display: grid;
    grid-template-columns: repeat(auto-fit, var(--col-width));
    grid-template-rows: masonry;
    justify-content: center;
    grid-gap: var(--grid-gap);
    padding: var(--grid-gap);
  }
  :global(.__grid--masonry > *) {
    align-self: start;
  }
  :global(.__grid--masonry.__stretch-first > *:first-child) {
    grid-column: 1 / -1;
  }
</style>
