```jsx harmony
import { useRef, useCallback, useEffect } from "react";
import { Reaction } from "mobx";

const useForceUpdate = () => {
    const [, setTick] = useState(0);

    return useCallback(() => {
        setTick(tick => tick + 1)
    }, []);
}

export const useObserver = (func, baseComponentName="observed") => {
    const forceUpdate = useForceUpdate();
    const reaction = useRef(null);
    if (!reaction.current) {
        reaction.current = new Reaction(`observer(${baseComponentName})`, () => {
            forceUpdate();
        })
    }
   
    // unmount时候的取消
    useEffect(() => reaction.current.dispose(), []);
    
    // 定义真正要返回的结果
    let rendering = null;
    
    reaction.current.track(() => {
         rendering = fn();
    })
    
    return rendering;
}
```
