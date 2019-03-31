# ES6 学习记录

使用webpack后不需要加引号

## let
在块级作用域外不可访问

    {
        var i = 1
        let j = 2
    }
    // j 在外部不可访问，i在外部可访问
    // let 不可以重复声明， var可以

## const

    const a=1
    const obj = {a:1}
    // a 不可修改， obj.a可以修改


## 解构赋值

    {
        let a=1, b = 2
        [a, b] = [4, 5]
        // 给 a, b 分别赋值
        [a, b] = [b, a] //换值

        let a, b
        {{a, b} = {b: 1, a: 2}}
        // 类似python函数传参

        [a, b] = '321'
        // a=3, b=2
    }


## rest 参数

    function func(a, b, ...c){
        console.log(a)
        console.log(b)
        console.log(c)
    }

    func(1, 2, 3, 4, 5, 6, {a: 1} 7, 8)
    // a = 1, b = 2, c为剩下的Array
    // 类似python的字典参数

    let a, b
    [a, ...b] = [1, 2, 3, 4, 5, 6]
    [a, ...b] = '1232142142'
    // b永远为Array型数据


## 字符串
模板

    let age = 16
    let name = 'nisha'
    let str = `my name is ${name}, age ${age}`
    // 省去加号组合，很方便

字符串判断

    let str = 'nisha'
    str.includes('ish')
    // 返回true或者false
    str.startsWith('nish')
    str.endsWith('sha')
    
字符串复制

    let str = 'a'
    str.repeat(10)

padStart padEnd


## 对象
    let a = 1, b = 2
    let obj = {a, b}
    // obj表达为 {a:1, b:2}

    let str= 'name'
    let obj = {
        sayHello(){
            // 相当于 sayHello:function()
            console.log('hello')
        },
        [str]: 'nisha'
    }
    obj.sayHello()
    // 相当于定义成员函数
    obj.name
    // [str] 相当于以该值作为key名


## 箭头函数
箭头函数作用域绑定在当前代码处

    let a = num => num * 2
    // num为参数， a为函数名

    let add = (n1, n2) => {
        return n1 + n2
    }

    setTimeOut(() => {}, 10000)


## 数值

    Number.isFinite(123)
    // true 判断是否有限，NaN返回false

    Number.isNaN(NaN)
    // true 判断是否为NaN

    Number.isInteger(123)
    // true 是否为整数

    Number.MAX_SAFE_INTEGER
    Number.MIN_SAFE_INTEGER
    Number.isSafeInteger(12)
    
    Math.trunc(7.7)
    // 7 返回整数部分

    Math.cbrt(8)
    // 2 立方根

    Math.sign(2)
    // 1 返回正负 1  0 -1


