<template>
  <div id="employee-form">
    <form class="my-div form-container" @submit.prevent="handleSubmit">
      <label>Employee Name</label>
      <input
        class="my-div inputbox"
        ref="first"
        type="text"
        :class="{ 'has-error': submitting && isNameValid }"
        v-model="employee.name"
        @focus="clearStatus"
        @keypress="clearStatus"
      />
      <label class="my-div">Employee Email</label>
      <input
        class="my-div inputbox"
        type="text"
        :class="{ 'has-error': submitting && isEmailValid }"
        v-model="employee.email"
        @focus="clearStatus"
      />
      <!-- <p v-if="error && submitting" class="error-message">
        ❗Please fill out all required fields and valid email address
      </p> -->
      <label class="my-div">Employee Age</label>
      <input
        class="my-div inputbox"
        type="text"
        :class="{ 'has-error': submitting && isAgeValid }"
        v-model="employee.age"
        @focus="clearStatus"
      />
      <p v-if="error && submitting" class="error-message">
        ❗Please fill out all required fields and valid email address
      </p>
      <p v-if="success" class="success-message">
        ✅ Employee successfully added
      </p>
      <button class="button-width my-div">Add Employee</button>
    </form>
  </div>
</template>

<script>
export default {
  name: "employee-form",
  data() {
    return {
      submitting: false,
      error: false,
      success: false,
      employee: {
        name: "",
        email: "",
        reg: /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,24}))$/,
        age:null,
      },
    };
  },
  methods: {
    handleSubmit() {
      this.submitting = true;
      this.clearStatus();

      if (this.isNameValid || this.isEmailValid) {
        this.error = true;
        return;
      }

      this.$emit("add:employee", this.employee);
      this.$refs.first.focus();
      this.employee = {
        name: "",
        email: "",
        reg: /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,24}))$/,
      };
      this.error = false;
      this.success = true;
      this.submitting = false;
    },

    clearStatus() {
      this.success = false;
      this.error = false;
    },
  },
  computed: {
    isNameValid() {
      return this.employee.name === "";
    },
    isEmailValid() {
      return this.employee.email === ""
        ? true
        : this.employee.reg.test(this.employee.email)
        ? false
        : true;
    },
    isAgeValid() {
      const age = parseInt(this.age);
      return !isNaN(age) && age >= 18 && age <= 120;
    }
  },
};
</script>

<style scoped>
form {
  margin-bottom: 2rem;
}

[class*="-message"] {
  font-weight: 500;
}

.error-message {
  color: #d33c40;
}

.success-message {
  color: #32a95d;
}


.my-div {
  /* margin: 10px;
  padding: 10px; */
  margin-top: 10px;
  /* padding-top: 10px; */
}

.form-container {
  display: flex;
  flex-direction: column;
}

.button-width {
  width: 100px;
}

.inputbox {
  height: 40px;
  border-radius: 5px;
  /* border-width: 1px; */
  /* border-color:azure; */
  box-shadow: none;
  border: 1px solid #ccc;
}

.inputbox:hover{
    border-color: #444;
}
</style>