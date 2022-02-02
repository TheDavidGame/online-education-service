<template>
  <v-container>
    <v-row>
      <v-col cols="12" md="6">
        <v-select
          v-model="mainSelect"
          :items="mainItem"
          label="Раздел"
          @change="changeShowLangSelect"
        ></v-select>
      </v-col>
      <v-col cols="12" md="6">
        <v-select
          v-if="showLangSelect"
          v-model="langSelect"
          :items="langItem"
          label="Язык"
          @change="changeShowTable"
        ></v-select>
      </v-col>
    </v-row>
    <v-data-table
      v-if="showTable"
      :headers="headers"
      :items="itemTable"
      class="elevation-1"
    >
      <template v-slot:top>
        <v-toolbar flat>
          <v-toolbar-title>{{ mainSelect }}</v-toolbar-title>
          <v-divider class="mx-4" inset vertical></v-divider>
          <v-spacer></v-spacer>
          <v-dialog v-model="dialog" max-width="500px">
            <template v-slot:activator="{ on, attrs }">
              <v-btn color="primary" dark class="mb-2" v-bind="attrs" v-on="on">
                Добавить поле
              </v-btn>
            </template>
            <v-card>
              <v-card-title>
                <span class="text-h5">{{ formTitle }}</span>
              </v-card-title>

              <v-card-text>
                <v-container>
                  <!-- добавить поле -->
                  <v-row>
                    <v-col cols="12" md="6">
                      <v-text-field
                        v-model="editedItem.name"
                        label="Название"
                      ></v-text-field>
                    </v-col>
                    <v-col cols="12" md="6">
                      <!-- <v-text-field
                        v-model="editedItem.description"
                        label="Описание"
                      ></v-text-field> -->
                      <v-textarea
                        v-model="editedItem.description"
                        clearable
                        auto-grow
                        rows="2"
                        row-height="10"
                        clear-icon="mdi-close-circle"
                        label="Описание"
                      ></v-textarea>
                    </v-col>
                  </v-row>
                </v-container>
              </v-card-text>

              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn color="blue darken-1" text @click="close">
                  Отменить
                </v-btn>
                <v-btn color="blue darken-1" text @click="save">
                  Сохранить
                </v-btn>
              </v-card-actions>
            </v-card>
          </v-dialog>
          <v-dialog v-model="dialogDelete" max-width="500px">
            <v-card>
              <v-card-title class="text-h5"
                >Вы точно хотите удалить?</v-card-title
              >
              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn color="blue darken-1" text @click="closeDelete"
                  >Отменить</v-btn
                >
                <v-btn color="blue darken-1" text @click="deleteItemConfirm"
                  >Удалить</v-btn
                >
                <v-spacer></v-spacer>
              </v-card-actions>
            </v-card>
          </v-dialog>
        </v-toolbar>
      </template>
      <template v-slot:item.actions="{ item }">
        <v-icon small class="mr-2" @click="editItem(item)"> mdi-pencil </v-icon>
        <v-icon small @click="deleteItem(item)"> mdi-delete </v-icon>
      </template>
      <template v-slot:no-data>
        <v-btn color="primary" @click="initialize"> Reset </v-btn>
      </template>
    </v-data-table>
  </v-container>
</template>

<script>
export default {
  name: 'LearningModule',
  components: {},
  data() {
    return {
      mainSelect: '',
      mainItem: ['Справочники', 'Предметы', 'Темы', 'Подтемы'],
      showLangSelect: false,
      showTable: false,
      langSelect: '',
      langItem: ['ru', 'en', 'he'],
      dialog: false,
      dialogDelete: false,
      headers: [
        {
          text: 'Название',
          align: 'start',
          sortable: false,
          value: 'name'
        },
        { text: 'Описание', value: 'description' },
        { text: 'Измененить', value: 'actions', sortable: false }
      ],
      itemTable: [],
      editedIndex: -1,
      editedItem: {
        name: '',
        description: ''
      },
      defaultItem: {
        name: '',
        description: ''
      }
    }
  },
  computed: {
    formTitle() {
      return this.editedIndex === -1 ? 'Добавить поле' : 'Редактировать поле'
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
  mounted() {},
  created() {
    this.initialize()
  },
  methods: {
    changeShowLangSelect() {
      this.showLangSelect = true
    },
    changeShowTable() {
      this.showTable = true
    },
    initialize() {
      this.itemTable = [
        {
          name: 'Физика',
          description: 'Наука о природе'
        },
        {
          name: 'Математика',
          description:
            'Фундаментальная наука, предоставляющая (общие) языковые средства другим наукам'
        }
      ]
    },

    editItem(item) {
      this.editedIndex = this.itemTable.indexOf(item)
      this.editedItem = Object.assign({}, item)
      this.dialog = true
    },

    deleteItem(item) {
      this.editedIndex = this.itemTable.indexOf(item)
      this.editedItem = Object.assign({}, item)
      this.dialogDelete = true
    },

    deleteItemConfirm() {
      this.itemTable.splice(this.editedIndex, 1)
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
        Object.assign(this.itemTable[this.editedIndex], this.editedItem)
      } else {
        this.itemTable.push(this.editedItem)
      }
      this.close()
    }
  }
}
</script>
