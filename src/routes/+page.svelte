<script lang="ts">

  interface Ingredient {
    name: string;
    packageCost: number;
    packageVolume: number;
    recipeVolume: number;
  }

  interface Consumable {
    name: string;
    cost: number;
  }

  //default values for test purposes
  //TODO: cant be negative value
  let ingredients = $state<Ingredient[]>([{ name: '', packageCost: 2, packageVolume: 3, recipeVolume: 5 }]);
  let consumables = $state<Consumable[]>([{ name: '', cost: 5 }]);

  let ingredientTotal = $state(0);
  let consumableTotal = $state(0);
  let totalCost = $state(0);

  function addIngredient() {
    ingredients.push({ name: '', packageCost: 0, packageVolume: 0, recipeVolume: 0 });
  }

  function addConsumable() {
    consumables.push({ name: '', cost: 0 });
  }

  function deleteIngredient(index: number) {
  ingredients.splice(index, 1);
  }

  function deleteConsumable(index: number) {
  consumables.splice(index, 1);
  }

  function clearIngredients() {
    ingredients = [{ name: '', packageCost: 0, packageVolume: 0, recipeVolume: 0 }];
  }

  function clearConsumables() {
    consumables = [{ name: '', cost: 0 }];
  }



  $effect(() => {
    ingredientTotal = ingredients.reduce((sum, ingredient) => sum + (ingredient.packageVolume ? ingredient.packageCost * (ingredient.recipeVolume / ingredient.packageVolume) : 0), 0);
    consumableTotal = consumables.reduce((sum, consumable) => sum + consumable.cost, 0);
    totalCost = ingredientTotal + consumableTotal;
	});

</script>

<main>
  <h2>Ингредиенты</h2>
  <table>
    <thead>
      <tr>
        <th>#</th>
        <th>Ингредиенты</th>
        <th>Стоимость упаковки</th>
        <th>Объем в упаковке</th>
        <th>Объем в рецепте</th>
        <th></th>
      </tr>
    </thead>
    <tbody>
        {#each ingredients as ingredient, i}
        <tr>
          <td>{i + 1}</td>
          <td><input bind:value={ingredient.name} type="text" /></td>
          <td><input bind:value={ingredient.packageCost} type="number" /></td>
          <td><input bind:value={ingredient.packageVolume} type="number" /></td>
          <td><input bind:value={ingredient.recipeVolume} type="number" /></td>
          <td><button onclick={() => deleteIngredient(i)}>Delete</button></td>
        </tr>
      {/each}
    </tbody>
  </table>
  <button onclick={addIngredient}>Добавить ингредиент</button>
  <button onclick={clearIngredients}>Clear</button>
  <p>Общая стоимость ингредиентов: {ingredientTotal.toFixed(2)}</p>

  <h2>Расходники</h2>
  <table>
    <thead>
      <tr>
        <th>#</th>
        <th>Расходник</th>
        <th>Стоимость</th>
        <th></th>
      </tr>
    </thead>
    <tbody>
        {#each consumables as consumable, i}
        <tr>
          <td>{i + 1}</td>
          <td><input bind:value={consumable.name} type="text" /></td>
          <td><input bind:value={consumable.cost} type="number" /></td>
          <td><button onclick={() => deleteConsumable(i)}>Delete</button></td>
        </tr>
      {/each}
    </tbody>
  </table>
  <button onclick={addConsumable}>Добавить расходник</button>
  <button onclick={clearConsumables}>Очистить</button>
  
  <p>Стоимость расходников: {consumableTotal.toFixed(2)}</p>

  <h2>Себестоимость рецепта: {totalCost.toFixed(2)} ₽</h2>
</main>

<style>
  table {
    border-collapse: collapse;
    width: 100%;
    margin-bottom: 1em;
  }
  th, td {
    border: 1px solid #ddd;
    padding: 8px;
    text-align: left;
  }
  th {
    background-color: #f2f2f2;
  }
  input {
    width: 100%;
    box-sizing: border-box;
  }
</style>
