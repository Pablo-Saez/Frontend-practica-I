<template>
  <q-page class="window-height window-width row justify-center items-center">
    <div class="column">

      <div class="row">
        <q-card square bordered class="q-pa-lg shadow-1">
          <q-card-section>
            <div class="row">
              <img class="img-login"
              style="width: 200px; "
              src="~assets/loginsa.png"
            />
            </div>
          </q-card-section>
          <q-card-section>
            <q-form class="q-gutter-md">
              <q-input square filled clearable v-model="email" type="email" label="email" />
              <q-input square filled clearable v-model="password" type="password" label="password" />
            </q-form>
          </q-card-section>
          <q-card-actions class="q-px-md">
            <q-btn unelevated color="primary"  @click="iniciar_sesion" size="lg" class="full-width" label="Login" type="submit" />
          </q-card-actions>
          <q-card-section class="text-center q-pa-none">
            <div class="q-mt-sm q-gutter-lg">

                <q-btn class="text-black"  @click="dialogo=true"> Crear una cuenta </q-btn>
            </div>
          </q-card-section>
        </q-card>
      </div>
    </div>
    <registrarUsuario v-if="dialogo==true" @cerrarDialogo="aux"></registrarUsuario>
  </q-page>
</template>

<script>
import registrarUsuario from 'components/registrarUsuario'
import axios from "axios";
import { mapState } from 'vuex';


export default {
  name: 'Login',
  computed: {
    ...mapState(["baseurl"])
  },
  data () {
    return {
      data: {},
      email: null,
      password: null,
      dialogo: false
    }
  },
  methods: {
    iniciar_sesion () {
      if(this.password==null || this.email==null){
        alert("Todos los campos son obligatorios");
        return
      }
      else{
      let logindto = {
        email:this.email,
        password:this.password
      }
      axios
      .post(this.baseurl+ "/login",logindto).then(response=>{
        if(response.data.id!=null){
        sessionStorage.setItem("id",response.data.id);
        sessionStorage.setItem("email",response.data.email);
        sessionStorage.setItem("nombre",response.data.name);
        sessionStorage.setItem("last_name",response.data.last_name);
        sessionStorage.setItem("company",response.data.company);
        sessionStorage.setItem("rutcompany",response.data.rutcompany);
        this.$router.push({ path: "/index" });

        }
        else{
          alert("Credenciales invalidas");
        }
      })
      .catch(e=>{
        console.log(e);
      });
      }
      console.log(sessionStorage);


    },
    aux () {
      this.dialogo = false
    }


  },
  components: {
    registrarUsuario
  },
  watch: {
    dialogo () {
      console.log(this.dialogo)
    },
    baseurl(){
      console.log(this.baseurl)
    }


  },
  created(){
    //console.log(this.baseurl);
  }

}


</script>

<style>
.q-card {
  width: 360px;
}
.img-login{
  display:block;
  margin:auto;
}

</style>
