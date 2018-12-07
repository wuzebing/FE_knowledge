### <font color=#0099ff >JS拷贝</font>


#### <font color=#0099ff >栈内存和堆内存</font>

![图片](/img/copy_01.jpg)

#### <font color=#0099ff >1、递归深拷贝</font>

```
function deepClone(obj){
    let objClone = Array.isArray(obj)?[]:{};
    if(obj && typeof obj==="object"){
        for(key in obj){
            if(obj.hasOwnProperty(key)){
                //判断ojb子元素是否为对象，如果是，递归复制
                if(obj[key]&&typeof obj[key] ==="object"){
                    objClone[key] = deepClone(obj[key]);
                }else{
                    //如果不是，简单复制
                    objClone[key] = obj[key];
                }
            }
        }
    }
    return objClone;
}    
let a=[1,2,3,4],
    b=deepClone(a);
a[0]=2;
console.log(a,b);
```

#### <font color=#0099ff >2、JSON的parse和stringify</font>

```
function deepClone(obj){
    let _obj = JSON.stringify(obj),
        objClone = JSON.parse(_obj);
    return objClone
}    
let a=[0,1,[2,3],4],
    b=deepClone(a);
a[0]=1;
a[2][0]=1;
console.log(a,b);
```

#### <font color=#0099ff >3、JQ的extend</font>

$.extend( [deep ], target, object1 [, objectN ] )

deep表示是否深拷贝，为true为深拷贝，为false，则为浅拷贝

target Object类型 目标对象，其他对象的成员属性将被附加到该对象上。

object1  objectN可选。 Object类型 第一个以及第N个被合并的对象。 

```
let a=[0,1,[2,3],4],
    b=$.extend(true,[],a);
a[0]=1;
a[2][0]=1;
console.log(a,b);
```

#### <font color=#0099ff >其它</font>

还有一些Object.assgin(), 数组的slice()这些，这些只深复制了<span color=red>基本类型数据类型</span>，不是真正意义的深复制，当然，如果要复制的对象或者数组都是简单数据类型，那就大胆用吧。




