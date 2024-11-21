<template>
    <div>
      <h2>Partners</h2>
      <div v-if="partners.length">
        <div v-for="partner in partners" :key="partner.id" class="partner-item">
          <img :src="partner?.partner?.value?.data?.image" :alt="partner?.partner?.value?.data?.name" class="partner-image" />
          <h3>{{ partner?.partner?.value?.data?.name }}</h3>
          <a :href="partner?.partner?.value?.data?.link" target="_blank">{{ partner?.partner?.value?.data?.link }}</a>
        </div>
      </div>
      <p v-else>No partners available.</p>
    </div>
  </template>
  
  <script>
  export default {
    props: {
      partners: {
        type: Array, // Expecting an array of partner objects
        required: true,
      },
    },
    mounted() {
      console.log(this.partners); // Log the partners prop to verify its structure
      this.partners.forEach(partner => {
        console.log(partner.partner?.value?.data); // Log the actual partner data (name, link, image)
      });
    },
  };
  </script>
  
  <style scoped>
  .partner-item {
    display: flex;
    align-items: center;
    margin-bottom: 20px;
  }
  
  .partner-image {
    width: 50px;
    height: 50px;
    margin-right: 15px;
    border-radius: 50%;
  }
  
  a {
    text-decoration: none;
    color: #007bff;
  }
  
  a:hover {
    text-decoration: underline;
  }
  </style>
  