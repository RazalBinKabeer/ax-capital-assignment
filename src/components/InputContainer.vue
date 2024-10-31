<template>
  <div class="input-container">
    <label>Title</label>
    <input
      type="text"
      class="title-input"
      :value="title"
      @input="$emit('update:title', $event.target.value)"
    />

    <label>Price</label>
    <input
      type="text"
      class="price-input"
      :value="price"
      @input="$emit('update:price', $event.target.value)"
    />

    <div class="description-container">
      <div class="description-options">
        <button><i class="fa-solid fa-bold"></i></button>
        <button><i class="fa-solid fa-underline"></i></button>
        <button><i class="fa-solid fa-italic"></i></button>
        <button><i class="fa-solid fa-strikethrough"></i></button>
        <button><i class="fa-solid fa-align-left"></i></button>
        <button><i class="fa-solid fa-align-center"></i></button>
        <button><i class="fa-solid fa-align-right"></i></button>
        <button><i class="fa-solid fa-align-justify"></i></button>
      </div>
      <textarea
        name="description"
        class="description-input"
        :value="description"
        @input="$emit('update:description', $event.target.value)"
      ></textarea>
      <p>{{ description.length }}</p>
    </div>

    <!-- Amenities Selection -->
    <div class="amenities-container">
      <p>Select amenities:</p>
      <div class="amenities-buttons">
        <button
          v-for="amenity in amenities"
          :key="amenity"
          :class="{ selected: selectedAmenities.includes(amenity) }"
          @click="toggleAmenity(amenity)"
        >
          {{ amenity }}
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ["title", "price", "description", "selectedAmenities", "amenities"],
  methods: {
    toggleAmenity(amenity) {
      const newAmenities = this.selectedAmenities.includes(amenity)
        ? this.selectedAmenities.filter((item) => item !== amenity)
        : [...this.selectedAmenities, amenity];
      this.$emit("update:selectedAmenities", newAmenities);
    },
  },
};
</script>
