<template>
  <div class="container">
    <div class="row card-panel">
      <form @submit.prevent="agregarUsuario">
        <div class="col m12">
          <div class="col m4">
            <label>Nombre</label>
            <input type="text" v-model="nombre" />
          </div>
          <div class="col m4">
            <label>Apellido</label>
            <input type="text" v-model="apellido" />
          </div>
        </div>

        <div class="row">
          <div class="col m4">
            <label>Edad</label>
            <input type="number" v-model="edad" />
          </div>
          <div class="col m4">
            <label>Estado Civil</label>
            <select class="browser-default" id="select_estado_civil" v-model="estado_civil">
              <option value="">Seleccione</option>
              <option
                v-for="estado in estados_civiles"
                v-bind:key="estado"
                v-bind:value="estado.id"
              >
                {{ estado.descripcion }}
              </option>
            </select>
          </div>

          <div class="col m4">
            <label>Email</label>
            <input type="email" v-model="correo" />
          </div>
        </div>

        <div class="row">
          <form @submit.prevent="agregarPasatiempo" >
            <div class="col m4">
              <label>Hobbies</label>
              <input type="text" v-model="pasatiempo" />
              <button type="submit" class="btn indigo darken-3">
                Agregar<i class="material-icons right">send</i>
              </button>
              <br />
              <ul>
                <li v-for="(pasatiempo, index) in pasatiempos" v-bind:key="pasatiempo">
                  {{ pasatiempo.id }} - {{ pasatiempo.descripcion }}
                  <a href="!#" @click="pasatiempos.splice(index, 1)"><i class="material-icons right">close</i></a>
                </li>
              </ul>
            </div>
          </form>

        <div class="row right s6">
          <div class="col m12 s12">
            <label>
              <input type="checkbox" v-model="suscribirse" />
              <span>Suscripción a nuestro newletter</span>
            </label>
          </div>
        </div>
        </div>

        <div class="row">
          <button type="submit" class="btn indigo darken-4 col s12">
            {{indice == -1 ? 'Agregar' : 'Actualizar'}} usuario
          </button>
        </div>
      </form>
    </div>

    <div class="col m12">
      <table class="table cordered striped">
        <thead>
          <tr>
            <th>Nombre</th>
            <th>Apellido</th>
            <th>Edad</th>
            <th>Estado Civil</th>
            <th>Email</th>
            <th>Pasatiempos</th>
            <th>Suscripcion</th>
            <th>Editar</th>
            <th>Eliminar</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(usuario, index) in usuarios" v-bind:key="usuario" v-bind:class="{'indigo darken-4 white-text': index == indice}">
            <td>{{ usuario.nombre }}</td>
            <td>{{ usuario.apellido }}</td>
            <td>{{ usuario.edad }}</td>
            <td>{{ usuario.estado_civil_descripcion }}</td>
            <td>{{ usuario.correo }}</td>
            <td>
              <ul>
                <li
                  v-for="pasatiempo in usuario.pasatiempos"
                  v-bind:key="pasatiempo"
                >
                  {{ pasatiempo.id }}- {{ pasatiempo.descripcion }}
                </li>
              </ul>
            </td>
            <td>
              <label
                ><input
                  type="checkbox"
                  disabled
                  v-model="usuario.suscripto" /><span></span
              ></label>
            </td>
            <td>
              <a href="!#" @click="editar(index)"><i class="material-icons">create</i></a>
            </td>
            <td>
              <a href="!#" @click="eliminar(index)"><i class="material-icons">delete</i></a>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>




<script>
import M from "materialize-css";
export default {
  name: "App",
  data() {
    return {
      indice: -1,
      nombre: "",
      apellido: "",
      edad: 0,
      estado_civil: "",
      correo: "",
      suscribirse: false,
      pasatiempo: "",
      pasatiempos: [],
      usuarios: [],

      select_instancias: [],

      estados_civiles: [
        {
          id: 'S',
          descripcion: "Soltero",
        },
        {
          id: 'C',
          descripcion: "Casado",
        },
        {
          id: 'D',
          descripcion: "Divorciado",
        },
        {
          id: 'V',
          descripcion: "Viudo",
        },
      ]
    };
  },
  mounted() {
    var elems = document.querySelectorAll("select");
    this.select_instancias = M.FormSelect.init(elems, null);
  },
  methods: {
    agregarUsuario() {
      var index_estado_civil = this.estados_civiles.findIndex(x=>x.id == this.estado_civil)
      if(index_estado_civil == -1){
          M.toast({html: 'Seleccione un estado Civil'});
          return;
      }
      var data = {
        nombre: this.nombre,
        apellido: this.apellido,
        edad: this.edad,
        estado_civil: this.estado_civil,
        estado_civil_descripcion: this.estados_civiles[index_estado_civil].descripcion,
        correo: this.correo,
        suscripto: this.suscribirse,
        pasatiempos: this.pasatiempos,
      };
      if(this.indice == -1){
        this.usuarios.push(data);
      }else{
        this.usuarios[this.indice] = data;
      }
      this.indice = -1,
      (this.nombre = ""),
        (this.apellido = ""),
        (this.edad = 0),
        (this.estado_civil = ""),
        (this.correo = ""),
        (this.suscribirse = false),
        (this.pasatiempo = ""),
        (this.pasatiempos = []);
    },
    agregarPasatiempo() {
      var total = this.pasatiempos.length;
      var ultimo = 0;
      if (total > 0) {
        ultimo = this.pasatiempos[total - 1].id;
      }
      var data = {
        id: ultimo + 1,
        descripcion: this.pasatiempo,
      };
      this.pasatiempos.push(data);
      this.pasatiempo = "";
    },
    eliminar(index){
      if(!confirm('¿Desea eliminar este registro?'))return;

      this.usuarios.splice(index, 1)
    },
    editar(index){
      var usuario = this.usuarios[index];
      this.indice = index;
      this.nombre = usuario.nombre;
      this.apellido = usuario.apellido;
      this.edad = usuario.edad;
      this.correo = usuario.correo;
      this.estado_civil = usuario.estado_civil;
      this.pasatiempos = usuario.pasatiempos
    }
  },
};
</script>

<style></style>
