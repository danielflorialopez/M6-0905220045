<template>
  <div id="app">
    <div class="hero is-white is-gradient is-bold">
      <div class="hero-body">
        <h1 class="title">
          <span class="has-text-success">Ronald Ulysses Swanson</span>
        </h1>
        <h2 class="title">
          <span class="subtitle">The Man. The Myth. The Legend.</span>
        </h2>
        <br />

        <div class="field has-addons is-pulled-right">
          <div class="control">
            <input
              v-model="search"
              type="text"
              class="input is-rounded"
              v-on:keyup.enter="searchData"
            />
          </div>
          <div class="control">
            <button
              class="button is-success is-rounded"
              v-on:click="searchData"
            >
              Search
            </button>
          </div>
        </div>
      </div>
    </div>

    <div class="container">
      <div
        class="columns is-desktop is-mobile is-tablet is-multiline is-centered"
      ></div>
      <quote
        v-for="quote of quotes"
        v-bind:key="quote.id"
        v-bind:quote="quote"
      />
      <nav class="pagination" role="navegation" aria-label="pagination">
        <a class="pagination-previous" v-on:click="changePage(page - 1)"
          >Previous</a
        >
        <ul class="pagination-list">
          <li>
            <a class="pagination-link is-current">{{ page }}</a>
          </li>
        </ul>
        <a class="pagination-next" v-on:click="changePage(page + 1)">Next</a>
      </nav>
    </div>

    <!--<div v-for="quote of quotes" v-bind:key="quote.id">
      {{ quote }}
    </div>-->
  </div>
</template>

<script>
import axios from "axios";

import Quote from "./components/Quote";

export default {
  name: "App",
  components: {
    Quote,
  },
  data: function () {
    return {
      quotes: [],
      page: 1,
      pages: 1,
      search: "",
    };
  },
  created() {
    this.fetch();
  },
  methods: {
    fetch() {
      const params = {
        page: this.page,
        stringtomatch: this.search,
      };

      let result = axios
        .get("https://ron-swanson-quotes.herokuapp.com/v2/quotes/search/%20/", {
          params,
        })
        .then((response) => {
          this.quotes = response.data;
          console.log(response.data.info);
          this.pages = response.data.info.pages;
          console.log(response.data);
        })
        .catch((error) => {
          console.log(error);
        });
    },
    changePage(page) {
      this.page = page <= 0 || page > this.pages ? this.page : page;
      this.fetch();
    },
    searchData() {
      this.page = 1;
      this.fetch();
    },
  },
};
</script>