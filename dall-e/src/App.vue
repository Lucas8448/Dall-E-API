<template>
  <div id="app" class="container">
    <h1 class="text-center my-4">OpenAI Image Generator</h1>
    <div class="mb-3">
      <label for="prompt" class="form-label">Image Description:</label>
      <div class="input-group">
        <input type="text" id="prompt" v-model="prompt" class="form-control" />
        <button @click="generateImage" class="btn btn-primary">
          Generate Image
        </button>
      </div>
    </div>
    <div class="mb-3">
      <label for="img_size" class="form-label">Image Size:</label>
      <select id="img_size" v-model="img_size" class="form-control">
        <option v-for="size in img_sizes" :key="size" :value="size">
          {{ size }}
        </option>
      </select>
    </div>
    <div v-if="imageUrl">
      <h2>Generated Image:</h2>
      <img :src="imageUrl" class="img-fluid" />
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      prompt: "",
      imageUrl: "",
      num_images: [1, 2, 3, 4, 5, 6, 7, 8, 9, 10],
      num_image: 1,
      img_sizes: ["1024x1024", "512x512", "256x256"],
      img_size: "256x256",
    };
  },
  methods: {
    async generateImage() {
      if (this.prompt === "") {
        alert("Please enter a description for the image.");
        return;
      }

      const API_KEY = "api_key";
      const API_URL = "https://api.openai.com/v1/images/generations";

      try {
        const response = await fetch(API_URL, {
          method: "POST",
          headers: {
            "Content-Type": "application/json",
            Authorization: `Bearer ${API_KEY}`,
          },
          body: JSON.stringify({
            prompt: this.prompt,
            n: 1,
            size: this.img_size,
          }),
        });

        if (response.ok) {
          const data = await response.json();
          if (data && data.data.length > 0) {
            this.imageUrl = data.data[0].url;
          } else {
            alert("Failed to generate the image. Please try again.");
          }
        } else {
          alert("Error generating image. Please check the console for details.");
        }
      } catch (error) {
        console.log("Error generating image:", error);
        alert("Error generating image. Please check the console for details.");
      }
    },
  },
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>