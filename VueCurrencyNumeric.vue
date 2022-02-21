<template>
    <input type="text" v-model="displayValue" @blur="isInputActive = false" @focus="isInputActive = true"/>
</template>

<script>

export default ({
    name: 'VueCurrencyNumeric',
    props: ['value', "fixednumber", 'prefix', 'type', 'max', 'min'],
    data: function() {
        return {
            isInputActive: false,
            fixedLength: 0
        }
    },
    mounted(){
        this.fixedLength = this.fixednumber;
    },
    computed: {
        displayValue: {
            get: function() {
                if (this.isInputActive) {
                    if(isNaN(this.value) || this.value == null)
                        return '';

                    console.log('fixednumber');
                    console.log(this.fixednumber);
                    return this.value == 0 ? '0' : this.value;
                } else {
                    // this.fixedLength = 2;
                    let prefix = '$ ';
                    if(this.fixednumber != undefined)
                        this.fixedLength = this.fixednumber;

                    if(this.prefix != undefined)
                        prefix = this.prefix + ' ';

                    if(this.value == 0 || this.value == undefined || isNaN(this.value) || this.value == null) {
                        return prefix + this._number_format('0', this.fixedLength);
                    }
                    
                    let val = 0;
                    if (this.min != undefined && this.value < this.min) {
                        val = this.min;
                    }

                    if (this.max != undefined && this.value > this.max) {
                        val = this.max;
                    }

                    // this.val = this.value.toFixed(this.fixednumber)

                    val = this.value.toString().replace(/^-/, '');
                    return prefix + this._number_format(val, this.fixedLength);
                }
            },
            set: function(modifiedValue) {
                if (modifiedValue == 0 || modifiedValue == undefined || isNaN(modifiedValue) || modifiedValue < 0) {
                    modifiedValue = 0
                }

                console.log('set');
                console.log(modifiedValue);
                let m = this._number_format(modifiedValue, this.fixedLength);
                let n = m.replace(/,|\s/g, '');

                this.$emit('input', n);
            },
        },
    },
    methods: {
        calcTotal: function(e) {

        },
        keymonitor: function(e) {
            if(e.keyCode == 9 || e.keyCode == 13)
                $(e.target).select()
        },
        setValue: function() {

        }
    },
    watch: {
        setFocus: function(e) {
            $(e.target).select();
        }
    }
})
</script>

