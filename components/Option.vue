<script setup lang="ts">
  import { Gender, Popularity, Length } from '@/data.ts'

  interface OptionProps {
    option: {
      title: string,
      category: string,
      buttons: Gender[] | Popularity[] | Length[]
    },
    options: {
      gender: Gender,
      popularity: Popularity,
      length: Length
    }
  }

  const props = defineProps<OptionProps>()

  const computedButtonClasses = (value, index) => {
    const className = []

    if (props.options[props.option.category] === value) {
      className.push('option-active')
    }
    if (index === 0) className.push('option-left')
    if (index === props.option.buttons.length - 1) className.push('option-right')

    return className.join(" ")
  }

</script>

<template>
  <div class="option-container">
        <h4>{{ option.title }}</h4>
        <div class="option-buttons">
          <button v-for="(value, index) in option.buttons"
           class="option"
           :class="computedButtonClasses(value, index)"
           @click="options[option.category] = value"
          >{{ value }}
          </button>
        </div>
      </div>
</template>

<style scoped>
  .option-container {
    margin-bottom: 2rem;
  }

  .option {
    background-color: white;
    outline: 0.15rem solid rgb(249, 87, 89);
    border: none;
    padding: 0.75rem;
    width: 12rem;
    font-size: 1rem;
    color: rgb(27, 60, 138);
    cursor: pointer;
    font-weight: 200;
  }

  .option-left {
    border-radius: 1rem 0 0 1rem;
  }

  .option-right {
    border-radius: 0rem 1rem 1rem 0;
  }

  .option-active {
    background-color: rgb(249, 87, 89);
    color: white;
  }
</style>