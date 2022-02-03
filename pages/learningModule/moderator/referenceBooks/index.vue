<template>
  <v-container>
    <v-row>
      <v-col cols="12" md="6">
        <v-select
          v-if="showMainSelect"
          v-model="mainSelect"
          :items="mainItem"
          item-value="key"
          item-text="name"
          label="Раздел"
          @change="changeShowLangSelect"
        ></v-select>
      </v-col>
      <v-col cols="12" md="6">
        <v-select
          v-if="showLangSelect"
          v-model="langSelect"
          :items="langItem"
          item-value="key"
          item-text="name"
          label="Язык"
          @change="changeShowTable"
        ></v-select>
      </v-col>
      <v-col v-if="showChangeSelectsBtn">
        <v-btn color="blue darken-1" text @click="changeMainAndLang">
          Поменять раздел и язык
        </v-btn>
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
          <v-spacer></v-spacer>
          <v-dialog v-model="dialog" max-width="80%">
            <template v-slot:activator="{ on, attrs }">
              <v-btn
                color="primary"
                dark
                class="mb-2"
                v-bind="attrs"
                v-on="on"
                @click="addItem"
              >
                Добавить поле
              </v-btn>
            </template>
            <!-- добавить или изменить поле -->
            <v-card height="700px">
              <v-card-title>
                <span class="text-h5">{{ formTitle }}</span>
              </v-card-title>

              <v-card-text>
                <v-container>
                  <v-row>
                    <v-col cols="12" md="6">
                      <v-text-field
                        v-model="editedItem.name"
                        label="Название"
                      ></v-text-field>
                    </v-col>
                    <v-col cols="12" md="6">
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
                    <v-col cols="12" md="6">
                      <v-select
                        v-if="showItemLinks"
                        v-model="editedItem.links"
                        :items="itemLinks"
                        attach
                        chips
                        label="Связи"
                        multiple
                      ></v-select>
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
          <!-- подтверджение удаления -->
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
    </v-data-table>
  </v-container>
</template>

<script>
import { mapGetters } from 'vuex'
export default {
  name: 'LearningModule',
  components: {},
  data() {
    return {
      mainSelect: '',
      mainItem: [
        {
          key: 'category',
          name: 'Категории'
        },
        {
          key: 'subject',
          name: 'Предмет'
        },
        {
          key: 'themes',
          name: 'Темы'
        },
        {
          key: 'subthemes',
          name: 'Подтемы'
        },
        {
          key: 'university',
          name: 'Университет'
        },
        {
          key: 'faculties',
          name: 'Факультет'
        }
      ],
      showMainSelect: true,
      itemLinks: [],
      showLangSelect: false,
      showTable: false,
      showChangeSelectsBtn: false,
      showItemLinks: true,
      langSelect: '',
      langItem: [
        {
          key: 'ru',
          name: 'Русский'
        },
        {
          key: 'en',
          name: 'Английский'
        },
        {
          key: 'he',
          name: 'Ифрит'
        }
      ],
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
        { text: 'Связи', value: 'links' },
        { text: 'Измененить', value: 'actions', sortable: false }
      ],
      itemTable: [],
      editedIndex: -1,
      editedItem: {
        name: '',
        description: '',
        links: ''
      },
      defaultItem: {
        name: '',
        description: '',
        links: ''
      },
      comparisonArray: {
        category: '',
        subject: 'category',
        themes: 'subject'
      }
    }
  },
  computed: {
    ...mapGetters(['getListData']),
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
  methods: {
    changeShowLangSelect() {
      this.showLangSelect = true
    },
    changeMainAndLang() {
      this.mainSelect = null
      this.langSelect = null
      this.showMainSelect = true
      this.showChangeSelectsBtn = false
      this.showTable = false
    },
    async changeShowTable() {
      const obj = { ln: this.langSelect, name: this.mainSelect }
      await this.$store.dispatch('GET_LIST', obj)
      this.itemTable = this.getListData
      console.log(this.getListData)
      this.showMainSelect = false
      this.showLangSelect = false
      this.showChangeSelectsBtn = true
      this.showTable = true
    },
    async addItem() {
      if (this.mainSelect !== 'category') {
        this.showItemLinks = true
        const obj = {
          ln: this.langSelect,
          name: this.comparisonArray[this.mainSelect]
        }
        await this.$store.dispatch('GET_LIST', obj)
        this.itemLinks = this.getListData.map(x => x.name)
      } else {
        this.showItemLinks = false
      }
      this.dialog = true
    },
    async editItem(item) {
      this.editedIndex = this.itemTable.indexOf(item)
      this.editedItem = Object.assign({}, item)
      if (this.mainSelect !== 'category') {
        this.showItemLinks = true
        const obj = {
          ln: this.langSelect,
          name: this.comparisonArray[this.mainSelect]
        }
        await this.$store.dispatch('GET_LIST', obj)
        this.itemLinks = this.getListData.map(x => x.name)
      } else {
        this.showItemLinks = false
      }
      this.dialog = true
    },

    deleteItem(item) {
      this.editedIndex = this.itemTable.indexOf(item)
      this.editedItem = Object.assign({}, item)
      this.dialogDelete = true
    },

    async deleteItemConfirm() {
      const data = {
        id: this.editedItem._id,
        list: this.mainSelect,
        ln: this.langSelect
      }
      await this.$store.dispatch('DELETE_LIST', data)

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

    async save() {
      if (this.editedIndex > -1) {
        // редактирование
        const data = {
          id: this.editedItem._id,
          list: this.mainSelect,
          language: this.langSelect,
          name: this.editedItem.name,
          description: this.editedItem.description,
          links: this.editedItem.links
        }
        await this.$store.dispatch('PUT_LIST', data)
        Object.assign(this.itemTable[this.editedIndex], this.getListData)
      } else {
        // добавление
        const data = {
          list: this.mainSelect,
          language: this.langSelect,
          name: this.editedItem.name,
          description: this.editedItem.description,
          links: this.editedItem.links
        }
        const obj = { ln: this.langSelect, name: this.mainSelect }
        await this.$store.dispatch('GET_LIST', obj)
        await this.$store.dispatch('POST_LIST', data)
        this.itemTable = [...this.getListData]
      }
      this.close()
    }
  }
}
</script>
