<script>
  import Category from './Category.svelte';

  const categories = [
    {
      name: 'Health',
      icon: './public/assets/health.jpg',
      subcategories: ['Exercise', 'Sleep']
    },
    {
      name: 'Work',
      icon: './public/assets/work.jpg',
      subcategories: ['At Home', 'At the Office']
    },
    {
      name: 'Leisure',
      icon: './public/assets/leisure.jpg',
      subcategories: ['Socializing', 'Entertainment']
    }
  ];
</script>

<main>
  <h1>Organize Your Day</h1>
  <div class="categories">
    {#each categories as category}
      <Category {category} />
    {/each}
  </div>
</main>

<style>
  main {
    text-align: center;
    padding: 1em;
    margin: 0 auto;
    max-width: 800px;
  }

  .categories {
    display: flex;
    justify-content: space-around;
    margin-top: 2em;
  }
</style>
