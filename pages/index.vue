<template>
  <div>
    <!-- <img :src="originalImagePath" alt="Original Image" /> -->
    <img
      v-if="modifiedImagePath"
      :src="modifiedImagePath"
      alt="Modified Image"
    />
  </div>
</template>

<script>
import { fabric } from "fabric";

export default {
  data() {
    return {
      originalImagePath: require("@/assets/images/template.jpeg"),
      modifiedImagePath: "",
    };
  },
  async mounted() {
    console.log(fabric);
    // Modify the original image and update the modifiedImagePath
    await this.modifyImage();
    console.log(this.modifiedImagePath, "asdd");
  },
  methods: {
    async modifyImage() {

      const canvas = new fabric.Canvas("canvas");

      // Load the original image onto the canvas
      fabric.Image.fromURL(this.originalImagePath, (img) => {
        // Set the position and size of the image
        canvas.setDimensions({ width: img.width, height: img.height });
        img.set({
          left: 0,
          top: 0,
          scaleX: canvas.width / img.width,
          scaleY: canvas.height / img.height,
        });

        // Add the image to the canvas
        canvas.add(img);

        // Calculate position based on canvas width
        const textRight = canvas.width - 2000; // 10 pixels from the right edge
        const imageLeft = canvas.width * 0.802; // 80% from the left edge

        const to = this.$route.query.to || 'Recipient'
        const text = new fabric.Text(to, {
          // originX: 'right',
          left: imageLeft,
          top: 140,
          fill: "black",
        });
        canvas.add(text);

        // Render canvas to data URL
        this.modifiedImagePath = canvas.toDataURL();
      });
    },
  },
};
</script>

<style>
body {
  padding: 0;
  margin: 0;
}
img {
  width: 100%;
}
</style>
