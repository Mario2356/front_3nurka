<template>
  <div class="container">
    <div class="row m-5">
      <h4>TELLIMUSE VORMISTAMINE</h4>
    </div>
    <div class="row justify-content-center ">
      <div class=" col-6 align-items-center ms-5">
        <div class="row">
          <h5>1. Minu jalgrattad</h5>
        </div>
        <div class="row justify-content-center">
          <table class="table table-success table-striped">
            <thead>
            <tr>
              <th scope="col">Ratas</th>
              <th scope="col">Ratta mudel/kirjeldus</th>
              <th scope="col">Kustuta valikust</th>
            </tr>
            </thead>
            <tbody>
            <tr v-for="bike in bikeResponse">
              <th scope="row">{{ bike.brandName }}</th>
              <td>{{ bike.bikeModel }}</td>
              <td><i v-on:click="deleteBikeInfo(bike.bikeId)" class="fa-solid fa-flag fa-trash-can"></i></td>
            </tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>


    <div class="row align-items-center mt-4">
      <p><h5>2. Jalgratta lisamiseks:</h5>
      <p><h6>Vali rippmenüüst jalgratta mark, lisa mudeli number või kirjeldus</h6>
    </div>


    <div class="row justify-content-center">
      <div class="col-3">
        <select v-on:change="clickSelectBrandEvent" v-model="selectedBrandId" class="form-select"
                aria-label="Default select example">
          <option selected disabled value="0">--Ratta mark--</option>
          <option v-for="brand in brands" :key="brand.brandId" :value="brand.brandId">
            {{ brand.brandName }}
          </option>
        </select>
      </div>
      <div class="col-3">
        <label for="Input"></label>
        <input v-model="bikeRequest.bikeModel" placeholder="Ratta mudel/kirjeldus">
      </div>
    </div>


    <div class="row justify-content-center mt-4 ">
      <div class="col-6">
        <button v-on:click="addBike" class="btn btn-outline-dark">Lisa ratas valikusse</button>
      </div>
    </div>



    <!-- ALERT MESSAGE - HETKEL EI TÖÖTA -->

    <!--    <div class="modal fade" id="exampleModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">-->
    <!--      <div class="modal-dialog">-->
    <!--        <div class="modal-content">-->
    <!--          <div class="modal-header">-->
    <!--            <h1 class="modal-title fs-5" id="exampleModalLabel">Kas soovid selle ratta kustutada?</h1>-->
    <!--            <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>-->
    <!--          </div>-->
    <!--          <div class="modal-fullscreen-sm-down">-->
    <!--          </div>-->
    <!--          <div class="modal-footer">-->
    <!--            <button v-on:click="deleteBikeInfo(bike.bikeId)" class="btn btn-success" type="button">Jah</button>-->
    <!--            <button type="button" class="btn btn-danger" data-bs-dismiss="modal">Ei</button>-->
    <!--          </div>-->
    <!--        </div>-->
    <!--      </div>-->
    <!--    </div>-->
    <div class="row justify-content-center mt-5">
      <p><h5>3. Vali teenus</h5>
    </div>


    <div class="justify-content-center">
      <button v-on:click="clickToRepairEvent" class=" btn btn-outline-success btn-lg m-5"> REMONT </button>
      <button v-on:click="clickToMaintenanceEvent" class="btn btn-outline-primary btn-lg m-5">HOOLDUS</button>
      <button v-on:click="clickToStorageEvent" class="btn btn-outline-warning btn-lg m-5">HOIUSTAMINE</button>
    </div>
    <div class="row align-items-end">
     <h5>4. Soovi korral lisa veel teenuseid</h5>
    </div>


    <div v-if="orderId!==null" class="row justify-content-center m-5">

      <h5>Kogutellimuse number: {{ bikeOrderResponse.orderNumber }}</h5>
      <div class="col-lg-11">
        <table class="table table-success table-striped">
          <thead>
          <tr>
            <th scope="col">Teenus</th>
            <th scope="col">Ratta mark</th>
            <th scope="col">Ratta mudel</th>
            <th scope="col">Probleemi kirjeldus</th>
            <th scope="col">Valitud pakett</th>
            <th scope="col">Teenuse hind</th>
          </tr>
          </thead>

          <tbody>
          <tr v-for="bikeOrder in bikeOrderResponse.bikeOrders" :key="bikeOrderResponse.bikeOrders.bikeOrderId">
            <td>{{ bikeOrder.workTypeName }}</td>
            <td>{{ bikeOrder.bikeBrandName }}</td>
            <td>{{ bikeOrder.bikeModel }}</td>
            <td>{{ bikeOrder.customerComment }}</td>
            <td>{{ bikeOrder.packageFieldName }}</td>
            <td>{{ bikeOrder.packageFieldPrice + " eurot" }}</td>
          </tr>
          </tbody>
        </table>
        <div class="row align-items-end">
        <h5>Kogumaksumus: {{ bikeOrderResponse.totalPrice + " eurot"}}</h5>
        </div>
      </div>
    </div>
    <div class="row">
      <p><h5>5. Täida kontaktandmed</h5>
    </div>

    <div class="row justify-content-center">
      <div class="col-3">
        <div class="form-check">
          <input v-model="selectedAddress" value="1" v-on:click="showProfileAddress" class="form-check-input"
                 type="radio"
                 name="flexRadioDefault" id="flexRadioDefault1">
          <label class="form-check-label" for="flexRadioDefault1">
            Kasuta profiiliaadressi
          </label>
        </div>

        <div class="form-check">
          <input v-model="selectedAddress" value="2" v-on:click="showCustomAddress" class="form-check-input"
                 type="radio"
                 name="flexRadioDefault" id="flexRadioDefault1">
          <label class="form-check-label" for="flexRadioDefault1">
            Kasuta teist aadressi
          </label>
        </div>
      </div>
    </div>

    <div v-if="selectedAddress == 1" class="row justify-content-center">
      <div class="col-4">

        <div class="row">
          <label for="exampleFormControlInput1"></label>
          <input selected disabled value v-model="locationRequest.streetName" class="form-control"
                 placeholder="Tänava nimi ja maja/maja ja korteri number*">
        </div>

        <div class="row">
          <label for="exampleFormControlInput1"></label>
          <input selected disabled value v-model="locationRequest.districtName" class="form-control"
                 placeholder="Linnaosa nimi">
        </div>

        <div class="row">
          <label for="exampleFormControlInput1"></label>
          <input selected disabled value v-model="locationRequest.phone" class="form-control"
                 placeholder="Telefoninumber*">
        </div>
      </div>
    </div>

    <div v-if="selectedAddress == 2" class="row justify-content-center">
      <div class="col-4">
        <div class="row">
          <label for="exampleFormControlInput1"></label>
          <input v-model="locationRequest.streetName" class="form-control"
                 placeholder="Tänava nimi ja maja/maja ja korteri number*">
        </div>

        <div class="row">
          <select v-on:change="clickSelectedDistrictEvent" v-model="selectedDistrictId" class="form-select" aria-label="Default select example">
            <option selected disabled value="0">Tallinna linnaosa*</option>
            <option v-for="district in districts" :key="district.districtId" :value="district.districtId">
              {{ district.districtName }}
            </option>
          </select>
        </div>

        <div class="row">
          <label for="exampleFormControlInput1"></label>
          <input v-model="locationRequest.phone" class="form-control" placeholder="Telefoninumber*">
        </div>
      </div>
    </div>
    <div class="m-5">
      <button v-on:click="submitOrder()" class="btn btn-success">Kinnita tellimus</button>
    </div>

  </div>

</template>

<script>
export default {
  name: 'OrderView',
  components: {},


  data: function () {
    return {
      userId: Number(sessionStorage.getItem('userId')),
      orderId: sessionStorage.getItem('orderId'),
      workTypeId: sessionStorage.getItem('workTypeId'),
      selectedBrandId: 0,

      selectedAddress: 1,

      selectedDistrictId: 0,

      errorSubmit: '',

      districts: {
        districtId: 0,
        districtName: '',
      },

      locationRequest: {
        useDefaultAddress: true,
        userId: 0,
        districtId: 0,
        streetName: '',
        phone: '',
        districtName: '',
      },

      brands: [
        {
          brandId: 0,
          brandName: '',
          brandIsOther: false,
        }
      ],

      bikeRequest: {
        userId: 0,
        bikeModel: '',
        brandId: 0
      },

      bikeResponse: {
        bikeId: 0,
        brandId: 0,
        brandName: '',
        bikeModel: '',
      },

      orderResponse: {
        orderId: 0,
        userId: 0,
        orderStatusId: 0,
        orderStatusName: '',
        addressId: 0,
        addressStreetName: '',
        number: '',
        dateFrom: '',
        dateTo: '',
        price: 0

      },
      bikeOrderResponse: [{
        orderNumber: '',
        totalPrice: 0,
        bikeOrders: [{
          bikeOrderId: 0,
          bikeBrandName: '',
          bikeModel: '',
          workTypeName: '',
          packageFieldName: '',
          packageFieldPrice: 0,
          customerComment: '',
          techComment: '',
        }]
      }]
    }
  },

  methods: {

    clickSelectedDistrictEvent: function () {
      this.$emit('clickSelectedDistrictEvent', this.selectedDistrictId)
    },

    getDistrictSelectBoxInfo: function () {
      this.$http.get("/order/districts")
          .then(response => {
            this.districts = response.data
          })
          .catch(error => {
            console.log(error)
          })
    },

    getBikeOrderInfo: function () {
      if (this.orderId !== null) {
        this.$http.get("/order/info", {
              params: {
                orderId: this.orderId,
              }
            }
        ).then(response => {
          this.bikeOrderResponse = response.data
        }).catch(error => {
          console.log(error)
        })
      }
    },

    submitOrder: function () {
      this.locationRequest.districtId = this.selectedDistrictId
      this.locationRequest.userId = this.userId
      this.locationRequest.useDefaultAddress = this.selectedAddress == 1
      this.errorSubmit = 'Tellimus kinnitatud!'
      this.addOrder()
      sessionStorage.removeItem('orderId')
      this.$router.push({name:'home'})

    },

    addOrder: function () {
      this.$http.put("/order/submit-order", null, {
            params: {
              orderId: this.orderId
            }
          }
      ).then(response => {
        this.addNewAddressToOrder()
      }).catch(error => {
        console.log(error)
      })
    },

    addNewAddressToOrder: function () {
      this.$http.post("/order/address", this.locationRequest
      ).then(response => {
        console.log(response.data)
      }).catch(error => {
        console.log(error)
      })
    },

    showProfileAddress: function () {
      this.selectedAddress = 1;
      this.$http.get("/order/profile", {
            params: {
              userId: this.userId,
            }
          }
      ).then(response => {
        this.locationRequest = response.data
      }).catch(error => {
        console.log(error)
      })
    },

    showCustomAddress: function () {
      this.selectedAddress = 2;
      this.locationRequest.streetName = ''
      this.locationRequest.phone = ''
      this.locationRequest.districtId = 0
    },

    deleteBikeInfo: function (bikeId) {
      this.$http.delete("/order/bike", {
            params: {
              bikeId: bikeId,
            }
          }
      ).then(response => {
        this.getBike()
        console.log(response.data)
      }).catch(error => {
        console.log(error)
      })
    },

    getBrandsSelectBoxInfo: function () {
      this.$http.get("/order/brand")
          .then(response => {
            this.brands = response.data
          })
          .catch(error => {
            console.log(error)
          });
    },

    clickSelectBrandEvent: function () {
      this.$emit('clickSelectBrandEvent', this.selectedBrandId)
    },

    addBike: function () {
      this.bikeRequest.userId = this.userId
      this.bikeRequest.brandId = this.selectedBrandId
      this.$http.post("/order/bike", this.bikeRequest
      ).then(response => {
        this.getBike();
        console.log(response.data)
      }).catch(error => {
        console.log(error)
      })
    },

    getBike: function () {
      this.$http.get("/order/bike", {
        params: {
          userId: this.userId
        }
      }).then(response => {
        this.bikeResponse = response.data
      })
          .catch(error => {
            console.log(error)
          })
    },

    clickToRepairEvent: function () {
      if (this.orderId === null) {

        this.$http.post("/order/start", null, {
              params: {
                userId: this.userId,
              }
            }
        ).then(response => {
          this.orderResponse = response.data
          sessionStorage.setItem('orderId', this.orderResponse.orderId)
          sessionStorage.setItem('workTypeId', '1')

          console.log(response.data)
          this.$router.push({
            name: 'repairRoute'
          })
        }).catch(error => {
          console.log(error)
        })
      } else {
        this.$router.push({
          name: 'repairRoute'
        })
        sessionStorage.setItem('workTypeId', '1')
      }
    },

    clickToStorageEvent: function () {
      if (this.orderId === null) {

        this.$http.post("/order/start", null, {
              params: {
                userId: this.userId,
              }
            }
        ).then(response => {
          this.orderResponse = response.data
          sessionStorage.setItem('orderId', this.orderResponse.orderId)
          sessionStorage.setItem('workTypeId', '3')
          console.log(response.data)
          this.$router.push({
            name: 'storageRoute'
          })

        }).catch(error => {
          console.log(error)
        })
      } else {
        this.$router.push({
          name: 'storageRoute'
        })
        sessionStorage.setItem('workTypeId', '3')
      }
    },

    clickToMaintenanceEvent: function () {
      if (this.orderId === null) {

        this.$http.post("/order/start", null, {
              params: {
                userId: this.userId,
              }
            }
        ).then(response => {
          this.orderResponse = response.data
          sessionStorage.setItem('orderId', this.orderResponse.orderId)
          sessionStorage.setItem('workTypeId', '2')

          console.log(response.data)
          this.$router.push({
            name: 'maintenanceRoute'
          })
        }).catch(error => {
          console.log(error)
        })
      } else {
        this.$router.push({
          name: 'maintenanceRoute'
        })
        sessionStorage.setItem('workTypeId', '2')
      }
    }
  },

  beforeMount() {
    this.showProfileAddress()
    this.getBrandsSelectBoxInfo()
    this.getBike()
    this.getBikeOrderInfo()
    this.getDistrictSelectBoxInfo()
  }
}
</script>

