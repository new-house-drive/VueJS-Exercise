<template>
  <!DOCTYPE html>
  <html>
    <div id="app">
      <div class="table-top">
        <div>Currencies</div>

        <!-- Edit Table Button and Dropdown Menu -->
        <div class="edit-table">
          <button @click="viewEditTable = !viewEditTable">
            Edit table
          </button>

          <div v-show="viewEditTable" class="edit-table-menu">
            <!-- todo: add pictures for showing whether the column is visible -->

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

        <!-- Last Update Column -->
        <div v-show="viewLastUpdateCol" class="column">
          
          <button @click="sortByLastUpdate">
            Last update
          </button>

          <div v-for="currency in filteredCurrencies()" :key="currency.key">
            {{ currency.last_update_at }}
          </div>

        </div>
        <!-- Code Column -->
        <div v-show="viewCodeCol" class="column">

          <button @click="sortByCode">
            Code
          </button>

          <div v-for="currency in filteredCurrencies()" :key="currency.key">
            {{ currency.code }}
          </div>
        </div>
        <!-- Name Column -->
        <div v-show="viewNameCol" class="column">

          <button @click="sortByName">
            Name
          </button>

          <div v-for="currency in filteredCurrencies()" :key="currency.key">
            {{ currency.name }}
          </div>
        </div>

        <!-- Deposit Enabled Column -->
        <div v-show="viewDepositEnabledCol" class="column">

          <button @click="sortByDepositEnabled">
            Deposit enabled
          </button>

          <div v-for="currency in filteredCurrencies()" :key="currency.key">
            {{ currency.deposit_enabled }}
          </div>
        </div>

        <!-- Withdrawal Enabled Column -->
        <div v-show="viewWithdrawEnabledCol" class="column">
          <button @click="sortByWithdrawalEnabled">
            Withdrawal enabled
          </button>
          <div v-for="currency in filteredCurrencies()" :key="currency.key">
            {{ currency.withdrawal_enabled }}
          </div>
        </div>

        <!-- Trading enabled Column -->
        <div v-show="viewTradingEnabledCol" class="column">
          <button @click="sortByTradingEnabled">
            Trading enabled
          </button>
          <div v-for="currency in filteredCurrencies()" :key="currency.key">
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
      // Current array displayed
      currencies: [],

      // EditTable Visible Boolean
      viewEditTable: false,

      //Columns Visibility Booleans
      viewLastUpdateCol: true,
      viewCodeCol: true,
      viewNameCol: true,
      viewDepositEnabledCol: true,
      viewWithdrawEnabledCol: true,
      viewTradingEnabledCol: true,

      //Name sorting Booleans
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

    /** Booleans showing the visibility of the columns */
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


    /* Main method for sorting columns */
    sortColumn(isSorted, field) {
      this.currencies.sort((a, b) => {
        if (isSorted) return a[field] < b[field];

        return a[field] > b[field];
      });
    },


    /* Final array received as a resulf of sorting */
    filteredCurrencies() {
      let newCurrencies = []

      if (this.searchInput) {
        newCurrencies = this.currencies.filter((item) => {
          return this.filterByNameOrCode(item);
        });
        return newCurrencies;
        
      }
      return this.currencies;
    },

    /* Method checks whether the fields are available for search at the moment */

    filterByNameOrCode(item) {
        let sortByName = this.viewNameCol;
        let sortByCode = this.viewCodeCol;
        
        let input = this.searchInput.toLowerCase()
        let name = item['name'].toLowerCase()
        let code = item['code'].toLowerCase()

        if (sortByName && sortByCode){
          if(name.includes(input) || code.includes(input)) return item;
        }

        if (sortByName && name.includes(input)) return item;

        if (sortByCode && code.includes(input)) return item;
    }
  },
};
</script>
