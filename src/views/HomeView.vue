<template>
  <div id="main">
    <img src="/img/logo.png" id="logo"/>
    <div id="fon">
      <h1>Netwrix Partner Locator</h1>
      <div class="logotext">
        <p>Hundreds of Netwrix partners around the world are standing by to help you. </p>
        <p>With our Partner Locator you can easily find the list of authorized partners in your area.</p>
      </div>
      <div id="form">
        <div class="container text-center">
          <div class="row justify-content-center">
            <div class="col-9">
              <div class="input-group mb-3">
                <div class="search">
                  <input type="text" class="my-control" v-model="SelectedPartner"
                         placeholder="Search by company name or address" id="search"
                         name="search">
                  <button type="button" @click="GetData"><i class="fa fa-search"></i></button>
                </div>
              </div>
            </div>
          </div>
          <div class="row justify-content-center">
            <div class="col-md-3 pad">
              <FormSelect :options="types" v-model="SelectedType" :default_value="DefaultValueType"
                          class="form-select no-bg" id="type"/>
            </div>
            <div class="col-md-3 pad">
              <FormSelect :options="countries" v-model="SelectedCountry" :default_value="DefaultValueCountry"
                          class="form-select no-bg" id="country"/>
            </div>
            <div class="col-md-3 pad">
              <FormSelect :disabled="EnabledState" :options="states" v-model="SelectedState" :default_value="DefaultValueState"
                          class="form-select no-bg" :class="EnabledState?'sel_dis':'sel_en'" id="state"/>
            </div>
          </div>
        </div>
      </div>
    </div>
    <div class="partners">
      <div class="container">
        <partnerBox v-for="partner in partners" v-model="partners" :partner="partner" :key="partner.id"/>
      </div>
    </div>
  </div>
</template>

<script setup>
import axios from "axios";
import {onMounted, ref, watch} from "vue";
import partnerBox from "@/components/templates/PartnerBox";

const countries = ref([]);
const states = ref([]);
const types = ref([]);
const partners = ref([]);
const SelectedPartner = ref('');
const SelectedCountry = ref('');
const SelectedType = ref('');
const SelectedState = ref('');
const DefaultValueCountry = ref('Country');
const DefaultValueState = ref('State');
const DefaultValueType = ref('Type');
const EnabledState = ref(true);
//const states = ref([]);
//const partners = ref([]);
const GetData = async () => {
  const response = await axios.get('partner/all?company=' + SelectedPartner.value);
  partners.value = response.data;
  console.log(partners.value);
};
watch(SelectedType, async (newValue) => {
  const response_partner = await axios.get('partner/all?type=' + newValue);
  partners.value = response_partner.data;
});
watch(SelectedCountry, async (newValue) => {
  const response_partner = await axios.get('partner/all?country=' + newValue);
  partners.value = response_partner.data;
  const response_state = await axios.get('states/' + newValue);
  states.value = response_state.data.data;
  if(states.value.length > 0){
    EnabledState.value = false;
  }
})
watch(SelectedState, async (newValue) => {
  const response_partner = await axios.get('partner/all?state=' + newValue);
  partners.value = response_partner.data;
});
onMounted(async () => {
  const response = await axios.get('countries');
  countries.value = response.data.data;
  const response_type = await axios.get('types');
  types.value = response_type.data;
  const response_partner = await axios.get('partner/all');
  partners.value = response_partner.data;
});

</script>