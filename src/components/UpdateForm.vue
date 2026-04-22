<script setup>
import { ref, computed, onMounted, watch } from 'vue'

const isloading = ref(false)
const firstName = ref('')
const lastName = ref('')
const email = ref('')
const isUpdated = ref(false)
const isValid = ref(true)
const errorMessage = ref({})

const fullName = computed(() => {
  return `${firstName.value} ${lastName.value}`
})

const updateProfile = async () => {
  isloading.value = true
  await (new Promise(resolve => setTimeout(resolve, 2000))) // จำลองการอัพเดตข้อมูล
  isloading.value = false
  isUpdated.value = true

}

const validateName = (name) => {
  const re = /\d/
  return !re.test(name)
}

const validateEmail = (email) => {
  const re = /^[^\s@]+@[^\s@]+\.[^\s@]+$/
  return re.test(email)
}

watch([firstName, lastName, email], () => {
  isValid.value = true
  isUpdated.value = false
  errorMessage.value = {}

  if(!validateName(firstName.value)) {
    isValid.value = false
    errorMessage.value.firstName = 'First name should not contain numbers.'
  }

  if(!validateName(lastName.value)) {
    isValid.value = false
    errorMessage.value.lastName = 'Last name should not contain numbers.'
  }

  if(!validateEmail(email.value)) {
    isValid.value = false
    errorMessage.value.email = 'Please enter a valid email address.'
  }
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
      <div class="error" v-if="errorMessage.firstName">{{ errorMessage.firstName }}</div>
    </div>

    <div class="form-group">
      <label>Last Name</label>
      <input type="text" placeholder="Enter your last name" v-model="lastName"/>
      <div class="error" v-if="errorMessage.lastName">{{ errorMessage.lastName }}</div>
    </div>

    <div class="form-group">
      <label>Email Address</label>
      <input type="email" placeholder="example@mail.com" v-model="email"/>
      <div class="error" v-if="errorMessage.email">{{ errorMessage.email }}</div>
    </div>

    <div class="status-loading" v-if="isloading">
      <span class="spinner"></span> 
      Updating...
    </div>

    <div class="status-success" v-if="isUpdated">
      Profile updated successfully!
    </div>
    
    <button :disabled="!isValid" @click="updateProfile" class="btn-update">Update Profile</button>
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

.status-loading .spinner {
  width: 16px;
  height: 16px;
  border: 2px solid #ddd;
  border-top-color: #42b983;
  border-radius: 50%;
  animation: spin 1s linear infinite;
}

.status-success {
  text-align: center;
  color: #42b983;
  font-weight: 600;
  margin: 15px 0;
}
/* 5. ปุ่มที่มีมิติ */
.btn-update {
  width: 100%;
  padding: 12px;
  background-color: #42b983;
  color: #fff;
  border: none;
  border-radius: 5px;
}

.btn-update:disabled {
  background-color: #bbb1b1;   /* สีเทา */
  cursor: not-allowed;      /* เปลี่ยน cursor */
  opacity: 0.7;             /* จางลงนิดนึง */
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

.error {
  color: #e74c3c;
  font-size: 0.85rem;
  margin-top: 5px;
}
</style>