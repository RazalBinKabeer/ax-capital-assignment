<template>
  <div class="preview-container" ref="previewContainer">
    <div class="location-price">
      <p class="location">
        Empire Height 2, Empire Heights, Business Bay, Dubai, UAE
      </p>
      <p class="price">{{ price ? `AED ${formattedPrice}` : "AED 0" }}</p>
    </div>

    <input
      type="file"
      ref="imageInput"
      @change="selectImage"
      style="display: none"
      accept="image/*"
    />

    <div class="image-gallery">
      <div class="image-grid">
        <div v-for="(image, index) in images" :key="index" class="image-item">
          <input
            type="file"
            accept="image/*"
            @change="onImageUpload($event, index)"
            ref="fileInputs"
            class="file-input"
          />
          <img
            :src="image"
            :alt="'Image ' + (index + 1)"
            class="image"
            @click="triggerFileInput(index)"
          />
        </div>
      </div>
    </div>

    <div class="preview-description-container">
      <div class="description">
        <h4>{{ title }}</h4>
        <p>
          {{ description }}
        </p>
      </div>

      <div class="preview-amenities">
        <ul>
          <li v-for="amenity in selectedAmenities" :key="amenity">
            {{ amenity }}
          </li>
        </ul>
      </div>
    </div>

    <div class="agent-details-container">
      <div class="agent-detail" v-if="selectedAgent === 'with'">
        <img src="./assets/images/profile.jpg" alt="profile-picture" />
        <div class="agent-info">
          <h6>Mohamed Razal</h6>
          <p>
            <i class="fa-regular fa-envelope"></i>
            razalkabeer@hotmail.com
          </p>
          <p><i class="fa-solid fa-phone"></i> +971 55 873 6619</p>
        </div>
      </div>
      <div class="qr-code">
        <img src="./assets/images/qr-code.png" alt="qr-code" />
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: [
    "title",
    "price",
    "description",
    "selectedAmenities",
    "images",
    "formattedPrice",
    "selectedAgent",
  ],
  methods: {
    onImageUpload(event, index) {
      const file = event.target.files[0];
      if (file) {
        const reader = new FileReader();
        reader.onload = (e) => {
          // Update the image directly
          this.images[index] = e.target.result; // Update the image directly
          console.log(`Image ${index + 1} updated!`); // Debugging line
        };
        reader.readAsDataURL(file);
      } else {
        console.error("No file selected for image:", index);
      }
    },
    triggerFileInput(index) {
      this.$refs.fileInputs[index].click(); // Access file input directly from array
    },
  },
};
</script>
