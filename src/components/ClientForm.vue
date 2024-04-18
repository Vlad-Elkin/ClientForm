<template>
  <div class="client-form">
    <h1>Форма создания клиента</h1>
    <form @submit.prevent="submitForm">
      <label for="lastName">Фамилия *</label>
      <input type="text" id="lastName" :class="{'error-field':$v.client.lastName.$error}"
      v-model="client.lastName" @blur="$v.client.lastName.$touch()">
      <span class="error-message" v-if="$v.client.lastName.$error">Фамилия обязательна для заполнения</span>

      <label for="firstName">Имя *</label>
      <input type="text" id="firstName" :class="{'error-field':$v.client.firstName.$error}"
      v-model="client.firstName" @blur="$v.client.firstName.$touch()">
      <span class="error-message" v-if="$v.client.firstName.$error">Имя обязательно для заполнения</span>

      <label for="middleName">Отчество</label>
      <input type="text" id="middleName" v-model="client.middleName">

      <label for="birthDate">Дата рождения *</label>
      <input type="date" id="birthDate" :class="{'error-field':$v.client.birthDate.$error}"
      v-model="client.birthDate" @blur="$v.client.birthDate.$touch()">
      <span class="error-message" v-if="$v.client.birthDate.$error">Дата рождения обязательна для заполнения</span>

      <label for="phone">Номер телефона *</label>
      <input type="tel" id="phone" :class="{'error-field':$v.client.phone.$error}"
      v-model="client.phone" placeholder="Начинается с 7" @blur="$v.client.phone.$touch()">
      <span class="error-message" v-if="$v.client.phone.$error">Номер телефона обязателен для заполнения и должен содержать 11 цифр</span>

      <label for="gender">Пол</label>
      <select id="gender" v-model="client.gender">
        <option value="male">Мужской</option>
        <option value="female">Женский</option>
      </select>

      <label for="clientGroup">Группа клиентов *</label>
      <select id="clientGroup" :class="{'error-field':$v.client.clientGroup.$error}"
      v-model="client.clientGroup" multiple @blur="$v.client.clientGroup.$touch()">
        <option value="VIP">VIP</option>
        <option value="Проблемные">Проблемные</option>
        <option value="ОМС">ОМС</option>
      </select>
      <span class="error-message" v-if="$v.client.clientGroup.$error">Выберите хотя бы одну группу клиентов</span>

      <label for="doctor">Лечащий врач</label>
      <select id="doctor" v-model="client.doctor">
        <option value="Иванов">Иванов</option>
        <option value="Захаров">Захаров</option>
        <option value="Чернышева">Чернышева</option>
      </select>

      <label for="noSMS">
        <input type="checkbox" v-model="client.noSMS"> Не отправлять СМС
      </label>

      <h2>Адрес</h2>
      <label for="zip">Индекс</label>
      <input type="text" id="zip" v-model="client.address.zip">

      <label for="country">Страна</label>
      <input type="text" id="country" v-model="client.address.country">

      <label for="region">Область</label>
      <input type="text" id="region" v-model="client.address.region">

      <label for="city">Город *</label>
      <input type="text" id="city" :class="{'error-field':$v.client.address.city.$error}"
      v-model="client.address.city" @blur="$v.client.address.city.$touch()">
      <span class="error-message" v-if="$v.client.address.city.$error">Город обязателен для заполнения</span>

      <label for="street">Улица</label>
      <input type="text" id="street" v-model="client.address.street">

      <label for="house">Дом</label>
      <input type="text" id="house" v-model="client.address.house">

      <h2>Паспорт</h2>
      <label for="documentType">Тип документа *</label>
      <select id="documentType" :class="{'error-field':$v.client.passport.type.$error}"
      v-model="client.passport.type" @blur="$v.client.passport.type.$touch()">
        <option value="Паспорт">Паспорт</option>
        <option value="Свидетельство о рождении">Свидетельство о рождении</option>
        <option value="Водительское удостоверение">Водительское удостоверение</option>
      </select>
      <span class="error-message" v-if="$v.client.passport.type.$error">Выберите тип документа</span>

      <label for="series">Серия</label>
      <input type="text" id="series" v-model="client.passport.series">

      <label for="number">Номер</label>
      <input type="text" id="number" v-model="client.passport.number">

      <label for="issuedBy">Кем выдан</label>
      <input type="text" id="issuedBy" v-model="client.passport.issuedBy">

      <label for="issueDate">Дата выдачи *</label>
      <input type="date" id="issueDate" :class="{'error-field':$v.client.passport.issueDate.$error}"
      v-model="client.passport.issueDate" @blur="$v.client.passport.issueDate.$touch()">
      <span class="error-message" v-if="$v.client.passport.issueDate.$error">Дата выдачи обязательна для заполнения</span>

      <button type="submit" class="submit-btn">Создать клиента</button>
      <p v-if="successMessage" class="success-message">{{ successMessage }}</p>
    </form>
  </div>
</template>

<script>
import { maxValue } from 'vuelidate/lib/validators'
import { minValue } from 'vuelidate/lib/validators'
import { required, minLength, maxLength } from 'vuelidate/lib/validators'

export default {
  data() {
    return {
      client: {
        lastName: '',
        firstName: '',
        middleName: '',
        birthDate: '',
        phone: '',
        gender: '',
        clientGroup: [],
        doctor: '',
        noSMS: false,
        address: {
          zip: '',
          country: '',
          region: '',
          city: '',
          street: '',
          house: ''
        },
        passport: {
          type: '',
          series: '',
          number: '',
          issuedBy: '',
          issueDate: ''
        }
      },
      successMessage: ''
    }
  },
  validations: {
    client: {
      lastName: { required },
      firstName: { required },
      birthDate: { required },
      phone: { required, 
        minLength: minLength(11), maxLength: maxLength(11),
        minValue: minValue(70000000000),maxValue:maxValue(79999999999)},
      clientGroup: { required },
      address:{
        city: { required }
      },
      passport:{
        type: { required },
        issueDate: { required }
      }
    }
  },
  methods: {
    submitForm() {
      this.$v.$touch()
      if (!this.$v.$invalid) {
        /// Реализовать здесь отправку данных в базу данных на серверной части
        this.successMessage = 'Новый клиент успешно создан!'
      }
    }
  }
}
</script>

<style lang="scss">
.client-form {
  max-width: 600px;
  margin: 0 auto;
  padding: 20px;
  background-color: #f8f9fa;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);

  h1 {
    font-size: 24px;
    margin-bottom: 20px;
    text-align: center;
    color: #333;
  }

  form {
    label {
      display: block;
      margin: 10px;
      font-size: 16px;
      color: #555;
    }

    input[type='text'],
    input[type='tel'],
    input[type='date'],
    select,
    textarea {
      width: 100%;
      padding: 10px;
      margin-bottom: 18px;
      border: 1px solid #ccc;
      border-radius: 5px;
      font-size: 16px;
      transition: border-color 0.3s ease;
      box-sizing: border-box;
      
      &:focus {
        border-color: #007bff;
        outline: none;
      }
      &.error-field{
        border-color: #dc3545;
        margin-bottom: 0;
      }
    }

    .error-message {
      color: #dc3545;
      font-size: 14px;
      margin-top: 5px;
    }

    .submit-btn {
      width: 100%;
      padding: 12px 0;
      background-color: #007bff;
      color: #fff;
      border: none;
      border-radius: 5px;
      font-size: 16px;
      cursor: pointer;
      transition: background-color 0.3s ease;

      &:hover {
        background-color: #0056b3;
      }
    }

    .success-message {
      color: #28a745;
      font-size: 18px;
      text-align: center;
      margin-top: 20px;
    }
  }

  @media only screen and (max-width: 768px) {
    padding: 10px;

    h1 {
      font-size: 20px;
      margin-bottom: 15px;
    }

    form {
      input[type='text'],
      input[type='tel'],
      select,
      textarea {
        font-size: 14px;
        padding: 8px;
      }
    }
  }
}
</style>
