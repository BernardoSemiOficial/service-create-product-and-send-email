<script lang="ts">
import { defineComponent } from "vue";
import api from "../services/api";

interface FormData {
  product: {
    title: string;
    description: string;
    price: number;
    active: boolean;
  };
  isViewFeedback: boolean;
  isSuccess: boolean;
}

interface CreateProduct {
  title: string;
  description: string;
  price: number;
  active: boolean;
}

export default defineComponent({
  name: "Form",
  data(): FormData {
    return {
      product: {
        title: "",
        description: "",
        price: 0,
        active: true,
      },
      isViewFeedback: false,
      isSuccess: true,
    };
  },
  methods: {
    async createProduct(product: CreateProduct) {
      const body = product;
      try {
        const response = await api.post("/products", body);
        console.log(response);
        this.isViewFeedback = true;
        this.isSuccess = true;
      } catch (error) {
        console.log(error);
        this.isViewFeedback = true;
        this.isSuccess = false;
      }
    },
    handleCreateProduct(event: Event) {
      event.preventDefault();
      console.log(this.product);
      if (
        this.product.title != "" &&
        this.product.description != "" &&
        this.product.price > 0
      ) {
        console.log("oi");
        this.createProduct(this.product);
      }
    },
  },
});
</script>

<template>
  <section class="cadastro">
    <h1>Cadastro de produtos</h1>
    <form class="form" @submit="handleCreateProduct($event)">
      <div class="form__field">
        <label for="titulo">título</label>
        <input type="text" name="titulo" id="titulo" v-model="product.title" />
      </div>
      <div class="form__field">
        <label for="description">descrição</label>
        <input
          type="text"
          name="description"
          id="description"
          v-model="product.description"
        />
      </div>
      <div class="form__field">
        <label for="price">preço</label>
        <input type="number" name="price" id="price" v-model="product.price" />
      </div>
      <button type="submit">criar produto</button>
    </form>
    <p
      class="feedback"
      :class="[isSuccess ? 'success' : 'error']"
      v-show="isViewFeedback"
    >
      {{
        isSuccess
          ? "Produto criado com sucesso! =)"
          : "Não foi possível criar o produto =("
      }}
    </p>
  </section>
</template>

<style scoped>
.cadastro {
  width: 400px;
}
.cadastro h1 {
  text-align: center;
  margin: 50px 30px;
}
.form {
  width: 100%;
}
.form__field {
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: flex-start;
  margin: 10px 0px;
}
.form__field label {
  margin-right: 15px;
  font-size: 16px;
  font-weight: bold;
  min-width: 100px;
}
.form__field input {
  width: 100%;
  border-radius: 8px;
  border: 2px solid #222222;
  padding: 6px 10px;
  font-size: 14px;
  font-weight: 400;
}
.form button {
  width: 100%;
  padding: 10px 10px;
  border: 2px solid #222222;
  background-color: #ffffff;
  font-size: 16px;
  font-weight: bold;
  border-radius: 8px;
  margin-top: 20px;
  cursor: pointer;
}
.feedback {
  margin: 20px 0px;
  text-align: center;
  font-size: 16px;
  font-weight: bold;
  border: 2px solid #2bff00;
  border-radius: 8px;
  padding: 8px;
}
.feedback.error {
  border: 2px solid #ca0a0a;
}
.feedback.success {
  border: 2px solid #2bff00;
}
</style>
