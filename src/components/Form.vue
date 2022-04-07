<template>
  <form class="form" @submit.prevent="">
    <div class="conteiner">
      <h1 class="content__title">
        Форма подачи заявки в отдел сервиса и качества
      </h1>
      <section class="content">
        <div class="select">
          <p class="select__title">Ваш филиал*</p>
          <select
            class="select__list"
            v-model="selected"
            :disabled="cheked"
            required
          >
            <option disabled value="">Выберите город</option>
            <option
              v-for="city in cities"
              :key="city.id"
              v-bind:value="cities.title"
            >
              {{ city.title }}
            </option>
          </select>
          <input
            class="select__checkbox"
            type="checkbox"
            id="checkbox"
            v-model="checked"
          />
          <label for="checkbox">{{}}</label>
        </div>
        <div class="request-subject">
          <p class="request-subject__title">Тема обращения*</p>
          <div class="request-subject__wraper-input">
            <input
              class="request-subject__input-radio"
              type="radio"
              id="one"
              value="Один"
              v-model="picked"
            />
            <label for="one">Недоволен качеством услуг</label>
          </div>
          <div class="request-subject__wraper-input">
            <input
              class="request-subject__input-radio"
              type="radio"
              id="two"
              value="Два"
              v-model="picked"
            />
            <label for="two">Расторжение договора</label>
          </div>
          <div class="request-subject__wraper-input">
            <input
              class="request-subject__input-radio"
              type="radio"
              id="three"
              value="три"
              v-model="picked"
            />
            <label for="three">Не приходит письмо активации на почту</label>
          </div>
          <div class="request-subject__wraper-input">
            <input
              class="request-subject__input-radio"
              type="radio"
              id="four"
              value="четыре"
              v-model="picked"
            />
            <label for="four">Не работает личный кабинет</label>
          </div>
          <input
            class="request-subject_other-request"
            v-model="shortmessage"
            type="text"
            placeholder="другое"
          />
        </div>
        <div class="description-problem">
          <p class="description-problem__title">Описание проблемы*</p>
          <textarea
            class="description-problem__textarea"
            v-model="longmessage"
            placeholder="введите несколько строчек"
            required
          ></textarea>
        </div>
        <div class="download-documents">
          <p class="download-documents__title">Загрузка документов</p>
          <p class="clas">
            Приложите,пожалуйста, полноценный скриншот. Это поможет быстрее
            решить проблему
          </p>
          <input
            type="file"
            id="file"
            ref="file"
            v-on:change="handleFileUpload()"
          />
        </div>
        <button class="buttonSubmit" @click="createPost">ОТПРАВИТЬ</button>
      </section>
    </div>
  </form>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      selected: {},
      cities: [],
      checked: false,
      picked: false,
      shortmessage: "",
      longmessage: "",
    };
  },
  mounted() {
    axios
      .get("https://624d935653326d0cfe4f0ab4.mockapi.io/api/v1/cities")
      .then((res) => {
        console.log(res);
        this.cities = res.data;
        console.log(res.data);
        console.log(this.cities);
      });
  },
};
</script>


<style scoped>
.conteiner {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
.content {
  display: flex;
  flex-direction: column;
  max-width: 700px;
  width: 100%;
  border: 2px solid grey;
  min-height: 750px;
}
.content__title {
  font-size: 16px;
  align-self: center;
}
.select {
  display: flex;
  flex-direction: column;
  justify-content: start;
  align-items: flex-start;
  margin: 30px 0 0 25px;
}
.select__title {
  margin-bottom: 10px;
}
.select__list {
  padding: 5px;
  max-width: 250px;
  width: 100%;
}
.select__checkbox {
  margin-top: 10px;
  min-height: 30px;
  max-width: 30px;
  width: 100%;
}
.request-subject {
  display: flex;
  flex-direction: column;
  justify-content: start;
  align-items: flex-start;
  margin: 30px 0 0 25px;
}
.request-subject__title {
  margin-bottom: 10px;
}
.request-subject__input-radio {
  min-height: 30px;
  max-width: 30px;
  width: 100%;
  margin-right: 10px;
}
.request-subject__wraper-input {
  margin-bottom: 10px;
  display: flex;
  align-items: center;
  width: 100%;
}

.request-subject_other-request {
  padding: 5px;
  max-width: 250px;
  width: 100%;
}
.description-problem {
  display: flex;
  flex-direction: column;
  justify-content: start;
  align-items: flex-start;
  margin: 30px 0 0 25px;
}

.description-problem__textarea {
  max-width: 650px;
  width: 100%;
  height: 100px;
}
.download-documents {
  display: flex;
  flex-direction: column;
  justify-content: start;
  align-items: flex-start;
  margin: 30px 0 0 25px;
}

.buttonSubmit {
  padding: 10px;
  max-width: 150px;
  width: 100%;
  color: aliceblue;
  background: grey;
  border: none;
  margin: 30px 0 0 25px;
}
</style>
