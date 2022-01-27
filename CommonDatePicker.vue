<template>
  <div>
    <date-picker
      v-model="datePickerValue"
      :lang="datePickerLanguage"
      :format="datePickerFormat"
      v-bind="$attrs"
    />
    <vuelidate-feedback v-if="vuelidate" :validation="value"/>
  </div>
</template>

<script>
  import DatePicker from 'vue2-datepicker';
  import moment from 'moment';
  import VuelidateFeedback from '@/components/VuelidateFeedback';
  import {languages} from '@/config/i18n/i18n';

  export default {
    components: {
      DatePicker,
      VuelidateFeedback
    },
    props: {
      value: {},
      format: String,
      vuelidate: Boolean
    },
    computed: {
      datePickerValue: {
        get() {
          return moment(this.vuelidate ? this.value.$model : this.value, 'YYYY-MM-DD').toDate();
        },
        set($event) {
          const value = $event ? moment($event).format('YYYY-MM-DD') : null;
          if (this.vuelidate) {
            this.value.$model = value;
          } else {
            this.$emit('input', value);
          }
        }
      },
      datePickerLanguage() {
        return require(`vue2-datepicker/locale/${this.$i18n.locale}`);
      },
      datePickerFormat() {
        const format = this.format ?? languages[this.$i18n.locale].datepicker.format;
        return {
          stringify: date => date ? moment(date).locale(this.$i18n.locale).format(format) : '',
          parse: value => value ? moment(value, format, this.$i18n.locale).toDate() : null
        };
      }
    }
  };
</script>

<style scoped lang="scss">
  @import '~@/config/bootstrap-vue-variables';

  ::v-deep {
    .mx-input {
      font-size: $input-font-size;
      line-height: $input-line-height;
      height: $input-height;
      padding-top: $input-padding-y;
      padding-bottom: $input-padding-y;
      border: $input-border-width solid $input-border-color;
      border-radius: $input-border-radius;
      box-shadow: none;
    }
  }
</style>
