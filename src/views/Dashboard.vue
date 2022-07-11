<template>
     <div>
          <h1>Dashboard</h1>
          <p>
               User: {{user.name}} <br>
               Token: {{token}}
          </p>
     </div>
</template>
<script>

import { useAuthStore } from '../stores/auth'
import { useRouter } from 'vue-router'
export default {
     data:()=>{
          return {
               user:{}
          }
     },

    async  mounted(){
     await fetch('http://localhost:8000/api/user',{
          method:'get',
          headers:{
               "Accept":"application/json",
               "Authorization":"Bearer " + this.token
          }
        }).then(response=>response.json())
        .then(data=>{
          this.user = data
        })
     },

     setup() {

          const { user, token } =  useAuthStore()    
          const router = useRouter()    

          if(token==""){
               router.replace('/login')
          }
          
               return{
                    token
         }
     },
}
</script>