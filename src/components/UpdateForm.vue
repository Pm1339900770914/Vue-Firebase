<script setup>
import { ref, computed, onMounted } from 'vue'

const isloading = ref(false)
const firstName = ref('')
const lastName = ref('')
const email = ref('')

const fullName = computed(() => {
  return `${firstName.value} ${lastName.value}`
})

onMounted(() => {
  firstName.value = 'John'
  lastName.value = 'Doe'
  email.value = 'john.doe@example.com'
})

</script>

<template>
  <div class="card">
    <h2 class="title">Update Profile</h2>

    <div class ="profile-info">
        <div><span class="showinfo">FullName:</span> {{fullName}}</div>
        <div><span class="showinfo">Email:</span> {{email}} </div>
    </div>
    
    <div class="form-group">
      <label>First Name</label>
      <input type="text" placeholder="Enter your first name" v-model="firstName"/>
    </div>

    <div class="form-group">
      <label>Last Name</label>
      <input type="text" placeholder="Enter your last name" v-model="lastName"/>
    </div>

    <div class="form-group">
      <label>Email Address</label>
      <input type="email" placeholder="example@mail.com" v-model="email"/>
    </div>

    <div class="status-loading" v-if="isloading">
      <span class="spinner"></span> 
      Updating...
    </div>

    <button class="btn-update">Update Profile</button>
  </div>
</template>

<style scoped>
/* 1. จัดกึ่งกลางหน้าจอแบบสวยๆ */
.card {
  font-family: 'Inter', sans-serif; /* แนะนำ font แนวสะอาดๆ */
  max-width: 400px;
  margin: 40px auto;
  padding: 30px;
  background-color: #ffffff;
  border-radius: 12px;
  box-shadow: 0 10px 25px rgba(0, 0, 0, 0.05); /* เงาบางๆ ให้ดูนูนขึ้นมา */
  border: 1px solid #f0f0f0;
}

.title {
  margin-bottom: 25px;
  color: #333;
  text-align: center;
  font-size: 1.5rem;
}

/* 2. จัดกลุ่ม Input และ Label */
.form-group {
  margin-bottom: 18px;
}

.form-group label {
  display: block;
  margin-bottom: 6px;
  font-weight: 600;
  color: #666;
  font-size: 0.9rem;
}

/* 3. แต่ง Input ให้ดูนุ่มนวล */
input {
  width: 100%;
  padding: 10px 12px;
  border: 1px solid #ddd;
  border-radius: 8px;
  font-size: 1rem;
  transition: all 0.2s ease;
  box-sizing: border-box; /* สำคัญมาก: เพื่อไม่ให้ padding ดันความกว้างล้น */
}

input:focus {
  outline: none;
  border-color: #42b983;
  box-shadow: 0 0 0 3px rgba(66, 185, 131, 0.1); /* ไฮไลท์เวลาคลิก */
}

/* 4. แต่งข้อความ Loading */
.status-loading {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
  font-size: 0.85rem;
  color: #999;
  margin: 15px 0;
}

/* 5. ปุ่มที่มีมิติ */
.btn-update {
  width: 100%;
  padding: 12px;
  background-color: #42b983;
  color: white;
  border: none;
  border-radius: 8px;
  font-weight: bold;
  font-size: 1rem;
  cursor: pointer;
  transition: background-color 0.2s, transform 0.1s;
}

.btn-update:hover {
  background-color: #3aa876;
}

.btn-update:active {
  transform: scale(0.98); /* กดแล้วยุบนิดนึง */
}

.profile-info {
  background-color: #f9f9f9;
  padding: 15px;
  border-radius: 8px;
  margin-bottom: 20px;
  color: #555;
  font-size: 0.9rem;
}

.showinfo {
  font-weight: 600;
  color: #333;
}
</style>