<template>
  <div class="relative overflow-x-auto">
    <table id="editable-table" class="w-full text-sm text-left text-gray-500 dark:text-gray-400">
      <thead class="text-xs text-gray-700 uppercase bg-gray-50 dark:bg-gray-700 dark:text-gray-400">
        <tr>
          <th scope="col" class="px-6 py-3">Product Code</th>
          <th scope="col" class="px-6 py-3">Product Name</th>
          <th scope="col" class="px-6 py-3">Color</th>
          <th scope="col" class="px-6 py-3">Category</th>
          <th scope="col" class="px-6 py-3">Price</th>
        </tr>
      </thead>
      <tbody id="table-body">
        <tr v-for="(product, index) in products" :key="index"
          class="bg-white border-b dark:bg-gray-800 dark:border-gray-700">
          <td class="px-6 py-4">
            <select v-model="product.code" @change="fillData(index)" class="w-full border-none outline-none bg-white rounded-sm">
              <option disabled value="">Please select a product code</option>
              <option v-for="data in productDataArray" :key="data.code" :value="data.code">{{ data.code }}</option>
            </select>
          </td>
          <td class="px-6 py-4">
            <input type="text" class="border-none bg-white rounded-xs px-3 py-2 focus:outline-none focus:ring-1 focus:ring-gray-300 focus:ring-opacity-50"
              v-model="product.name" placeholder="Product Name" />
          </td>
          <td class="px-6 py-4">
            <input type="text" class="w-full border-none outline-none bg-white rounded-sm"
              v-model="product.color" placeholder="Color" />
          </td>
          <td class="px-6 py-4">
            <input type="text" class="w-full border-none outline-none bg-white rounded-sm"
              v-model="product.category" placeholder="Category" />
          </td>
          <td class="px-6 py-4">
            <input type="text" class="w-full border-none outline-none bg-white rounded-sm"
              v-model="product.price" placeholder="Price" />
          </td>
        </tr>
      </tbody>
    </table>
    <button id="add-row-btn" class="mt-4 px-4 py-2 bg-blue-500 text-white" @click="addRow">
      Add Row
    </button>
    <button id="save-btn" class="mt-4 px-4 py-2 ml-4 bg-green-500 text-white" @click="saveChanges">
      Save Changes
    </button>
    <pre class="mt-4">{{ products }}</pre>
  </div>
</template>

<script setup>
import { ref, computed, onMounted } from "vue";

const products = ref([]);

const productDataArray = [
  {
    code: "2313",
    name: "Apple MacBook Pro 17",
    color: "Silver",
    category: "Laptop",
    price: "$2999",
  },
  {
    code: "2314",
    name: "Microsoft Surface Pro",
    color: "White",
    category: "Laptop PC",
    price: "$1999",
  },
  {
    code: "2315",
    name: "Magic Mouse 2",
    color: "Black",
    category: "Accessories",
    price: "$99",
  },
];

const allFieldsFilled = computed(() => {
  return products.value.every(
    (product) =>
      product.code && product.name && product.color && product.category && product.price
  );
});

function addRow() {
  if (allFieldsFilled.value) {
    products.value.push({ code: "", name: "", color: "", category: "", price: "" });
    localStorage.setItem("products", JSON.stringify(products.value));
    console.log(products.value);
  } else {
    alert("Please fill in all fields of the previous row before adding a new one.");
  }
}

function fillData(index) {
  const selectedProduct = productDataArray.find(p => p.code === products.value[index].code);
  if (selectedProduct) {
    products.value[index] = {...selectedProduct};
  }
}

function saveChanges() {
  if (confirm("Are you sure you want to save changes?")) {
    alert("Products:\n" + JSON.stringify(products.value, null, 2));
    console.log(products.value);
    products.value = [];
    localStorage.removeItem("products");
  } else {
    alert("Changes not saved");
  }
}

onMounted(() => {
  const savedProducts = localStorage.getItem("products");
  if (savedProducts) {
    products.value = JSON.parse(savedProducts);
  } else {
    addRow();
  }
});
</script>
