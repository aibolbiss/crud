<template>
  <div class="container mt-3">
    <div class="row">
      <div class="col">
        <p class="h3 fw-bold">Добавить пользователя</p>
        <p>
          Lorem ipsum dolor sit amet consectetur, adipisicing elit. Cumque,
          excepturi tempora? Ducimus nesciunt rerum eius quaerat vel
          praesentium, dolore, alias consectetur culpa, perspiciatis magni nisi
          dolorum odio! Aspernatur, velit. Molestias.
        </p>
      </div>
    </div>
  </div>
  <div class="container mt-3">
    <div class="row">
      <div class="col-md-3">
        <form @submit.prevent="submitCreate()">
          <div class="mb-2">
            <input
              v-model="contact.name"
              type="text"
              class="form-control"
              placeholder="Имя"
              required
            />
          </div>
          <div class="mb-2">
            <input
              v-model="contact.email"
              type="email"
              class="form-control"
              placeholder="Почта"
              required
            />
          </div>
          <div class="mb-2">
            <input
              v-model="contact.mobile"
              type="number"
              class="form-control"
              placeholder="Мобильный"
              required
            />
          </div>
          <div class="mb-2">
            <input
              v-model="contact.city"
              type="text"
              class="form-control"
              placeholder="Город"
              required
            />
          </div>
          <div class="mb-2">
            <input
              v-model="contact.company"
              type="text"
              class="form-control"
              placeholder="Компания"
              required
            />
          </div>
          <div class="mb-2">
            <input
              v-model="contact.photo"
              type="text"
              class="form-control"
              placeholder="URL Фото"
              required
            />
          </div>
          <div class="mb-2">
            <select
              v-model="contact.group_id"
              class="form-control"
              v-if="groups.length > 0"
              required
            >
              <option selected disabled value>Выберите специалность</option>
              <option :value="group.id" v-for="group in groups" :key="group.id">
                {{ group.name }}
              </option>
            </select>
          </div>
          <div class="mb-2">
            <input
              type="submit"
              class="btn btn-info text-white"
              value="Создать"
            />
          </div>
        </form>
      </div>
      <div class="col-md-4">
        <img
          src="https://icon-library.com/images/businessman_863430.png"
          alt="User Icon"
          class="contact-img"
        />
      </div>
    </div>
  </div>
</template>

<script>
import { ContactService } from "../services/ContactService";

export default {
  data() {
    return {
      contact: {
        name: "",
        email: "",
        mobile: "",
        city: "",
        company: "",
        photo: "",
        group_id: "",
      },
      groups: [],
    };
  },
  created: async function () {
    try {
      let response = await ContactService.getAllGroups();
      this.groups = response.data;
    } catch (error) {
      console.log(error);
    }
  },
  methods: {
    submitCreate: async function () {
      try {
        let response = await ContactService.createContact(this.contact);
        if (response) {
          return this.$router.push("/");
        } else {
          return this.$router.push("/contacts/add");
        }
      } catch (error) {
        console.log(error);
      }
    },
  },
};
</script>