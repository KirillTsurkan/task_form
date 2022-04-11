<template>
  <div class="container">
    <form
      ref="forma"
      class="form"
      @submit.prevent="sendMessage"
    >
      <h1 class="content__title">
        Форма подачи заявки в отдел сервиса и качества
      </h1>

      <section class="content">
        <div class="select">
          <h4 class="select__title title">
            Ваш филиал<span class="required-mark">*</span>
          </h4>
          <select
            class="select__list"
            v-model="form.selected"
            :disabled="!!form.checked"
            required
          >
            <option disabled value="">Выберите город</option>
            <option
              v-for="city in cities"
              :key="city.id"
              v-bind:value="city.title"
            >
              {{ city.title }}
            </option>
          </select>
          <div class="select__checkbox-wrapper">
            <input
              class="select__checkbox"
              type="checkbox"
              id="checkbox"
              @change="clearCity"
              v-model="form.checked"
            />
            <label for="checkbox">{{}}</label>
            <p class="select__online">Онлайн</p>
          </div>
        </div>
        <div class="request-subject">
          <h4 class="request-subject__title title">
            Тема обращения<span class="required-mark">*</span>
          </h4>
          <div class="request-subject__wraper-input">
            <input
              class="request-subject__input-radio"
              type="radio"
              id="one"
              value="Недоволен качеством услуг"
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
              value="Расторжение договора"
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
              value="Не приходит письмо активации на почту"
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
              value="Не работает личный кабинет"
              @change="form.shortmessage = null"
              v-model="form.picked"
            />
            <label for="four">Не работает личный кабинет</label>
          </div>
          <input
            class="request-subject_other-request"
            v-model="form.shortmessage"
            type="text"
            @input="clearRadio"
            placeholder="другое"
          />
        </div>
        <div class="description-problem">
          <h4 class="description-problem__title title">
            Описание проблемы<span class="required-mark">*</span>
          </h4>
          <textarea
            class="description-problem__textarea"
            v-model="form.longmessage"
            placeholder="опишите свою проблему"
            required
          ></textarea>
        </div>
        <div class="download-documents">
          <h4 class="download-documents__title title">Загрузка документов</h4>
          <p class="download-documents__message-info">
            Приложите,пожалуйста, полноценный скриншот. Это поможет быстрее
            решить проблему
          </p>
          <input
            class="download-documents__button"
            type="file"
            @change="selectFile($event)"
            ref="file"
          />
        </div>
        <button
          :disabled="isButtonDisabled"
          :class="{
            disabled: isButtonDisabled,
          }"
          class="buttonSubmit"
        >
          ОТПРАВИТЬ
        </button>
      </section>
    </form>
  </div>

  <modal v-model:show="modalVisible"> </modal>
</template>

<script>
import Modal from "@/components/Modal";
import axios from "axios";
export default {
  components: {
    Modal,
  },
  data() {
    return {
      form: {
        selected: "",
        checked: false,
        picked: false,
        shortmessage: "",
        longmessage: "",
      },
      file: null,
      cities: "",
      isActive: true,
      modalVisible: false,
    };
  },
  //жизненный цикл (получаем запрос с сервера)
  mounted() {
    axios
      .get("https://624d935653326d0cfe4f0ab4.mockapi.io/api/v1/cities")
      .then((res) => {
        this.cities = res.data;
      })
      .catch((error) => console.log(error));
  },
  methods: {
    selectFile(event) {
      this.file = event.target.files[0];
    },
    showModal() {
      this.modalVisible = true;
    },
    clearRadio() {
      this.form.picked = null;
    },
    clearCity() {
      this.form.selected = null;
    },
    resetForm() {
      this.$refs.file.value = "";
      this.file = null;
      this.form.checked = false;
      this.form.picked = false;
      this.form.shortmessage = null;
      this.form.longmessage = null;
      this.form.selected = null;
    },
    //функция отправки запроса
    sendMessage() {
      const formData = new FormData();
      formData.append("file", this.file);
      formData.append("data", JSON.stringify(this.form));
      axios
        .post(
          "https://624d935653326d0cfe4f0ab4.mockapi.io/api/v1/send-form", // если без файла, то можно  использовать this.form
          formData,
          { "Content-Type": "multipart/form-data" }
        )
        .then((res) => {
          if (res.data.success === true) {
            this.showModal();
            this.resetForm();
          } else {
            alert("Ошибка отправка заявки");
          }
        })
        .catch((error) => console.log(error));
    },
  },
  computed: {
    isButtonDisabled() {
      const { selected, checked, picked, shortmessage, longmessage } =
        this.form;
      return (
        (!selected && !checked) || (!picked && !shortmessage) || !longmessage
      );
    },
  },
};
</script>

//стилизация
<style scoped>
.container {
  position: relative;
}
.title {
  margin-bottom: 10px;
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
  font-size: 20px;
  align-self: center;
}
.select {
  display: flex;
  flex-direction: column;
  justify-content: start;
  align-items: flex-start;
  margin: 30px 0 0 25px;
}

.select__online {
  margin-left: 15px;
  align-self: center;
}
.select__list {
  padding: 5px;
  max-width: 250px;
  width: 100%;
  cursor: pointer;
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
  cursor: pointer;
}
.form {
  margin-top: 20px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
.request-subject {
  display: flex;
  flex-direction: column;
  justify-content: start;
  align-items: flex-start;
  margin: 30px 0 0 25px;
}

.request-subject__input-radio {
  min-height: 30px;
  max-width: 30px;
  width: 100%;
  margin-right: 10px;
  cursor: pointer;
}
.request-subject__wraper-input {
  margin-bottom: 10px;
  display: flex;
  align-items: center;
  width: 100%;
}

.request-subject_other-request {
  font-size: 16px;
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
  font-size: 18px;
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
  max-width: 400px;
}
.download-documents__message-info {
  text-align: left;
  margin-bottom: 20px;
}
.download-documents__button {
  cursor: pointer;
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
  transition: 1s linear;
}

.buttonSubmit:hover {
  transition: linear 1s;
}
.disabled {
  padding: 10px;
  max-width: 100px;
  width: 100%;
  color: aliceblue;
  border: none;
  margin: 30px 0 0 25px;
  cursor: pointer;
  background: rgb(67, 65, 63);
}
.required-mark {
  color: red;
}
</style>
