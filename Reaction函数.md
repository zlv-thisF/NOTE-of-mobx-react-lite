Reaction

接收三个参数： name, onInvalidate, errorHandler?  

其中track函数很重要

```jsx harmony
function track (fn) {
        startBatch();
        const notify = isSpyEnabled();
        let startTime;
        if (notify) {
            startTime = Date.now();
            spyReportStart({
                name: this.name,
                type: "reaction"
            });
        }
        this._isRunning = true;
        // trackDerivedFunction是最重要的一步！
        const result = trackDerivedFunction(this, fn, undefined);
        //
        this._isRunning = false;
        this._isTrackPending = false;
        if (this.isDisposed) {
            clearObserving(this);
        }
        if (isCaughtException(result)) this.reportExceptionInDerivation(result.cause)
        if (notify) {
            spyReportEnd({
                time: Date.now() - startTime
            });
        }
        endBatch();
    }
```

startBatch是通知global(window/global).inBatch + 1, 这在mobx的事务处理逻辑中是个标志；

isSpyEnabled:  
```jsx harmony
const isSpyEnabled = () => {
    // 看到仅发生在开发环境并且global中不能有别的handler存在；
    return process.env.NODE_ENV !== "production" && !!globalState.spyListeners.length
}
```

spyReportStart:  
```jsx harmony
const spyReportStart = (event) => {
    const change = { ...event, spyReportStart: true }
    spyReport(change)
}
```

spyReport:  
```jsx harmony
const spyReport = (event) => {
    // 省略一些判断逻辑，下方的spyListeners此时为empty array；
    const listeners = globalState.spyListeners;
    for (let i = 0, l = listeners.length; i < l; i++) listeners[i](event)
}
```

总算来到最终要的一步了：  
trackDerivedFunction  
这个函数根据track函数中的observer，自动的分析所有归属于这个observer的
observable变量，并存在derivation对象中；

trackDerivedFunction(this, fn, undefined)这里，
this即new Reaction的实例；
fn为observer；

```jsx harmony
const trackDerivedFunction = () => {
    
};

const IDerivationState = {
    // before being run or (outside batch and not being observed)
    // at this point derivation is not holding any data about dependency tree
    NOT_TRACKING: -1,
    // no shallow dependency changed since last computation
    // won't recalculate derivation
    // this is what makes mobx fast
    UP_TO_DATE: 0,
    // some deep dependency changed, but don't know if shallow dependency changed
    // will require to check first if UP_TO_DATE or POSSIBLY_STALE
    // currently only ComputedValue will propagate POSSIBLY_STALE
    //
    // having this state is second big optimization:
    // don't have to recompute on every dependency change, but only when it's needed
    POSSIBLY_STALE: 1,
    // A shallow dependency has changed since last computation and the derivation
    // will need to recompute when it's needed next.
    STALE: 2
}


export function changeDependenciesStateTo0(derivation) {
    // derivation这里就是new Reaction的实例
    if (derivation.dependenciesState === IDerivationState.UP_TO_DATE) return
    // 不是up-to-date就还原为初始无数据无依赖的情况
    derivation.dependenciesState = IDerivationState.UP_TO_DATE

    const obs = derivation.observing
    let i = obs.length
    while (i--) obs[i].lowestObserverState = IDerivationState.UP_TO_DATE
}
```




