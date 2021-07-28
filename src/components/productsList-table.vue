<template>
  <q-scroll-area
    :visible="false"
    style="width:100%; height:70vh;"
    :bar-style="{ opacity: 0 }"
    :thumb-style="{ opacity: 0 }"
  >
    <q-table
      style="border: 1px solid #ccc; "
      virtual-scroll
      v-if="tableData.length"
      :selected-rows-label="getSelectedString"
      selection="multiple"
      :rows="tableData"
      :columns="columns"
      class="q-pr-none text-subtitle"
      v-model:selected="selected"
      no-data-label="No items found"
      row-key="id"
      :filter="filter"
      :filter-method="myfilterMethod"
      :pagination="{rowsPerPage: 20}"
    >
      <template v-slot:top>
        <div class="row full-width">
          <div class="col-sm-12 col-xs-12 col-md-5">
            <q-input
              placeholder="Search product name, sku or barcode"
              debounce="500"
              dense
              outlined
              flat
              color="primary"
              v-on:keyUp="count(tableData)"
              v-model="filter"
              class="q-pl-sm"
            >
              <template v-slot:prepend>
                <q-icon name="search"/>
              </template>
            </q-input>
          </div>
          <div class="col-sm-4 col-xs-4 col-md-4 q-pl-sm">
            <q-btn
              color="white"
              class="text-black text-small"
              icon-right="sort"
              label="Filters"
              outline
              no-caps
            />
          </div>
          <div class="col-sm-4 col-xs-4 col-md-3 text-right">
            <q-btn
              color="white"
              class="text-black text-left"
              icon-right="swap_vert"
              label="Sort"
              no-caps
              outline
            />
          </div>
        </div>
      </template>
      <template v-slot:body-cell-img="props">
        <q-td style="width:6vw" :props="props">
          <q-img style src="../assets/jacket.png"></q-img>
        </q-td>
      </template>

      <template v-slot:body-cell-status="props">
        <q-td :props="props">
          <div
            :class="'text-center '+ props.row.status + ' bg-lime-1'"
            style="border-radius: 25px; padding: 8px 0"
          >{{props.row.status}}</div>
        </q-td>
      </template>

      <template v-slot:body-cell-action="props">
        <q-td :props="props"></q-td>
      </template>
    </q-table>
  </q-scroll-area>
</template>

<script>
import { defineComponent } from "vue";
import { ref } from "vue";

const selected = ref([]);

export default defineComponent({
  name: "ProductsDesktop",
  props: ["columns", "tableData", "count"],
  data() {
    return {
      filter: "",
      selected
    };
  },
  methods: {
    getSelectedString: function() {
      return selected.value.length === 0
        ? ""
        : `${selected.value.length} record${
            selected.value.length > 1 ? "s" : ""
          } selected of ${this.tableData.length}`;
    },
    myfilterMethod() {
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

