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
        <input placeholder="Search" />
      </div>

      <div class="table">
        <div v-show="viewLastUpdateCol" class="column">
          <button @click="sortTransactionsByLastUpdate">
            Last update
            </button>
          <div v-for="transaction in transactions" :key="transaction.key">
            {{ transaction.last_update_at }}
          </div>
        </div>

        <div v-show="viewCodeCol" class="column">
          <button @click="sortTransactionsByCode">
            Code
            </button>
          <div v-for="transaction in transactions" :key="transaction.key">
            {{ transaction.code }}
          </div>
        </div>

        <div v-show="viewNameCol" class="column">
          <button @click="sortTransactionsByName">
            Name
          </button>
          <div v-for="transaction in transactions" :key="transaction.key">
            {{ transaction.name }}
          </div>
        </div>

        <div v-show="viewDepositEnabledCol" class="column">
          <button @click="sortTransactionsByDepositEnabled">
            Deposit enabled
            </button>
          <div v-for="transaction in transactions" :key="transaction.key">
            {{ transaction.deposit_enabled }}
          </div>
        </div>

        <div v-show="viewWithdrawEnabledCol" class="column">
          <button @click="sortTransactionsByWithdrawalEnabled">
            Withdrawal enabled
            </button>
          <div v-for="transaction in transactions" :key="transaction.key">
            {{ transaction.withdrawal_enabled }}
          </div>
        </div>

        <div v-show="viewTradingEnabledCol" class="column">
          <button @click="sortTransactionsByTradingEnabled">
            Trading enabled
            </button>
          <div v-for="transaction in transactions" :key="transaction.key">
            {{ transaction.trading_enabled }}
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
      transactions: [],

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
      sortingByLastUpdateAscending: false,
      sortingByCodeAscending: false,
      sortingByNameAscending: false,
      sortingByDepositEnabledAscending: false,
      sortingByWithdrawalEnabledAscending: false,
      sortingByTradingEnabledAscending: false,
    };
  },

  mounted() {
    this.reloadJSON();
  },

  methods: {
    reloadJSON() {
      let path = "/exercise.json";
      let xhr = new XMLHttpRequest();
      xhr.responseType = "json";

      xhr.open("GET", path, true);
      xhr.onload = () => {
        this.transactions = xhr.response;
      };

      xhr.send();
    },

    sortTransactionsByLastUpdate() {
      this.sortColumn(this.sortingByLastUpdateAscending, "last_update_at")
      this.sortingByLastUpdateAscending = !this.sortingByLastUpdateAscending;
    },

    sortTransactionsByCode() {
      this.sortColumn(this.sortingByCodeAscending, 'code')
      this.sortingByCodeAscending = !this.sortingByCodeAscending;
    },

    sortTransactionsByName() {
      this.sortColumn(this.sortingByNameAscending, 'name')
      this.sortingByNameAscending = !this.sortingByNameAscending;
    },

    sortTransactionsByDepositEnabled() {
      this.sortColumn(this.sortingByDepositEnabledAscending, "deposit_enabled")
      this.sortingByDepositEnabledAscending = !this.sortingByDepositEnabledAscending;
    },

    sortTransactionsByWithdrawalEnabled() {
      this.sortColumn(this.sortingByWithdrawalEnabledAscending, "withdrawal_enabled")
      this.sortingByWithdrawalEnabledAscending = !this.sortingByWithdrawalEnabledAscending;
    },

    sortTransactionsByTradingEnabled() {
      this.sortColumn(this.sortingByTradingEnabledAscending, "trading_enabled")
      this.sortingByTradingEnabledAscending = !this.sortingByTradingEnabledAscending;
    },

    sortColumn(isSorted, field) {
      if (isSorted) {
        this.transactions.sort((a, b) => {
          return a[field] < b[field];
        });
      } else {
        this.transactions.sort((a, b) => {
          return a[field] > b[field];
        });
      }
    },
    
  },
};
</script>
