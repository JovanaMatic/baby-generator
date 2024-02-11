<script setup lang="ts">

  import { Gender, Popularity, Length, names } from '@/data'

  interface OptionsState {
    gender: string,
    popularity: string,
    length: string,
  }

  const options = ref <OptionsState>({
    gender: Gender.BOY,
    popularity: Popularity.TRENDY,
    length: Length.SHORT
  })

  // create another state - Array of names and specify it as an array type of strings

  const selectedNames = ref<string[]>([])

  const computeSelectedNames = () => {
    const filteredNames = names.filter(name => name.gender === options.value.gender)
    .filter(name => name.popularity === options.value.popularity)
    .filter(name => {
      if (name.length === Length.ALL) return true
      else return name.length === options.value.length
    })

    selectedNames.value = filteredNames.map(name => name.name)
  }

  const optionsArray = [
    {
      title: '1) Choose a gender',
      category: 'gender',
      buttons: [ Gender.BOY, Gender.UNISEX, Gender.GIRL ]
    },
    {
      title: '2) Choose the name popularity',
      category: 'popularity',
      buttons: [ Popularity.TRENDY, Popularity.UNIQUE ]
    },
    {
      title: '3) Choose name length',
      category: 'length',
      buttons: [ Length.LONG, Length.ALL, Length.SHORT ]
    }
  ]

  const handleDelete = (index: number) => {
    const filteredNames = [...selectedNames.value]
    filteredNames.splice(index, 1)
    selectedNames.value = filteredNames
  }
</script>

<template>
  <div class="container">
    <h1>Baby Name Generator</h1>
    <p>Choose your options and click "Find Names" button below</p>

    <div class="options-container">

      <Option v-for="option in optionsArray"
       :key="option.title"
       :option="option"
       :options="options"
       />

      <button @click="computeSelectedNames" class="primary">Find names</button>
    </div>
    <div class=cards-container>
      <Names v-for="(name, index) in selectedNames" :key="name" :name="name" @delete="handleDelete(index)" :index="index"/>
    </div>
  </div>
</template>

<style scoped>
  .container {
    font-family: Arial, Helvetica, sans-serif;
    color: rgb(27, 60, 138);
    max-width: 50rem;
    margin: 0 auto;
    text-align: center;
  }

  h1 {
    font-size: 3rem;
  }

  .options-container {
    background-color: rgb(255, 238, 236);
    border-radius: 2rem;
    padding: 1rem;
    width: 95%;
    margin: 0 auto;
    margin-top: 4rem;
    position: relative;
  }

  .primary {
    background-color: rgb(249, 87, 89);
    color: white;
    border-radius: 6.5rem;
    border: none;
    padding: 0.75rem 4rem;
    font-size: 1rem;
    margin-top: 1rem;
    cursor: pointer;
  }

  .cards-container {
    display: flex;
    margin-top: 3rem;
    flex-wrap: wrap;
  }

</style>
