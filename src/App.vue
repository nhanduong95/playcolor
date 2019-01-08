<template>
  <div>
    <form @submit.prevent="generateTint(hexColor)">
      <input v-model.trim="hexColor" class="form-control" type="text" />
      <button class="btn btn-large btn-primary">Generate Tint</button>
    </form>

    <tint-generation :rgb-color="convertedRGB"></tint-generation>
  </div>
</template>

<script>
    import TintGeneration from './components/TintGeneration';
    import Constant from './utils/_constants'
    import Helper from './utils/_helpers'

    export default {
        name: 'App',
        data: function() {
            return ({
                hexColor: '#000000',
                rgbColor: {
                    r: 0,
                    g: 0,
                    b: 0
                }
            })
        },
        computed: {
            convertedRGB: function () {
                return this.rgbColor;
            }
        },
        methods: {
            generateTint: function () {
                var processedData = this.stripHashChar(this.hexColor);
                if (this.checkInputLength(processedData)) {
                    this.rgbColor = Object.assign({}, Helper.parseToRGB(processedData));
                    if (this.rgbColor) {
                        Constant.EVENT_BUS.$emit('generateTint');
                    }
                } else {
                    this.setDefaultColor()
                }
            },
            stripHashChar: function (str) {
                return str.indexOf('#') === 0 ? str.substring(1) : str;
            },
            checkInputLength: function (str) {
                return str.length === 2 || str.length === 3 || str.length === 6;
            },
            setDefaultColor: function () {
                this.hexColor = '#000000';
                this.rgbColor = {
                    r: 0,
                    g: 0,
                    b: 0
                }
            }
        },
        components: {
            TintGeneration
        }
    };
</script>
