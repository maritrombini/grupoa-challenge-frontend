<template>
  <v-data-table
    :headers="headers"
    :items="students"
    :search="search"
    sort-by="RA"
    class="elevation-1"
  >
    <template v-slot:top>
      <v-toolbar flat>
        <v-toolbar-title>Alunos</v-toolbar-title>
        <v-divider class="mx-4" inset vertical></v-divider>
        <v-spacer></v-spacer>
        <v-card-title>
          <v-text-field
            v-model="search"
            append-icon="mdi-magnify"
            label="Pesquisar"
            single-line
            hide-details
          ></v-text-field>
        </v-card-title>
        <v-dialog v-model="dialog" max-width="500px">
          <template v-slot:activator="{ on, attrs }">
            <v-btn color="primary" dark class="mb-2" v-bind="attrs" v-on="on">
              Cadastrar
            </v-btn>
          </template>
          <v-card>
            <v-card-title>
              <span class="text-h5">{{ formTitle }}</span>
            </v-card-title>

            <v-card-text>
              <v-container>
                <v-row>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      v-model="editedItem.RA"
                      label="RA Aluno"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      v-model="editedItem.Nome"
                      label="Nome aluno"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      v-model="editedItem.Email"
                      label="Email"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      v-model="editedItem.CPF"
                      label="CPF"
                    ></v-text-field>
                  </v-col>
                </v-row>
              </v-container>
            </v-card-text>

            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="blue darken-1" text @click="close">
                Cancelar
              </v-btn>
              <v-btn color="blue darken-1" text @click="save">
                Salvar
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>
        <v-dialog v-model="dialogDelete" max-width="500px">
          <v-card>
            <v-card-title class="text-h5"
              >Tem certeza que deseja remover
              {{ editedItem.Nome }}?</v-card-title
            >
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="blue darken-1" text @click="closeDelete"
                >Cancelar</v-btn
              >
              <v-btn color="blue darken-1" text @click="deleteItemConfirm"
                >OK</v-btn
              >
              <v-spacer></v-spacer>
            </v-card-actions>
          </v-card>
        </v-dialog>
      </v-toolbar>
    </template>
    <template v-slot:item.actions="{ item }">
      <v-icon small class="mr-2" @click="editItem(item)">
        mdi-pencil
      </v-icon>
      <v-icon small @click="deleteItem(item)">
        mdi-delete
      </v-icon>
    </template>
    <template v-slot:no-data>
      <v-btn color="primary" @click="initialize">
        Reset
      </v-btn>
    </template>
  </v-data-table>
</template>

<script>
import axios from 'axios'
export default {
  data: () => ({
    dialog: false,
    dialogDelete: false,
    search: '',
    baseUrl: 'https://grupo-a.herokuapp.com/api/aluno',
    headers: [
      {
        text: 'RA',
        align: 'start',
        sortable: false,
        value: 'RA'
      },
      { text: 'Nome', value: 'Nome' },
      { text: 'Email', value: 'Email' },
      { text: 'CPF', value: 'CPF' },
      { text: 'Actions', value: 'actions', sortable: false }
    ],
    students: [],
    editedIndex: -1,
    editedItem: {
      RA: 0,
      Nome: '',
      Email: '',
      CPF: 0,
      id: -1
    },
    defaultItem: {
      RA: 0,
      Nome: '',
      Email: '',
      CPF: 0,
      id: -1
    }
  }),

  computed: {
    formTitle() {
      return this.editedIndex === -1 ? 'Novo Aluno' : 'Editar Aluno'
    }
  },

  watch: {
    dialog(val) {
      val || this.close()
    },
    dialogDelete(val) {
      val || this.closeDelete()
    }
  },

  created() {
    this.initialize()
  },

  methods: {
    initialize() {
      axios.get(this.baseUrl).then(response => (this.students = response.data))
      console.log(this.students)
    },

    editItem(item) {
      this.editedIndex = this.students.indexOf(item)
      this.editedItem = Object.assign({}, item)
      this.dialog = true
    },

    deleteItem(item) {
      this.editedIndex = this.students.indexOf(item)
      this.editedItem = Object.assign({}, item)
      this.dialogDelete = true
    },

    deleteItemConfirm() {
      this.students.splice(this.editedIndex, 1)

      let url = this.baseUrl + '/' + this.editedItem.id

      let formData = new FormData()
      formData.append('_method', 'delete')

      let config = {
        headers: {
          Accept: 'application/json'
        }
      }
      axios.post(url, formData, config).then(response => {
        alert(
          'Aluno ' + this.editedItem.Nome + 'removido com sucesso',
          response.data
        )
      })
      this.closeDelete()
    },

    close() {
      this.dialog = false
      this.$nextTick(() => {
        this.editedItem = Object.assign({}, this.defaultItem)
        this.editedIndex = -1
      })
    },

    closeDelete() {
      this.dialogDelete = false
      this.$nextTick(() => {
        this.editedItem = Object.assign({}, this.defaultItem)
        this.editedIndex = -1
      })
    },

    save() {
      if (this.editedIndex > -1) {
        let url = this.baseUrl + '/' + this.editedItem.id

        let formData = new FormData()
        formData.append('_method', 'patch')
        formData.append('Nome', this.editedItem.Nome)
        formData.append('Email', this.editedItem.Email)

        let config = {
          headers: {
            Accept: 'application/json'
          }
        }
        axios.post(url, formData, config).then(response => {
          console.log(response)
        })
        alert('Alterações feitas com sucesso.')
        Object.assign(this.students[this.editedIndex], this.editedItem)
      } else {
        let formData = new FormData()
        formData.append('RA', this.editedItem.RA)
        formData.append('Nome', this.editedItem.Nome)
        formData.append('Email', this.editedItem.Email)
        formData.append('CPF', this.editedItem.CPF)

        let config = {
          headers: {
            Accept: 'application/json'
          }
        }
        axios.post(this.baseUrl, formData, config).then(response => {
          alert('Aluno cadastrado com sucesso', response.data)
          this.initialize()
        })
        this.initialize()
      }

      this.close()
    }
  }
}
</script>
