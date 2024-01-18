
<template>
  <div>
    <h2>Edit Product</h2>
    <form @submit.prevent="updateProduct">
      <label for="editedTitle">Title:</label>
      <input v-model="editedTitle" required />
      
      <label for="editedPrice">Price:</label>
      <input v-model="editedPrice" type="number" required />
      
      <label for="editedDescription">Description:</label>
      <textarea v-model="editedDescription" required></textarea>
      
      <label for="editedBrand">Brand:</label>
      <input v-model="editedBrand" required />

      <button type="submit">Update</button>
      <button @click="closeForm">Cancel</button>
    </form>
  </div>
</template>

<script>
import axios from 'axios';

const baseURL = 'http://mail.zaxtom.com:3000';

export default {
  props: {
    productId: {
      type: Number,
      required: true,
    },
    onClose: {
      type: Function,
      required: true,
    },
    onUpdate: {
      type: Function,
      required: true,
    },
  },
  data() {
    return {
      editedTitle: '',
      editedPrice: 0,
      editedDescription: '',
      editedBrand: '',
    };
  },
  methods: {
    async updateProduct() {
      const updatedProduct = {
        title: this.editedTitle,
        price: this.editedPrice,
        description: this.editedDescription,
        brand: this.editedBrand,
      };

      try {
        await axios.patch(`${baseURL}/products/${this.productId}`, updatedProduct);
        this.onUpdate(); 
        this.closeForm();
      } catch (error) {
        console.error('Error updating product:', error);
      }
    },
    closeForm() {
      this.onClose();
      this.resetEditedProduct();
    },
    resetEditedProduct() {
      this.editedTitle = '';
      this.editedPrice = 0;
      this.editedDescription = '';
      this.editedBrand = '';
    },
  },
};
</script>

