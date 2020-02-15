<template>
  <div class="home">
    <Uploader @onSuccess="handleOnSuccess"/>
  </div>
</template>

<script>
import Uploader from '@/components/Uploader.vue';
export default {
  name: 'Home',
  components: {
    Uploader
  },
  data: () => ({
    file: null,
    images: [],
  }),
  methods: {
    handleOnSuccess(id) {
      console.log('successful upload: ', id);
    }
  }
}
</script>
