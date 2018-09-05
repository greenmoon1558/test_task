<template>
  <v-container fluid>
    <v-layout row wrap align-center>
        <v-flex v-if="errored" md12 lg6
        offset-lg3>
          <v-card color="red" class="white--text">
            <v-card-title primary-title>
              <div>
                <h3 class="headline mb-0">Error</h3>
                <div>No data</div>
                <div>{{error}}</div>
              </div>
            </v-card-title>
          </v-card>
        </v-flex>
        <v-flex 
         v-else
         md12 lg6 offset-lg3>
          <v-text-field
            label="Name"
            v-model="nameFilter"
            color="indigo"
          ></v-text-field>
          <br>
          <v-text-field
            label="Lastname"
            v-model="lastnameFilter"
            color="indigo"
          ></v-text-field>
          <br/>
          <v-text-field
            label="Age"
            v-model="ageFilter"
            color="indigo"
          ></v-text-field>
          <br/>
          <v-checkbox
            label="Male"
            v-model="IsMale"
            color="indigo"
            lg3
          ></v-checkbox>
          <v-checkbox
            label="Female"
            v-model="IsFemale"
            color="indigo"
            lg3
          ></v-checkbox>
          <div v-for="(user, index) in filteredUsers" :key="index">
            <v-card  color="indigo" dark> 
              <v-card-title primary-title>
                <div>
                  <h3 class="headline mb-0">{{user.name}} {{user.lastname}}</h3>
                  <div>{{user.age}} {{user.sex == "m"? "male": "female"}}</div>
                </div>
              </v-card-title>
            </v-card>
            <br/>
          </div>
        </v-flex>
      </v-layout>

  </v-container>
</template>

<script>
import axios from 'axios';

export default {
  name: 'FilteredContainer',
   data () {
    return {
      users: [],
      error: "",
      nameFilter: "",
      lastnameFilter: "",
      ageFilter: "",
      IsMale: true,
      IsFemale: true,
      errored: false
    }
  },
 mounted() {
      axios.get(`https://venbest-test.herokuapp.com/`)
      .then(response => (this.users = response.data))
      .catch(error => {
        this.error = error.message;
        this.errored = true;
      })
  },
  computed: {
    filteredUsers: function () {
        return this.users.filter((user) =>(this.nameFilter?user.name.match(this.nameFilter): user))
        .filter((user) =>(this.lastnameFilter?user.lastname.match(this.lastnameFilter):user))
        .filter((user) =>(this.ageFilter?user.age == this.ageFilter:user))
        .filter((user) =>{
          let sex = [];
          if(this.IsMale) sex.push("m")
          if(this.IsFemale) sex.push("f")
          return sex.some(s=> s.match(user.sex));
          });
      }
  }
}

</script>