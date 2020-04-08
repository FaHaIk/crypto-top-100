<template>
  <div class="table-wrapper">
    <div v-if="loading" class="loader">
      <div class="spinner"></div>
    </div>
    <table class="table">
      <tr class="col-header">
        <!-- <th v-for="column in columns" :key="column.key">{{ column.title }}</th> -->
        <th class="col-1 clickable p-left" @click="sortMCap">#</th>
        <th class="col-2"></th>
        <th class="col-3 clickable" @click="sortName">Name</th>
        <th class="col-4 clickable" @click="sortMCap">Market Cap</th>
        <th class="col-5">Price</th>
        <th class="col-6 clickable" @click="sortVol">Volume</th>
        <th class="col-7">Price Change(24h)</th>
        <th class="col-8">Price Change(7d)</th>
      </tr>
      <tr v-for="coin in data" :key="coin.id">
        <td class="text-left p-left">{{ coin.market_cap_rank }}</td>
        <td class="icon text-left">
          <img width="16" height="16" :src="coin.image" />
        </td>
        <td class="text-left">
          <b>{{ coin.name }}</b>
        </td>
        <td class="m-cap">${{ curr(coin.market_cap) }}</td>
        <td class="curr-price">${{ curr(coin.current_price) }}</td>
        <td class>${{ curr(coin.total_volume) }}</td>
        <td v-html="upDown(coin.price_change_percentage_24h)"></td>
        <td v-html="upDown(coin.price_change_percentage_7d_in_currency)"></td>
      </tr>
    </table>
  </div>
</template>

<script>
const axios = require("axios");

export default {
  name: "Table",
  methods: {
    load: function() {
      this.loading = true;
      axios
        .get(
          "https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&order=" +
            this.order +
            "&per_page=100&page=1&sparkline=false&price_change_percentage=7d"
        )
        .then(response => {
          this.data = response.data;
          this.loading = false;
        })
        .catch(error => {
          console.log(error);
          this.loading = false;
        });
    },
    sortMCap: function() {
      if (this.order == "market_cap_asc") {
        this.order = "market_cap_desc";
      } else if (this.order == "market_cap_desc") {
        this.order = "market_cap_asc";
      } else {
        this.order = "market_cap_desc";
      }
      this.load();
    },
    sortName: function() {
      if (this.order == "id_asc") {
        this.order = "id_desc";
      } else if (this.order == "id_desc") {
        this.order = "id_asc";
      } else {
        this.order = "id_asc";
      }
      this.load();
    },
    sortVol: function() {
      if (this.order == "volume_asc") {
        this.order = "volume_desc";
      } else if (this.order == "volume_desc") {
        this.order = "volume_asc";
      } else {
        this.order = "volume_desc";
      }
      this.load();
    },
    curr: function(marketCap) {
      if (marketCap == null) return "";
      return marketCap.toLocaleString({ style: "currency" });
    },
    upDown: function(number) {
      if (number == null) return "";
      let className = "isNeg";
      if (number.toString().substring(0, 1) === "-") {
        className = "isPos";
      }
      return '<span class="' + className + '"' + ">" + number + "%</span>";
    }
  },
  data() {
    return {
      data: [],
      order: "market_cap_desc",
      page: 1,
      loading: false,
      columns: [
        {
          title: "#",
          key: "number"
        },
        {
          title: "",
          key: "image"
        },
        {
          title: "Name",
          key: "name"
        },
        {
          title: "Market Cap",
          key: "market_cap"
        },
        {
          title: "Price",
          key: "price"
        }
      ]
    };
  },
  mounted() {
    this.loading = true;
    axios
      .get(
        "https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&order=" +
          this.order +
          "&per_page=100&page=1&sparkline=false&price_change_percentage=7d"
      )
      .then(response => {
        this.data = response.data;
        this.loading = false;
      })
      .catch(error => {
        console.log(error);
        this.loading = false;
      });
  }
};
</script>

<style scoped>
.spinner {
  width: 40px;
  height: 40px;
  background-color: #3de3bc69;

  margin: 200px auto;
  -webkit-animation: sk-rotateplane 1.2s infinite ease-in-out;
  animation: sk-rotateplane 1.2s infinite ease-in-out;
}

@-webkit-keyframes sk-rotateplane {
  0% {
    -webkit-transform: perspective(120px);
  }
  50% {
    -webkit-transform: perspective(120px) rotateY(180deg);
  }
  100% {
    -webkit-transform: perspective(120px) rotateY(180deg) rotateX(180deg);
  }
}

@keyframes sk-rotateplane {
  0% {
    transform: perspective(120px) rotateX(0deg) rotateY(0deg);
    -webkit-transform: perspective(120px) rotateX(0deg) rotateY(0deg);
  }
  50% {
    transform: perspective(120px) rotateX(-180.1deg) rotateY(0deg);
    -webkit-transform: perspective(120px) rotateX(-180.1deg) rotateY(0deg);
  }
  100% {
    transform: perspective(120px) rotateX(-180deg) rotateY(-179.9deg);
    -webkit-transform: perspective(120px) rotateX(-180deg) rotateY(-179.9deg);
  }
}

.loader {
  position: absolute;
  width: 1150px;
  height: 100%;
  background-color: #ffffff4f;
  /* background-color: #3de3bc27;  */
  backdrop-filter: blur(5px);
}
.col-1 {
  width: 50px;
}
.col-2 {
  width: 20px;
}

.clickable {
  cursor: pointer;
}
.clickable:hover {
  text-decoration: underline;
}
/* .col-3 {
    width: 140px
}
.col-4 {
    width: 150px
}
.col-5 {
    
} */
.p-left {
  padding-left: 30px;
}
td > img {
  margin-right: 10px;
  display: block;
  margin: 0;
}
.text-left {
  text-align: left;
}
.icon {
  padding: 0;
}
.table {
  border-collapse: collapse;
  width: 100%;
  /* text-align: right; */
  text-align: left;
  font-size: 14px;
  table-layout: fixed;
}
.table-wrapper {
  /* padding: 20px; */
  /* padding-top: 10px; */
  /* box-shadow: 0 18px 20px 8px rgba(223, 223, 223, 0.493); */
  border: 1px solid #eaeef2;
}
td {
  height: 50px;
  vertical-align: middle;
}
tr:hover {
  box-shadow: inset 1px 0px 0 0 #3de3bb;
}
tr {
  border-bottom: 1px solid #eaeef2;
  /* box-shadow: inset 0 3px 1px rgb(241, 241, 241); */
  /* border-radius: 3px; */
}
.col-header {
  box-shadow: none;
  /* border-bottom: 1px solid #3de3bb; */
}
th {
  height: 50px;
  background-color: #f0f3f594;
  color: #919191;
}

tr:nth-child(odd) {
  background-color: #dbf1ec44;
}

/* 
td > img {
  margin-right: 10px;
  display: block;
  margin: 0;
}
.text-left {
    text-align:left;
}
.icon {
  padding: 0;
}
.table {
  border-collapse: collapse;
  width: 100%;
  text-align: right;
  font-size: 14px;
}
.table-wrapper {
  border-radius: 5px;
  border: 1px solid #eaeef2;
}
td {
  height: 50px;
  padding-left: 30px;
  vertical-align: middle;
}
tr:hover {
  box-shadow: inset 0px -1px 0 0 #3de3bb;
}
tr {
  border-bottom: 1px solid #eaeef2;
}
.col-header {
  box-shadow: none;
  border-bottom: 2px solid #3de3bb;
}
th {
  height: 50px;
  padding-left: 30px;
  background-color: #f0f3f594;
  background-color: #2b2b33;
  color: white;
}
tr:nth-child(odd) {
  background-color: #f0f3f594;
  background-color: #dbf1ec69;
} */
</style>

