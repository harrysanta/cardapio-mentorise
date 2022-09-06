<template>
  <div class="container">
    <div class="container-resume" >
      <h1>Cardápio</h1>
      <div class="resume" >
        <div class="products-content" >
          <span>Produtos </span>
          <span>
            {{ qtdTotalProducts }}
          </span>
        </div>
        <div class="delivery-content">
          <span>Taxa de Entrega</span>
          <span>
            R$ {{ resume.delivery.toFixed(2)}}
          </span>
        </div>
        <div class="total-content">
          <span>Total</span>
          <span>
            R$ {{ total.toFixed(2) }}
          </span>
        </div>
      </div>
    </div>
    <div class="container-menu">
      <h1 class="h1Menu">Menu</h1>
        <ul class="list-group" v-for="product in products" v-bind:key="product.name">
            <li class="list-group-item">
            <span>
              {{ product.name }}
            </span>
            <span>
              Qtd: {{ product.qtd }}
            </span>
            <span> 
              R$ {{ product.price.toFixed(2) }}
            </span>
            <span>
              R$ {{ product.sum.toFixed(2) }}
            </span>
            </li>
          <input v-on:change="addToCart($event, product.name)" placeholder="Quantidade" type="number" min="0" name="add-to-cart" class="inputQtd">
        </ul>
    <button @click="showModal = true" class="button">Confirmar Compra</button>
    </div> 
</div>

<ModalForm @close="closeModal" v-if="showModal == true"/>

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
      
      qtdTotalProducts: 0,
      total: 0,
      
      resume: {
        delivery: 10,
      },

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
          name: "Maionese",
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
      
      let item = this.products.find(item => item.name === name);
      let indexItem = this.products.indexOf(item);
     
      this.products[indexItem].qtd = (Number(e.target.value));
      this.products[indexItem].sum = (Number(e.target.value)) * (Number(this.products[indexItem].price));

      this.qtdTotalProducts = this.products.map(products => products.qtd).reduce((a, b) => a + b);
      this.total = this.products.map(products => products.sum).reduce((a, b) => a + b) + (Number(this.resume.delivery));

    },
    confirmBuy(){
      this.showModal = true
     
    },
    closeModal(){
      this.showModal = false
    },

}}
</script>

<style >
  *{
    font-size: 20px;
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    box-sizing: border-box;
  }

  h1{
    font-size: 35px;
    margin: 50px;
  }

  .container {
    border-width: 2px;
    border-style:solid;
    border-color:black;
  }
  .container-resume {
    display: row;
    justify-content: center;
    gap: 10px;
  }
  .resume {
    border-width: 2px;
    border-style:solid;
    border-color:black;
    
    display: flex;
    width: 80%;
    justify-content: space-between;
    margin: auto;
    padding: 20px;
  }

  .products-content, .delivery-content, .total-content{
    display: inline-grid;
    justify-items: center;
  }
  
  .container-menu{
    display: inline-grid;
    width: 100%;
    margin: auto;
    justify-items: center;
  }
  
  ul{
    border-width: 2px;
    border-style:solid;
    border-color:black;

    outline: none;
    width: 80%;
    padding: 10px;
  }

  .list-group-item{  
    list-style-type: none;
    justify-content: space-between;
    display: flex;
  }

  .inputQtd {
    font-size: 15px;
    height: 20px;
    margin-left: 24%;
  }

  .button {
    margin-bottom: 30px;
    margin-top: 15px;
  }

  .h1Menu{
    font-size: 35px;
    display: flex;
    justify-self: left;
    padding-left: 30px;
  }
</style>
