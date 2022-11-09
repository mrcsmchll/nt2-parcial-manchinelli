<template>

  <section class="src-componentes-ingreso">
    <div class="jumbotron">
      <h2>Ingreso de Gastos</h2>
      <hr>

      <vue-form :state="formstate" @submit.prevent="enviar()">
        
        <!-- Campo nombre -->
        <validate tag="div">
          <label for="nombre">Nombre</label>
          <input type="text" id="nombre" v-model.trim="formData.nombre" required name="nombre" autocomplete="off" class="form-control" no-espacios :minlength="nombreMinLength"
          :maxlength="nombreMaxLength" />
        <!-- validacion -->
          <field-messages name="nombre" show="$dirty">
            <div slot="required" class="alert alert-danger mt-1">Este campo es obligatorio</div>
            <div slot="minlength" class="alert alert-danger mt-1">Nombre debe poseer al menos {{nombreMinLength}} caracteres.
            </div>
            <div slot="maxlength" class="alert alert-danger mt-1">Nombre debe poseer un maximo de {{nombreMaxLength}} caracteres.
            </div>
          </field-messages>

          
        </validate>
        <br>

        <!-- Campo Descripcion -->
        <validate tag="div">
          <label for="desc">Descripción</label>
          <input type="text" id="desc" v-model="formData.desc" required name="desc" autocomplete="off" class="form-control" />
    
          <field-messages name="desc" show="$dirty">
            <div slot="required" class="alert alert-danger mt-1">Este campo es obligatorio</div>
          </field-messages>
        </validate>
        <br>
        
        <!-- Campo importe -->
        <validate tag="div">
          <label for="importe">Importe</label>
          <input type="number" id="importe" v-model.number="formData.importe" required name="importe" autocomplete="off" class="form-control" />
    
          <field-messages name="importe" show="$dirty">
            <div slot="required" class="alert alert-danger mt-1">Este campo es obligatorio</div>
          </field-messages>
        </validate>

        <br>
        
        <button class="btn btn-success my-3" :disabled="formstate.$invalid" type="submit">Enviar</button>
      </vue-form>
      <br>
      <hr>

      <!-- Tabla para representar los datos ingresados -->
      <h2>Detalle de Gastos</h2>
      <br>

      <!-- <pre>{{ gastos }}</pre> -->

      <div v-if="gastos.length" class="table-responsive">
        <table class="table">
          <tr>
            <th>Nombre</th>
            <th>Descripcion</th>
            <th>Importe</th>
            <th>Fecha</th>
          </tr>
          <tr v-for="(gasto,index) in gastos" :key="index">
            <td>{{ gasto.nombre }}</td>
            <td>{{ gasto.desc }}</td>
            <td> $ {{ gasto.importe }}</td>
            <td>{{ gasto.fecha }}</td>
          </tr>
          <tr v-for="(gasto, index) in gastos" :key="index" :style="{color: analizarSaldo(gasto).color }">
            <td></td>
            <td>TOTAL</td>
            <td>{{total}}</td>
            <td></td>
          </tr>
        </table>
      </div>
      <h3 v-else class="alert alert-info">No hay gastos ingresados</h3>

    </div>
  </section>

</template>

<script>

  export default  {
    name: 'src-componentes-ingreso',
    props: [],
    mounted () {

    },
    data () {
      return {
        formstate : {},
        formData : this.getInitialData(),
        nombreMinLength: 3,
        nombreMaxLength: 15,
        gastos : [],
        total: 0
      }
    },
    methods: {
      getInitialData() {
        return {
          nombre : null,
          desc: null,
          importe: null
        }
      },
      enviar() {
        let gasto = {...this.formData}
        gasto.fecha = new Date().toLocaleString()
        this.total = this.gastos.forEach(gasto => gasto + gasto.importe)

        console.log(gasto)
        this.gastos.push(gasto)

        this.formData = this.getInitialData()
        this.formstate._reset()
      },
      analizarSaldo(gasto) {
        this.total = gasto + this.gastos.forEach(gasto => gasto + gasto.importe)
        let color = '#080'
        if(this.total > 5000) color = '#00F'
        if(this.total < 1000) color = '#F00'
        return {
          valor : this.total,
          color
        }
      },
    },
    computed: {
    }
}


</script>

<style scoped lang="css">
  .src-componentes-ingreso {

  }

  .jumbotron {
    background-color: rgb(163, 216, 248);
    color: brown;
  }

  hr {
    background-color: #eee;
  }

  pre {
    color: white;
  }
</style>
