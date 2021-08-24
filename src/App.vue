<template>
  <!DOCTYPE html>
  <html>
    <div id="app">
      <div class="navbar">
        <div class="navbar-header">Currencies info</div>

        <!-- Edit Table Button and Dropdown Menu -->
        <div class="navbar-edit-table">
          <button
            @click="viewEditTable = !viewEditTable"
            class="navbar-edit-table-button"
          >
            Edit table
          </button>

          <div v-show="viewEditTable" class="navbar-edit-table-dropdown">
            <!-- todo: add pictures for showing whether the column is visible -->

            <button
              @click="viewLastUpdateCol = !viewLastUpdateCol"
              class="navbar-edit-table-dropdown-button"
            >
              <img
                v-show="viewLastUpdateCol"
                src="pic/show-icon.svg"
                class="navbar-edit-table-dropdown-pic"
              />

              <img
                v-show="!viewLastUpdateCol"
                src="pic/hide-icon.svg"
                class="navbar-edit-table-dropdown-pic"
              />

              Last update
            </button>

            <button
              @click="viewCodeCol = !viewCodeCol"
              class="navbar-edit-table-dropdown-button"
            >
              <img
                v-show="viewCodeCol"
                src="pic/show-icon.svg"
                class="navbar-edit-table-dropdown-pic"
              />

              <img
                v-show="!viewCodeCol"
                src="pic/hide-icon.svg"
                class="navbar-edit-table-dropdown-pic"
              />

              Code
            </button>

            <button
              @click="viewNameCol = !viewNameCol"
              class="navbar-edit-table-dropdown-button"
            >
              <img
                v-show="viewNameCol"
                src="pic/show-icon.svg"
                class="navbar-edit-table-dropdown-pic"
              />

              <img
                v-show="!viewNameCol"
                src="pic/hide-icon.svg"
                class="navbar-edit-table-dropdown-pic"
              />

              Name
            </button>

            <button
              @click="viewDepositEnabledCol = !viewDepositEnabledCol"
              class="navbar-edit-table-dropdown-button"
            >
              <img
                v-show="viewDepositEnabledCol"
                src="pic/show-icon.svg"
                class="navbar-edit-table-dropdown-pic"
              />

              <img
                v-show="!viewDepositEnabledCol"
                src="pic/hide-icon.svg"
                class="navbar-edit-table-dropdown-pic"
              />

              Deposit enabled
            </button>

            <button
              @click="viewWithdrawEnabledCol = !viewWithdrawEnabledCol"
              class="navbar-edit-table-dropdown-button"
            >
              <img
                v-show="viewWithdrawEnabledCol"
                src="pic/show-icon.svg"
                class="navbar-edit-table-dropdown-pic"
              />

              <img
                v-show="!viewWithdrawEnabledCol"
                src="pic/hide-icon.svg"
                class="navbar-edit-table-dropdown-pic"
              />
              Withdrawal enabled
            </button>

            <button
              @click="viewTradingEnabledCol = !viewTradingEnabledCol"
              class="navbar-edit-table-dropdown-button"
            >
              <img
                v-show="viewTradingEnabledCol"
                src="pic/show-icon.svg"
                class="navbar-edit-table-dropdown-pic"
              />

              <img
                v-show="!viewTradingEnabledCol"
                src="pic/hide-icon.svg"
                class="navbar-edit-table-dropdown-pic"
              />
              Trading enabled
            </button>
          </div>
        </div>

        <input v-model="searchInput"
                placeholder="Search"
                class='navbar-search' />
      </div>
      <hr class="sort-column-button-upper-border" />
      <div class="table">
        <hr class="sort-column-button-bottom-border" />

        <!-- Last Update Column -->
        <div v-show="viewLastUpdateCol" class="column">
          <button @click="sortByLastUpdate" class="sort-column-button">
            Last update
            <div class="sort-column-button-wrapping"></div>
          </button>

          <div
            v-for="currency in filteredCurrencies()"
            :key="currency.key"
            class="table-element"
          >
            {{ convertToDate(currency.last_update_at) }}
            <hr class="table-element-border" />
            <div class="table-element-wrapping"></div>
          </div>
        </div>

        <!-- Code Column -->
        <div v-show="viewCodeCol" class="column">
          <button @click="sortByCode" class="sort-column-button">
            Code
            <div class="sort-column-button-wrapping"></div>
          </button>

          <div
            v-for="currency in filteredCurrencies()"
            :key="currency.key"
            class="table-element"
          >
            {{ currency.code }}
            <hr class="table-element-border" />
            <div class="table-element-wrapping"></div>
          </div>
        </div>

        <!-- Name Column -->
        <div v-show="viewNameCol" class="column">
          <button @click="sortByName" class="sort-column-button">
            Name
            <div class="sort-column-button-wrapping"></div>
          </button>

          <div
            v-for="currency in filteredCurrencies()"
            :key="currency.key"
            class="table-element"
          >
            {{ currency.name }}
            <hr class="table-element-border" />
            <div class="table-element-wrapping"></div>
          </div>
        </div>

        <!-- Deposit Enabled Column -->
        <div v-show="viewDepositEnabledCol" class="column">
          <button @click="sortByDepositEnabled" class="sort-column-button">
            Deposit enabled
            <div class="sort-column-button-wrapping"></div>
          </button>

          <div
            v-for="currency in filteredCurrencies()"
            :key="currency.key"
            class="table-element"
          >
            <span v-show="currency.deposit_enabled == '1'"> Yes </span>
            <span v-show="currency.deposit_enabled == '0'"> No </span>
            <hr class="table-element-border" />
            <div class="table-element-wrapping"></div>
          </div>
        </div>

        <!-- Withdrawal Enabled Column -->
        <div v-show="viewWithdrawEnabledCol" class="column">
          <button @click="sortByWithdrawalEnabled" class="sort-column-button">
            Withdrawal enabled
            <div class="sort-column-button-wrapping"></div>
          </button>

          <div
            v-for="currency in filteredCurrencies()"
            :key="currency.key"
            class="table-element"
          >
            <span v-show="currency.withdrawal_enabled == '1'"> Yes </span>
            <span v-show="currency.withdrawal_enabled == '0'"> No </span>
            <hr class="table-element-border" />
            <div class="table-element-wrapping"></div>
          </div>
        </div>

        <!-- Trading enabled Column -->
        <div v-show="viewTradingEnabledCol" class="column">
          <button @click="sortByTradingEnabled" class="sort-column-button">
            Trading enabled
            
          </button>

          <div
            v-for="currency in filteredCurrencies()"
            :key="currency.key"
            class="table-element"
          >
            <span v-show="currency.trading_enabled == '1'"> Yes </span>
            <span v-show="currency.trading_enabled == '0'"> No </span>
            <hr class="table-element-border" />
            <div class="table-element-wrapping"></div>
          </div>
        </div>

        <div class="bottom-total-amount">
          Total: {{ filteredCurrencies().length }}
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
      let newCurrencies = [];

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

      let input = this.searchInput.toLowerCase();
      let name = item["name"].toLowerCase();
      let code = item["code"].toLowerCase();

      if (sortByName && sortByCode) {
        if (name.includes(input) || code.includes(input)) return item;
      }

      if (sortByName && name.includes(input)) return item;

      if (sortByCode && code.includes(input)) return item;
    },

    /* Prettifies the Last Update column */
    /* todo: rename */
    convertToDate(date) {
      return (
        date.slice(0, 10).toString() + ", " + date.slice(11, 19).toString()
      );
    },
  },
};
</script>
