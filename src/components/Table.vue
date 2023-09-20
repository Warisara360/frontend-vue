<template>
    <div>
    <v-data-table
      :headers="headers"
      :items="employeeItem"
      sort-by="calories"
      class="elevation-1"
    >
      <template v-slot:top>
        <v-toolbar
          flat
        >
          <v-toolbar-title>จัดการข้อมูล</v-toolbar-title>
          <v-divider
            class="mx-4"
            inset
            vertical
            
          ></v-divider>
          <v-spacer></v-spacer>
          <v-btn
                color="primary"
                dark
                class="mb-2"
                @click="openDialog('add',defaultItem)"
              >
                เพิ่มข้อมูล
              </v-btn>
        </v-toolbar>
      </template>

      <template v-slot:[`item.role`]="{ item }">
        {{ item.role.name }}
      </template>
      <template v-slot:[`item.actions`]="{ item }">


        <v-icon
          small
          class="mr-2"
          @click="openDialog('edit',item)"
        >
          mdi-pencil
        </v-icon>
        <v-icon
          small
          @click="deleteItem(item)"
        >
          mdi-delete
        </v-icon>
      </template>
      <template v-slot:no-data>
        <v-btn
          color="primary"
          @click="initialize"
        >
          Reset
        </v-btn>
      </template>
    </v-data-table>
    <v-dialog
            v-model="dialog"
            max-width="500px"
          >
            <v-card>
              <v-card-title>
                <span class="text-h5">{{ formTitle }}</span>
              </v-card-title>
  
              <v-card-text>
                <v-container>
                  <v-row>
                    <v-col
                      cols="12"
                      sm="6"
                      md="6"
                    >
                      <v-text-field
                        v-model="fristname "
                        label="ชื่อ"
                      ></v-text-field>
                    </v-col>
                    <v-col
                      cols="12"
                      sm="6"
                      md="6"
                    >
                      <v-text-field
                        v-model="lastname"
                        label="นามสกุล"
                      ></v-text-field>
                    </v-col>
                    <v-col
                      cols="12"
                      sm="6"
                      md="6"
                    >
                      <v-text-field
                        v-model="saraly"
                        label="เงินเดือน"
                      ></v-text-field>
                    </v-col>
                    <v-col
                      cols="12"
                      sm="6"
                      md="6"
                    >
                      <v-text-field
                        v-model="roles"
                        label="ตำแหน่ง"
                      ></v-text-field>
                    </v-col>
                  
                  </v-row>
                </v-container>
              </v-card-text>
  
              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn
                  color="blue darken-1"
                  text
                  @click="close"
                >
                  Cancel
                </v-btn>
                <v-btn
                  color="blue darken-1"
                  text
                  @click="save(formTitle)"
                >
                  บันทึกข้อมูล
                </v-btn>
              </v-card-actions>
            </v-card>
          </v-dialog>
          <v-dialog v-model="dialogDelete" max-width="500px">
            <v-card>
              <v-card-title>
                <span class="text-h5">ลบข้อมูล</span>
              </v-card-title>
              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn color="blue darken-1" text @click="closeDelete">ยกเลิก</v-btn>
                <v-btn color="blue darken-1" text @click="deleteItemConfirm">ตกลง</v-btn>
                <v-spacer></v-spacer>
              </v-card-actions>
            </v-card>
          </v-dialog>
  </div>
  </template>


  <script>
  export default {
    data: () => ({
      fristname : '',
      lastname : '',
      saraly: '',
      roles: '',



      dialog: false,
      dialogDelete: false,
      headers: [
        {
          text: 'ไอดี',
          align: 'start',
          sortable: false,
          value: 'id',
        },
        { text: 'ชื่อ', value: 'fristName' },
        { text: 'นามสกุล', value: 'lastName' },
        { text: 'เงินเดือน', value: 'saraly' },
        { text: 'ตำแหน่ง', value: 'role' },
        { text: 'Actions', value: 'actions', sortable: false },
      ],
      employeeItem: [],
      editedIndex: -1,
      editedItem: {
        name: '',
        calories: 0,
        fat: 0,
        carbs: 0,
        protein: 0,
      },
      dialog : false,
      defaultItem: {
        name: '',
        calories: 0,
        fat: 0,
        carbs: 0,
        protein: 0,
      },
      formTitle:'',
      idEmployee:'',
      idForDelete:''


    }),

    computed: {
      formTitle () {
        return this.editedIndex === -1 ? 'New Item' : 'Edit Item'
      },
    },

    watch: {
      dialog (val) {
        val || this.close()
      },
      dialogDelete (val) {
        val || this.closeDelete()
      },
    },

    created () {
      this.initialize()
    },

    methods: {
      async initialize() {
        this.employeeItem =[]
        try {
          var data = await this.axios.get('http://localhost:9000/employee')
          console.log('data employee ====> ',data)
          this.employeeItem = data.data
          
        } catch (error) {
          
        }
        
      }
    ,
    openDialog (Action, item){
    
    this.formTitle = "";
    if (Action === 'add'){
        this.formTitle = 'เพิ่มข้อมูล'
        this.editItem = item
        this.dialog = true
    }else{
        //console.log(Action,item);
        this.dialog = true;
        //this.editedIndex = this.desserts.indexOf(item);
        this.formTitle = 'แก้ไขข้อมูล'
        //this.editedItem = item;
        this.fristname = item.fristName
        this.lastname = item.lastName
        this.saraly = item.saraly
        this.roles = item.roles
        this.idEmployee = item.id
    }
    },


      editItem (item) {
        // console.log('item select',item)
        // console.log('item item',this.desserts.index(item))
       this.editedIndex = this.desserts.indexOf(item)
       this.editedItem = Object.assign({}, item)
       this.dialog = true
      },

      deleteItem (item) {
        //this.editedIndex = this.desserts.indexOf(item)
        //this.editedItem = item
        this.idForDelete = item.id
        this.dialogDelete = true
      },
      
      async deleteItemConfirm(){
        try {
          var response = await this.axios.delete('http://localhost:9000/employee/' + this.idForDelete)
              //console.log('dataResponse ===>',dataResponse)
              this.initialize()
        } catch (error) {
          console.log(error.message)
        }
        this.closeDelete()
      },

      close () {
        this.dialogCreate = false
        this.editedItem = []
        this.defaultItem = {
        
        fristname : '',
        lastname : '',
        saraly : '',
        roles :''
        }
        this.$nextTick(() => {
          this.editedItem = Object.assign({}, this.defaultItem)
          this.editedIndex = -1
        })
      },

      closeDelete () {
        this.dialogDelete = false
        this.editedItem = []
        this.editedIndex = -1
        
      },

      close () {
    
        this.dialog = false
        this.fristname = '',
        this.lastname = '',
        this.saraly = '',
        this.roles = ''
      },

      async save (action) {
        var data = {
        fristName : this.fristname,
        lastName : this.lastname,
        saraly : this.saraly,
        roles : {
        name : this.roles
        },
        skills :[
        {skills:''}
        ]
        }
        if ( action === 'เพิ่มข้อมูล' ){
            //this.desserts.push(this.editedItem);
            //this.dialog = false
            //this.close();
            console.log('data after send ===>',data)
            try {
              var dataResponse = await this.axios.post('http://localhost:9000/employee',data)
              //console.log('dataResponse ===>',dataResponse)
              this.close()
              this.initialize()
            } catch (error) {
              console.log(error.message)
            }

        } else{
            try { 
              var dataResponse = await this.axios.put('http://localhost:9000/employee/' + this.idEmployee ,data)
              //console.log('dataResponse ===>',dataResponse)
              this.close()
              this.initialize()
            } catch (error) {
              console.log(error.message)
            }
        }
        this.close()      
      },
    }
  }
</script>