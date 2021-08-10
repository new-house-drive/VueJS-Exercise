<template>
  <!DOCTYPE html>
  <html>
    <div id="app">
      {{ this.transactions }}
    </div>
  </html>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      transactions: [],
    };
  },

  mounted() {
    let path = "/public/exercise";
    let xhr = new XMLHttpRequest();
    xhr.responseType = 'json';

    xhr.open("GET", path, true);
    xhr.onload = () => {
      this.transactions = xhr.response;
    };

    xhr.send();


  },
};
</script>

<style>
#content {
  text-align: center;
  color: rgb(0, 0, 0);
  background-color: rgb(249, 255, 255);

  border: black 1px solid;

  font-size: larger;
  max-width: 50em;

  padding-bottom: 2em;
}

.top {
  display: grid;
  grid-template-columns: auto auto auto;
  padding: 0.5em 1em;
}

#top-edit-table {
  position: relative;
}

#top-edit-table-button {
  width: 15em;
  border-radius: 0px;
  border: black 1px solid;
}

#top-edit-table-button:active {
  background-color: rgb(182, 182, 181);
}

.top-dropdown-menu {
  display: none;
  grid-auto-columns: auto;
  position: absolute;

  width: 12em;
  font-size: smaller;

  left: 4.5em;
}

.dropdown-element {
  border: black 1px solid;
  text-align: left;
}

#top-name {
  font-weight: 600;
}

#top-search-input {
  min-width: 80%;
  padding-left: 2em;
}

.table {
  display: grid;
  grid-template-columns: repeat(6, auto);
}

.table-element {
  font-size: 0.8em;
  font-weight: 100;

  padding: 0.3em 1em;
}

.table-element::after {
  border: black 1px solid;
}

.table-top {
  font-weight: 700;
}
</style>
