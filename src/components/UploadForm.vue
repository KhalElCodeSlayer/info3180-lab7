<template>
    <form id="uploadForm" enctype="multipart/form-data" @submit.prevent="uploadPhoto">
        <label for = "description">Description: </label>
        <br>
        <textarea name = "description" cols="50" rows="5" maxlength="250">
        </textarea>
        <br>
        <label for = "photo">Photo Upload</label>
        <br>
        <input type = 'file' name = 'photo'>
        <br>
        <br>
        <button type="submit" class="btn btn-primary">Submit</button>
    </form>
</template>

<script>
export default {
    data() {
        return{
            csrf_token: ''
        }
    },
    created() {
        this.getCsrfToken();
    },
    methods:{
        uploadPhoto(){
            let uploadForm = document.getElementById('uploadForm');
            let form_data = new FormData(uploadForm);

            fetch("/api/upload", {
                method: 'POST',
                body: form_data,
                headers: {
                    'X-CSRFToken': this.csrf_token
                }
            })
            .then(function (response) {
                return response.json();
            })
            .then(function (data) {
                // display a success message
                console.log(data);
            })
            .catch(function (error) {
                console.log(error);
            });
        },
        
        getCsrfToken() {
            let self = this;
            fetch('/api/csrf-token')
                .then((response) => response.json())
                .then((data) => {
                    console.log(data);
                    self.csrf_token = data.csrf_token;
                })
        }
    }
}
</script>
