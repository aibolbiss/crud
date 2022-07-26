<template>
  <div class="container mt-3">
    <div class="row">
      <div class="col">
        <p class="h3 fw-bold">
          CRM для создания контактов&nbsp;
          <router-link
            to="/contacts/add"
            class="btn btn-info text-white btn-sm"
          >
            <i class="fa fa-plus-circle"></i>&nbsp;Создать</router-link
          >
        </p>
        <p>
          Lorem ipsum dolor sit amet consectetur, adipisicing elit. Cumque,
          excepturi tempora? Ducimus nesciunt rerum eius quaerat vel
          praesentium, dolore, alias consectetur culpa, perspiciatis magni nisi
          dolorum odio! Aspernatur, velit. Molestias.
        </p>
        <form>
          <div class="row">
            <div class="col-md-6">
              <div class="row">
                <div class="col">
                  <input
                    type="text"
                    class="form-control"
                    placeholder="Поиск по имени"
                  />
                </div>
                <div class="col">
                  <input
                    type="submit"
                    class="btn btn-outline-dark"
                    value="Поиск"
                  />
                </div>
              </div>
            </div>
          </div>
        </form>
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

  <div class="container mt-3" v-if="contacts.length > 0">
    <div class="row">
      <div
        class="col-md-6"
        v-for="contact in contacts.reverse()"
        :key="contact"
      >
        <div class="card my-2 list-group-item-info shadow-lg">
          <div class="card-body">
            <div class="row align-items-center">
              <div class="col-sm-4">
                <img :src="contact.photo" alt="User Icon" class="contact-img" />
              </div>
              <div class="col-sm-7">
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
                </ul>
              </div>
              <div
                class="
                  col-sm-1
                  d-flex
                  flex-column
                  justify-content-center
                  align-items-center
                "
              >
                <router-link
                  :to="`/contacts/view/${contact.id}`"
                  class="btn btn-warning my-1"
                  ><i class="fa fa-eye"></i>
                </router-link>
                <router-link
                  :to="`/contacts/edit/${contact.id}`"
                  class="btn btn-primary my-1"
                  ><i class="fa fa-pen"></i>
                </router-link>
                <button
                  class="btn btn-danger my-1"
                  @click="clickDeleteContact(contact.id)"
                >
                  <i class="fa fa-trash"></i>
                </button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>

  <div class="container mt-3 d-flex justify-content-center">
    <nav aria-label="Page navigation example">
      <ul class="pagination">
        <li class="page-item">
          <a class="page-link" href="#" aria-label="Previous">
            <span aria-hidden="true">&laquo;</span>
            <span class="sr-only">Назад</span>
          </a>
        </li>
        <li class="d-flex">
          <a
            class="page-link"
            href="#"
            v-for="contact in contacts.length"
            :key="contact"
            >{{ contact }}</a
          >
        </li>

        <li class="page-item">
          <a class="page-link" href="#" aria-label="Next">
            <span aria-hidden="true">&raquo;</span>
            <span class="sr-only">Вперед</span>
          </a>
        </li>
      </ul>
    </nav>

    <!-- <button @click="nazad">Назад</button>
    <button
      v-for="contact in contacts.length"
      :key="contact"
      @click="number(index)"
    >
      {{ contact }}
    </button>
    <button @click="vpered">Вперед</button>
    <h5>Страница {{ page }} из {{ contacts.length }}</h5> -->
  </div>
</template>

<script>
import { ContactService } from "@/services/ContactService.js";
import Spinner from "@/components/Spinner.vue";

export default {
  data() {
    return {
      loading: false,
      contacts: [],
      errorMessage: null,
    };
  },
  created: async function () {
    try {
      this.loading = true;
      let response = await ContactService.getAllContacts();
      this.contacts = response.data;
      this.loading = false;
    } catch (error) {
      this.errorMessage = error;
      this.loading = false;
    }
  },
  methods: {
    async clickDeleteContact(contactId) {
      try {
        this.loading = true;
        let response = await ContactService.deleteContact(contactId);
        if (response) {
          let response = await ContactService.getAllContacts();
          this.contacts = response.data;
          this.loading = false;
        }
      } catch (error) {
        this.errorMessage = error;
        this.loading = false;
      }
    },
  },
  components: { Spinner },
};
</script>