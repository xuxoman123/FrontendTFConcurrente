<template>
  <v-container>
    <v-layout>
      <v-flex xs12>
        <v-toolbar>DATOS DE ENTRADA</v-toolbar>
        <v-row>
          <v-col cols="12" sm="6" md="3">
            <v-text-field
              label="Longitud"
              type="number"
              v-model.number="body.x" 
              placeholder="Escriba la longitud"
            ></v-text-field>
          </v-col>
          <v-col cols="12" sm="6" md="3">
            <v-text-field
              label="Latitud"
              type="number"
              v-model.number="body.y" 
              placeholder="Escriba la latitud"
            ></v-text-field>
          </v-col>
          <v-col cols="12" sm="6" md="3">
            <v-text-field
              label="K"
              v-model="body.k" 
              placeholder="Escriba número de vecinos"
            ></v-text-field>
          </v-col>
          <v-col cols="12" sm="6" md="3">
            <v-btn @click="callAPI">
              Calcular
            </v-btn>
          </v-col>
        </v-row>
        <v-toolbar>DATOS DE SALIDA</v-toolbar>

        <v-divider
            class="mx-4"
            inset
          ></v-divider>
        
        <v-row justify="center">
          <template>
            <v-data-table
              :headers="headers_distancias"
              :items="distancias"
              sort-by="calories"
              class="elevation-1"
            >
              <template v-slot:top>
                <v-toolbar
                  flat
                >
                  <v-toolbar-title>DISTANCIA EUCLIDIANA</v-toolbar-title>
                  <v-divider
                    class="mx-4"
                    inset
                    vertical
                  ></v-divider>
                  <v-spacer></v-spacer>
                </v-toolbar>
              </template>
              <template v-slot:no-data>
                <v-btn>
                  NO HAY DATA
                </v-btn>
              </template>
            </v-data-table>
          </template>
        </v-row>

        <v-divider
            class="mx-4"
            inset
          ></v-divider>

          <v-row justify="center">
          <template>
            <v-data-table
              :headers="headers_clusters"
              :items="clusters"
              sort-by="calories"
              class="elevation-1"
            >
              <template v-slot:top>
                <v-toolbar
                  flat
                >
                  <v-toolbar-title>VECINOS</v-toolbar-title>
                  <v-divider
                    class="mx-4"
                    inset
                    vertical
                  ></v-divider>
                  <v-spacer></v-spacer>
                </v-toolbar>
              </template>
              <template v-slot:no-data>
                <v-btn>
                  NO HAY DATA
                </v-btn>
              </template>
            </v-data-table>
          </template>
        </v-row>
      </v-flex>
    </v-layout>
    
  </v-container>
</template>

<script>
  import axios from 'axios'
  export default {
    name: 'HelloWorld',

    data: () => ({
      body: {
          x: null,
          y: null,
          k: ""
        },

      headers_distancias: [
        { text: 'Longitud', value: 'x' },
        { text: 'Latitud', value: 'y' },
        { text: 'Euclidiana', value: 'euclidiana' },
        { text: 'Clase', value: 'label' },
        { text: 'Tipo', value: 'tipo' },
        { text: 'Estado', value: 'estado' },
      ],
      
      distancias: [],

      headers_clusters: [
        { text: 'Vecinos', value: 'cluster' },
        { text: 'Clase', value: 'clase' }
      ],
      
      clusters: [],
    }),
    methods: {
      callAPI(){
        let me = this;
        me.distancias = []
        me.clusters = []

        //
        let body_aux = {
          x: null,
          y: null,
          k: ""
        }
        body_aux.x = me.body.x
        body_aux.y = me.body.y
        body_aux.k = me.body.k

        body_aux.k = body_aux.k.split(",")
        for (let i = 0; i < body_aux.k.length; i++) {
          body_aux.k[i] = Number(body_aux.k[i])
          
        }

        axios.post(axios.defaults.baseURL + '/postman/KnnConcu  ', body_aux)
        .then(res => {
          //console.log('Info del Servidor:')
          //console.log(res)
          //console.log('DATA del Servidor')
          //console.log(res.data)

          me.process_Data(res.data.data)
          me.process_Caminos(res.data.paths, res.data.classes)
        })
        .catch(err =>{
          console.log('Error:')
          console.log(err)
        })
      },

      process_Data(data){
        let me = this
        let distancia = {}
        for (let i = 0; i < data.length; i++) {
          distancia.x = data[i].punto.x
          distancia.y = data[i].punto.y
          distancia.euclidiana = data[i].distancia
          distancia.label = data[i].punto.label
          distancia.tipo = data[i].tipo
          distancia.estado = data[i].estado

          
          me.distancias.push(distancia)
          distancia = {}
        }
      },

      process_Caminos(paths, classes){
        let me = this
        let cluster = ""
        paths.forEach(path => {
          path.forEach(element => {
            cluster += `${element.nombre} ${element.cont},`
          });
          me.clusters.push({cluster})
          cluster = ""
        }); 

        for (let i = 0; i < classes.length; i++) {
          me.clusters[i].clase = classes[i]
        }
      }

    },
  }
</script>
