```jsx harmony
import { useRef } from "react";
import { observable } from "mobx";

export const useObservable = (initialValue) => {
    // 利用useRef保存一个mutable的值
    const observableRef = useRef(null);
    if (!observableRef.current) {
        // observable函数返回一个boxed observable
        observableRef.current = observable(initialValue);
    }
    return observableRef.current;
};
```
