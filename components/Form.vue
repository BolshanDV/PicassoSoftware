<template>
    <section class="company">
      <section class="container">
        <section class="company_title">
          <p>Компания</p>
        </section>
      </section>
      <section>
        <input
            type="text"
            placeholder="Введите название компании"
            v-model="company"
            class="text-field__input"
            :class="{input_active: autocompleteObj.length}"
        >
        <section class="catalog"
                 v-if="autocompleteObj.length"
        >
          <div
              v-for="(companyItem, index) in autocompleteObj"
              class="card"
              @click="chooseCard(companyItem)"
          >
            <section class="card__img">
              <img :src="companyItem.logo" alt="" class="card__logo">
            </section>
            <section class="card__body">
              <div class="card__name">{{companyItem.name}}</div>
              <div class="card__domain">{{companyItem.domain}}</div>
            </section>
          </div>
        </section>
      </section>
    </section>
</template>

<script>
import axios from "axios";
export default {
  name: "Form",
  data() {
    return {
      company: "",
      autocompleteObj: [],
      debounceTimeout: null,
    }
  },
  watch: {
    company: function () {
      if (this.company === '') {
        this.autocompleteObj.length = 0
      }
      this.debounceSearch()
    }
  },

  methods: {
    debounceSearch: function() {
      if (this.debounceTimeout) clearTimeout(this.debounceTimeout);
      this.debounceTimeout = setTimeout(() => {
        this.getCompany();
      }, 500);
    },
    getCompany() {
      axios
        .get(`https://autocomplete.clearbit.com/v1/companies/suggest?query=${this.company}`)
        .then(response => {
          this.autocompleteObj = response.data
        })
        .catch(error => {
          console.log(error);
        })
    },

    chooseCard(companyItem){
      this.company = companyItem.name
    }
  }
}
</script>

<style scoped>
.company {
  width: 30%;
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
}
.company_title {
  font-family: 'Roboto',serif;
  font-style: normal;
  font-weight: 400;
  font-size: 18px;
  line-height: 20px;
  color: #484848;
}
.container {
  margin-bottom: 8px;
  display: flex;
  flex-direction: row;
  justify-content: flex-start;
}
.card {
  display: flex;
  flex-direction: row;
  justify-content: flex-start;
  align-items: center;
  padding: 2% 1%;
}
.card__logo {
  width: 54px;
  height: 54px;
}
.card__img {
  margin-right: 5%;
}
.card__name {
  font-family: 'Roboto';
  font-style: normal;
  font-weight: 400;
  font-size: 14px;
  line-height: 16px;
  color: #000000;
}
.card__domain {
  font-weight: 400;
  font-size: 12px;
  line-height: 14px;
  color: #9F9F9F;
  margin-top: 4px;
}
.catalog {
  border-width: 0 1px 1px 1px;
  border-color: #E5E5E5;
  border-style: solid ;
  box-shadow: 2px 2px 4px rgba(0, 0, 0, 0.1);
  border-radius: 0 0 4px 4px;
  padding: 2% 10%;
}
.input_active {
  border: 1px solid #E5E5E5;
  border-radius: 4px 4px 0 0;
}
</style>
