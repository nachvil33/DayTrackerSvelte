<script>
  export let category;
  let selectedSubcategory = '';
  let activity = '';
  let hours = 0;
  let activities = JSON.parse(localStorage.getItem(category.name)) || [];
  let dayCount = 1;

  function addActivity() {
    if (activity && hours && parseFloat(hours) >= 1 && parseFloat(hours) <= 24) {
      activities = [
        ...activities,
        { id: Date.now(), subcategory: selectedSubcategory, activity, hours: parseFloat(hours), day: dayCount }
      ];
      updateLocalStorage();
      activity = '';
      hours = 0;
    } else {
      alert('Please enter a valid activity and hours (between 1 and 24)');
    }
  }

  function deleteActivity(id) {
    activities = activities.filter(act => act.id !== id);
    updateLocalStorage();
  }

  function updateLocalStorage() {
    localStorage.setItem(category.name, JSON.stringify(activities));
  }

  function selectSubcategory(subcategory) {
    selectedSubcategory = subcategory;
    activity = '';
    hours = 0;
  }

  function newDay() {
    dayCount++;
  }

  function activitiesByDay(day) {
    return activities.filter(activity => activity.day === day);
  }

  function incrementHours() {
    hours++;
  }

  function decrementHours() {
    if (hours > 0) {
      hours--;
    }
  }
</script>

<div class="category">
  <img src={category.icon} alt={category.name} on:click={() => selectedSubcategory = ''} />
  <h2>{category.name}</h2>
  {#if selectedSubcategory === ''}
    <div class="subcategories">
      {#each category.subcategories as subcategory}
        <button on:click={() => selectSubcategory(subcategory)}>{subcategory}</button>
      {/each}
    </div>
  {:else}
    <div class="activity-form">
      <h3>{selectedSubcategory}</h3>
      <input type="text" placeholder="Activity" bind:value={activity} />
      <div>
        <input type="number" placeholder="Hours" bind:value={hours} min="0" max="24" />
        <button on:click={incrementHours}>+</button>
        <button on:click={decrementHours}>-</button>
      </div>
      <button on:click={addActivity}>Add Activity</button>
    </div>
  {/if}
  {#each Array.from({ length: dayCount }, (_, i) => i + 1) as day}
    <div>
      <h3>Day {day}</h3>
      <ul>
        {#each activitiesByDay(day) as act (act.id)}
          <li>
            {act.activity}: {act.hours} hours
            <button on:click={() => deleteActivity(act.id)}>Delete</button>
          </li>
        {/each}
      </ul>
    </div>
  {/each}
  <button on:click={newDay}>New Day</button>
</div>

<style>
  .category {
    border: 1px solid #ccc;
    padding: 1em;
    border-radius: 8px;
    width: 30%;
    text-align: center;
    margin: 1em;
  }

  img {
    width: 100px;
    height: 100px;
    cursor: pointer;
  }

  .subcategories button {
    margin: 0.5em;
    padding: 0.5em 1em;
  }

  .activity-form {
    margin-top: 1em;
  }

  input[type="text"], input[type="number"] {
    margin: 0.5em;
    padding: 0.5em;
    width: calc(100% - 2.5em); /* Adjust the width to be responsive */
    box-sizing: border-box; /* Include padding and border in the element's total width and height */
  }

  button {
    padding: 0.5em 1em;
    margin-top: 0.5em;
  }

  ul {
    list-style-type: none;
    padding: 0;
  }

  li {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin: 0.5em 0;
  }

  li button {
    padding: 0.3em 0.5em;
  }
</style>
