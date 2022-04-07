<template>
  <form class="form" @submit.prevent="">
    <div class="conteiner">
      <h1 class="content__title">
        Форма подачи заявки в отдел сервиса и качества
      </h1>
      <section class="content">
        <div class="select">
          <h4 class="select__title">Ваш филиал*</h4>
          <select
            class="select__list"
            v-model="form.value"
            :disabled="!!form.checked"
            required
          >
            <option disabled value="">Выберите город</option>
            <option
              v-for="city in form.cities"
              :key="city.id"
              v-bind:value="form.cities.title"
            >
              {{ city.title }}
            </option>
          </select>
          <div class="select__checkbox-wrapper">
            <input
              class="select__checkbox"
              type="checkbox"
              id="checkbox"
              v-model="form.checked"
            />
            <label for="checkbox">{{}}</label>
            <p class="select__online">Онлайн</p>
          </div>
        </div>
        <div class="request-subject">
          <h4 class="request-subject__title">Тема обращения*</h4>
          <div class="request-subject__wraper-input">
            <input
              class="request-subject__input-radio"
              type="radio"
              id="one"
              value="Один"
              @change="form.shortmessage = null"
              v-model="form.picked"
              required
            />
            <label for="one" required>Недоволен качеством услуг</label>
          </div>
          <div class="request-subject__wraper-input">
            <input
              class="request-subject__input-radio"
              type="radio"
              id="two"
              value="Два"
              @change="form.shortmessage = null"
              v-model="form.picked"
              required
            />
            <label for="two">Расторжение договора</label>
          </div>
          <div class="request-subject__wraper-input">
            <input
              class="request-subject__input-radio"
              type="radio"
              id="three"
              value="три"
              @change="form.shortmessage = null"
              v-model="form.picked"
            />
            <label for="three">Не приходит письмо активации на почту</label>
          </div>
          <div class="request-subject__wraper-input">
            <input
              class="request-subject__input-radio"
              type="radio"
              id="four"
              value="четыре"
              @change="form.shortmessage = null"
              v-model="form.picked"
            />
            <label for="four">Не работает личный кабинет</label>
          </div>
          <input
            class="request-subject_other-request"
            v-model.lazy="form.shortmessage"
            type="text"
            @input="clearRadio"
            placeholder="другое"
          />
        </div>
        <div class="description-problem">
          <h4 class="description-problem__title">Описание проблемы*</h4>
          <textarea
            class="description-problem__textarea"
            v-model.lazy="form.longmessage"
            placeholder="введите несколько строчек"
            required
          ></textarea>
        </div>
        <div class="download-documents">
          <h4 class="download-documents__title">Загрузка документов</h4>
          <p class="download-documents__message-info">
            Приложите,пожалуйста, полноценный скриншот. Это поможет быстрее
            решить проблему
          </p>
          <input type="file" id="file" ref="file" />
        </div>
        <button class="buttonSubmit" @click="sendMessage">ОТПРАВИТЬ</button>
      </section>
    </div>
  </form>
</template>

<script>
import axios from "axios";
// import FileInput from "vue-simple-file-input";
export default {
  // components: {
  //   FileInput,
  // },
  data() {
    return {
      form: {
        value: '',
        cities: [],
        checked: false,
        picked: false,
        shortmessage: "",
        longmessage: "",
        file: null,
      },
    };
  },
  created() {
    axios
      .get("https://624d935653326d0cfe4f0ab4.mockapi.io/api/v1/cities")
      .then((res) => {
        console.log(res);
        this.form.cities = res.data;
      })
      .catch((error) => console.log(error));
  },

  methods: {
    // clearText() {
    //   this.form.checked = "";
    // },
    clearRadio() {
      console.log("clearRadio");
      this.form.picked = null;
    },
    sendMessage() {
      axios
        .post("https://624d935653326d0cfe4f0ab4.mockapi.io/api/v1/send-form", {})
        .then((res) => {console.log(res)})
        .catch((error) => console.log(error));
      this.form.value = null;
      this.form.cities = null;
      this.form.checked = false;
      this.form.picked = false;
      this.form.shortmessage = null;
      this.form.longmessage = null;
      this.form.file = null;
    },
  },
};
</script>


<style scoped>
.conteiner {
  margin-top: 20px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
.content {
  margin-top: 15px;
  display: flex;
  flex-direction: column;
  max-width: 700px;
  width: 100%;
  border: 2px solid grey;
  min-height: 800px;
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
.select__online {
  margin-left: 15px;
  align-self: center;
}
.select__list {
  padding: 5px;
  max-width: 250px;
  width: 100%;
}
.select__checkbox-wrapper {
  margin-top: 20px;
  display: flex;
  max-width: 250px;
  width: 100%;
  justify-content: start;
  align-items: center;
}
.select__checkbox {
  min-height: 30px;
  max-width: 30px;
  width: 100%;
}
.form {
  position: relative;
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

.description-problem__title {
  margin-bottom: 10px;
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
.download-documents__title {
  margin-bottom: 10px;
}
.download-documents {
  display: flex;
  flex-direction: column;
  justify-content: start;
  align-items: flex-start;
  margin: 30px 0 0 25px;
  max-width: 400px;
}
.download-documents__message-info {
  text-align: left;
  margin-bottom: 20px;
}

.buttonSubmit {
  padding: 10px;
  max-width: 100px;
  width: 100%;
  color: aliceblue;
  background: rgb(228, 112, 34);
  border: none;
  margin: 30px 0 0 25px;
  cursor: pointer;
}
</style>
