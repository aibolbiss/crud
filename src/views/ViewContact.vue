<template>
  <div class="container mt-3">
    <div class="row">
      <div class="col">
        <p class="h3 fw-bold">Данные пользователя</p>
        <p>
          Lorem ipsum dolor sit amet consectetur, adipisicing elit. Cumque,
          excepturi tempora? Ducimus nesciunt rerum eius quaerat vel
          praesentium, dolore, alias consectetur culpa, perspiciatis magni nisi
          dolorum odio! Aspernatur, velit. Molestias.
        </p>
      </div>
    </div>
  </div>

  <!-- Spinner -->
  <div v-if="loading">
    <div class="container">
      <div class="row">
        <div class="col">
          <spinner />
        </div>
      </div>
    </div>
  </div>
  <!-- Spinner -->

  <!-- Error Message -->
  <div v-if="!loading && errorMessage">
    <div class="container mt-5">
      <div class="row">
        <div class="col">
          <p class="h3 text-danger fw-bold">{{ errorMessage }}</p>
        </div>
      </div>
    </div>
  </div>
  <!-- Error Message -->

  <div class="container" v-if="!loading && isDone()">
    <div class="row align-items-center">
      <div class="col-md-4">
        <img :src="contact.photo" alt="User Icon" class="contact-img-big" />
      </div>
      <div class="col-md-6">
        <ul class="list-group">
          <li class="list-group-item">
            Имя: <span class="fw-bold">{{ contact.name }}</span>
          </li>
          <li class="list-group-item">
            Почта: <span class="fw-bold">{{ contact.email }}</span>
          </li>
          <li class="list-group-item">
            Мобильный: <span class="fw-bold">{{ contact.mobile }}</span>
          </li>
          <li class="list-group-item">
            Город: <span class="fw-bold">{{ contact.city }}</span>
          </li>
          <li class="list-group-item">
            Компания: <span class="fw-bold">{{ contact.company }}</span>
          </li>
          <li class="list-group-item">
            Специалность: <span class="fw-bold">{{ group.name }}</span>
          </li>
          <li class="list-group-item">
            Contact ID: <span class="fw-bold">{{ contactId }}</span>
          </li>
        </ul>
      </div>
    </div>
    <div class="row mt-5">
      <div class="col">
        <router-link to="/" class="btn btn-info text-white"
          ><i class="fa fa-arrow-alt-circle-left"></i>&nbsp;Назад</router-link
        >
      </div>
    </div>
  </div>
</template>

<script>
import { ContactService } from "@/services/ContactService";
import Spinner from "@/components/Spinner.vue";

export default {
  components: {
    Spinner,
  },
  data() {
    return {
      contactId: this.$route.params.contactId,
      contact: {},
      loading: false,
      errorMessage: null,
      group: {},
    };
  },
  created: async function () {
    try {
      this.loading = true;
      let response = await ContactService.getContact(this.contactId);
      let groupResponse = await ContactService.getGroup(response.data);
      this.contact = response.data;
      this.group = groupResponse.data;
      this.loading = false;
    } catch (error) {
      this.loading = false;
      this.errorMessage = error;
    }
  },
  methods: {
    isDone: function () {
      return (
        Object.keys(this.contact).length > 0 &&
        Object.keys(this.group).length > 0
      );
    },
  },
};
</script>