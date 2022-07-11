<template>
     <div class="row mt-3">
          <div class="col-md-4 offset-md-4">
               <div class="card">
                    <div class="card-header bg-light text-center">
                         <h4 >User Registry</h4>
                    </div>
                    <form @submit.prevent="handleSubmit">
                          <div class="card-body">
                              <div class="mb-3">
                                        <label for="name">User Name</label>
                                        <input type="text" id="name" class="form-control" v-model="user.name">
                               </div>
                               <div class="mb-3">
                                        <label for="email">Email</label>
                                        <input type="email" id="email" class="form-control" v-model="user.email">
                                </div>
                                <div class="mb-3">
                                        <label for="password">Password</label>
                                        <input type="password" id="password" class="form-control" v-model="user.password">
                                </div>
                                <div class="card-footer">
                                        <button type="submit" class="btn btn-success">
                                             Register 
                                        </button>
                                </div>
                         </div>
                     </form>
               </div>
          </div>     
     </div>
</template>
<script>
import  {ref}  from 'vue'
import { useAuthStore } from '../stores/auth'
import { useRouter } from 'vue-router'

export default {
     setup() {
          const user = ref({name:"",  email:"",  password:"" })
          const authStore = useAuthStore()
          const router = useRouter()

          async function handleSubmit(){
               await fetch('http://localhost:8000/api/register',{
                    method: 'post',
                    headers: {
                         "Accept":"application/json",       
                         "Content-Type":"application/json",       
                    },
                    body: JSON.stringify(user.value)
                    
               }).then(response=>response.json())
               .then(data=>{
                    if(data.status=="success"){
                         authStore.saveAuth(data.user, data.token)
                         router.push('/dashboard')
                    }
               })
          }
          return {
               user,
               handleSubmit
          }
     },
}
</script>