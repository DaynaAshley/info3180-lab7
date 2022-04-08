<template>
    <h1>Upload Form</h1>
<hr>

    <form @submit.prevent="uploadPhoto" id="uploadForm" method="post" >
    
        <div id="form">
            <div class="form-group">
               <label for="description">Description:</label>
                <textarea id="description" name="description" rows="4" cols="50"></textarea>
            </div>

            <div class="form-group">
                <label for="file">File:</label>
                <input type="file" name="file">
            </div>
            
            <button id="but" class="btn btn-primary">Submit</button>
        </div>

        
    </form>
</template>

<script>
     
export default {   
        data() {     
            return {
                 csrf_token: '' 
            }  
            }, 
        created() {     
                this.getCsrfToken(); 
            },
            
            methods: { 
               
                uploadPhoto() {  
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
};
</script>

<style>
.form-group{
    padding: 20px;
    display: flex;
}
label{
    padding-right: 5px;
}


</style>