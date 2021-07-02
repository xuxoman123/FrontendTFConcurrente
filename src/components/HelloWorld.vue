<template>
  <v-container>
    
    <v-layout>
      <v-tabs grow centered>
        <v-tab>Aprender</v-tab>
        <v-tab-item>
          <v-flex xs12>
            <v-toolbar>
              <v-spacer></v-spacer>
              <v-toolbar-title>
                DATOS DE ENTRADA
              </v-toolbar-title>
              <v-spacer></v-spacer>
            </v-toolbar>
    
            <v-row>
              <v-col cols="12" sm="6" md="3">
                <v-text-field
                  label="Pos X"
                  type="number"
                  v-model.number="body.x" 
                  placeholder="4.20"
                ></v-text-field>
              </v-col>
              <v-col cols="12" sm="6" md="3">
                <v-text-field
                  label="Pos Y"
                  type="number"
                  v-model.number="body.y" 
                  placeholder="4.20"
                ></v-text-field>
              </v-col>
              <v-col cols="12" sm="6" md="3">
                <v-text-field
                  label="K"
                  v-model="body.k" 
                  placeholder="K"
                ></v-text-field>
              </v-col>
              <v-col cols="12" sm="6" md="3">
                <v-btn @click="callAPI_train">
                  Calcular
                </v-btn>
              </v-col>
            </v-row>
    
            <v-toolbar>
              <v-spacer></v-spacer>
              <v-toolbar-title>
                DATOS DE SALIDA
              </v-toolbar-title>
              <v-spacer></v-spacer>
            </v-toolbar>
            
            <v-row justify="center">
              <v-col>
                <template>
                  <v-data-table
                    :headers="headers_distancias_train"
                    :items="distancias_train"
                    sort-by="calories"
                    class="elevation-1"
                  >
                    <template v-slot:top>
                      <v-toolbar
                        flat
                      >
                        <v-spacer></v-spacer>
                        <v-toolbar-title>DISTANCIA EUCLIDIANA</v-toolbar-title>
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
              </v-col>
    
              <v-divider
                class="mx-4"
                inset
                vertical
              ></v-divider>
    
              <v-col>
                <template>
                  <v-data-table
                    :headers="headers_clusters_train"
                    :items="clusters_train"
                    sort-by="calories"
                    class="elevation-1"
                  >
                    <template v-slot:top>
                      <v-toolbar
                        flat
                      >
                        <v-spacer></v-spacer>
                        <v-toolbar-title>VECINOS</v-toolbar-title>
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
              </v-col>
            </v-row>
    
          </v-flex>
        </v-tab-item>
        <v-tab>Entrenada</v-tab>
        <v-tab-item>
          <v-flex xs12>
            <v-toolbar>
              <v-spacer></v-spacer>
              <v-toolbar-title>
                DATOS DE ENTRADA
              </v-toolbar-title>
              <v-spacer></v-spacer>
            </v-toolbar>
    
            <v-row>
              <v-col cols="12" sm="6" md="3">
                <v-text-field
                  label="Pos X"
                  type="number"
                  v-model.number="body.x" 
                  placeholder="4.20"
                ></v-text-field>
              </v-col>
              <v-col cols="12" sm="6" md="3">
                <v-text-field
                  label="Pos Y"
                  type="number"
                  v-model.number="body.y" 
                  placeholder="4.20"
                ></v-text-field>
              </v-col>
              <v-col cols="12" sm="6" md="3">
                <v-text-field
                  label="K"
                  v-model="body.k" 
                  placeholder="K"
                ></v-text-field>
              </v-col>
              <v-col cols="12" sm="6" md="3">
                <v-btn @click="callAPI_trained">
                  Calcular
                </v-btn>
              </v-col>
            </v-row>
    
            <v-toolbar>
              <v-spacer></v-spacer>
              <v-toolbar-title>
                DATOS DE SALIDA
              </v-toolbar-title>
              <v-spacer></v-spacer>
            </v-toolbar>
            
            <v-row justify="center">
              <v-col>
                <template>
                  <v-data-table
                    :headers="headers_distancias_trained"
                    :items="distancias_trained"
                    sort-by="calories"
                    class="elevation-1"
                  >
                    <template v-slot:top>
                      <v-toolbar
                        flat
                      >
                        <v-spacer></v-spacer>
                        <v-toolbar-title>DISTANCIA EUCLIDIANA</v-toolbar-title>
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
              </v-col>
    
              <v-divider
                class="mx-4"
                inset
                vertical
              ></v-divider>
    
              <v-col>
                <template>
                  <v-data-table
                    :headers="headers_clusters_trained"
                    :items="clusters_trained"
                    sort-by="calories"
                    class="elevation-1"
                  >
                    <template v-slot:top>
                      <v-toolbar
                        flat
                      >
                        <v-spacer></v-spacer>
                        <v-toolbar-title>VECINOS</v-toolbar-title>
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
              </v-col>
            </v-row>
    
          </v-flex>
        </v-tab-item>
      </v-tabs>
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

      headers_distancias_train: [
        { text: 'Pos X', value: 'x' },
        { text: 'Pos Y', value: 'y' },
        { text: 'Euclidiana', value: 'euclidiana' },
        { text: 'Actividad (Clase)', value: 'label' },
        { text: 'Tipo', value: 'tipo' },
        { text: 'Fecha', value: 'fecha' },
      ],
      
      distancias_train: [],

      headers_clusters_train: [
        { text: 'Vecinos', value: 'cluster' },
        { text: 'Clase', value: 'clase' }
      ],
      
      clusters_train: [],

      headers_distancias_trained: [
        { text: 'Pos X', value: 'x' },
        { text: 'Pos Y', value: 'y' },
        { text: 'Euclidiana', value: 'euclidiana' },
        { text: 'Actividad (Clase)', value: 'label' },
        { text: 'Tipo', value: 'tipo' },
        { text: 'Fecha', value: 'fecha' },
      ],
      
      distancias_trained: [],

      headers_clusters_trained: [
        { text: 'Vecinos', value: 'cluster' },
        { text: 'Clase', value: 'clase' }
      ],
      
      clusters_trained: [],
    }),
    methods: {
      callAPI_train(){
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

        axios.post(axios.defaults.baseURL + '/api/knn', body_aux)
        .then(res => {
          console.log('Info del Servidor:')
          console.log(res)
          console.log('DATA del Servidor')
          console.log(res.data)

          me.process_Data_train(res.data.data)
          me.process_Caminos_train(res.data.paths, res.data.classes)
        })
        .catch(err =>{
          console.log('Error:')
          console.log(err)
        })
      },
      callAPI_trained(){
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

        axios.post(axios.defaults.baseURL + '/api/knn_entrenado', body_aux)
        .then(res => {
          console.log('Info del Servidor:')
          console.log(res)
          console.log('DATA del Servidor')
          console.log(res.data)

          me.process_Data_trained(res.data.data)
          me.process_Caminos_trained(res.data.paths, res.data.classes)
        })
        .catch(err =>{
          console.log('Error:')
          console.log(err)
        })
      },
      process_Data_train(data){
        let me = this
        let distancia = {}
        for (let i = 0; i < data.length; i++) {
          distancia.x = data[i].punto.x
          distancia.y = data[i].punto.y
          distancia.euclidiana = data[i].distancia
          distancia.label = data[i].punto.label
          distancia.extra1 = data[i].tipo
          distancia.extra2 = data[i].fecha

          
          me.distancias_train.push(distancia)
          distancia = {}
        }
      },
      process_Data_trained(data){
        let me = this
        let distancia = {}
        for (let i = 0; i < data.length; i++) {
          distancia.x = data[i].punto.x
          distancia.y = data[i].punto.y
          distancia.euclidiana = data[i].distancia
          distancia.label = data[i].punto.label
          distancia.extra1 = data[i].tipo
          distancia.extra2 = data[i].fecha

          
          me.distancias_trained.push(distancia)
          distancia = {}
        }
      },
      process_Caminos_train(paths, classes){
        let me = this
        let cluster = ""
        paths.forEach(path => {
          path.forEach(element => {
            cluster += `${element.nombre} ${element.cont},`
          });
          me.clusters_train.push({cluster})
          cluster = ""
        }); 

        for (let i = 0; i < classes.length; i++) {
          me.clusters_train[i].clase = classes[i]
        }
      },
      process_Caminos_trained(paths, classes){
        let me = this
        let cluster = ""
        paths.forEach(path => {
          path.forEach(element => {
            cluster += `${element.nombre} ${element.cont},`
          });
          me.clusters_trained.push({cluster})
          cluster = ""
        }); 

        for (let i = 0; i < classes.length; i++) {
          me.clusters_trained[i].clase = classes[i]
        }
      }
    },
  }
</script>
