```jsx harmony
import { useEffect, useRef } from "react";

const useDisposable = (disposerGenerator, inputs) => {
    // dispose函数的存储值
    const disposerRef = useRef(null);
    
    // 是否需要dispose的boolean判断值；
    const earlyDisposedRef = useRef(false);
    
    const lazyCreateDisposer = (earlyDisposal) => {
            if (earlyDisposedRef.current) {
                return () => {};
                // 空函数是useEffect的unmount时的执行函数；
            }
            if (!disposerRef.current) {
                //
                disposerRef.current = disposerGenerator();
            }
            return () => {
                if (disposerRef.current) {
                    disposerRef.current();
                    // 执行函数后还原
                    disposerRef.current = null;
                }
                if (earlyDisposal) {
                    // 是否lazy
                    earlyDisposedRef.current = true
                }
            }
        }
    
    // 仅在inputs变化或者unmount时候触发dispose；
    useEffect(() => {
        return lazyCreateDisposer(false);
    }, inputs)
    
    //这个返回值 ？？？
    return lazyCreateDisposer(true);
};
```
