# 一、frida脚本

## 1、Hook Str

```javascript
Java.perform(function () {
    console.log('Come to frida...')
    const str = Java.use("java.lang.String");
    str.getBytes.overload('java.lang.String').implementation = function () {
        var response = this.getBytes();
        if (this.toString().indexOf('100038004359') > 0) {
            console.log(this.toString());
            console.log(Java.use("android.util.Log").getStackTraceString(Java.use("java.lang.Throwable").$new()));
        }
        return response;
    }
})

```

## 2、Hook HashMap

```javascript
Java.perform(function () {
    console.log('Come to frida...')
    var linkerHashMap = Java.use('java.util.HashMap');
    linkerHashMap.put.implementation = function (a, b) {
        if (a.equals("sign")) {
            console.log('---------------put sign-----------------');
            console.log(a);
            console.log(b);
            console.log(Java.use("android.util.Log").getStackTraceString(Java.use("java.lang.Throwable").$new()));
        }
        var data = this.put(a, b);
        return data;
    }
})

```

# 二、工具

## 1、字符串2字节

```javascript
function stringToByte (str) {
    var ch, st, re = [];
    for (var i = 0; i < str.length; i++ ) {
        ch = str.charCodeAt(i);
        st = [];
        do {
             st.push( ch & 0xFF );
             ch = ch >> 8;
        } while ( ch );
        re = re.concat( st.reverse() );
     }   // return an array of bytes
     return re;
}
```

