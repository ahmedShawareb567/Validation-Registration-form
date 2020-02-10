<template>
  <div>
    <section class="addingItemsSec">
      <div class="container">
        <h3 v-if="registerHeading">Register</h3>
        <div class="row">
          <div class="col-2"></div>
          <div class="col-8">
            <form role="form" action method="get">
              <div class="firstRegister" v-if="firstReg">
                <input type="text" placeholder="First Name" v-model="firstname" ref="firstname" />
                <input
                  type="text"
                  placeholder="Last Name"
                  v-model="lastname"
                  ref="lastname"
                  @keyup.13="nextstep()"
                />
              </div>
              <div class="firstRegister" v-if="secondReg">
                <input type="text" placeholder="User Name" v-model="username" ref="username" />
                <input type="text" placeholder="Email" v-model="email" ref="email" />
                <div class="passSec">
                  <input type="password" placeholder="Password" v-model="password" ref="password" />
                  <i :class="faElement" @click="showPassword()"></i>
                </div>
                <input
                  type="password"
                  placeholder="Confirm Password"
                  v-model="confirmpassword"
                  ref="confpassword"
                  @keyup.13="submitfunc()"
                />
              </div>
            </form>
            <div class="registerSec" v-if="successSec">
              <p>Congratulations ?! For Registration</p>
            </div>
            <button v-if="nextShow" @click="nextstep()" ref="nextbutton" :disabled="beDisabled">Next</button>
            <button v-if="prevShow" ref="prevButton" @click="prevStep()">
              Prev
              <i class="fa fa-chevron-right"></i>
            </button>
            <p
              @click="submitfunc()"
              class="submitCl"
              v-if="submitShow"
              :disabled="beDisabled"
            >submit</p>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>
<script>
export default {
  created() {},
  mounted() {},
  updated() {},
  data() {
    return {
      firstReg: true,
      secondReg: false,
      nextShow: true,
      prevShow: false,
      submitShow: false,
      successSec: false,
      faElement: "fa fa-eye-slash",
      registerHeading: true,
      firstname: "",
      lastname: "",
      username: "",
      email: "",
      password: "",
      confirmpassword: "",
      beDisabled: false,
      errorArr: []
    };
  },
  watch: {
    firstname() {
      this.validateFunc(this.firstname, this.$refs.firstname);
    },
    lastname() {
      this.validateFunc(this.lastname, this.$refs.lastname);
    },
    username() {
      this.validateFunc(this.username, this.$refs.username);
    },
    email() {
      this.validateEmailFunc();
    },
    password() {
      this.validateFunc(this.password, this.$refs.password);
    },
    confirmpassword() {
      if (this.confirmpassword != this.password || this.confirmpassword == "") {
        this.errorArr[0] = "Required !!";
        this.$refs.confpassword.classList.add("errorClass");
      } else {
        this.errorArr.splice(0, 1);
        this.$refs.confpassword.classList.remove("errorClass");
      }
    },
    errorArr() {
      if (this.errorArr.length > 0) {
        this.beDisabled = true;
      } else {
        this.beDisabled = false;
      }
    }
  },
  methods: {
    nextstep() {
      this.validateFunc(this.firstname, this.$refs.firstname);
      this.validateFunc(this.lastname, this.$refs.lastname);
      if (this.errorArr.length == 0) {
        this.beDisabled = false;
        this.firstReg = false;
        this.secondReg = true;
        this.prevShow = true;
        this.nextShow = false;
        this.submitShow = true;
      } else {
        this.beDisabled = true;
      }
    },
    prevStep() {
      this.firstReg = true;
      this.secondReg = false;
      this.prevShow = false;
      this.nextShow = true;
      this.submitShow = false;
      this.errorArr.splice(0);
    },
    validateFunc(elmentWat, elmento) {
      if (elmentWat == "") {
        this.errorArr.push("Required !!");
        elmento.classList.add("errorClass");
      } else {
        this.errorArr.pop();
        elmento.classList.remove("errorClass");
      }
    },
    submitfunc() {
      this.validateFunc(this.username, this.$refs.username);
      this.validateFunc(this.password, this.$refs.password);
      this.validateEmailFunc();
      if (this.confirmpassword != this.password || this.confirmpassword == "") {
        this.errorArr[0] = "Required !!";
        this.$refs.confpassword.classList.add("errorClass");
      } else {
        this.errorArr.splice(0, 1);
        this.$refs.confpassword.classList.remove("errorClass");
      }
      if (this.errorArr.length == 0) {
        this.firstReg = false;
        this.secondReg = false;
        this.successSec = true;
        this.prevShow = false;
        this.nextShow = false;
        this.submitShow = false;
        this.beDisabled = false;
        this.registerHeading = false;
      } else {
        this.beDisabled = true;
      }
    },
    validateEmailFunc() {
      var tester = /^[-!#$%&'*+/0-9=?A-Z^_a-z`{|}~](.?[-!#$%&'*+/0-9=?A-Z^_a-z`{|}~])*@[a-zA-Z0-9](-*.?[a-zA-Z0-9])*.[a-zA-Z](-?[a-zA-Z0-9])+$/;
      let valid = tester.test(this.email);
      if (!valid || this.email == "") {
        this.errorArr.push("Required !!");
        this.$refs.email.classList.add("errorClass");
      } else {
        this.errorArr.splice(0, 1);
        this.$refs.email.classList.remove("errorClass");
      }
    },
    showPassword() {
      if (this.$refs.password.getAttribute("type") == "password") {
        this.$refs.password.setAttribute("type", "text");
        this.faElement = "fa fa-eye";
      } else {
        this.$refs.password.setAttribute("type", "password");
        this.faElement = "fa fa-eye-slash";
      }
    }
  }
};
</script>
