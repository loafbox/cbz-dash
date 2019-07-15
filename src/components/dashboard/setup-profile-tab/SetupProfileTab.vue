<template>
  <div class="setup-profile-tab dashboard-tab">
    <vuestic-wizard :steps="steps" wizard-layout="vertical" :wizard-type="wizardType">
      <div slot="page1" class="form-wizard-tab-content">
        <h2>Welcome!</h2>
        <p
          class="form-subtitle"
        >This is your personal business dashboard. Let's begin by setting up a profile for your business.</p>
        <div
          :class="{'has-error': errors.has('name'), 'valid': isFormFieldValid('name')}"
          class="md-layout-item md-small-size-100"
        >
          <md-field class="md-size-100">
            <label for="name">Enter Business Name</label>
            <md-input
              v-validate="'required'"
              class="md-input-border"
              name="name"
              id="name"
              v-model="name"
            />
          </md-field>
          <small v-show="errors.has('name')" class="help text-danger">{{ errors.first('name') }}</small>
        </div>
      </div>
      <div slot="page2" class="form-wizard-tab-content">
        <h2>{{'forms.profile.location' | translate}}</h2>
        <p class="form-subtitle">{{'forms.profile.locationSubtitle' | translate}}</p>
        <form novalidate class="md-layout">
          <md-card class="md-layout-item md-size-100 md-small-size-100">
            <md-card-content>
              <div class="md-layout md-gutter">
                <div class="md-layout-item md-small-size-100">
                  <md-field>
                    <label for="street">Street Address</label>
                    <md-input
                      v-validate="'required'"
                      class="md-input-border"
                      name="street"
                      id="street"
                      v-model="street"
                    />
                  </md-field>
                  <small
                    v-show="errors.has('street')"
                    class="help text-danger"
                  >{{ errors.first('street') }}</small>
                </div>
              </div>
              <div class="md-layout md-gutter">
                <div class="md-layout-item md-size-100 md-small-size-100">
                  <md-field>
                    <label for="city">City</label>
                    <md-input
                      v-validate="'required'"
                      name="city"
                      id="city"
                      class="md-input-border"
                      v-model="city"
                    />
                  </md-field>
                  <small
                    v-show="errors.has('city')"
                    class="help text-danger"
                  >{{ errors.first('city') }}</small>
                </div>

                <div class="md-layout-item md-size-40 md-small-size-100">
                  <md-field>
                    <label for="state">State</label>
                    <md-select
                      v-validate="'required'"
                      name="state"
                      id="state"
                      v-model="selectedState"
                      md-dense
                    >
                      <md-option
                        v-for="state in statesList"
                        :value="state.name"
                        :key="state.id"
                      >{{state.name}}</md-option>
                    </md-select>
                  </md-field>
                  <small
                    v-show="errors.has('state')"
                    class="help text-danger"
                  >{{ errors.first('state') }}</small>
                </div>

                <div class="md-layout-item md-size-60 md-small-size-100">
                  <md-field>
                    <label for="state">Country</label>
                    <md-select name="country" id="country" v-model="selectedCountry" md-dense>
                      <md-option
                        v-for="country in countriesList"
                        :value="country.name"
                        :key="country.id"
                      >{{country.name}}</md-option>
                    </md-select>
                  </md-field>
                </div>
              </div>
            </md-card-content>
          </md-card>
        </form>
      </div>
      <div slot="page3" class="form-wizard-tab-content">
        <h4>Confirm selection</h4>
        <p>
          Zebras communicate with facial expressions and sounds. They make loud
          braying or barking sounds and
          soft snorting sounds. The position of their ears, how wide open their
          eyes are, and whether they show
          their teeth all send a signal. For example, ears flat back means
          trouble, or "you better follow orders!"
        </p>
      </div>
      <div slot="wizardCompleted" class="form-wizard-tab-content wizard-completed-tab">
        <h4>Wizard completed!</h4>
        <p>
          Zebras communicate with facial expressions and sounds. They make loud
          braying or barking sounds and
          soft snorting sounds. The position of their ears, how wide open their
          eyes are, and whether they show
          their teeth all send a signal. For example, ears flat back means
          trouble, or "you better follow orders!"
        </p>
      </div>
    </vuestic-wizard>
  </div>
</template>

<script>
import AddressForm from "./components/AddressForm";
import csc from "country-state-city";

const CountriesList = csc.getAllCountries();
const UnitedStates = csc.getCountryById("231");
const StatesList = csc.getStatesOfCountry("231");

export default {
  name: "setup-profile-tab",
  components: {
    AddressForm
  },
  props: {
    wizardType: {
      default: "rich"
    }
  },
  data() {
    return {
      steps: [
        {
          label: "Step 1. Welcome",
          slot: "page1",
          onNext: () => {
            this.validateFormField("name");
          },
          isValid: () => {
            return this.isFormFieldValid("name");
          }
        },
        {
          label: "Step 2. Location",
          slot: "page2",
          onNext: () => {
            this.validateFormField("street");
            this.validateFormField("city");
            this.validateFormField("state");
          },
          isValid: () => {
            return this.validateAllFields(["street", "city", "state"]);
          }
        },
        {
          label: "Step 3. Keywords",
          slot: "page3"
        }
      ],
      name: "",
      street: "",
      city: "",
      selectedState: "",
      statesList: StatesList,
      selectedCountry: UnitedStates.name,
      countriesList: CountriesList
    };
  },
  methods: {
    isFormFieldValid(field) {
      let isValid = false;
      if (this.formFields[field]) {
        isValid =
          this.formFields[field].validated && this.formFields[field].valid;
      }
      return isValid;
    },
    validateFormField(fieldName) {
      this.$validator.validate(fieldName, this[fieldName]);
    },
    validateAllFields(fields) {
      return fields.every(field => {
        console.log(field);
        return this.formFields[field].validated && this.formFields[field].valid;
      });
    }
  }
};
</script>

<style lang="scss" scoped>
.form-group {
  min-width: 200px;
  max-width: 360px;
  width: 80%;
}

.form-subtitle {
  font-size: 18px;
  font-weight: 300;
  text-align: center;
}

.wizard-completed-tab {
  @include media-breakpoint-up(md) {
    margin-top: -$tab-content-pt;
  }
}

.md-field .md-input {
  border-bottom-width: 2px;
  border-bottom-color: $cbz-light-blue;
  border-bottom-style: inset;
}

.md-list-item {
  background-color: $white;
}

.has-error label {
  color: red;
}
</style>
