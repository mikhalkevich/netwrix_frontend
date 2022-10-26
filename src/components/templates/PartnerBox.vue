<template>
  <div class="row justify-content-center box">
    <div class="col-12">
      <div class="container">
        <div class="row">
          <div class="col-md-2">
            <img src="/img/standard-logo.png" />
          </div>
          <div class="col-md-6">
            <h2>{{partner.company}}</h2>
            <div class="under_name">{{partner.address}}</div>
          </div>
          <div class="col-md-2">
            <div><a :href="partner.website" target="_blank">website</a></div>
            <div>{{partner.phone}}</div>
          </div>
          <div class="col-md-2">
            {{partner.status}}
          </div>
        </div>
      </div>

    </div>
  </div>
</template>

<script>
export default {
  name: "PartnerBox",
  props: ['partner']
}
</script>

<style scoped>

</style>