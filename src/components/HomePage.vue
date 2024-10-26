<template>
  <v-container>
    <v-row class="d-flex justify-center px-9">
      <v-col cols="12" md="10" lg="7" class="body my-10 px-9">
        <v-col class="pa-0">
          <div class="heading my-4">Contact Us</div>
        </v-col>
        <v-col class="d-flex flex-wrap pa-0">
          <v-col cols="12" sm="6" class="pa-0 pr-0 pr-sm-2">
            <label class="text">First Name <span class="star">*</span></label>
            <input
              type="text"
              v-model="firstName"
              :class="[errors.firstName ? 'input-field-error' : 'input-field']"
            />
            <div class="error-message" v-if="errors.firstName">
              This field is required
            </div>
          </v-col>
          <v-col cols="12" sm="6" class="pa-0 pl-0 pl-sm-2">
            <label class="text">Last Name <span class="star">*</span></label>
            <input
              type="text"
              v-model="lastName"
              :class="[errors.lastName ? 'input-field-error' : 'input-field']"
            />
            <div class="error-message" v-if="errors.lastName">
              This field is required
            </div>
          </v-col>
        </v-col>
        <v-col cols="12" class="pa-0 my-5">
          <label class="text">Email Address <span class="star">*</span></label>
          <input
            type="email"
            v-model="email"
            :class="[errors.email ? 'input-field-error' : 'input-field']"
          />
          <div class="error-message" v-if="errors.email">
            Please enter a valid email address
          </div>
        </v-col>
        <v-col class="pa-0">
          <v-col cols="12" class="pa-0">
            <label class="text">Query Type<span class="star">*</span></label>
          </v-col>

          <v-col class="d-flex flex-wrap pa-0 mt-3">
            <!-- Radio Button 1 -->
            <v-col cols="12" sm="6" class="pa-0 pr-0 pr-sm-2 mb-sm-0 mb-5">
              <div
                class="radio-box"
                :class="{ 'selected-input': queryType === 'General Enquiry' }"
              >
                <input
                  type="radio"
                  id="general"
                  value="General Enquiry"
                  v-model="queryType"
                  class="custom-radio"
                />
                <label
                  for="general"
                  class="radio-label text d-flex align-center pl-3"
                >
                  <span
                    class="radio-circle1"
                    :class="{
                      'checked-circle': queryType === 'General Enquiry',
                    }"
                  ></span>
                  General Enquiry
                </label>
              </div>
            </v-col>

            <!-- Radio Button 2 -->
            <v-col cols="12" sm="6" class="pa-0 pl-0 pl-sm-2">
              <div
                class="radio-box"
                :class="{ 'selected-input': queryType === 'Support Request' }"
              >
                <input
                  type="radio"
                  id="support"
                  value="Support Request"
                  v-model="queryType"
                  class="custom-radio"
                />
                <label
                  for="support"
                  class="radio-label text d-flex align-center pl-3"
                >
                  <span
                    class="radio-circle"
                    :class="{
                      'checked-circle': queryType === 'Support Request',
                    }"
                  ></span>
                  Support Request
                </label>
              </div>
            </v-col>
          </v-col>

          <v-col cols="12">
            <div class="error-message" v-if="errors.queryType">
              Please select a query type
            </div>
          </v-col>
        </v-col>

        <v-col cols="12" class="pa-0">
          <label class="text">Message<span class="star">*</span></label>
          <textarea
            v-model="message"
            class="text textarea-field"
            :class="[errors.message ? 'input-field-error' : 'input-field']"
            rows="3"
          ></textarea>
          <div class="error-message" v-if="errors.message">
            This field is required
          </div>
        </v-col>
        <v-col cols="12" class="pa-0 my-5">
          <div class="checkbox-box">
            <input
              type="checkbox"
              id="consent"
              v-model="consent"
              class="custom-checkbox"
            />
            <label for="consent" class="checkbox-label text">
              <span class="checkbox-square"></span>
              I consent to being contacted by the team<span class="star"
                >*</span
              >
            </label>
            <div class="error-message" v-if="errors.consent">
              To submit this form, please consent to being contacted
            </div>
          </div>
        </v-col>
        <v-col cols="12" class="pa-0">
          <v-btn
            block
            color=""
            class="text-capitalize btn mb-6 py-6"
            @click="submitForm"
          >
            Submit
          </v-btn>
        </v-col>
        <v-dialog
          class="dialog"
          v-model="showDialog"
          :scrim="false"
          width="398"
          :persistent="false"
        >
          <v-col class="body2 px-5 py-5">
            <div class="heading2 d-flex align-start">
              <div class="img20 mr-2 d-flex align-center">
                <img src="/src/assets/icon-success-check.svg" alt="" />
              </div>
              <div class="text20 mb-2">Message Sent!</div>
            </div>
            <div class="para20">
              Thanks for completing the form. We'll be in touch soon!
            </div>
          </v-col>
        </v-dialog>
      </v-col>
    </v-row>
  </v-container>
</template>
  
  <script setup>
import { ref } from "vue";

const firstName = ref("");
const lastName = ref("");
const email = ref("");
const queryType = ref("");
const message = ref("");
const consent = ref(false);
const showDialog = ref(false);

const errors = ref({
  firstName: false,
  lastName: false,
  email: false,
  queryType: false,
  message: false,
  consent: false,
});

const validateEmail = (email) => /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email);

const resetForm = () => {
  firstName.value = "";
  lastName.value = "";
  email.value = "";
  queryType.value = "";
  message.value = "";
  consent.value = false;
};

const submitForm = () => {
  // Reset errors
  errors.value = {
    firstName: !firstName.value,
    lastName: !lastName.value,
    email: !validateEmail(email.value),
    queryType: !queryType.value,
    message: !message.value,
    consent: !consent.value,
  };

  // If all fields are valid, show the confirmation dialog and reset form
  if (Object.values(errors.value).every((err) => !err)) {
    showDialog.value = true;
    resetForm();
  }
};
</script>
  
  <style scoped>
@font-face {
  font-family: f1;
  src: url(/src/assets/Karla-Bold.ttf);
}
@font-face {
  font-family: f2;
  src: url(/src/assets/Karla-Regular.ttf);
}
.body {
  background-color: #ffffff;
  border-radius: 15px;
}
.input-field {
  width: 100%;
  padding: 10px;
  margin-top: 4px;
  border: 1.5px solid hsl(170, 3%, 62%);
  border-radius: 7px;
  font-size: 16px;
  color: hsl(200, 7%, 25%);
  padding-left: 20px;
  padding-right: 20px;
}
.input-field-error {
  width: 100%;
  padding: 10px;
  margin-top: 4px;
  border: 1.5px solid hsl(0, 53%, 50%);
  border-radius: 7px;
  font-size: 16px;
  color: hsl(200, 7%, 25%);
  padding-left: 20px;
  padding-right: 20px;
}
.input-field:hover {
  border: 1.5px solid hsl(169, 82%, 27%);
  cursor: pointer;
}
.input-field-error:hover {
  border: 1.5px solid hsl(169, 82%, 27%);
  cursor: pointer;
}
.radio-box {
  border: 1.5px solid hsl(170, 3%, 62%);
  border-radius: 10px;
  padding: 10px;
  display: flex;
  align-items: center;
  gap: 10px;
  cursor: pointer;
}

.error-message {
  color: hsl(0, 53%, 50%);
  font-size: 13px;
  margin-top: 6px;
  font-weight: 400;
}
/* Hide the native radio input */
.custom-radio {
  display: none;
}

/* Custom radio circle */
.radio-circle {
  display: inline-block;
  width: 18px;
  height: 18px;
  border: 2px solid hsl(30, 2%, 82%);
  border-radius: 50%;
  position: relative;
  margin-right: 8px;
  cursor: pointer;
  transition: 0.3s;
}
.radio-circle1 {
  display: inline-block;
  width: 18px;
  height: 18px;
  border: 2px solid hsl(30, 2%, 82%);
  border-radius: 50%;
  position: relative;
  margin-right: 8px;
  cursor: pointer;
  transition: 0.3s;
}

/* Hollow circle turns red when checked */
.checked-circle {
  border-color: hsl(169, 71%, 30%);
}

.radio-circle::after {
  content: "";
  position: absolute;
  top: 50%;
  left: 50%;
  width: 10px;
  height: 10px;
  border-radius: 50%;
  transform: translate(-48%, -53%);
  transition: background-color 0.3s;
}
.radio-circle1::after {
  content: "";
  position: absolute;
  top: 50%;
  left: 50%;
  width: 10px;
  height: 10px;
  border-radius: 50%;
  transform: translate(-48%, -54%);
  transition: background-color 0.3s;
}

/* Filled circle changes color */
input:checked + .radio-label .radio-circle::after {
  background-color: hsl(169, 82%, 27%);
}
input:checked + .radio-label .radio-circle1::after {
  background-color: hsl(169, 82%, 27%);
}
.radio-label {
  cursor: pointer;
}
/* Background and border color changes for selected input */
.selected-input {
  background-color: hsl(145, 38%, 91%);
  border: 1.5px solid hsl(169, 82%, 27%);
  border-radius: 7px;
  padding: 10px;
}

.radio-label {
  padding-top: 0.75px;
  padding-bottom: 0.75px;
}

/* Hide the native checkbox input */
.custom-checkbox {
  display: none;
}

/* Unchecked state: Red hollow square */
.checkbox-square {
  display: inline-block;
  width: 15px;
  height: 15px;
  border: 2px solid hsl(30, 2%, 82%);
  position: relative;
  margin-right: 8px;
  cursor: pointer;
  transition: background-color 0.3s;
  top: 3px;
}

/* Checked state: Green filled square with white checkmark */
input:checked + .checkbox-label .checkbox-square {
  background: url("/src/assets/icon-checkbox-check.svg") no-repeat center center;
  background-size: cover;
  scale: 1.3;
  border: 2px solid transparent;
}
/* Disable resize for textarea */
.textarea-field {
  resize: none; /* Prevent resizing */
  overflow: hidden; /* Avoid scrollbars */
  min-height: 95px; /* Set a default minimum height */
  max-height: 300px; /* Optional: Set a max height */
}

/* Textarea will expand on input */
.textarea-field:focus {
  height: auto;
}
/* //////////////////////////////////////////////////////////////// */
.heading {
  color: hsl(188, 21%, 14%);
  font-size: 28px;
  font-family: f1;
}
.text {
  font-size: 14px;
  color: hsl(200, 7%, 25%);
  font-family: f2;
}
.star {
  color: hsl(169, 82%, 27%);
  margin-left: 4px;
  font-weight: bold;
}
.checkbox-box {
  margin-top: 25px;
  margin-bottom: 34px;
}
.btn {
  background-color: hsl(169, 82%, 27%);
}
.btn:hover {
  background-color: hsl(171, 83%, 14%);
}
.body2 {
  background-color: hsl(185, 24%, 22%);
  border-radius: 10px;
}

.img20 {
  scale: 0.86;
}
.text20 {
  font-size: 14px;
}
.para20 {
  color: hsl(185, 14%, 64%);
  font-size: 14px;
  letter-spacing: 0.25px;
}
.dialog {
  position: fixed;
  top: -500px; /* Adjust this value as needed */
  z-index: 10; /* Ensure it appears above other elements */
}
/* ////////////////////////////////////////////////////////////////////// */
/* For screens between 599.5px and 959.5px */
@media (min-width: 599.5px) and (max-width: 959.5px) {
  /* Styles for tablets or medium screens */
  .radio-circle::after {
    transform: translate(-54%, -53%);
  }
  .radio-circle1::after {
    transform: translate(-54%, -54%);
  }
  .textarea-field {
    min-height: 180px; /* Set a default minimum height */
  }
}

/* For screens below 599.5px */
@media (max-width: 599.5px) {
  .radio-circle::after {
    transform: translate(-54%, -53%);
  }
  .radio-circle1::after {
    transform: translate(-54%, -54%);
  }
  .textarea-field {
    min-height: 215px; /* Set a default minimum height */
  }
  .dialog {
    top: -480px; /* Adjust this value as needed */
  }
}
</style>
  