<template>
  <div class="public-filter">
    <form @submit.prevent>
      <h1>Keyword</h1>
      <div class="form-group">
        <input
          type="text"
          class="form-control"
          id="keyword_search"
          v-model="search_string"
          @keyup.enter="updateQuery"
        />
      </div>

      <h1>Product Type</h1>
      <div class="form-group">
        <label for="inverter">
          <input type="checkbox" id="inverter" value="Inverter" v-model="query_data.ProductType" /> Inverter
        </label>
        <label for="Module">
          <input type="checkbox" id="Module" value="Module" v-model="query_data.ProductType" /> Module
        </label>
        <label for="Battery">
          <input type="checkbox" id="Battery" value="Battery" v-model="query_data.Battery" /> Storage
        </label>
      </div>
      <div class="button-group">
        <button type="button" class="btn btn-primary" @click="updateQuery">
          <v-icon name="search" class="search-icon" />&nbsp; Search
        </button>
        <button type="button" class="btn btn-primary" @click="clearFilters">
          <v-icon name="times" class="clear-icon" />&nbsp;&nbsp;Clear filters
        </button>
      </div>
    </form>
  </div>
</template>

<script>
export default {
  data() {
    return {
      query_data: {
        ProductType: [],
        ProductRegion: [],
        ProductCertificationType: []
      },
      search_string: ""
    };
  },
  methods: {
    updateQuery() {
      this.$store.commit("toggleAPILoading");
      // console.log(this.query_data);
      let query_search_string = "";
      if (this.search_string) {
        query_search_string = "search=" + this.search_string;
      }

      let query_string = "?";
      Object.keys(this.query_data).forEach(key => {
        if (this.query_data[key].length != 0) {
          if (query_string == "?") {
            query_string += key + "__in=";
          } else {
            query_string += "&" + key + "__in=";
          }
          for (let i in this.query_data[key]) {
            query_string += this.query_data[key][i] + ",";
          }
          query_string = query_string.slice(0, -1);
        }
      });

      if (query_string == "?") {
        query_string = query_string + query_search_string;
      } else {
        query_string = query_string + "&" + query_search_string;
      }
      // console.log(query_string);
      this.$store.commit("callAPI", query_string);
    },
    clearFilters() {
      this.$store.commit("toggleAPILoading");

      this.search_string = "";
      this.query_data.ProductType = [];
      this.query_data.ProductRegion = [];
      this.query_data.ProductCertificationType = [];
      this.$store.commit("clearQueryString");
      this.$store.commit("callAPI");
      this.$store.commit("updateCurrentPage", 1);
    },
    updateSearch() {
      this.$store.commit("toggleAPILoading");

      let query_search_string = "?search=" + this.search_string;
      // console.log(query_search_string);
      this.$store.commit("callAPI", query_search_string);
    }
  }
};
</script>

<style scoped>
.clear-icon {
  color: #db4437;
  margin-bottom: 3px;
}

.search-icon {
  color: #4285f4;
  margin-bottom: 3px;
}
h1 {
  font-size: 18px;
  color: #4b4e52;
  font-family: "Roboto Condensed";
  font-weight: bold;
}

.public-filter {
  padding-left: 20px;
  padding-top: 5px;
}

.form-group {
  font-family: "Roboto Condensed";

  display: block;
}

button {
  margin: 5px;
  margin-bottom: 15px;
}

label {
  display: block;
}

.button-group {
  display: flex;
  justify-content: center;
  margin-bottom: 5px;
  margin-left: -17px;
}

.btn-primary,
.btn-primary:active,
.btn-primary:visited,
.btn-primary:focus,
.btn-primary:disabled {
  background-color: white;
  border-color: #4b4e52;
  color: #4b4e52;
}

.btn-primary:hover {
  background-color: #eeeeee;
  color: #4b4e52;
  border-color: #4b4e52;
}

label {
  margin-top: 3px;
  margin-bottom: 3px;
}

#keyword_search {
  width: 214px;
}
</style>
