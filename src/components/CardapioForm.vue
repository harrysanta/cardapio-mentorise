<template>
  <div class="container">
    <div class="container-resume">
      <h1>Cardápio</h1>
      <div class="resume">
        <div class="products-content">
          <span>Produtos </span>
          <span>
            {{ qtdTotalProducts }}
          </span>
        </div>
        <div class="delivery-content">
          <span>Taxa de Entrega</span>
          <span> R$ {{ deliveryTax.toFixed(2) }} </span>
        </div>
        <div class="total-content">
          <span>Total</span>
          <span> R$ {{ total.toFixed(2) }} </span>
        </div>
      </div>
    </div>

    <div class="container-menu">
      <h1 class="h1Menu">Menu</h1>
      <ul
        class="list-group"
        v-for="product in products"
        v-bind:key="product.name"
      >
        <li class="list-group-item">
          <span class="span-list">
            {{ product.name }}
          </span>

          <div class="qtd-container">
            <span> Qtd: </span>
            <input
              v-on:change="addToCart($event, product.name)"
              placeholder="Quantidade"
              type="number"
              min="0"
              name="add-to-cart"
              class="inputQtd"
            />
          </div>

          <span class="span-list"> R$ {{ product.price.toFixed(2) }} </span>

          <span class="span-list"> R$ {{ product.sum.toFixed(2) }} </span>
        </li>
        <div class="comments-container">
          <p>
            <textarea
              v-model="message"
              placeholder="Faça uma oberservação sobre este pedido"
              name="message"
              class="message-input"
            ></textarea>
          </p>
          <button v-on:click="addMessage" type="submit" class="btn-addMessage">
            Adicionar Observação
          </button>
        </div>
      </ul>
      <button @click="showModal = true" class="button">Confirmar Compra</button>
    </div>
  </div>

  <ModalForm @close="closeModal" v-if="showModal == true" />
</template>
  
  <script>
  import ModalForm from "./ModalForm.vue";

  export default {
    name: "App",
    components: {
      ModalForm,
    },

    data() {
      return {
        showModal: false,

        qtdTotalProducts: 0,
        total: 0,
        deliveryTax: 10,

        products: [
          {
            name: "Caipirinha",
            id: "1",
            price: 10,
            qtd: 0,
            sum: 0,
          },
          { name: "Churrasco", price: 80, qtd: 0, sum: 0 },
          {
            name: "Vinho",
            price: 15,
            qtd: 0,
            sum: 0,
          },
          {
            name: "Pão de Alho",
            price: 10,
            qtd: 0,
            sum: 0,
          },
          {
            name: "Maionese",
            price: 10,
            qtd: 0,
            sum: 0,
          },
        ],
      };
    },

    methods: {
      addToCart(e, name) {
        e.preventDefault();

        let item = this.products.find((item) => item.name === name);
        let indexItem = this.products.indexOf(item);

        this.products[indexItem].qtd = Number(e.target.value);
        this.products[indexItem].sum =
          Number(e.target.value) * Number(this.products[indexItem].price);

        this.qtdTotalProducts = this.products
          .map((products) => products.qtd)
          .reduce((a, b) => a + b);
        this.total =
          this.products.map((products) => products.sum).reduce((a, b) => a + b) +
          Number(this.deliveryTax);
      },
      confirmBuy() {
        this.showModal = true;
      },
      closeModal() {
        this.showModal = false;
        this.total = 0;
        this.qtdTotalProducts = 0;

        this.products.forEach((item) => {
          item.sum = 0;
        });
        // this.addToCart(e.event.target.reset())
      },
    },
  };
</script>
  
  <style >
  * {
    font-size: 20px;
    font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
    box-sizing: border-box;
  }

  h1 {
    font-size: 35px;
    margin: 50px;
  }

  .container {
    border-width: 2px;
    border-style: solid;
    border-color: black;
  }
  .container-resume {
    display: row;
    justify-content: center;
    gap: 10px;
  }
  .resume {
    border-width: 2px;
    border-style: solid;
    border-color: black;

    display: flex;
    width: 80%;
    justify-content: space-between;
    margin: auto;
    padding: 20px;
  }

  .products-content,
  .delivery-content,
  .total-content {
    display: inline-grid;
    justify-items: center;
  }

  .container-menu {
    display: inline-grid;
    width: 100%;
    margin: auto;
    justify-items: center;
  }

  ul {
    border-width: 2px;
    border-style: solid;
    border-color: black;

    outline: none;
    width: 80%;
    padding: 10px;
  }

  .list-group-item {
    list-style-type: none;
    justify-content: space-between;
    display: flex;
  }

  .button {
    margin-bottom: 30px;
    margin-top: 15px;
  }

  .h1Menu {
    font-size: 35px;
    display: flex;
    justify-self: left;
    padding-left: 30px;
  }

  .span-list {
    min-width: 107px;
  }

  .inputQtd {
    font-size: 13px;
    height: 25px;
    margin-left: 29%;
    width: 95px;
    /* margin-top: -3px; */
    position: relative;
    top: 2px;
  }

  .qtd-container {
    display: flex;
    flex-flow: row;
    justify-content: space-around;
    align-items: center;
    margin-top: 5px;
  }

  .qtd-container span {
    min-width: 10px;
  }

  .message-input {
    font-size: 15px;
    min-width: 26%;
    height: 85px;
    position: relative;
    top: 10px;
  }

  .btn-addMessage {
    font-size: 15px;
    min-width: 10%;
    height: 10%;
    position: relative;
    bottom: 10px;
  }
</style>
  