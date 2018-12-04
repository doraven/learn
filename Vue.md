# Vue

## Instance
    
    var vm = new Vue({
        el: '#app-id',
        data: data_dic,

        beforeCreated: function () {},
        created: function () {},
        beforeMount: function () {},
        mounted: function () {},
        beforeUpdate: function () {},
        updated: function () {},
        beforeDestroy: function () {},
        destroyed: function () {},

        computed: {
            functionName1: function () {
                return this.msg
            },
            functionName2: function () {
                return this.msg.split('').reverse().join('')
            },
        },
        methods:{},
        watched:{},

    })


- `el` is short for `element`

- `data_dic` is global variable, view re-renders when it changes.

- `Object.freeze(data_dic)` stops tracking change of `data_dic`.

- Don't use `this` in arrow function.


## Template
    
    <span
        v-once
        v-html="rawHtml"

        v-bind:id="spanId"    
        :id="spanId"
        :disabled="isDisabled"

        v-if="seen"
        v-on:click="myFunc"
        @click="myFunc"
    >
    {{ msg }}
    {{ num + 1 }}
    {{ ok ? 'Yes' : 'No' }}
    {{ msg.split('').reverse().join('') }}
    
    <span>

- `v-once`
    
    will (alternative) stops every bind inside `<span>` after first render.

- `v-html=`
    
    will replace inside of `<span>` with `rawHtml` of `vm.data.rawHtml`.
    It can cause XSS vulnerabilities.

- `v-bind:`
    
    will bind attributes of `<span>` with `val` of `vm.data.val`. 
    
    Short for `:`. 

    `isDisabled` can be `bool` type to deside whether included this attr.

- expression
    
    `{{ var a = 1 }}` won't work since it's actually two expressions.

- `v-if=`
    
    will deside whether show this `<span>` by `bool` of `seen`, `seen` could 
    also be expression.

- `v-on:`

    will bind DOM event `click` to function `vm.methods.myFunc`. 

    short for `@`

    `v-on:submit.prevent="onSubmit"` will prevent default event, change it to 
    `vm.methods.onSubmit`


## Methods
    
    var vm = new Vue({
        el: 'app',
        data: data_dic,

        computed: {
            functionName1: function () {
                return this.msg
            },
            functionName2: function () {
                return this.msg.split('').reverse().join('')
            },
        },
        methods:{},
        watched:{},

    })

- `computed`
    
    will update depends on `data_dic`. Thus won't re-compute if no change 
    found in `data_dic`.
    
    eg. `Date.now()` will always return same value if `data_dic` not changed.

    no-need of `brace`: `()` in Template. 

- `methods`

    `no-cache` verison of `computed`. Excutes everytime called.

- `watched`



# Vue-cli

## npm
`npm`为js包管理器，安装（安装npm后需要先更新）：

    sudo apt install npm
    sudo npm install npm -g
    sudo npm install -g cnpm --registry=https://registry.npm.taobao.org

之后使用cnpm安装使用淘宝镜像，会快很多。
