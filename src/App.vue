<template>
  <!DOCTYPE html>
  <html>
    <div id="app">
      <div class="table-top">
        <div>Currencies</div>

        <div class="edit-table">
          <button @click="viewEditTable = !viewEditTable">
            Edit table
          </button>

          <div v-show="viewEditTable" class="edit-table-menu">
            <button @click="viewLastUpdateCol = !viewLastUpdateCol">
              Last update
            </button>

            <button @click="viewCodeCol = !viewCodeCol">
              Code
            </button>

            <button @click="viewNameCol = !viewNameCol">
              Name
            </button>

            <button @click="viewDepositEnabledCol = !viewDepositEnabledCol">
              Deposit enabled
            </button>

            <button @click="viewWithdrawEnabledCol = !viewWithdrawEnabledCol">
              Withdrawal enabled
            </button>

            <button @click="viewTradingEnabledCol = !viewTradingEnabledCol">
              Trading enabled
            </button>
          </div>
        </div>

        <input v-model="searchInput" placeholder="Search" />
      </div>

      <div class="table">
        <div v-show="viewLastUpdateCol" class="column">
          <button @click="sortByLastUpdate">
            Last update
          </button>
          <div v-for="currency in currencies" :key="currency.key">
            {{ currency.last_update_at }}
          </div>
        </div>

        <div v-show="viewCodeCol" class="column">
          <button @click="sortByCode">
            Code
          </button>
          <div v-for="currency in currencies" :key="currency.key">
            {{ currency.code }}
          </div>
        </div>

        <div v-show="viewNameCol" class="column">
          <button @click="sortByName">
            Name
          </button>
          <div v-for="currency in currencies" :key="currency.key">
            {{ currency.name }}
          </div>
        </div>

        <div v-show="viewDepositEnabledCol" class="column">
          <button @click="sortByDepositEnabled">
            Deposit enabled
          </button>
          <div v-for="currency in currencies" :key="currency.key">
            {{ currency.deposit_enabled }}
          </div>
        </div>

        <div v-show="viewWithdrawEnabledCol" class="column">
          <button @click="sortByWithdrawalEnabled">
            Withdrawal enabled
          </button>
          <div v-for="currency in currencies" :key="currency.key">
            {{ currency.withdrawal_enabled }}
          </div>
        </div>

        <div v-show="viewTradingEnabledCol" class="column">
          <button @click="sortByTradingEnabled">
            Trading enabled
          </button>
          <div v-for="currency in currencies" :key="currency.key">
            {{ currency.trading_enabled }}
          </div>
        </div>
      </div>
    </div>
  </html>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      // JSON operations
      currencies: [],

      // EditTable variables
      viewEditTable: false,

      //Columns Visibility
      viewLastUpdateCol: true,
      viewCodeCol: true,
      viewNameCol: true,
      viewDepositEnabledCol: true,
      viewWithdrawEnabledCol: true,
      viewTradingEnabledCol: true,

      //Name sorting
      isSortedLastUpdateCol: false,
      isSortedCodeCol: false,
      isSortedNameCol: false,
      isSortedDepositCol: false,
      isSortedWithdrawalCol: false,
      isSortedTradingCol: false,

      //Search by input
      searchInput: "",
    };
  },

  mounted() {
    this.reloadJSON();
  },

  methods: {
    /** MOUNTED; RECEIVE CLEAR JSON FROM FILE */
    reloadJSON() {
      let path = "/exercise.json";
      let xhr = new XMLHttpRequest();
      xhr.responseType = "json";

      xhr.open("GET", path, true);
      xhr.onload = () => {
        this.currencies = xhr.response;
      };

      xhr.send();
    },

    /** SORT COLUMNS BY FIELD */
    /**todo: Abstract part of the following */
    sortByLastUpdate() {
      this.sortColumn(this.isSortedLastUpdateCol, "last_update_at");
      this.isSortedLastUpdateCol = !this.isSortedLastUpdateCol;
    },

    sortByCode() {
      this.sortColumn(this.isSortedCodeCol, "code");
      this.isSortedCodeCol = !this.isSortedCodeCol;
    },

    sortByName() {
      this.sortColumn(this.isSortedNameCol, "name");
      this.isSortedNameCol = !this.isSortedNameCol;
    },

    sortByDepositEnabled() {
      this.sortColumn(this.isSortedDepositCol, "deposit_enabled");
      this.isSortedDepositCol = !this.isSortedDepositCol;
    },

    sortByWithdrawalEnabled() {
      this.sortColumn(this.isSortedWithdrawalCol, "withdrawal_enabled");
      this.isSortedWithdrawalCol = !this.isSortedWithdrawalCol;
    },

    sortByTradingEnabled() {
      this.sortColumn(this.isSortedTradingCol, "trading_enabled");
      this.isSortedTradingCol = !this.isSortedTradingCol;
    },

    sortColumn(isSorted, field) {
      this.currencies.sort((a, b) => {
        if (isSorted) return a[field] < b[field];

        return a[field] > b[field];
      });
    },

    filteredCurrencies() {
      // sort currencies by user input
      if (this.searchInput) {
        let newcurrencies = this.currencies.filter((item) => {
          return item["name"].includes(this.searchInput);
        });
        return newcurrencies;
        
      }
      return this.currencies;
    },
  },
};
</script>
