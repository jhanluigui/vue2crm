<template>
  <v-container fluid>
    <v-flex xs12>
      <v-card class="grey lighten-4 elevation-0">
        <v-card-title>
         Nueva Entrevista
          <v-spacer></v-spacer>
          &nbsp;
          <v-btn fab small class="purple" @click.native="save()">
            <v-icon>save</v-icon>
          </v-btn>
          &nbsp;
          <v-btn fab small class="blue" @click.native="addProduct()">
            <v-icon>add</v-icon>
          </v-btn>
        </v-card-title>
        <v-card-text>
          <v-container fluid grid-list-md>
            <v-layout row wrap>
              <v-flex md12 xs12>
                  <v-select required autocomplete v-bind:items="alumnoList" label="Alumno" v-model="alumnoId"></v-select>
              </v-flex>
              <v-flex md8 xs12>
                  <v-select required v-bind:items="herramientaList" label="Herramienta de Seguimiento" ></v-select>
              </v-flex>
              <v-flex md8 xs12>
                  <v-select required v-bind:items="cartaList" label="Carta" ></v-select>
              </v-flex>
              <v-flex md8 xs12>
                  <v-select required v-bind:items="aeeList" label="AEE" ></v-select>
              </v-flex>
              <v-flex md8 xs12>
                  <v-select required v-bind:items="observacionList" label="Observación" ></v-select>
              </v-flex>

              <v-flex md8 xs12>
                  <v-select required v-bind:items="reportaList" label="Reporta" ></v-select>
              </v-flex>
              <v-flex md8 xs12>
                  <v-select required v-bind:items="motivoList" label="Motivo" ></v-select>
              </v-flex>
              <v-flex md8 xs12>
                  <v-select required v-bind:items="bimestreList" label="Bimestre" ></v-select>
              </v-flex>

      
            </v-layout>
          </v-container>
        </v-card-text>
      </v-card>
    </v-flex>
  </v-container>
</template>
<script>
export default {
  data () {
    return {
      alumnoId: null,
      errors: [],
      title: '',
      customer: {},
      rules: {
        age: [() => {
          if (this.customer.age < 18 || this.customer.age > 100) return 'Age is required. It must be bewteen 18 and 100'
        }],
        email: [() => {
          if (this.customer.email) {
            /* eslint-disable no-useless-escape */
            let re = /^(([^<>()\[\]\\.,;:\s@"]+(\.[^<>()\[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/
            if (!re.test(this.customer.email)) return 'Email is invalid.'
          }
        }]
      },
      alumnoList: [],
      herramientaList: [],
      cartaList: [],
      aeeList: [],
      observacionList: [],

      reportaList: [],
      motivoList: [],
      bimestreList: [],
    }
  },
  computed: {
  },
  watch: {
      alumnoId () {
        console.log("entro")
        if (this.alumnoId > 0) {
          console.log(this.alumnoId)
/*           this.products = []
          this.api.getData('products?categoryId=' + this.categoryId).then((res) => {
            res.data.forEach((c) => {
              let product = c
              product.text = c.productName
              product.value = c.id
              this.products.push(product)
            })
            return this.products
          }, (err) => {
            console.log(err)
          }) */
        }
      }
    },
  methods: {
    save () {
      let customer = this.customer
      if (!customer.id) {
        this.api.postData('customers', customer).then((res) => {
          this.$router.push({name: 'Customers'})
        }, (err) => {
          console.log(err)
        })
      } else {
        this.api.putData('customers/' + customer.id.toString(), customer).then((res) => {
          this.$router.push({name: 'Customers'})
        }, (err) => {
          console.log(err)
        })
      }
    },
    getById () {
      this.api.getData('customers/' + this.$route.params.id).then((res) => {
        this.customer = res.data
        // this.customer.avatar = '/assets/' + this.customer.avatar
      }, (err) => {
        console.log(err)
      })
    },
    cancel () {
      this.$router.push({name: 'Customers'})
    },
    getAlumnos () {
        this.api.getData('alumnos').then((res) => {
          this.alumnos = res.data
          this.alumnoList = []
          this.alumnos.forEach((c) => {
            let alumno = c
            alumno.text = c.SiglaCurso + " - " + c.Paterno + " " + c.Materno + ", " + c.Nombre
            alumno.value = c.idAlumno
            this.alumnoList.push(alumno)
          })
        }, (err) => {
          console.log(err)
        })
    },
    getHerramienta () {
        this.api.getData('herramientas').then((res) => {
          this.herramientas = res.data
          this.herramientaList = []
          this.herramientas.forEach((c) => {
            let herramienta = c
            herramienta.text = c.herramienta
            herramienta.value = c.idHerramienta
            this.herramientaList.push(herramienta)
          })
        }, (err) => {
          console.log(err)
        })
    },
    getCarta () {
        this.api.getData('carta').then((res) => {
          this.cartas = res.data
          this.cartaList = []
          this.cartas.forEach((c) => {
            let carta = c
            carta.text = c.descripcion
            carta.value = c.idCarta
            this.cartaList.push(carta)
          })
        }, (err) => {
          console.log(err)
        })
    },
    getAee () {
        this.api.getData('especialidad').then((res) => {
          this.especialidades = res.data
          this.aeeList = []
          this.especialidades.forEach((c) => {
            let especialidad = c
            especialidad.text = c.especialidad
            especialidad.value = c.idEspecialidad
            this.aeeList.push(especialidad)
          })
        }, (err) => {
          console.log(err)
        })
    },
    getObservacion () {
        this.api.getData('observacion').then((res) => {
          this.observaciones = res.data
          this.observacionList = []
          this.observaciones.forEach((c) => {
            let observacion = c
            observacion.text = c.observacion
            observacion.value = c.idObservacion
            this.observacionList.push(observacion)
          })
        }, (err) => {
          console.log(err)
        })
    },
    getReporta () {
        this.api.getData('personal').then((res) => {
          this.personales = res.data
          this.reportaList = []
          this.personales.forEach((c) => {
            let reporta = c
            reporta.text = c.Paterno + " " + c.Materno + ", " + c.Nombres
            reporta.value = c.IdPersonal
            this.reportaList.push(reporta)
          })
        }, (err) => {
          console.log(err)
        })
    },
    getMotivo () {
        this.api.getData('motivo').then((res) => {
          this.motivos = res.data
          this.motivoList = []
          this.motivos.forEach((c) => {
            let motivo = c
            motivo.text = c.motivo
            motivo.value = c.idMotivo
            this.motivoList.push(motivo)
          })
        }, (err) => {
          console.log(err)
        })
    },
    getBimestre () {
        this.api.getData('bimestre').then((res) => {
          this.bimestres = res.data
          this.bimestreList = []
          this.bimestres.forEach((c) => {
            let bimestre = c
            bimestre.text = c.bimestre
            bimestre.value = c.idBimestre
            this.bimestreList.push(bimestre)
          })
        }, (err) => {
          console.log(err)
        })
    },
  },
  mounted () {
    this.getAlumnos()
    this.getHerramienta()
    this.getCarta()
    this.getAee()
    this.getObservacion()
    this.getReporta()
    this.getMotivo()
    this.getBimestre()
    if (this.$route.params.id) {
      this.getById()
      this.title = 'Edit Customer'
    } else this.title = 'New Customer'
  }
}
</script>
