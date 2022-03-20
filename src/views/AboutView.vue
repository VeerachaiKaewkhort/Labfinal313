<template>
  <div  class="about">
    <h1>CONFIRM PAYMENT</h1>
    เลขคำสั่งซื้อ:<br>
    <input type="text" v-model="order" placeholder="order No."><br>
    <p>เลขสั่งซื้อ 5 หลักสามารถเรียกดูได้ในอีเมลยืนยันออเดอร์ </p>
    ชื่อ:<br>
    <input type="text" v-model="fname" placeholder="ป้อนชื่อ"><br>
    นามสกุล:<br>
    <input type="text" v-model="lname" placeholder="ป้อนนามสกุล"><br>
    ที่อยู่:<br>
    <textarea name="address" v-model="address" cols="35" rows="5" placeholder="กรุณาป้อนข้อมูลที่อยู่ให้ถูกต้องเพื่อการจัดส่งที่ถูกต้อง"></textarea><br>
    เบอร์โทรศัพท์:<br>
    <input type="tel" v-model="tel"><br>
    Email:<br>
    <input type="text" v-model="email" placeholder="ป้อนE-mailเพื่อรับข่าวสาร" ><br>
    <div>
    แนบเอกสาร(สลิปเงินconfirm):<br>
    <input type="file" ><br>
    </div>
    <button @click="addData()">SUBMIT</button>
    <button @click="readData()">อ่านข้อมูล</button>
    <h3>{{ csDoc }}</h3>
  </div>
</template>

<script>
import { collection, addDoc , getDocs } from "firebase/firestore";
import db from "../plugins/firebaselnit";

export default {
  data() {
    return{
      csDoc: [],
      fname:"",
      lname:"",
      address:"",
      tel:"",
      emil:"",
      order:"",
    };
  },

  methods: {
    async addData() {
      try {
        const docRef = await addDoc(collection(db, "ListShop"), {
          first: this.fname,
          last: this.lname,
          tel: this.tel,
          address:this.address,
          email : this.email,
          order: this.order,
        });
        console.log("Document written with ID: ", docRef.id);
      } catch (e) {
        console.error("Error adding document: ", e);
      }
    },
    async readData(){
      const querySnapshot = await getDocs(collection(db, "List"));
      querySnapshot.forEach((doc) => {
        console.log(`${doc.id} => ${doc.data()}`);
        this.csDoc.push({ id:doc.id, data: doc.data () });
      });
    }
  },
};
</script>

<style>
@media (min-width: 1024px) {
  .about {
    min-height: 100vh;
    display: flex;
    align-items: center;
  }
}
</style> 