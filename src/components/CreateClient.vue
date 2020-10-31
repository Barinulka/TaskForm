<template>
    <div class="container">
        <header>
            <h1 class="header-title">Форма создания Клиента</h1>
        </header>
        <main>
            <form v-if="!createClientPassed" class="create" @submit.prevent="checkForm">
                <h2 class="form-header" >Информация о клиенте</h2>
                <section>
                    <div class="field">
                        <label class="field-name" for="lastname">Фамилия*</label>
                        <input 
                            id="lastname" 
                            type="text" 
                            class="form-control"
                            v-model.trim="form.lastname" 
                        >
                        <p v-if="$v.form.lastname.$dirty && !$v.form.lastname.required" class="invalid-feedback">
                            Поле обязательно для заполнения
                        </p>
                        <p v-else-if="$v.form.lastname.$dirty && !$v.form.lastname.alpha" class="invalid-feedback">
                            Только буквы кириллицы
                        </p>
                    </div>
                    <div class="field">
                        <label class="field-name" for="firstname">Имя*</label>
                        <input 
                            id="firstname" 
                            type="text" 
                            class="form-control"
                            v-model.trim="form.firstname" 
                        >
                        <p v-if="$v.form.firstname.$dirty && !$v.form.firstname.required" class="invalid-feedback">
                            Поле обязательно для заполнения
                        </p>
                        <p v-else-if="$v.form.firstname.$dirty && !$v.form.firstname.alpha" class="invalid-feedback">
                            Только буквы кириллицы
                        </p>
                    </div>
                    <div class="field">
                        <label class="field-name" for="patronymic">Отчество</label>
                        <input 
                            id="patronymic" 
                            type="text" 
                            class="form-control"
                            v-model.trim="form.patronymic"
                        >
                    </div>
                    <div class="field">
                        <label class="field-name" for="birthday">Дата рождения*</label>
                        <input 
                            id="birthday" 
                            type="date" 
                            class="form-control"
                            v-model="form.birthday"
                        >
                        <p v-if="$v.form.birthday.$dirty && !$v.form.birthday.required" class="invalid-feedback">
                            Поле обязательно для заполнения
                        </p>
                    </div>
                    <div class="field">
                        <label class="field-name" for="phone">Телефон*</label>
                        <input 
                            id="phone" 
                            type="tel" 
                            class="form-control"
                            v-model.trim="form.phone" 
                            placeholder="+79999999999"
                        >
                        <p v-if="$v.form.phone.$dirty && !$v.form.phone.required" class="invalid-feedback">
                            Поле обязательно для заполнения
                        </p>
                        <p v-else-if="$v.form.phone.$dirty && !$v.form.phone.validPhone" class="invalid-feedback">
                            Формат +79991234567
                        </p>
                        <!-- не работает 
                        <p v-else-if="$v.form.phone.$dirty && !$v.form.phone.maxLength" class="invalid-feedback"> 
                            Не более {{ $v.phone.$params.maxLength.max }} символов  
                        </p>
                        -->
                    </div>
                    <div class="field">
                        <label class="field-name" for="doctor">Лечащий врач* </label>
                        <select 
                            id="doctor"
                            class="form-control"
                            v-model="form.doc"
                        >
                            <option 
                                class="options"
                                v-for="(doctor, index) in doctors" 
                                :key="index" 
                                :value="doctor.value"
                            >
                                {{ doctor.lastname }}
                            </option>
                        </select>
                        <p v-if="$v.form.doc.$dirty && !$v.form.doc.required" class="invalid-feedback">
                            Веберите врача!
                        </p>
                    </div>
                    <div class="field">
                        <label class="field-name">Пол </label>
                        <div class="field-radio">
                            <input type="radio" id="male" value="male" v-model="form.gendere">
                            <label for="male" class="radio">Мужской</label>
                            <input type="radio" id="female" value="female" v-model="form.gendere">
                            <label for="female" class="radio">Женской</label>
                        </div>
                    </div>
                     <div class="field">
                        <label class="field-name" for="clientGroup">Группа клиентов </label>
                        <select 
                            id="clientGroup" multiple
                            class="form-multiple"
                            v-model="form.groups"
                        >
                            <option 
                                v-for="(group, index) in clientGroups" 
                                :key="index" 
                                :value="group.value"
                            >
                                {{ group.name }}
                            </option>
                        </select>
                    </div>
                    <div class="field">
                        <div class="field-checkbox">
                            <input class="checkbox" type="checkbox" id="sendSms" form.="form.disableSendSms">
                            <label for="sendSms">Не отправлять СМС</label>
                        </div>
                    </div>
                </section>
                <h2 class="form-header">Место проживания</h2>
                <section>
                    <div class="field">
                        <label class="field-name" for="cityIndex">Индекс</label>
                        <input 
                            id="cityIndex" 
                            type="number" 
                            class="form-control"
                            v-model.trim="form.cityIndex" 
                        >
                    </div>
                    <div class="field">
                        <label class="field-name" for="country">Страна</label>
                        <input 
                            id="country" 
                            type="text" 
                            class="form-control"
                            v-model.trim="form.country" 
                        >
                    </div>
                    <div class="field">
                        <label class="field-name" for="region">Область</label>
                        <input 
                            id="region" 
                            type="text" 
                            class="form-control"
                            v-model.trim="form.region" 
                        >
                    </div>
                    <div class="field">
                        <label class="field-name" for="city">Город*</label>
                        <input 
                            id="city" 
                            type="text" 
                            class="form-control"
                            v-model.trim="form.city" 
                        >
                        <p v-if="$v.form.city.$dirty && !$v.form.city.required" class="invalid-feedback">
                            Поле обязателно для заполнения
                        </p>
                        <p v-else-if="$v.form.city.$dirty && !$v.form.city.alpha" class="invalid-feedback">
                            Только буквы кириллицы
                        </p>
                    </div>
                    <div class="field">
                        <label class="field-name" for="street">Улица</label>
                        <input 
                            id="street" 
                            type="text" 
                            class="form-control"
                            v-model.trim="form.street" 
                        >
                    </div>
                    <div class="field">
                        <label class="field-name" for="houseNumber">Дом</label>
                        <input 
                            id="houseNumber" 
                            type="number" 
                            class="form-control"
                            v-model.trim="form.houseNumber" 
                        >
                    </div>
                </section>
                <h2 class="form-header">Паспортные данные</h2>
                <section>
                    <div class="field">
                        <label class="field-name" for="documents">Тип документа* </label>
                        <select 
                            id="documents"
                            class="form-control"
                            v-model="form.documentsType"
                        >
                            <option 
                                v-for="(document, index) in documents" 
                                :key="index" 
                                :value="document.value"
                            >
                                {{ document.type }}
                            </option>
                        </select>
                        <p v-if="$v.form.documentsType.$dirty && !$v.form.documentsType.required" class="invalid-feedback">
                            Веберите тип документа
                        </p>
                    </div>
                    <div class="field">
                        <label class="field-name" for="documentsSerial">Серия</label>
                        <input 
                            id="documentsSerial" 
                            type="text" 
                            class="form-control"
                            v-model.trim="form.documentsSerial" 
                        >
                    </div>
                    <div class="field">
                        <label class="field-name" for="documentsNumber">Номер</label>
                        <input 
                            id="documentsNumber" 
                            type="number" 
                            class="form-control"
                            v-model.trim="form.documentsNumber" 
                        >
                    </div>
                    <div class="field">
                        <label class="field-name" for="documentsIssued ">Кем выдан</label>
                        <input 
                            id="documentsIssued" 
                            type="text" 
                            class="form-control"
                            v-model.trim="form.documentsIssued" 
                        >
                    </div>
                    <div class="field">
                        <label class="field-name" for="documentsDataIssued ">Дата выдачи*</label>
                        <input 
                            id="documentsDataIssued" 
                            type="date" 
                            class="form-control"
                            value="Поле обязательно для записи"
                            v-model.trim="form.documentsDataIssued" 
                        >
                        <p v-if="$v.form.documentsDataIssued.$dirty && !$v.form.documentsDataIssued.required" class="invalid-feedback">
                            Поле обязательно для заполнения
                        </p>
                    </div>
                </section>
                <div class="buttom">    
                    <button type="submit" class="btn">Сохранить</button> 
                </div> 
            </form>
            
            <div v-else class="yes">
                <h1>
                    {{ `Клиент ${ form.firstname } успешно добавлен!` }}
                </h1>
                <div class="buttom">    
                    <button @click="back" class="btn">Назад</button> 
                </div> 
            </div>
            
        </main> 
    </div>
</template>

<script>
import { required, maxLength } from 'vuelidate/lib/validators';


export default {
    data() {
        return {
            createClientPassed: false,
            form: {
                lastname: null,
                firstname: null,
                patronymic: null,
                birthday: null,
                phone: null,
                disableSendSms: false,
                gendere: '',
                groups: [],
                doc: '',
                cityIndex: null,
                country: null,
                region: null,
                city: null,
                street: null,
                houseNumber: null,
                documentsType: '',
                documentsSerial: null,
                documentsNumber: null,
                documentsIssued: null,
                documentsDataIssued: null,
            },           
            clientGroups: [
                {
                    name: 'VIP',
                    value: 'vip'
                },
                {
                    name: 'Проблемные',
                    value: 'problem'
                },
                {
                    name: 'ОМС',
                    value: 'omc'
                }
            ],
            doctors: [
                {
                    lastname: 'Иванов',
                    value: 'd1'
                },
                {
                    lastname: 'Чернышева',
                    value: 'd2'
                },
                {
                    lastname: 'Захаров',
                    value: 'd3'
                },
            ],
            documents: [
                {
                    type: 'Паспорт',
                    value: 'passport'
                },
                {
                    type: 'Свидетельство о рождении',
                    value: 'birth certificate'
                },
                {
                    type: 'Вод. удостоверение',
                    value: 'driver license'
                },
            ],
        }
    }, 
    validations: {
        form: {
            lastname: {
                required,
                alpha: val => /^[а-яё]*$/i.test(val),
            },
            firstname: {
                required, 
                maxLength: maxLength(15),
                alpha: val => /^[а-яё]*$/i.test(val),
            },
            birthday: { required },
            phone: {
                required, 
                // maxLength: maxLength(12),
                validPhone: val => /^\+7\d{3}\d{7}$/.test(val),
            },
            doc: { required },
            city: {
                required,
                alpha: val => /^[а-яё]*$/i.test(val),
            },
            documentsType: { required },
            documentsDataIssued: { required }
        } 
    },
    methods: {
        checkForm() {
            this.$v.form.$touch()
            if (!this.$v.form.$error){
                this.createClientPassed = true
                // alert("Клиент успешно добавлен")
            }
        },
        back(){
            this.createClientPassed = false
            window.location.reload()
        }
    }
}
</script>

<style lang="scss">
/* mixin */
$small: 620px;
$large: 1024px;

@mixin respond-to($media) {
  @if $media == handhelds {
    @media only screen and (max-width: $small) { @content; }
  }
  @else if $media == medium-screens {
    @media only screen and (min-width: $small + 1) and (max-width: $large - 1) { @content; }
  }
  @else if $media == wide-screens {
    @media only screen and (min-width: $large) { @content; }
  }
}
@mixin font-source-sans($size: false, $colour: false, $weight: false,  $lh: false) {
  font-family: 'Source Sans Pro', Helvetica, Arial, sans-serif;
  @if $size { font-size: $size; }
  @if $colour { color: $colour; }
  @if $weight { font-weight: $weight; }
  @if $lh { line-height: $lh; }
}
@mixin auto { width: 80%; margin: 0 auto; }
@mixin flex-header {display: flex; justify-content: center; text-align: center;}

body{
    @include font-source-sans(25px, black, 300, 50px)
}
/* component */
.container{
    display: grid;
    @include auto()
}
/* header */
header{
    @include flex-header()
}
.header-title{
    color: darkblue;  
}
/* main */
/* form */
.create{
    display: grid;
    grid-gap: 15px;

    @include respond-to(handhelds){ grid-gap: 0; }
}
/* sections */
.form-header{
    @include flex-header()
}
section{
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    grid-gap: 10px;

    @include respond-to(wide-screens){ grid-template-columns: repeat(3, 1fr); }
    @include respond-to(medium-screens){ grid-template-columns: 1fr 1fr; }
    @include respond-to(handhelds){ grid-template-columns: 1fr; }
}
/* field */
.field {
    padding-bottom: 25px;
    display: flex;
    flex-direction: column;
    align-content: center;
    justify-content: center;
}
.field-name {
    align-self: center;
    padding-bottom: 10px;
    color: #2c3e50;
}
.field-radio {
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 15px;
}
.radio{
    padding-right: 15px;
}
.field-checkbox{
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 15px;
}
.form-multiple{
    width: auto;
    height: 60px;
    text-align: center;
    padding: 5px 20px;
    background: #E9EFF6;
    border-width: 0;
    
}
.form-control{
    display: flex;
    width: auto;
    height: 40px;
    text-align: center;
    padding: 0 20px;
    margin-bottom: 10px;
    background: #E9EFF6;
    line-height: 20px;
    border-width: 0;
    border-radius: 20px;
}
/* btn */
.buttom{
    display: flex;
    justify-content: center;
    padding-bottom: 20px;
}
.btn{
    width: 150px;
    padding: 0 15px;
    margin: 10px 0 15px;
    border-width: 0;
    line-height: 40px;
    border-radius: 20px;
    background: #B9EFF6;
}
/* Error */
.invalid-feedback{
    align-self: center;
    font-size: 10px;
    color: red;
    line-height: 0;
    margin: 0;
}
/* added */
.yes{
    text-align: center;
}
</style>