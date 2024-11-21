<template>
  <div id="app">
    <h1>Голосование</h1>
    <div v-if="loading">Загрузка...</div>
    <NominationList v-else :members="members" :answers="answers" />
  </div>
</template>

<script>
import { ref, onMounted } from "vue";
import axios from "axios";
import NominationList from "./components/NominationList.vue";

export default {
  name: "App",
  components: {
    NominationList,
  },
  setup() {
    const members = ref([]);
    const answers = ref([]);
    const loading = ref(true);

    // Загрузка данных с API
    const fetchData = async () => {
      try {
        // Получаем список людей
        const responseMembers = await axios.get(
          "https://b5862cf2cca63d34.mokky.dev/members"
        );
        members.value = responseMembers.data;

        // Получаем ответы на голосование
        const responseAnswers = await axios.get(
          "https://b5862cf2cca63d34.mokky.dev/answers"
        );
        answers.value = responseAnswers.data;
      } catch (error) {
        console.error("Ошибка при загрузке данных:", error);
      } finally {
        loading.value = false;
      }
    };

    // Загружаем данные при монтировании компонента
    onMounted(fetchData);

    return {
      members,
      answers,
      loading,
    };
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
}
</style>
