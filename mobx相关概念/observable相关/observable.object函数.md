```typescript jsx
export function deepEnhancer(v, _, name) {
    if (isObservable(v)) return v
    if (Array.isArray(v)) return observable.array(v, { name })
    if (isPlainObject(v)) return observable.object(v, undefined, { name })
    if (isES6Map(v)) return observable.map(v, { name })
    if (isES6Set(v)) return observable.set(v, { name })
    return v
}

const observableMap = (initialValues, options) => {
    return new ObservableMap(initialValues, deepEnhancer)
};
```
