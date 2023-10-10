<script setup lang="ts">
import { names } from "@/data";
interface OptionState {
  gender: "Girl" | "Boy" | "Unisex";
  popularity: "Trendy" | "Unique";
  length: "Long" | "Short" | "All";
}

const options = reactive<OptionState>({
  gender: "Girl",
  popularity: "Unique",
  length: "Short",
});

const selectedNames = ref<string[]>([]);

const removeName = (index: number) => {
  const filteredNames = [...selectedNames.value]
  filteredNames.splice(index, 1)
  selectedNames.value = filteredNames
}

const computeSelectedNames = (): void => {
  const filterNames = names
    .filter((name) => name.gender == options.gender)
    .filter((name) => name.popularity == options.popularity)
    .filter((name) => {
      if (options.length === "All") {
        return true;
      } else return name.length === options.length;
    });

  selectedNames.value = filterNames.map((name) => name.name);
};

const optionsArray = [
  {
    title: "1) Choose a gender",
    category: "gender",
    buttons: ["Girl", "Unisex", "Boy"],
  },
  {
    title: "2) Choose the name's popularity",
    category: "popularity",
    buttons: ["Trendy", "Unique"],
  },
  {
    title: "3) Choose name's length",
    category: "length",
    buttons: ["Short", "All", "Long"],
  },
];
</script>

<template>
  <div class="container">
    <h1>Name Generator</h1>
    <p>Choose your options and click the "Find Names" buttom below</p>
    <div class="options-container">
      <Option
        v-for="option in optionsArray"
        :key="option.title"
        :option="option"
        :options="options"
      />
      <button class="primary" @click="computeSelectedNames">Find Names</button>
    </div>
    <div class="cards-container">
      <CardName v-for="(name, index) in selectedNames" :key="name" :name="name" @remove="() => removeName(index)" :index="index" />
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


</style>
