<template>
  <nav class="navbar navbar-default">
    <div class="container-fluid">
      <div class="navbar-header">
        <router-link to="/" class="navbar-brand">Stock Trader</router-link>
      </div>

      <!-- Collect the nav links, forms, and other content for toggling -->
      <div class="collapse navbar-collapse">
        <ul class="nav navbar-nav">
          <router-link to="/portfolio" activeClass="active" tag="li">
            <a>Portfolio</a>
          </router-link>
          <router-link to="/stocks" activeClass="active" tag="li">
            <a>Stocks</a>
          </router-link>
        </ul>

        <strong class="navbar-text navbar-right">Funds: {{ funds | currency }}</strong>
        <ul class="nav navbar-nav navbar-right">
          <li>
            <a href="#" @click="endDay">End Day</a>
          </li>
          <li class="dropdown" :class="{open:isDropDown}" @click="isDropDown = !isDropDown">
            <a
              href="#"
              class="dropdown-toggle"
              data-toggle="dropdown"
              role="button"
              aria-haspopup="true"
              aria-expanded="false"
            >
              Save & Load
              <span class="caret"></span>
            </a>
            <ul class="dropdown-menu">
              <li>
                <a href="#" @click="saveData">Save Data</a>
              </li>
              <li>
                <a href="#" @click="loadData">Load Data</a>
              </li>
            </ul>
          </li>
        </ul>
      </div>
    </div>
  </nav>
</template>

<script>
import { mapGetters, mapActions } from "vuex";
export default {
  data() {
    return {
      ...mapGetters({
        myFunds: "funds",
        stockPortfolio: "stockPortfolio",
        stocks: "stocks",
      }),
      isDropDown: false,
    };
  },

  computed: {
    funds() {
      // return this.$store.getters.funds;
      return this.myFunds();
    },
  },
  methods: {
    ...mapActions({
      randomize: "randomizeStocks",
      fetchData: "loadData",
    }),
    endDay() {
      this.randomize();
    },
    saveData() {
      const data = {
        funds: this.myFunds(),
        stockPortfolio: this.stockPortfolio(),
        stocks: this.stocks(),
      };
      console.log(data);
      this.$http.put("data.json", data);
    },
    loadData() {
      this.fetchData();
    },
  },
};
</script>
