<template>
  <div>
    <button v-on:click="upload">Test Cloundinary Upload</button><br>
    
  </div>
</template>

<script>

export default {
    name: 'CloudinaryComponent',
    data(){
        return{
            // To use this as part of the Vue app, going to declare a widget here
            // It will be configured in the mounted lifecycle hook and called
            // when someone presses the upload button on the page.
            myWidget: {}
        }

    },
    methods: {
        upload(){
            let text = "Allow access to Photos and Camera";
            if (confirm(text) == true) {
                console.log("Opening")
                this.myWidget.open();
            } else {
               text = "You canceled!";
             }
        }
    },
    mounted(){
        // Going to wait until the mounted lifecycle hook to start doing Cloudinary stuff
        // Set the cloud name globally here, instead of in each call
        // Reading value from .env
        window.cloudinary.setCloudName(process.env.VUE_APP_CLOUDINARY_CLOUD_NAME)
        // the Cloudinary widget is technically part of the window, so the calls shown in the  
        // Cloudinary documentation must be preceded by window 
        // Creating window with uploadPreset from .env
        this.myWidget = window.cloudinary.createUploadWidget({
                uploadPreset: process.env.VUE_APP_CLOUDINARY_CLOUD_UPLOAD,
                // sources allows us to choose where the Cloudinary app gets files from
                // in this case, going to only allow file upload, URL, or camera image
                sources: ['local', 'url', 'camera']
            }, (error, result) => { 

            if (!error && result && result.event === "success") { 
              console.log('Done! Here is the image info: ', result.info); 
              console.log("Image URL: " + result.info.url);
            }
        });
    }

}
</script>

<style>

</style>