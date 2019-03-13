### mobx-react-lite的源码阅读

####六个主要的API

useObservable;  
创造一个Observable的对象，主要是mobx的observable(value);  
[useObservable源码注释](https://github.com/zlv-thisF/NOTE-of-mobx-react-lite/blob/master/useObservable%E5%87%BD%E6%95%B0.md)

observer;  
observer: 将组件整体作为observer观察者，从而订阅observable的对象的变化；  
[observer源码注释](https://github.com/zlv-thisF/NOTE-of-mobx-react-lite/blob/master/observer%E5%87%BD%E6%95%B0.md)


Observer:   
颗粒度更细的组件化形式的observer；  
本质是利用了useObserver；  
[Observer组件的源码及注释]()


useObserver;  
在一定的情况下，可以仅在return处以return userObserver(() => jsx)的形式设置观察者，
这主要是为了让组件外层可以更直观的加入memo、forwardRef的react API；  
[useObserver源码注释](https://github.com/zlv-thisF/NOTE-of-mobx-react-lite/blob/master/useObserver%E5%87%BD%E6%95%B0.md)



useComputed;  
计算属性，主要利用了useMemo(memoized)和mobx的computed.get();  
[useComputed源码注释](https://github.com/zlv-thisF/NOTE-of-mobx-react-lite/blob/master/useComputed%E5%87%BD%E6%95%B0.md)

useDisposable;  
useDisposable(func, inputs);  
在unmount或者inputs变化时，可以利用autorun、reaction这样的函数定制一些需要的处理逻辑；  
[useDisposable源码及注释](https://github.com/zlv-thisF/NOTE-of-mobx-react-lite/blob/master/useDisposable%E5%87%BD%E6%95%B0.md)
