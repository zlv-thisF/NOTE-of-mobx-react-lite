```jsx harmony
import { useMemo } from "rect";
import { computed } from "mobx";

export const useComputed = (func, inputs) => {
    // 类似memoized-one, inputs的变化决定了计算属性的值是否重新计算
    // 这里用mobx的computed封装了一层
    const memoized = useMemo(() => computed(func), inputs);
    // 于是这里是computed(func).get()
    return memoized.get();
}
```
