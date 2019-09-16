<template>
    <select>
        <slot ></slot>
    </select>
</template>

<script>
    export default {
        props: ['options', 'value', 'match'],
        data(){
          return {
              source: this.options
          }
        },
        mounted: function () {
            let vm = this;
            $(this.$el)
                .select2({data: this.options})
                .val(this.value)
                .trigger('change')
                .on('change', function () {
                    vm.$emit('input', this.value)
                });
        },
        watch: {
            match: function (source) {
                let op = this.options;
                let pattern = new RegExp('^' + source, 'i' );
                if(source) {
                    for (let i = 0; i < op.length; i++) {
                        let str = op[i]['name'];
                        let result = str.search(pattern);
                        if (result != -1) {
                            $(this.$el).val(op[i]['id']).trigger('change');
                            return true;
                        }
                    }
                }
            },
            value: function (value) {
                $(this.$el)
                    .val(value)
                    .trigger('change')
            },
            options: function (options) {
                $(this.$el).empty().select2({data: options})
            }
        },
        destroyed: function () {
            $(this.$el).off().select2('destroy')
        }
    }
</script>

