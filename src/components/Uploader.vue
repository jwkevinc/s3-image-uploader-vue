<template>
  <div class="home pa-2">
    <v-img v-if="imgURL" :src="imgURL" contain aspect-ratio="2" class="mb-3" />
    <v-file-input
      accept="image/*"
      v-model="file"
      label="File input"
      :loading="isLoading"
      :error="hasError"
      @change="onChange"></v-file-input>
    <v-btn @click.prevent="upload" class="ml-2 mt-3"> Upload </v-btn>
  </div>
</template>

<script>
import axios from 'axios';
export default {
  name: 'Home',
  components: {
  },

  data: () => ({
    file: null,
    isLoading: false,
    hasError: false,
    imgURL: null,
  }),

  computed: {
    fileType: function() {
      return this.file.type || 'application/octet-stream';
    }
  },

  methods: {
    filePreview() {
      var reader = new FileReader();
      reader.onload = (e) => {
        this.imgURL = e.target.result;
      }
      reader.readAsDataURL(this.file);
    },

    // Get signed URL
    async getSignedURL() {
      return axios.get('http://localhost:8000/s3/signed', {
        params: {
          name: this.file.name,
          contentType: this.fileType,
        }
      }).then(r => r.data.data);
    },
  
    // Upload to signed URL
    async upload() {
      if (!this.file) {
        this.hasError = true;
        return;
      }
      this.hasError = false;
      this.isLoading = true;
      let { url, id } = await this.getSignedURL();
      try {
        await axios.put(url, this.file, {
          headers: {
            'Content-Type': this.fileType
          }
        });
        this.$emit('onSuccess', id);
        this.file = null;
      } catch (e) {
        console.log(e);
      }
      this.isLoading = false;
    },

    onChange() {
      if (this.file) {
        this.hasError = false;
        this.filePreview();
      } else {
        this.imgURL = null;
      }
    }
  }
}
</script>
