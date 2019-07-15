<template>
  <div>
    <form novalidate class="md-layout" @submit.prevent="validateUser">
      <md-card class="md-layout-item md-size-100 md-small-size-100">
        <md-card-content>
          <div class="md-layout md-gutter">
            <div class="md-layout-item md-small-size-100">
              <md-field>
                <label for="street-name">Street Address</label>
                <md-input class="md-input-border" name="street-name" id="street-name" />
                <hr />
              </md-field>
            </div>
          </div>
          <div class="md-layout md-gutter">
            <div class="md-layout-item md-small-size-100">
              <md-field>
                <label for="first-name">City</label>
                <md-input name="first-name" id="first-name" />
              </md-field>
            </div>

            <div class="md-layout-item md-small-size-100">
              <md-field>
                <label for="first-name">State</label>
                <md-input name="first-name" id="first-name" />
              </md-field>
            </div>
          </div>
        </md-card-content>
      </md-card>
    </form>
  </div>
</template>

<script>
import CountriesList from "data/CountriesList";

export default {
  name: "address-form",
  computed: {
    datePickerDisabled: () => [date => !(date.getDate() % 5)],
    isSuccessfulEmailValid() {
      let isValid = false;
      if (this.formFields.successfulEmail) {
        isValid =
          this.formFields.successfulEmail.validated &&
          this.formFields.successfulEmail.valid;
      }
      return isValid;
    }
  },
  data() {
    return {
      isMale: true,
      countriesList: CountriesList,
      chosenCountry: "",
      clearableText: "",
      successfulEmail: "andrei@dreamsupport.io",
      wrongEmail: "andrei@dreamsupport",
      simpleOptions: [
        {
          id: 1,
          description: "First option"
        },
        {
          id: 2,
          description: "Second option"
        },
        {
          id: 3,
          description: "Third option"
        }
      ],
      simpleSelectModel: "",
      multiSelectModel: [],
      multiSelectCountriesModel: [],
      radioSelectedOption: "option1",
      radioSelectedDisableOption: "option1",
      checkbox: {
        unselected: false,
        selected: true,
        readonly: true,
        disabled: true,
        error: false,
        errorMessages: true
      },
      errorMessages: ["error message 1", "error message 2"],
      datepicker: {
        simple: "2018-05-09",
        time: "2018-05-08 14:10",
        range: "2018-05-08 to 2018-05-23",
        disabled: "2018-05-09",
        multiple: "2018-04-25, 2018-04-27",
        customFirstDay: "2018-05-09",
        customDate: "2018-05-09"
      }
    };
  },
  methods: {
    clear(field) {
      this[field] = "";
    }
  },
  created() {
    this.$nextTick(() => {
      this.$validator.validateAll();
    });
  }
};
</script>

<style scoped>
.md-field .md-input {
  border-bottom-width: 2px;
  border-bottom-color: $cbz-light-blue;
  border-bottom-style: inset;
}
</style>
