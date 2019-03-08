# Vue

## Instance

    var vm = new Vue({
        el: '#app-id',
        data: data_dic,

        beforeCreated: function () {},
        created, beforeMount, mounted,
        beforeUpdate, updated, beforeDestroy,
        destroyed,

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
            fn: function () {
                get: function () {
                    return this.msg
                },
                set: function (val) {
                    do some set things with val.
                }
            }
        },
        methods:{},
        watched:{
            watchedVal: function (newVal, oldVal){
                do something
            },
        },

    })

- `computed`

    will re-compute depends on `data_dic`.
    Vue will automatic check if `data` has been used in this funtion.
    Excute if used `data` changes.

    eg. `Date.now()` always returns same even if `data_dic` changes.
    But `this.msg + Date.now()` will change when `this.msg` changes.

    no-need of `brace`: `()` in Template.

    default: only `get` function. Use `vm.setFunction = val` to pass val.

- `methods`

    `no-cache` verison of `computed`.

    If rendered in html, excutes everytime `data_dic` changes.

    `{{ functionMethod() }}`: `brace`: `()` is NECESSARY!

    > Vue calls re-render when: 
    >
    > 1.directly rendered data changes;
    >
    > 2.rendered data which used in methods or computed changes;
    >
    > Difference between computed and methods is that, methods re-excutes when re-render called.

- `watched`

    Mostly computed is much-better than this.
    `watched` is used in asynchronous or expensive operations to change data.

    eg. use `lodash:_.debounce` to wait some time before `funcMethods()`.


## Class/Style

    <div class="static"
        :class="{ class1: bool1, 'class2': bool2 }"
        :class="classObject"
        :class="[class1, class2]"
        :class="[{ class1: bool1 }, class2]"

        :style="{ color: dColor, fontSize: dFontSize + 'px' }"
        :style="{ styleObject }"
        :style="[baseStyle, overRidingStyle]"
    >
    </div>

- `classObject`

    A dic of class.
    Mostly returned in computed.

- list of classes

    `class1, class2` should all be vals of data.


## `v-if` Conditional Rendering

    <li v-if="ok">yes</li>
    <li v-else-if="name === 'Dora'">
        Dora found v-if can use expression
    </li>
    <li v-else>No</li>

    <template v-if="ok">
        showed html
    </template>

- `key`
    Vue re-uses components to render faster.
    
    Set key as attributes will let v-else-if separate values.

- `v-show`

    will render DOM and hide it with `display`

- `v-for`
    `v-for` has higher priority than `v-if`


## `v-for` List Rendering

    <ul id="ul-1">
        <li v-for="item in item_list">
        //or
        //<li v-for="(item,index) in item_list">
            {{ item.msg }}
        </li>
    </ul>

    <ul id="ul-2">
        <li v-for="value in item_obj">
        //or
        //<li v-for="(value,key) in item_obj">
        //<li v-for="(value,key,index) in item_obj">
            {{ value }} {{ key }} {{ index }}
        </li>
    </ul>

- `key`

    it's strongly recommended to set a `key` attributes for each `<li>`.

- Mutation Caveats

    push, pop, shift, unshift, splice, sort, reverse

    `filter, concat, slice` return new array

    For item lists:

    `Vue.set(vm.itemList, indexOfItem, newValue)` or
    `Vue.itemList.splice(indexOfItem, 1, newValue)` or
    `vm.$set(vm.itemList, indexOfItem, newValue)`

    For Object:

    `Vue.set(vm.obj, 'key', value)` or
    `vm.$set(vm.obj, 'key', value)` or

        vm.obj = Object.assign({}, vm.obj, {
            'key1': value1,
            'key2': value2
        })


## Event Handling

    <button @click="funcName or expression">
        Button
    </button>

- `funcName` or `expression`, no brace needed

    inline `expression`:

    - `say('hey')` can pass value

    - `warn('warnning', $event)`

        pass default event. can be used to prevent default event.

- Event Modifiers

    Common to call `event.preventDefault` or `event.stopPropagation()`.

    `@click.stop="doThis"` will stop default Event

    `.prevent .capture .self .once .passive`

-`key`

    - `@keyup.enter .tab .delete .esc .space .up .down .left .right` or other keyCode

    - `@keyup.page-down`

    - `@keyup.alt.67` or `@click.ctrl` systemd modifier keys.

    - `@click.ctrl.exact` no other keys should be pressed

    - `@click.left .right .middle`


## Form Input Bindings

- `v-model="var"` uses var to binding data of input

    - `text textarea` -- text input or area, message is the value

    - `checkbox` id is very important, returns bool type data

            <input type="checkbox" id="checkboxId" v-model="checked">
            <label for="checkboxId"> {{checked}}</label>

        can set `true-value="yes" false-value="no"` for input attributes.  Radio may sometimes be better.

    - `radio`

    - `select` it's import to set disabled select for iOS

            <select v-model="selected" (multiple)>
            <option disabled value="">Please select one</option>
            <option>A</option>
            <option>B</option>
            <option>C</option>
            </select>
            <span>Selected: {{ selected }}</span>

        option can also bind value instead of what inside `<option>` tag. Used as

            <option :value="{ key: 123 }">123</option>

- `.lazy` sync only after `change` event

        <input v-model.lazy="msg">

- `v-model.number` `v-model.trim`


## Components

    Vue.component('componentName',{
        props: ['prop1', 'prop2'],
        data: fuction () {
            return data_dic
        },
        template: '<html template>{{ prop1}}'
    })

    new Vue({ el:'#components-parent'})

    <div id="components-parent">
        <componentName></componentName>
    </div>

- components are like Vue Instances except for `el`. one more instance is created everytime a component used.

- `data` must be function returning `data_dic`, or every component uses one `data_dic`.

- `props` is used to passing data to template

        <componentName prop1="prop1 things"></componentName>

    now it's possible to use `v-for itemList ` to bind values of item in itemList.

    if there are many props, a `prop_dic1` shoule be used to pass values to component.

        Vue.component('componentName', {
        props: ['prop_dic1'],
        template: `
        <div class="some class">
          <h3>{{ prop_dic1.key1 }}</h3>
          <div v-html="prop_dic1.key2"></div>
        </div>
        `
        })

- `$emit('funcName')` can be used to send a event.

    - `funcName` must be written braced as string.

    - `v-on:funcName="methodFunc"` or `@funcName="methodFunc"`

    inline expression supported

    - `$emit('funcName', val)` pass params to function.

- `input` should bind

        Vue.component('custom-input', {
        props: ['value'],
        template: `
        <input
            v-bind:value="value"
            v-on:input="$emit('input', $event.target.value)"
        >
        `
        })

- `slot`

        Vue.component('alert-box', {
        template: `
        <div class="demo-alert-box">
        <strong>Error!</strong>
        <slot></slot>
        </div>
        `
        })

        <!-- it's will work as below -->
        <alert-box>
          Something bad happened.
        </alert-box>

- `v-bind:is="componentName"` is used to switch different components

# Vue suggested pack:

## axios
- get

        axios.get(url)
        .then(function(response){
            do something with response
        })
        .catch(function (error) {
            do something with error
        })


## lodash
-`_debounce(funcName, time)`

    execute function `funcName`, after `time` milliseconds.
    If recalled under `time` millisecs, new funcName called.

- `_.capitalize([string=''])`

    Converts the first character of string to upper case and the remaining to lower case.


## element-ui


# Vue-cli

## npm
`npm`为js包管理器，安装（安装npm后需要先更新）：

    sudo apt install npm
    sudo npm install npm -g
    sudo npm install -g cnpm --registry=https://registry.npm.taobao.org

之后使用cnpm安装使用淘宝镜像，会快很多。
