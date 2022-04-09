<template>
  <form
    enctype="multipart/form-data"
    class="form"
    @submit.prevent="sendMessage"
  >
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
            v-model="form.shortmessage"
            type="text"
            @input="clearRadio"
            placeholder="другое"
          />
        </div>
        <div class="description-problem">
          <h4 class="description-problem__title">Описание проблемы*</h4>
          <textarea
            class="description-problem__textarea"
            v-model="form.longmessage"
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
          <input type="file" @change="selectFile($event)" ref="file" />
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
    </div>
  </form>
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
        value: "",
        cities: "",
        checked: false,
        picked: false,
        shortmessage: "",
        longmessage: "",
      },
      file: "",
      isActive: true,
      modalVisible: false,
    };
  },

  created() {
    axios
      .get("https://624d935653326d0cfe4f0ab4.mockapi.io/api/v1/cities")
      .then((res) => {
        this.form.cities = res.data;
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
      this.form.value = null;
    },
    sendMessage() {
      console.log("iii");
      const formData = new FormData();
      formData.append("file", this.file);
      formData.append("data", this.form);

      // formData.append("data", this.form);
      axios
        .post(
          "https://624d935653326d0cfe4f0ab4.mockapi.io/api/v1/send-form",
          this.form,
          {
            method: "POST",
            headers: {
              "Content-Type": "multipart/form-data",
            },
            body: formData,
          }
        )
        .then((res) => {
          if (res.data.success === true) {
            this.form.value = null;
            this.form.checked = false;
            this.form.picked = false;
            this.form.shortmessage = null;
            this.form.longmessage = null;
            this.showModal();
          } else {
            alert("Ошибка отправка заявки");
          }
          console.log(res);
        })
        .catch((error) => console.log(error));
    },
  },
  computed: {
    isButtonDisabled() {
      const { value, checked, picked, shortmessage, longmessage } = this.form;
      return (!value && !checked) || (!picked && !shortmessage) || !longmessage;
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
</style>
