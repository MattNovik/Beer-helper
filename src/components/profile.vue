<template lang="pug">
.loading(v-if="loading") Loading...
.profile
  span.preloader
      span.preloader__center
  button.profile__menu(v-on:click="open")
    img(:src='`${avatar}`' width="400" height="400")
  .profile__image
    img(:src='`${avatar}`' width="400" height="400")
  .profile__info
    p.profile__name {{ name }}
    p.profile__email Email:
     a(:href='`${emailTo}`') {{ email }}
    p.profile__number Phone:
      a(:href='`${phoneTo}`') {{ phoneNumber }}
    p.profile__date-of-birth Date of Birth: 
      p {{ dateBirth }}
    .profile__add-info
      p.profile__gender {{ gender }}
      p.profile__social-number {{ socialInsurance }}
      .profile__work
        p.progile__work-title {{ employmentTitle }}
        p.profile__work-skill {{ employmentSkill }}
      .profile__address
        p.profile__address-city {{ addressCity }}
        p.profile__address-country {{ addressCountry }}
        p.profile__address-state {{ addressState }}
        p.profile__address-street-adress {{ addressStreetAdress }}
        p.profile__address-street-name {{ addressStreetName }}
        p.profile__address-zip-code {{ addressZipCode }}
      p.profile__credit-card {{ creditCard  }}
      .profile__subcription
          p.profile__subcription-plan {{ subscriptionPlan }}
          p.profile__subcription-payment {{ subscriptionPayment }}
          p.profile__subcription-status {{ subscriptionStatus }}
          p.profile__subcription-term {{subscriptionTerm }}
</template>

<script>
import axios from 'axios'

//let userData = {};

/* const getData = () => {
  fetch('https://random-data-api.com/api/users/random_user')
    .then((response) => response.json())
    .then((user) => {
      for (let ket in user) {
        userData[ket] = user[ket];
      }
    });
}; */

export default {
  name: 'Profile',
  data () {
    return {  
      firstName: null,
      lastName: null,
      dateBirthBefore: null,
      email: null,
      gender: null,
      creditCard: null,
      addressCity: null,
      addressCountry: null,
      addressState: null,
      addressStreetAdress: null,
      addressStreetName: null,
      addressZipCode: null,
      employmentSkill: null,
      employmentTitle: null,
      phoneNumber: null,
      socialInsurance: null,
      subscriptionPlan: null,
      subscriptionPayment: null,
      subscriptionStatus: null,
      subscriptionTerm: null,
      userName: null,
      errored: false,
      avatar: null
    }
  },
  computed: {
    name: function() {
    return this.firstName + " " + this.lastName;
    },
    dateBirth: function() {
      let newDate = String(this.dateBirthBefore).split('-').reverse();
      const months = ["jan", "feb", "mar", "apr", "may", "jun", "jul", "aug", "sep", "oct", "nov", "dec"];
      return newDate[0] + ' ' + months[Number(newDate[1]) - 1] + ' ' + newDate[2];
    },
    emailTo: function() {
      return "mailto:" + this.email;
    },
    phoneTo: function() {
      return "tel:" + String(this.phoneNumber).replace(/\D/g, '');
    },
  },
  methods: {
    open: function(evt) {
      evt.preventDefault();
      const profile = document.querySelector('.profile');
      profile.classList.toggle('open');
    }
  },
  mounted() {
    axios
      .get('https://random-data-api.com/api/users/random_user')
      .then(response => (
        this.firstName= response.data.first_name,
        this.lastName= response.data.last_name,
        this.email= response.data.email,
        this.dateBirthBefore = response.data.date_of_birth,
        this.gender = response.data.gender,
        this.addressCity = response.data.address.city,
        this.addressCountry = response.data.address.country,
        this.addressState = response.data.address.state,
        this.addressStreetAdress = response.data.address.street_address,
        this.addressStreetName = response.data.address.street_name,
        this.addressZipCode = response.data.address.zip_code,
        this.employmentSkill = response.data.employment.key_skill,
        this.employmentTitle = response.data.employment.title,
        this.phoneNumber = response.data.phone_number,
        this.socialInsurance = response.data.social_insurance_number,
        this.subscriptionPlan = response.data.subscription.plan,
        this.subscriptionPayment = response.data.subscription.payment_method,
        this.subscriptionStatus = response.data.subscription.status,
        this.subscriptionTerm = response.data.subscription.term,
        this.userName = response.data.username,
        this.avatar = response.data.avatar,
        this.creditCard = response.data.credit_card.cc_number
        ))
      .catch(error => {
        console.log(error);
        this.errored = true;
      })
      .finally(() => {
        const profile = document.querySelector('.profile');
        profile.classList.add('loaded');
        });
  }
}
</script>
