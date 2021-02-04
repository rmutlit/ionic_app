<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-title>สินค้า</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content :fullscreen="true">
      <ion-header collapse="condense">
        <ion-toolbar>
          <ion-title size="large">สินค้า</ion-title>
        </ion-toolbar>
      </ion-header>

  <ion-button @click="addProduct" color="success">เพิ่มสินค้า</ion-button>
        <ion-card v-for="(product, index) in products" :key="index">
    <ion-card-header>
      <img src="https://i.ytimg.com/vi/sMCFHP-XKi0/maxresdefault.jpg"/>
      <ion-card-subtitle>ชื่อสินค้า</ion-card-subtitle>
      <ion-card-title>{{ product.p_name }}</ion-card-title>
      <ion-card-subtitle>รหัสสินค้า {{ product.p_id}}</ion-card-subtitle>
    
    </ion-card-header>
    
    <ion-card-content>
        ราคาทุน : {{ product.p_price_cost }} <br />
        ราคาขาย : {{ product.p_price_sell }}
    </ion-card-content>
  </ion-card>

    </ion-content>
  </ion-page>
</template>

<script>
import { 
  alertController,
  IonButton,
  IonCard, 
  IonCardHeader,
  IonCardSubtitle,
  IonCardTitle, 
  IonCardContent, 
  IonPage, 
  IonHeader, 
  IonToolbar,
  IonTitle,
  IonContent
  } from '@ionic/vue';
import axios from 'axios';

export default  {
  name: "product",
  components: { 
  IonButton,
  IonCard, 
  IonCardHeader,
  IonCardSubtitle,
  IonCardTitle, 
  IonCardContent, 
  IonHeader, 
  IonToolbar, 
  IonTitle, 
  IonContent,
  IonPage 
    },
  data() {
    return {
      products: null,
    };
  },
  async ionViewDidEnter() {
    console.log("ionViewDidEnter");
    this.loadProducts();
  },
  methods: {
    async loadProducts() {
    const res = await axios.get("http://localhost:3000/products");
    this.products = res.data;
    },
    async addProduct() {
      console.log("addProduct");
      const alert = await alertController.create({
        header: "เพิ่มข้อมูลสินค้า",
        inputs: [
          {
            name: "p_id",
            placeholder: "กรอกรหัสสินค้า",
            type: "number"
          },
          {
            name: "p_name",
            placeholder: "กอกชื่อสินค้า"
          },
          {
            name: "p_price_cost",
            placeholder: "กรอกราคาต้นทุน",
            type: "number"
          },
          {
            name: "p_price_sell",
            placeholder: "กรอกราคาขาย",
            type: "number"
          }
        ],
        buttons: [
          {
            text: "บันทึก",
            handler: async (value) => { //ส่งข้อมุลไปให้ backend ionicapi
              
              /* eslint-disable @typescript-eslint/camelcase */
              const { p_id, p_name, p_price_cost, p_price_sell } = value;

              if(p_id.length > 0 &&
               p_name.length > 0 &&
               p_price_cost.length > 0 &&
               p_price_sell.length > 0
               ) {
               const res = await axios.post("http://localhost:3000/products/insert",{
                  p_id, 
                  p_name, 
                  p_price_cost,
                  p_price_sell
                 });
              
              await this.loadProducts();

              console.log(p_id);
              console.log(p_name);
              console.log(p_price_cost);
              console.log(p_price_sell);
               } else {
                 console.log("กรุณากรอกข้อมูลให้ครบถ้วน");
               }
            },
            },
          {
            text: "ยกเลิก",
            handler: () => {
              console.log("ยกเลิก");
            }
          }
        ]
      });
    return alert.present();

    }
  }
};
</script>