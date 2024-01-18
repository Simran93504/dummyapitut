<template>
  <div>
    <h1>API</h1>
    <div>
      <AddProduct />
    </div>
   
    <div>
    <ProductUpdateForm
        v-if="showUpdateForm"
        :product-id="selectedProductId"
        :on-close="closeUpdateForm"
        :on-update="fetchLists"
      />
    </div>
    <div>
      <table class="product-table">
        <thead>
          <tr>
            <th>ID</th>
            <th>Title</th>
            <th>Price</th>
            <th>Description</th>
            <th>Brand</th>
            <th>Update</th>
            <th>Delete</th>

          </tr>
        </thead>
        <tbody>
          <tr v-for="item in lists" :key="item.id">
            <td>{{ item.id }}</td>
            <td>{{ item.title }}</td>
            <td>{{ item.price }}</td>
            <td>{{ item.description }}</td>
            <td>{{ item.brand }}</td>
            <td>
              <button @click="openUpdateForm(item.id)">Edit</button>
              
            </td>
            <td>
              <button @click="deleteProduct(item.id)">Delete</button>
            </td>
          </tr>
        </tbody>
      </table>
      
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import AddProduct from './components/AddProduct.vue';
import ProductUpdateForm from './components/ProductUpdateForm.vue';

export default {
  name: 'App',
  data() {
    return {
      lists: [],
      showUpdateForm: false,
      selectedProductId: null,
    };
  },
  components: {
    AddProduct,
    ProductUpdateForm,
  },
  async mounted() {
    await this.fetchLists();
  },
  methods: {
    async fetchLists() {
      try {
        const result = await axios.get('http://mail.zaxtom.com:3000/products');
        this.lists = result.data;
        this.showUpdateForm = false;
      } catch (error) {
        console.error('Error fetching products:', error);
      }
    },
    async deleteProduct(id) {
      try {
        await axios.delete(`http://mail.zaxtom.com:3000/products/${id}`);
        this.lists = this.lists.filter(p => p.id !== id);
      } catch (error) {
        console.error('Error deleting product:', error);
      }
    },
    openUpdateForm(productId) {
      console.log(productId);
      this.selectedProductId = productId;
      this.showUpdateForm = true;
    },
    closeUpdateForm() {
      this.showUpdateForm = false;
      this.selectedProductId = null;
    },
  },
};
</script>

<style scoped>
.product-table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 20px;
}

.product-table th,
.product-table td {
  border: 1px solid #6e5959;
  padding: 8px;
  text-align: left;
}

.product-table th {
  background-color: #f2f2f2;
}
</style>
