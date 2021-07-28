<template>
  <q-page :class="desktop ? 'page' : 'q-pt-md' + ' bg-grey-11'">
    <div
      :class="'full-width' + ' ' + desktop ? 'q-pa-sm' : ''"
      style="display:flex;"
    >
      <div style="margin:auto">
        <div class="text-block text-subtitle2">
          {{ "Products list \n(" + number + " " + "items)" }}
        </div>
      </div>
      <!-- <q-btn flat :label="'Products list ('+ data.length+' '+'items)'"></q-btn> -->
      <q-space />
      <q-btn v-if="desktop" label="Import" flat size="12px"></q-btn>
      <q-btn
        label="Export"
        v-if="desktop"
        class="q-ml-sm"
        flat
        size="12px"
      ></q-btn>
      <q-btn
        icon="more_horiz"
        outline
        v-if="!desktop"
        class="q-pr-lg q-pl-lg q-ml-sm"
        size="14px"
      ></q-btn>
      <q-btn
        :label="desktop ? 'Add new product' : 'Add'"
        :class="desktop ? 'q-ml-sm' : 'q-pr-lg q-pl-lg q-ml-sm'"
        color="green-6"
        :size="desktop ? 'large' : '14px'"
      ></q-btn>
    </div>
    <productsDesktop
      v-if="desktop"
      :tableData="tableData"
      :columns="columns"
      :count="count"
    />

    <productsMobile v-else :tableData="tableData" :count="count" />
  </q-page>
</template>

<script>
import { defineComponent } from "vue";
import ProductsDesktop from "components/productsList-table.vue/";
import ProductsMobile from "components/ProductList-grid.vue/";
import axios from "axios";
import { vue } from "vue";
import json from "../assets/testzam.json";

export default defineComponent({
  name: "PageIndex",
  components: {
    ProductsDesktop,
    ProductsMobile
  },
  data() {
    return {
      current: 1,
      totalPages: 50,
      tableData: [],
      filter: "",
      number: 0,
      desktop: false,
      columns: [
        {
          name: "img",
          label: "",
          align: "left",
          field: "img",
          sortable: false
        },
        {
          name: "name",
          label: "Product",
          align: "left",
          field: "name",
          sortable: false
        },
        {
          name: "status",
          label: "status",
          align: "center",
          field: "status",
          class: "text-caption",
          sortable: false
        },
        {
          name: "stock",
          label: "Stock",
          align: "left",
          field: "stock",
          sortable: false,
          format: (_, row) => {
            return row.stock + " in stock";
          }
        },
        {
          name: "price",
          label: "Price",
          align: "center",
          field: "price",
          sortable: false,
          format: (_, row) => {
            return row.price + " EGP";
          }
        },
        {
          name: "type",
          label: "Type",
          align: "center",
          field: "type",
          sortable: false
        }
      ]
    };
  },
  methods: {
    async init() {
      // Comment the next line and uncomment the rest in this function if you are testing after 15-18 hour from the
      // submitted time due to consuming all the free apis from mockaroo
      this.tableData = json;
      // this.tableData = await axios
      //   .get("https://api.mockaroo.com/api/422f8a50?count=1000&key=5ccc2440")
      //   .then(response => response.data);
      this.count(this.tableData);
    },
    count(array) {
      this.number = array.length;
    },
    myEventHandler() {
      if (window.innerWidth > 767) {
        this.desktop = true;
      } else this.desktop = false;
    }
  },
  created() {
    this.init();
    if (window.innerWidth > 767) {
      this.desktop = true;
    }
    window.addEventListener("resize", this.myEventHandler);
  },
  unmounted() {
    window.removeEventListener("resize", this.myEventHandler);
  }
});
</script>
<style scoped>
.page {
  padding: 3vh 10vw;
}
/* other styling */
</style>
