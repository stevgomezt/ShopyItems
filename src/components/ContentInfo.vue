<template>
  <div>
    <section class="content-title">
      <div class="container form-data">
        <div class="title">
          <span
            >"<strong>ShopyItems</strong> es mejor que cualquier otra plataforma
            con la que hayamos trabajado, ¡y vaya si hemos probado!".</span
          >
        </div>
        <div class="form">
          <b-row class="form-input-items">
            <b-col sm="2">
              <label for="input-small">►</label>
            </b-col>
            <b-col sm="10">
              <b-form-input
                id="input-small"
                v-model="selectItem.name"
                size="sm"
                placeholder="Enter name"
              ></b-form-input>
            </b-col>
          </b-row>
          <b-row class="form-input-items">
            <b-col sm="2">
              <label for="input-small">►</label>
            </b-col>
            <b-col sm="10">
              <b-form-input
                id="input-small"
                v-model="selectItem.description"
                size="sm"
                placeholder="Enter description"
              ></b-form-input>
            </b-col>
          </b-row>
          <b-row class="form-input-items">
            <b-col sm="2">
              <label for="input-small">►</label>
            </b-col>
            <b-col sm="10">
              <b-form-input
                id="input-small"
                v-model="selectItem.imageUrl"
                size="sm"
                placeholder="Enter image url"
              ></b-form-input>
            </b-col>
          </b-row>
          <b-row class="form-input-items">
            <b-col sm="2">
              <label for="input-small">►</label>
            </b-col>
            <b-col sm="10">
              <b-form-input
                id="input-small"
                v-model="selectItem.price"
                size="sm"
                placeholder="Enter price"
              ></b-form-input>
            </b-col>
          </b-row>
          <b-button
            class="save-button btn-primary"
            v-if="editing"
            @click="updateItem"
            >Edit</b-button
          >
          <b-button
            class="save-button btn-primary"
            v-if="!editing"
            @click="createItem"
            >Save</b-button
          >
        </div>
      </div>
    </section>
    <section class="content-info">
      <div class="container">
        <div class="main-table">
          <b-table
            label-sort-asc=""
            label-sort-desc=""
            label-sort-clear=""
            striped
            hover
            :items="items"
            :fields="fields"
          >
            <template #cell(imageUrl)="row">
              <a target="_blank" :href="row.item.imageUrl">View image</a>
            </template>
            <template #cell(actions)="row">
              <div class="actions">
                <b-button
                  size="sm"
                  @click="deleteItem(row.item)"
                  class="delete"
                >
                  Delete
                </b-button>
                <b-button size="sm" @click="onSelectItem(row.item)">
                  Update
                </b-button>
              </div>
            </template>
          </b-table>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
export default {
  data() {
    return {
      // Note 'isActive' is left out and will not appear in the rendered table
      fields: [
        {
          key: "name",
          label: "Name",
          sortable: true,
        },
        {
          key: "description",
          label: "Description",
          sortable: true,
        },
        {
          key: "imageUrl",
          label: "Image URL",
          sortable: true,
        },
        {
          key: "price",
          label: "Price",
          sortable: true,
          // Variant applies to the whole column, including the header and footer
          variant: "danger",
        },
        //Actions
        { key: "actions", label: "Actions" },
      ],
      items: [],
      selectItem: { name: "", description: "", imageUrl: "", price: 0 },
      editing: false,
    };
  },
  //Create
  created() {
    this.loadItems();
  },
  //Methods
  methods: {
    //LoadItems
    loadItems() {
      this.axios
        .get("https://shop-api3.herokuapp.com/api/products")
        .then((response) => {
          console.log(response.data);
          this.items = response.data.products;
        });
    },
    createItem() {
      let data = {
        name: this.selectItem.name,
        description: this.selectItem.description,
        imageUrl: this.selectItem.imageUrl,
        price: this.selectItem.price,
      };

      this.axios
        .post("https://shop-api3.herokuapp.com/api/products", data)
        .then((response) => {
          console.log(response.data);
          this.loadItems();
          this.editing = false;
          this.selectItem = {};
        });
    },
    updateItem() {
      let data = {
        name: this.selectItem.name,
        description: this.selectItem.description,
        imageUrl: this.selectItem.imageUrl,
        price: this.selectItem.price,
      };

      this.axios
        .put(
          `https://shop-api3.herokuapp.com/api/products/${this.selectItem._id}`,
          data
        )
        .then((response) => {
          console.log(response.data);
          this.loadItems();
          this.editing = false;
          this.selectItem = {};
        });
    },
    deleteItem(item) {
      this.axios
        .delete(`https://shop-api3.herokuapp.com/api/products/${item._id}`)
        .then((response) => {
          this.loadItems();
          console.log(response);
        });
    },
    onSelectItem(item) {
      this.selectItem = item;
      this.editing = true;
    },
  },
};
</script>

<style>
.content-title {
  background: rgb(84, 56, 158);
  background: linear-gradient(
    90deg,
    rgba(84, 56, 158, 1) 56%,
    rgba(178, 150, 255, 1) 100%
  );

  padding: 50px;
}
.content-info {
  background-color: #ffffff;
  padding: 50px;
}
.form-data {
  display: flex;
  flex-direction: row;
  align-items: center;
}
.form {
  padding: 30px;
  background-color: white;
  max-width: 800px;
  width: 500px;
  margin-left: auto;
}
.form-input-items {
  margin-top: 7px !important;
}
.btn-primary,
.btn-secondary {
  background-color: #54389e !important;
  color: white;
}
.save-button {
  margin-top: 15px;
  margin-left: auto;
}
.actions {
  display: flex;
  flex-direction: row;
  align-items: center;
}
.delete {
  background-color: #eccccf !important;
  color: black !important;
  margin-right: 10px;
  margin-left: auto;
}
.title {
  color: white;
  font-size: 25px;
  font-weight: normal;
  max-width: 600px;
}
.main-table {
  background-color: white;
}
</style>
