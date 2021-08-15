<template>
  <!DOCTYPE html>
  <html>
    <div id="app">
      <div class="table-top">
        <div>Currencies</div>

        <div class="edit-table">
          <button @click="viewEditTable = !viewEditTable">Edit table</button>

          <div v-show="viewEditTable" class="edit-table-menu">
            <button>Last update</button>
            <button>Code</button>
            <button>Name</button>
            <button>Deposit enabled</button>
            <button>Withdrawal enabled</button>
            <button>Trading enabled</button>
          </div>
        </div>
        <input placeholder="Search" />
      </div>

      <div class="table">
        <div class="column">
          <p>Last update</p>
          <div v-for="transaction in transactions" :key="transaction.key">
            {{ transaction.last_update_at }}
          </div>
        </div>

        <div class="column">
          <p>Code</p>
          <div v-for="transaction in transactions" :key="transaction.key">
            {{ transaction.code }}
          </div>
        </div>

        <div class="column">
          <p>Name</p>
          <div v-for="transaction in transactions" :key="transaction.key">
            {{ transaction.name }}
          </div>
        </div>

        <div class="column">
          <p>Deposit enabled</p>
          <div v-for="transaction in transactions" :key="transaction.key">
            {{ transaction.deposit_enabled }}
          </div>
        </div>

        <div class="column">
          <p>Withdrawal enabled</p>
          <div v-for="transaction in transactions" :key="transaction.key">
            {{ transaction.withdrawal_enabled }}
          </div>
        </div>

        <div class="column">
          <p>Trading enabled</p>
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
    };
  },

  mounted() {
    this.reloadJSON();
    //   let path = "/exercise.json";
    //   let xhr = new XMLHttpRequest();
    //   xhr.responseType = "json";

    //   xhr.open("GET", path, true);
    //   xhr.onload = () => {
    //     this.transactions = xhr.response;
    //   };

    //   xhr.send();
    // },
    //};
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
  },
};
</script>
