<template>
  <div class="product-listing">
    <div class="heading">
      <h3>Marketing</h3>
    </div>

    <div class="main-content">
      <div class="input-container">
        <label>Title</label>
        <input type="text" class="title-input" v-model="title" />

        <label>Price</label>
        <input
          type="text"
          class="price-input"
          v-model="price"
          @input="updatePrice($event.target.value)"
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
            v-model="description"
          ></textarea>
          <p>{{ description.length }}</p>
        </div>

        <div class="amenities-container">
          <p class="amenities-title">Select amenities:</p>
          <p class="amenities-note">
            Note: You can only select upto 9 amenities for the selected template
          </p>

          <div class="amenities-buttons">
            <button
              v-for="amenity in amenities"
              :key="amenity"
              :class="{ selected: isAmenitySelected(amenity) }"
              @click="toggleAmenity(amenity)"
            >
              {{ amenity }}
            </button>
          </div>
        </div>
      </div>
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
            <div
              v-for="(image, index) in images"
              :key="index"
              class="image-item"
            >
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
    </div>

    <div class="download-options">
      <h3>Download as:</h3>
      <div class="options">
        <div class="pdf-png-options">
          <label
            ><input
              type="radio"
              name="file-type"
              id="pdf"
              value="pdf"
              v-model="selectedFileType"
            /><i class="fa-solid fa-file-pdf"></i> PDF</label
          >
          <label
            ><input
              type="radio"
              name="file-type"
              id="png"
              value="png"
              v-model="selectedFileType"
            /><i class="fa-solid fa-file-image"></i> PNG</label
          >
        </div>
        <div class="agent-details-options">
          <label
            ><input
              value="with"
              type="radio"
              name="agent"
              id="with"
              v-model="selectedAgent"
            />With Agent</label
          >
          <label
            ><input
              value="without"
              type="radio"
              name="agent"
              id="without"
              v-model="selectedAgent"
            />Without Agent</label
          >
          <label
            ><input
              value="mine"
              type="radio"
              name="agent"
              id="without"
              v-model="selectedAgent"
            />Mine</label
          >
        </div>
      </div>
    </div>

    <div class="buttons">
      <button class="reset-btn btn">Reset</button>
      <button class="download-btn btn" @click="downloadFile">Download</button>
    </div>
  </div>
</template>

<script>
import html2canvas from "html2canvas";
import jsPDF from "jspdf";
export default {
  data() {
    return {
      title: "",
      price: "",
      description: "",
      selectedAmenities: [],
      selectedAgent: "without",
      amenities: [
        "Built-In Kitchen Appliances",
        "Maid's Room",
        "Private Jacuzzi",
        "Study",
        "View of Water",
      ],
      images: [
        "https://placehold.co/600x400",
        "https://placehold.co/600x400",
        "https://placehold.co/600x400",
        "https://placehold.co/600x400",
        "https://placehold.co/600x400",
        "https://placehold.co/600x400",
      ],
      currentImageIndex: null,
      selectedFileType: "pdf",
    };
  },

  computed: {
    formattedPrice() {
      return this.price ? parseFloat(this.price).toLocaleString() : "0";
    },
  },
  methods: {
    downloadFile() {
      const previewContainer = this.$refs.previewContainer;

      html2canvas(previewContainer, { scale: 2 }).then((canvas) => {
        const imgData = canvas.toDataURL("image/png");

        if (this.selectedFileType === "png") {
          const link = document.createElement("a");
          link.href = imgData;
          link.download = "preview.png";
          link.click();
        } else if (this.selectedFileType === "pdf") {
          const pdf = new jsPDF("p", "mm", "a4");
          const imgProps = pdf.getImageProperties(imgData);
          const pdfWidth = pdf.internal.pageSize.getWidth();
          const pdfHeight = (imgProps.height * pdfWidth) / imgProps.width;

          pdf.addImage(imgData, "PNG", 0, 0, pdfWidth, pdfHeight);
          pdf.save("preview.pdf");
        }
      });
    },
    resetForm() {
      this.title = "";
      this.price = "";
      this.description = "";
    },

    toggleAmenity(amenity) {
      if (this.selectedAmenities.includes(amenity)) {
        this.selectedAmenities = this.selectedAmenities.filter(
          (item) => item !== amenity
        );
      } else if (this.selectedAmenities.length < 9) {
        this.selectedAmenities.push(amenity);
      }
    },

    isAmenitySelected(amenity) {
      return this.selectedAmenities.includes(amenity);
    },

    triggerFileInput(index) {
      this.$refs.fileInputs[index].click(); // Access file input directly from array
    },
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

    updatePrice(value) {
      this.price = value.replace(/[^0-9.]/g, "");
    },
  },
};
</script>
