<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-title>ลูกค้า</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content :fullscreen="true">
      <ion-header collapse="condense">
        <ion-toolbar>
          <ion-title size="large">ลูกค้า</ion-title>
        </ion-toolbar>
      </ion-header>
     <ion-button @click="addCustomer" color="success">เพิ่มข้อมูลลูกค้า</ion-button>
        <ion-card v-for="(customer, index) in customers" :key="index">
    <ion-card-header>
      <img src="https://images.squarespace-cdn.com/content/v1/59bf8dc3e5dd5b141a2ba135/1516790730855-FS2J7B7R5EN621L5265W/ke17ZwdGBToddI8pDm48kMoBh-gUUpLxA945CkqE88RZw-zPPgdn4jUwVcJE1ZvWQUxwkmyExglNqGp0IvTJZUJFbgE-7XRK3dMEBRBhUpxc0vMDkiaG6QWQKJgwbRrAUhnwnjNmpNhyl7xGpff54do7opHqSz3AvDy5J2lrNB4/Homepage-section3-part-credit-card1.png"/>
      <ion-card-subtitle>ชื่อ-นามสกุล</ion-card-subtitle>
      <ion-card-title>{{ customer.c_name }} {{ customer.c_lastname}} </ion-card-title>
      <ion-card-subtitle>รหัสลูกค้า: {{ customer.c_id}} </ion-card-subtitle>
    </ion-card-header>
    
    <ion-card-content>
        อีเมล์ : {{ customer.c_email }} <br />
        เบอร์โทรศัพท์: {{ customer.c_tel}}
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
  name: 'customer',
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
      customers: null,
    };
  },
    async ionViewDidEnter() {
    console.log("ionViewDidEnter");
    this.loadCustomers();
    },
    methods: {
      async loadCustomers() {
        const res = await axios.get("http://localhost:3000/customers");
        this.customers = res.data;
      },
    async addCustomer() {
        console.log("addCustomer");
        const alert = await alertController.create({
          header: "เพิ่มข้อมูลลูกค้า",
          inputs: [
            {
              name: "c_id",
              placeholder: "กรอกรหัสสมาชิก",
              type: "number"
            },
            {
              name: "c_name",
              placeholder: "ชื่อ"
            },
            {
              name: "c_lastname",
              placeholder: "นามสกุล"
            },
            {
              name: "c_email",
              placeholder: "อีเมล์"
            },
            {
              name: "c_tel",
              placeholder: "เบอร์โทรศัพท์",
              type: "number"
            }
          ],
          buttons: [
            {
              text: "บันทึก",
              handler: async (value) => {
                /* eslint-disable @typescript-eslint/camelcase */
                const { c_id, c_name, c_lastname, c_email, c_tel } = value;

                if(c_id.length > 0 &&
                c_name.length > 0 &&
                c_lastname.length > 0 &&
                c_email.length > 0 &&
                c_tel.length > 0
                ) {
                  const res = axios.post("http://localhost:3000/customers/insert",{
                    c_id,
                    c_name,
                    c_lastname,
                    c_email,
                    c_tel
                  });
                  
                await this.loadCustomers();

                console.log(c_id);
                console.log(c_name);
                console.log(c_lastname);
                console.log(c_email);
                console.log(c_tel);
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