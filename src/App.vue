<script setup>
import { ref, computed } from "vue";

// ตัวแปร
const shopList = ref([
  { name: "Keep in touch", table_Num: 0, img: "https://scontent.fbkk29-1.fna.fbcdn.net/v/t39.30808-6/331972356_533268055539465_3792130442034699663_n.jpg?_nc_cat=100&ccb=1-7&_nc_sid=52f669&_nc_eui2=AeERPp5kHSZPylEKtNK7ZTw4TrN_xP5EkdBOs3_E_kSR0PyDbpVlkxGE7mjENc6IEDAGWNjTzaghNgI_tiOqSkYQ&_nc_ohc=I0yKECnJsoYAX-IqkoA&_nc_ht=scontent.fbkk29-1.fna&oh=00_AfC9PNaIMYfA5rmY3TKseOgONch55jwjGyiSJxNe3h-iyQ&oe=65095DAE" },
  { name: "Forest Bake", table_Num: 0, img: "https://www.bkkmenu.com/files/2018/03/ForestBake-7.jpg" },
  { name: "Dong Madame", table_Num: 0, img: "https://www.topchiangmai.com/wp-content/uploads/2019/06/Dong-Madame-%E0%B8%94%E0%B8%87%E0%B8%A1%E0%B8%B2%E0%B8%94%E0%B8%B2%E0%B8%A1-1.jpg" },
  { name: "Cheevit Cheeva", table_Num: 0, img: "https://www.bkkmenu.com/files/2018/09/CheevitCheevaTheCircle-1-1005x670.jpg" },
  { name: "Wabi Cha", table_Num: 0, img: "https://scontent.fbkk29-7.fna.fbcdn.net/v/t1.6435-9/49658202_1049050941943607_4141408698905919488_n.jpg?_nc_cat=106&ccb=1-7&_nc_sid=8bfeb9&_nc_eui2=AeHc4bgs37qY42gHLcrEhoAMTfliGmmR_hBN-WIaaZH-EBkbaXhFFLYfQpsjkbbjGhq28AGfukjamVdRpXoOSQe1&_nc_ohc=M2tCoe-Rli4AX9bmp5N&_nc_ht=scontent.fbkk29-7.fna&oh=00_AfADOLucQxlHxLTMXXROjKtmAFJ58q0IpLqRadoaTcD3yg&oe=652BF608" },
  { name: "Mao Coffee", table_Num: 0, img: "https://travelalotthailand.files.wordpress.com/2017/11/cover9.jpg" },
]);

const bookingList = ref([]);
const customerName = ref('');
const telNum = ref('');
const table_nums = ref(new Array(shopList.value.length).fill(0));

function booking(name, tableNum) {
  if (!isNaN(tableNum) && tableNum >= 1 && tableNum <= 10) {
    if (customerName.value.trim() === '' || telNum.value.trim() === '') {
      alert("กรุณากรอกชื่อลูกค้าและเบอร์โทรศัพท์");
    } else {
      const restaurant = shopList.value.find(item => item.name === name);
      if (restaurant) {
        restaurant.table_Num = tableNum;
        const timestamp = new Date().toLocaleString();
        bookingList.value.push({
          name: restaurant.name,
          table_Num: tableNum,
          customerName: customerName.value,
          telNum: telNum.value,
          timestamp: timestamp,
        });
        customerName.value = '';
        telNum.value = '';
      }
    }
  } else {
    alert("กรุณาใส่จำนวนโต๊ะที่ต้องการจอง (ไม่เกิน 10)");
  }
}

const isInputValid = computed(() => {
  return customerName.value.trim() !== '' && telNum.value.trim() !== '';
});
</script>

<template>
  <div class="p-3 m-0 border-0 bd-example m-0 border-0">
    <div class="text-center">
      <div class="row row-cols-1 row-cols-md-3 g-4">
        <div class="col" v-for="(restaurant, index) in shopList" :key="index">
          <div class="card bg-dark text-white" style="width: 18rem;">
            <img :src="restaurant.img" class="card-img-top fixed-size-image" alt="..." />
            <div class="card-body">
              <h5 class="card-title">{{ restaurant.name }}</h5>
              <p class="card-text">
                จำนวนจองโต๊ะ(สูงสุด 10)
                <input type="number" v-model="table_nums[index]" min="1" max="10" />
              </p>
              <a href="#" class="btn btn-primary" @click="booking(restaurant.name, table_nums[index])">จองโต๊ะ</a>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
  <div class="container mt-4">
    <div class="row">
      <div class="col-md-9">
        <div class="input-group" id="InputGroup">
          <span class="input-group-text">ชื่อลูกค้า และ เบอร์โทรศัพท์</span>
          <input type="text" class="form-control" id="customerName" v-model="customerName" />
          <input type="text" class="form-control" id="telNum" v-model="telNum" maxlength="10" />
        </div>
      </div>
      <div class="col-md-2">
        <button class="btn btn-primary" @click="booking(restaurant.name, table_nums[index])">ยืนยัน</button>
      </div>
    </div>
  </div>
  <br>
  <table class="table table-dark table-hover" v-if="bookingList.length > 0">
    <thead>
      <tr>
        <th scope="col">#</th>
        <th scope="col">ชื่อผู้จอง</th>
        <th scope="col">เบอร์โทรศัพท์</th>
        <th scope="col">รายชื่อร้านที่จอง</th>
        <th scope="col">จำนวนโต๊ะที่จอง</th>
        <th scope="col">วันที่และเวลาที่จอง</th> 
      </tr>
    </thead>
    <tbody>
      <tr v-for="(booking, index) in bookingList" :key="index">
        <th scope="row">{{ index + 1 }}</th>
        <td>{{ booking.customerName }}</td>
        <td>{{ booking.telNum }}</td>
        <td>{{ booking.name }}</td>
        <td>{{ booking.table_Num }}</td>
        <td>{{ booking.timestamp }}</td>
      </tr>
    </tbody>
  </table>
  <button type="button" class="btn btn-primary">ตกลง</button>
</template>