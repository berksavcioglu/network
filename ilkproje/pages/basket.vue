<template>
  <section class="h-100" style="background-color: #eee;">
  <div class="container h-100 py-5">
    <div class="row d-flex justify-content-center align-items-center h-100">
      <div class="col-10">

        <div class="d-flex justify-content-between align-items-center mb-4">
          <h3 class="fw-normal mb-0 text-black">Shopping Cart</h3>
          <div>
            <p class="mb-0"><span class="text-muted">Sort by:</span> <a href="#!" class="text-body">price <i
                  class="fas fa-angle-down mt-1"></i></a></p>
          </div>
        </div>

        <div v-for="(item,index) in basketData" :key="index" class="card rounded-3 mb-4">
          <div class="card-body p-4">
            <div class="row d-flex justify-content-between align-items-center">
              <div class="col-md-2 col-lg-2 col-xl-2">
                <img
                  :src="item.src"
                  class="img-fluid rounded-3" alt="Cotton T-shirt">
              </div>
              {{item.productName }}
              <div class="col-md-3 col-lg-3 col-xl-2 d-flex">
                <button class="btn btn-link px-2"
                  onclick="this.parentNode.querySelector('input[type=number]').stepDown()">
                  <i class="fas fa-minus"></i>
                </button>

                <input id="form1" min="0" name="quantity" value="1" type="number"
                  class="form-control form-control-sm" />

                <button class="btn btn-link px-2"
                  onclick="this.parentNode.querySelector('input[type=number]').stepUp()">
                  <i class="fas fa-plus"></i>
                </button>
              </div>
              <div class="col-md-3 col-lg-2 col-xl-2 offset-lg-1">
                <h5 class="mb-0">{{item.unitPrice}}</h5>
               
              
              </div>
              <BasketItem v-for="item in basketData" :key="item.id" :item="item" @deleteItem="deleteItem(item)" />
              </div>
             
              <div class="col-md-1 col-lg-1 col-xl-1 text-end">
                <a href="#!" class="text-danger"><i class="fas fa-trash fa-lg"></i></a>
              </div>
            </div>
          </div>
        </div>


        <div class="card">
          <div class="card-body">
            <hr class="my-4">

                    <div class="d-flex justify-content-between mb-4">
                      <h5 class="text-uppercase">items {{ basketData.length }}</h5>
                      <h5>$ {{ this.getTotalPrice.toFixed(2)}}</h5>
                    </div>
           
            <button type="button" class="btn btn-warning btn-block btn-lg">Proceed to Pay</button>
         
          </div>
        </div>

      </div>
    </div>
  
</section>
</template>

<script>
import { collection, query, where, getDocs,doc,updateDoc,arrayRemove } from "firebase/firestore";
import { mapGetters } from "vuex";
import { db } from "~/firebase";
import { ref } from 'vue'
export default {
  setup() {
    const count = ref(item)
    return {
      count
    }
  },
data() {
    return {
      dbBasketData: [],
      basketData: [],
      totalPrice:0
    }
  },

  async mounted() {
    const q = query(collection(db, "users"), where("email", "==", this.getUser.email));
    const basket = await getDocs(q);
    basket?.forEach((doc) => {

      this.dbBasketData?.push(doc.data().basket)

      this.dbBasketData?.forEach((item) => {
        item?.forEach((item) => {
          this.basketData.push(item)
        })
      })
    });
    this.basketData.forEach((item) => {


this.totalPrice = this.totalPrice + item.unitPrice

})

this.$store.commit('setTotalPrice', this.totalPrice)

console.log(this.getTotalPrice);

  },

  methods: {
    async deleteItem(item){
      const targetDB = doc(db, "users", this.getUser.email);


      await updateDoc(targetDB, {
        basket: arrayRemove(item)
      });

      this.basketData = this.basketData.filter(i => i.id != item.id)
      this.totalPrice = this.totalPrice - item.unitPrice
      this.$store.commit('setTotalPrice', this.totalPrice)
    }
  },

  computed: {

    ...mapGetters(['getUser', 'getTotalPrice'])

  }



}
</script>


<style>

html,body{
  background-color: #eee;
}

</style>
