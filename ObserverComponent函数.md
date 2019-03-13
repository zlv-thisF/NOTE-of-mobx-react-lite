```jsx harmony
const ObserverComponent = ({ children, render }) => {
    const component = children || render
    if (typeof component !== "function") {
        return null
    }
    // 无论是children还是render都不是出于render props的考虑，而是
    // useObserver的第一个参数是func；
    return useObserver(component)
}
```
