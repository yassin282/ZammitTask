<template>
  <div>
    <div class="row bg-white q-pa-sm">
      <div class="col-12 q-pa-sm">
        <q-input
          placeholder="Search product name, sku or barcode"
          debounce="300"
          dense
          outlined
          flat
          color="primary"
          v-model="filter"
        >
          <template v-slot:prepend>
            <q-icon name="search"/>
          </template>
        </q-input>
      </div>

      <div class="col-4 q-pa-sm">
        <q-btn
          color="white"
          class="text-black full-width"
          :icon-right="selected.length>0? 'remove_circle_outline':'add_circle_outline'"
          size="12px"
          label="Select"
          no-caps
          @click="selectRemoveAll()"
          outline
        />
      </div>
      <div class="q-pa-sm col-4">
        <q-btn
          color="white"
          class="q-pa-none text-left text-black full-width"
          size="12px"
          icon-right="sort"
          label="Filters"
          outline
          no-caps
        />
      </div>
      <div class="q-pa-sm col-4">
        <q-btn
          color="white"
          class="text-black full-width"
          icon-right="swap_vert"
          size="12px"
          label="Sort"
          no-caps
          outline
        />
      </div>
      <div
        :class="selected.indexOf(product)>-1?'Active row full-width ':'row full-width '"
        v-for="product in filterdProducts.slice(20*(current-1), 20*current<filterdProducts.length? 20*current:filterdProducts.length)"
        :key="product.id"
        @click="selectProduct(product)"
      >
        <div class="col-3 q-pa-md">
          <q-img src="../assets/jacket.png"></q-img>
        </div>
        <div class="col-6 q-pa-md">
          <div class="text-subtitle2">{{product.name}}</div>
          <div class="text-caption text-weight-light" style="color:#898993">{{product.type}}</div>
          <div class="text-caption text-weight-medium">
            {{product.stock}} in stock
            <span style="font-size:36px; font-weight: bold">.</span>
            {{product.price}} EGP
          </div>
        </div>
        <div class="col-3 q-pa-md text-center">
          <div
            :class="'text-center '+ product.status + ' bg-lime-1'"
            style="border-radius: 25px; margin:auto; padding:8px"
          >{{product.status}}</div>
        </div>
      </div>
      <q-pagination
        class="full-width"
        :max-pages="3"
        :max="Math.ceil(filterdProducts.length/20)"
        color="secondary"
        v-model="current"
        v-if="filterdProducts.length>19"
      ></q-pagination>
    </div>
  </div>
</template>

<script>
import { defineComponent } from "vue";
import { ref } from "vue";

export default defineComponent({
  name: "ProductsMobile",
  props: ["count", "tableData"],
  data() {
    return { filter: "", current: 1, selected: [] };
  },
  methods: {
    selectProduct(product) {
      const index = this.selected.indexOf(product);
      if (index > -1) {
        console.log("removed");
        this.selected.splice(index, 1);
      } else {
        this.selected.push(product);

        console.log("added");
      }
    },
    selectRemoveAll() {
      this.selected.length > 0
        ? (this.selected = [])
        : (this.selected = this.filterdProducts);
    }
  },
  computed: {
    filterdProducts() {
      if (this.filter.length) {
        let temp = this.tableData.filter(product => {
          return (
            product.name.toLowerCase().includes(this.filter) ||
            product.status.toLowerCase().includes(this.filter) ||
            product.price == this.filter ||
            product.type.toLowerCase().includes(this.filter)
          );
        });
        this.count(temp);
        return temp;
      } else {
        this.count(this.tableData);
        return this.tableData;
      }
    }
  }
});
</script>

<style lang="scss">
.Active {
  background: #d1e6d7 !important;
}
</style>

