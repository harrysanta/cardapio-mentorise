<template>
  <div class="container">

  <div class="container-resume" >
    <h1>Cardápio</h1>
    <div class="resume" >
      <div class="products-content" >
        <span>Produtos</span>
        <span>{{ qtdTotalProducts }}</span>
      </div>
      <div class="delivery-content">
        <span>Entrega</span>
        <span>R$ {{ resume.delivery}}</span>
      </div>
      <div class="total-content">
        <span>Total</span>
        <span>R$ {{ total }}</span>
      </div>
    </div>
  </div>

  <div class="container-menu">
    <ul class="list-group" v-for="product in products" v-bind:key="product.name">
      <li class="list-group-item">{{ product.name }} {{ product.qtd }} {{ product.price }} {{ product.sum }} </li>
      <input v-on:change="addToCart($event, product.name)" placeholder="Quantidade" type="number" min="0" name="add-to-cart">
    </ul>
    <button @click="showModal = true">Comfirmar Compra</button>
  </div>
  <ModalForm @close="closeModal" v-if="showModal == true"/>
</div>
</template>

<script>
import ModalForm from './components/ModalForm.vue'

export default {
  name: 'App',
  components: {
    ModalForm
},
  data(){
    return {
      showModal: false,

      resume: {
        qtdTotalProducts: '',
        delivery: 10,
        total: ''
      },
      sumTotalProducts: {
        teste: 0,
      },
      cart: [],
      products: [
        {
          name: "Caipirinha",
          id: '1',
          price: 10,
          qtd: 0,
          sum: 0
        },
        { name: "Churrasco",
          price: 8,
          qtd: 0,
          sum: 0
        },
        {
          name: "Vinho",
          price: 15,
          qtd: 0,
          sum: 0
        },
        {
          name: "Pão de Alho",
          price: 5,
          qtd: 0,
          sum: 0
        },
        {
          name: "butter",
          price: 4,
          qtd: 0,
          sum: 0
        },
      ],
    };
  },

  methods: {
    addToCart(e, name) {
      e.preventDefault();
      
      let item = this.products.find(item => item.name === name)
      let indexItem = this.products.indexOf(item)
     
      this.products[indexItem].qtd = (Number(e.target.value))
      this.products[indexItem].sum = (Number(e.target.value)) * (Number(this.products[indexItem].price))

      this.qtdTotalProducts = this.products.map(products => products.qtd).reduce((a, b) => a + b)
      this.total = this.products.map(products => products.sum).reduce((a, b) => a + b) + (Number(this.resume.delivery))

    },
    confirmBuy(){
      this.showModal = true
     
    },
    closeModal(){
      this.showModal = false
    },

}}
</script>

<style>
  .resume{
    display: flex;
    width: 400px;
    justify-content: space-between;
  }
</style>
