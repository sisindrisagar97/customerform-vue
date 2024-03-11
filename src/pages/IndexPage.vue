<template>
  <q-page>

    <div class="row justify-evenly">
      <div class="col-md-5 col-sm-12 col-xs-12">
      <q-card class="q-pa-md q-ma-sm q-mt-xl ">
        <q-card-section>
        <div class="text-h6">Customer Form</div>
      </q-card-section>
      <form @submit="saveData">
        <q-input
        ref="name"
          v-model="formData.name"
          label="Name"
          outlined
          dense
          placeholder="Enter Name"
          lazy-rules
          :rules="[val => !!val || 'Name is required']"
        />
        <q-input
        ref="employeeID"
          v-model="formData.employeeID"
          outlined
          dense
          type="number"
          label="Employee ID"
          placeholder="Enter Employee ID"
          lazy-rules
          :rules="[val => !!val || 'Employee ID is required']"
        />
        <q-select
        ref="city"
          v-model="formData.city"
          outlined
          dense
          :options="cityOptions"
          label="City"
          lazy-rules
          :rules="[
            val => val !== null && val !== '' || 'Please select your city',
          ]"
        />
        <div class="">
          <label>Gender</label><br/>
      <q-radio v-model="formData.gender" class="q-ma-0" checked-icon="task_alt" unchecked-icon="panorama_fish_eye" val="male" label="Male" required />
      <q-radio v-model="formData.gender" class="q-ma-0" checked-icon="task_alt" unchecked-icon="panorama_fish_eye" val="female" label="Female" required/>
      </div>

        <q-btn type="submit" class="full-width " color="primary" label="Save" />
      </form>
      </q-card>
      </div>
        <div class="col-md-5 col-sm-12 col-xs-12">
          <q-card class="q-pa-md q-ma-sm q-mt-xl col-6" v-if="rows.length > 0">
      <q-card-section>
        <!-- <q-table
          title="Customer Data "
          :rows="rows"
          :columns="columns"
          row-key="name"
        >
        <template v-slot:body-cell-actions="props">
             <q-td :props="props" >
              <q-btn round size="sm" class="q-mr-xs" color="primary" @click="edit(props)" icon="edit" />
              <q-btn round size="sm" color="red" icon="delete" @click="remove(props)"/>
            </q-td>
         </template>
        </q-table> -->
          <q-list bordered separator v-for="(value) in rows" :key="key">
            <q-item  v-ripple>
          <q-item-section  >
            <div class="row" >

            <div class="col-md-10 col-sm-8 col-xs-8">
              <strong>Customer Details</strong>
            </div>
            <div class="col-md-2 col-sm-4 col-xs-4 text--right">
               <q-btn round size="sm" class="q-mr-xs" color="primary" @click="edit(value)" icon="edit" />
               <q-btn round size="sm" class="q-mr-xs" color="red" @click="remove('props')" icon="delete" />
            </div>
            </div>
          </q-item-section>
        </q-item>
        <q-item  v-ripple>
          <q-item-section>Name: {{ value.name }}</q-item-section>
        </q-item>
        <q-item  v-ripple>
          <q-item-section>Employee ID: {{ value.employeeID }}</q-item-section>
        </q-item>
        <q-item  v-ripple>
          <q-item-section>City: {{ value.city }}</q-item-section>
        </q-item>
        <q-item  v-ripple>
          <q-item-section>Gender: {{ value.gender }}</q-item-section>
        </q-item>
      </q-list>

      </q-card-section>
    </q-card>
        </div>
      </div>
      <div class="row justify-center">

    </div>
      </q-page>


</template>

<script>
export default {
  data() {
    return {
      formData: {
        name: '',
        employeeID: null,
        city: null,
        gender: 'male'
      },
      columns :[
  { name: 'name', label: 'Name', field: 'name', sortable: true },
  { name: 'employeeID', label: 'Employee ID', field: 'employeeID', sortable: true },
  { name: 'city', label: 'City', field: 'city', sortable: true },
  { name: 'gender', label: 'Gender', field: 'gender', sortable: true },
  { name: 'actions', label: 'Actions', field: 'actions', sortable: true },

],
  rows:[],
      cityOptions: ['New York', 'London', 'Tokyo'],
      genderOptions: ['Male', 'Female'],
      savedData: null,
      localdata:null,
    };
  },
  mounted() {
    this.rows = [];
    this.localdata = JSON.parse(localStorage.getItem('customerData'));
    console.log(this.localdata,"hvbjhvhj");
    if(this.localdata){
    this.rows.push(this.localdata);
    }
  },
  methods: {
    remove(){
      console.log('remove');
      localStorage.removeItem("customerData");
      this.rows =[];
    },
    edit(val){
      console.log(val.name,"edit");
      this.formData.name = val.name;
      this.formData.employeeID = val.employeeID;
      this.formData.city = val.city;
      this.formData.gender = val.gender;
    },
    saveData() {
      this.rows =[];
      this.$refs.name.validate();
      this.$refs.employeeID.validate();
      this.$refs.city.validate();

      if ( this.$refs.name.hasError || this.$refs.employeeID.hasError || this.$refs.city.hasError) {
       return this.formHasError = true;
      }
      localStorage.setItem('customerData', JSON.stringify(this.formData));
      let data = this.formData;
      this.rows.push(data);
      this.formData = {name: '',
        employeeID: null,
        city: null,
        gender: 'male'};
    }
  }
};
</script>
