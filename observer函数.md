```jsx harmony
import { memo, forwardRef } from "react";

const hoistBlackList = {
    $$typeof: true,
    render: true,
    compare: true,
    type: true
}

const copyStaticProperties = (base, target) => {
    Object.keys(base).forEach(key => {
        if (base.hasOwnProperty(key) && !hoistBlackList[key]) {
            Object.defineProperty(target, key, Object.getOwnPropertyDescriptor(base, key)!)
        }
    })
}

const observer = (baseComponent, options) => {
    const realOptions = {
        // 在forward的情况下，存在第二个要换的参数ref
        // 默认不传
        forwardRef: false,
        ...options
    };
    
    const baseComponentName = baseComponent.displayName || baseComponent.name
    
    // 本质还是利用useObserver包裹了一层
    // props，ref为react.FC的参数
    const wrappedComponent = (props, ref) => {
        return useObserver(() => baseComponent(props, ref), baseComponentName)
    }
    
    // useObserver的组件名不变
    wrappedComponent.displayName = baseComponentName;
    
    let memoComponent = null;
    if (realOptions.forwardRef) {
        memoComponent = memo(forwardRef(wrappedComponent))
    } else {
        memoComponent = memo(wrappedComponent)
    }
    
    
    // useObserver中的 baseComponent(props, ref) 使得失去了static properties
    copyStaticProperties(baseComponent, memoComponent)
    
    // 还原为初始函数的过程中，函数名还是保持
    memoComponent.displayName = baseComponentName
    
    return memoComponent
}
```
