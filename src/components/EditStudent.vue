<template>
  <div>
    <v-flex sm8 offset-sm2>
      <v-card>
        <v-toolbar dark>
          <v-toolbar-title>Edit Student</v-toolbar-title>
        </v-toolbar>
        <v-container class="text-xs-center">
          <v-progress-circular
            v-if="!$store.getters.isLoaded"
            :size="70"
            :width="7"
            color="purple"
            indeterminate
          ></v-progress-circular>
        </v-container>
        <v-form v-if="$store.getters.isLoaded">
          <!-- v-if="isLoaded" -->
          <v-container>
            <v-layout>
              <v-flex xs12 md4>
                <v-text-field
                  @input="updateFirstName"
                  :value="findStudents(this.$route.params.id).firstName"
                  label="First Name"
                  required
                ></v-text-field>
                <v-text-field
                  @input="updatelastName"
                  :value="findStudents(this.$route.params.id).lastName"
                  label="Last Name"
                  required
                ></v-text-field>
              </v-flex>
            </v-layout>
          </v-container>
          <v-btn @click="submit">submit</v-btn>
        </v-form>
      </v-card>
    </v-flex>
    <br />
    <Students />
  </div>
</template>

<script>
import axios from "axios";
import Students from "./Students";
import { mapGetters } from "vuex";

export default {
  data() {
    return {
      firstName: "",
      lastName: "",
    };
  },
  computed: {
    student() {
      return this.$store.getters.findStudents(this.$route.params.id);
    },
    ...mapGetters([
      "findStudents", //another way for mapGetter
    ]),
  },
  methods: {
    async submit() {
      const student = this.findStudents(this.$route.params.id);
      const firstName = this.firstName || student.firstName;
      const lastName = this.lastName || student.lastName;

      this.$store.dispatch("editStudent", {
        id: this.$route.params.id,
        names: { firstName, lastName },
      });
      // axios.put(`http://localhost:3000/students/${this.$route.params.id}`, {
      //   firstName: this.student.firstName,
      //   lastName: this.student.lastName,
      // });
    },
    updatelastName(value) {
      this.lastName = value;
    },
    updateFirstName(value) {
      this.firstName = value;
    },
  },
  components: {
    Students,
  },
};
</script>