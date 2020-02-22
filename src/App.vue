<template>
  <div id="app">
    <b-navbar toggleable="lg" type="dark" variant="info">
      <b-navbar-brand href="#">Baannaidin</b-navbar-brand>
      <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>
      <b-collapse id="nav-collapse" is-nav>
        <!-- Right aligned nav items -->
        <b-navbar-nav class="ml-auto">
          <b-nav-form>
            <b-form-input size="sm" class="mr-sm-2" placeholder="Search"></b-form-input>
            <b-button size="sm" class="my-2 my-sm-0" type="submit">Search</b-button>
          </b-nav-form>

          <b-nav-item>
            <b-button v-b-modal="'cart'" variant="info">ตะกร้าสินค้า</b-button>
          </b-nav-item>
        </b-navbar-nav>
      </b-collapse>
    </b-navbar>

    <b-container>
      <b-row class="product-list pt-3 pb-3">
        <b-col v-for="(book, index) in books" cols="3" :key="`book-${index}`">
          <b-card
            :img-src="require(`@/assets/img/${book.img}.jpg`)"
            img-alt="Image"
            img-width="300"
            img-top
            style="max-width: 20rem;"
            class="mb-2 product">
            <b-card-title>{{ book.name }}</b-card-title>
            <b-card-sub-title class="mb-2">ราคา {{ book.price }} บาท</b-card-sub-title>
            <div class="d-flex justify-content-between">
              <b-button variant="info" @click="addToCart(book)">หยิบใส่ตะกร้า</b-button>
              <b-form-input v-model="book.amount" type="number" min="0" max="5" />
            </div>
          </b-card>
        </b-col>
      </b-row>
    </b-container>

    <b-modal id="cart" scrollable title="ตะกร้าสินค้า">
      <div v-if="cart.length > 0" class="cart-items">
        <b-card
          v-for="(item, index) in cart"
          no-body
          :img-src="require(`@/assets/img/${item.img}.jpg`)"
          img-alt="Image"
          img-width="100"
          img-left
          class="mb-2 cart-item"
          :key="`item-${index}`">
          <b-card-body>
            <b-card-title>{{ item.name }}</b-card-title>
            <b-card-sub-title class="mb-2">ราคา {{ item.price }} บาท</b-card-sub-title>
            <b-card-text>จำนวน {{ item.amount }} เล่ม</b-card-text>
          </b-card-body>
          <b-card-body class="align-items-center">
            <b-card-text>
              รวม {{ item.amount * item.price }} บาท
            </b-card-text>
          </b-card-body>
        </b-card>
      </div>
      <div v-else class="cart-items">
        ไม่มีสินค้าในตะกร้า
      </div>
      <template slot="modal-footer">
        <div>
          <p>สินค้าทั้งหมดราคา: {{ total }} บาท</p>
          <p>ส่วนลด: </p>
          <p>ราคา: </p>
        </div>
      </template>
    </b-modal>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      books: [
        {
          id: 1,
          img: 'H1',
          name: 'Harry Potter 1',
          price: 100,
          amount: 1
        },
        {
          id: 2,
          img: 'H2',
          name: 'Harry Potter 2',
          price: 100,
          amount: 1
        },
        {
          id: 3,
          img: 'H3',
          name: 'Harry Potter 3',
          price: 100,
          amount: 1
        },
        {
          id: 4,
          img: 'H4',
          name: 'Harry Potter 4',
          price: 100,
          amount: 1
        },
        {
          id: 5,
          img: 'H5',
          name: 'Harry Potter 5',
          price: 100,
          amount: 1
        },
        {
          id: 6,
          img: 'H6',
          name: 'Harry Potter 6',
          price: 100,
          amount: 1
        },
        {
          id: 7,
          img: 'H7',
          name: 'Harry Potter 7',
          price: 100,
          amount: 1
        }
      ],
      cart: []
    }
  },
  computed: {
    total () {
      let total = 0
      if (this.cart.length > 0) {
        total = this.cart.reduce((sumPrice, product) => sumPrice + (product.price * product.amount), 0)
      }
      return total 
    }
  },
  methods: {
    addToCart (data) {
      const index = this.cart.findIndex((r) => r.id === data.id)
      if (index !== -1) {
        this.cart[index].amount += parseInt(data.amount)
      } else {
        this.cart.push({
          ...data,
          amount: parseInt(data.amount)
        })
      }
    }
  }
}
</script>

<style>
.product .form-control {
  width: 65px;
}
#cart .modal-footer {
  justify-content: flex-start;
}
</style>