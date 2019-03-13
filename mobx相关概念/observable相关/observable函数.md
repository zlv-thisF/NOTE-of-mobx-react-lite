```typescript jsx
const observable = createObservable;

function createObservable(v: any, arg2?: any, arg3?: any) {
    // 这里是对装饰器的一层封装
    // (target, name, descriptor)
    // 这里即是name === "string"
    if (typeof arguments[1] === "string") {
        return deepDecorator.apply(null, arguments as any);
    }

    // 如果已经是observable直接返回；
    if (isObservable(v)) return v;

    // 直接调用函数的方式，需要针对不同的传入值类型产生封装后的observable对象
    const res = isPlainObject(v)
        ? observable.object(v, arg2, arg3)
        : Array.isArray(v)
            ? observable.array(v, arg2)
            : isES6Map(v)
                ? observable.map(v, arg2)
                : isES6Set(v)
                    ? observable.set(v, arg2)
                    : v;

    if (res !== v) return res;
}





