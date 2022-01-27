<template>
  <div>
    <b-form-input
      id="example-input"
      v-model="timepickerValue"
      type="text"
      placeholder="HH:mm"
    ></b-form-input>
    <b-input-group-append>
    <b-form-timepicker
      v-model="timepickerValue"
      :readonly="readonly || areaAuthorizer.readOnly"
      :state="vuelidate ? toVuelidateState(value) : null"
      :locale="$i18n.locale"
      v-bind="$attrs"
      reset-button
      button-only
    />
    </b-input-group-append>
    <vuelidate-feedback v-if="vuelidate" :validation="value"/>
  </div>
</template>
<script>
  import AreaAuthorizerMixin from '@/components/AreaAuthorizerMixin';
  import VuelidateFeedback from '@/components/VuelidateFeedback';
  import utils from '@/utils/utils';
  export default {
    inheritAttrs: false,
    components: {
      VuelidateFeedback
    },
    mixins: [
      AreaAuthorizerMixin
    ],
    props: {
      value: [String, Object],
      vuelidate: Boolean,
      readonly: Boolean
    },
    computed: {
      timepickerValue: {
        get() {
          return this.vuelidate ? this.value.$model : this.value;
        },
        set(value) {
          if (this.vuelidate) {
            this.value.$model = value;
          } else {
            this.$emit('input', value);
          }
        }
      }
    },
    methods: {
      toVuelidateState: utils.toVuelidateState
    }
  };
</script>
