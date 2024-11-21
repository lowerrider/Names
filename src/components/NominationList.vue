<template>
  <div class="container">
    <!-- Грид для расположения карточек в 3 колонки -->
    <div class="grid">
      <!-- Перебираем ответы -->
      <div v-for="(answer, index) in answers" :key="index" class="voter-card">
        <!-- Имя голосующего -->
        <h2 class="voter-name">{{ getPersonName(answer.personId) }}</h2>

        <!-- Перебираем номинации -->
        <div class="nominations">
          <div
            v-for="(nominationKey, index) in nominations"
            :key="index"
            class="nomination">
            <!-- Отображаем полное название номинации -->
            <h3>{{ nominationNames[nominationKey] }}</h3>
            <!-- Проверяем, есть ли голосование в этой номинации -->
            <p v-if="answer[nominationKey]">
              <span class="voted-name">{{
                getPersonName(answer[nominationKey])
              }}</span>
            </p>
            <p v-else class="no-vote">Не было голосования в этой номинации</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "NominationList",
  props: {
    answers: {
      type: Array,
      required: true,
    },
    members: {
      type: Array,
      required: true,
    },
  },
  data() {
    return {
      nominations: ["unesko", "maya", "tekila", "chily", "derty"],
      nominationNames: {
        unesko: "НАСЛЕДИЕ ЮНЕСКО",
        maya: "ЗАГАДОЧНЫЙ МАЙЯ",
        tekila: "ТЕКИЛА ЛЮБОВЬ",
        chily: "ОСТРЫЙ КАК ПЕРЕЦ ЧИЛИ",
        derty: "ГРЯЗНЫЙ САНЧЕС",
      },
    };
  },
  methods: {
    getPersonName(personId) {
      const person = this.members.find(
        (member) => member.personId === personId
      );
      return person ? person.title : "Неизвестный человек";
    },
  },
};
</script>

<style scoped>
/* Общий контейнер */
.container {
  display: flex;
  justify-content: center;
  padding: 20px;
  background-color: #f4f7fc;
}

/* Сеточная система для колонок */
.grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr); /* 3 колонки */
  gap: 20px; /* Отступы между карточками */
  width: 100%;
  max-width: 1200px;
  grid-auto-rows: 1fr;
}

/* Карточка голосующего */
.voter-card {
  background-color: #fff;
  border-radius: 12px;
  padding: 20px;
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.1);
  min-height: 300px;
  transition: all 0.3s ease;
}

.voter-card:hover {
  transform: translateY(-10px);
  box-shadow: 0 6px 20px rgba(0, 0, 0, 0.15);
}

/* Имя голосующего */
.voter-name {
  font-size: 1.6rem;
  font-weight: 700;
  color: #333;
  margin-bottom: 15px;
  font-family: "Roboto", sans-serif;
}

/* Блок с номинациями */
.nominations {
  display: flex;
  flex-direction: column;
  gap: 15px;
}

/* Заголовок номинации */
.nomination h3 {
  font-size: 1.2rem;
  font-weight: 600;
  color: #4a90e2;
  margin-bottom: 8px;
  font-family: "Roboto", sans-serif;
}

/* Имя выбранного человека (за кого проголосовали) */
.voted-name {
  font-size: 1.1rem;
  color: #4caf50;
  font-weight: 500;
}

/* Текст, когда голосования нет */
.no-vote {
  font-size: 1.1rem;
  color: #f44336;
  font-style: italic;
  font-weight: 500;
}

/* Добавим отступы и разделим карточки по колонкам */
@media (max-width: 1024px) {
  .grid {
    grid-template-columns: repeat(2, 1fr); /* 2 колонки на средних экранах */
  }
}

@media (max-width: 768px) {
  .grid {
    grid-template-columns: 1fr; /* 1 колонка на мобильных экранах */
  }
}
</style>
