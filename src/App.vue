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
        <div v-show='viewLastUpdateCol' class="column">
          <button>Last update</button>
          <div v-for="transaction in transactions" :key="transaction.key">
            {{ transaction.last_update_at }}
          </div>
        </div>

        <div v-show="viewCodeCol" class="column">
          <button>Code</button>
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
          <button>Deposit enabled</button>
          <div v-for="transaction in transactions" :key="transaction.key">
            {{ transaction.deposit_enabled }}
          </div>
        </div>

        <div v-show='viewWithdrawEnabledCol' class="column">
          <button>Withdrawal enabled</button>
          <div v-for="transaction in transactions" :key="transaction.key">
            {{ transaction.withdrawal_enabled }}
          </div>
        </div>

        <div v-show="viewTradingEnabledCol" class="column">
          <button>Trading enabled</button>
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

    sortTransactionsByName() {
      this.transactions.sort((a, b) => {
        return a.name > b.name
      })
    },
  },
};
</script>
