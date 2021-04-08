<template>
  <div class="w-full flex justify-center">
    <input
      type="text"
      placeholder="Enter Pokemon here"
      class="mt-10 p-2 border-blue-500 border-2"
    />
  </div>
  <div>{{ pokemons }}</div>
  <div>{{ testPokemon.name }}</div>
  <div class="mt-10 p-4 flex flex-wrap justify-center">
    <div
      class="ml-4 text-2x text-blue-200"
      v-for="(pokemon, idx) in pokemons"
      :key="idx"
    >
      {{ idx }}
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, reactive, toRefs } from 'vue';

interface IPokemonResponse {
  count: number;
  results: IResultPokemon[];
}

interface IResultPokemon {
  name: string;
  url: string;
}

interface IPokemon {
  [key: string]: number;
}

const testPokemon: IResultPokemon = {
  name: 'Pikachu',
  url: 'http://pikachu.com',
};

export default defineComponent({
  name: 'Home',
  setup() {
    const state = reactive<{
      pokemons: IPokemon[];
      urlIdLookup: Record<string, unknown>;
    }>({
      pokemons: [],
      urlIdLookup: {},
    });
    fetch('https://pokeapi.co/api/v2/pokemon?offset=0')
      .then((res) => res.json())
      .then((data: IPokemonResponse) => {
        state.pokemons = data.results.reduce(
          (acc, cur, idx) => (acc = { ...acc, [cur.name]: idx + 1 }),
          {} as IPokemon[]
        );
      })
      .catch((err) => console.log(err));
    return {
      testPokemon,
      ...toRefs(state),
    };
  },
  components: {},
});
</script>
