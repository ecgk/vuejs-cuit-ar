<template>
     <input
      type="text"
      ref="cuitInput"
      :value="value"
      @input="update()"
      :class="{'is-invalid':!isValid}"
      v-mask="'##-########-##'"
      placeholder="00-00000000-0"
      maxlength="13"
     />
</template>

<script>

export default {

    data() {
        return {
        };
    },

    props: {

        value: {
            type: String,
            required: false,
            default: ''
        },

    },

    mounted() {

    },

    computed: {
        isValid() {
            var valid = true;
            if ( this.value !== undefined && this.value != '' ) {
                var fiscalNumber = this.value;
                fiscalNumber = fiscalNumber.replace(/-/g,"");
                valid = false;
                if( fiscalNumber.length != 11 ) {
                    return false;
                }
                var accumulated = 0;
                var digits = fiscalNumber.split("");
                var digit = digits.pop();
                for( var i = 0; i < digits.length; i++ ) {
                    accumulated += digits[9 - i] * (2 + (i % 6));
                }
                var verif = 11 - ( accumulated % 11 );
                if ( verif == 11 ) {
                    verif = 0;
                }
                valid = digit == verif;
            }
            return valid;
        }
    },

    methods: {

        update() {
            this.$emit('input', this.$refs.cuitInput.value);
        },

    },
};
</script>
