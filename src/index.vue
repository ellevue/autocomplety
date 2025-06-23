<!-- 
    Permite usar el componente v-autocomplete de vuetify
 -->
<template>

    <!-- TODO: Pegar los dos inputs por temas de ux -->

    <v-autocomplete :rules="rules" v-model="selectedValue" :label="$attrs.label" ref="autocompleteMenu"
        :items="other ? (position === 'start' ? [other, ...items] : [...items, other]) : items" :variant="variant"
        v-bind="$attrs" autocomplete="off">

        <template v-slot:no-data v-if="other">
            <span text @click="onNoDataClick" class="pl-4 py-2 cursor-pointer">{{ noDataText }} {{
                other.toLowerCase() }}</span>
        </template>

    </v-autocomplete>

    <v-text-field v-show="showInput" :rules="rules" v-model="value" :variant="variant"
        :label="otherLabel" autocomplete="off"></v-text-field>
</template>
<script>
import isEmpty from 'is-empty';

export default {
    name: 'v-autocomplete-extended',

    props: {
        rules: {
            type: Array,
            default: () => []
        },

        modelValue: {
            type: String
        },

        variant: {
            type: String
        },

        items: {
            type: Array,
            default: () => []
        },

        other: {
            type: String,
        },

        position: {
            type: String,
            default: 'end',
            validator: (value) => ['start', 'end'].includes(value)
        },

        noDataText: {
            type: String,
            default: 'Clic para especificar'
        },

        otherLabel: {
            type: String,
            default: 'Otra opción (especifique)'
        },
    },

    mounted() {
        this.selectedValue = this.modelValue
    },

    data() {
        return {
            selectedValue: null,
            value: "",
            showInput: false,
        }
    },

    methods: {

        onNoDataClick() {
            this.showInput = true

            this.$refs.autocompleteMenu.menu = false;

            this.selectedValue = this.other
            this.$emit('update:modelValue', this.other);

            // Close the menu
            this.$nextTick(() => {
                const inputElement = this.$refs.autocompleteMenu.$el.querySelector('input');
                if (inputElement) {
                    inputElement.blur();
                }
            });
        }
    },

    watch: {

        modelValue(newValue, oldValue) {
            if (oldValue == undefined && newValue != undefined && isEmpty(this.other)) {
                this.selectedValue = newValue
            }

        },

        async selectedValue(newValue) {
            if (isEmpty(newValue)) {
                return
            }
            if (!isEmpty(this.other) && newValue == this.other) {
                this.value = "";
                this.showInput = true
            } else {
                this.value = newValue
                this.showInput = false
            }
        },

        value(newValue) {
            this.$emit('update:modelValue', newValue);
        },
    },
}
</script>