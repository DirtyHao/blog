react.js里面的key相信大家都不陌生。  

    <ul>     
        <ChildComponent id={1}></ChildComponent>
        <ChildComponent id={2}></ChildComponent>
    </ul>
假设有如上所示的两个子组件,让react.js浏览器渲染时会报出一串需要key的warning。

    Warning: Each child in an array or iterator should have a unique "key" prop. Check the render method of `ServiceInfo`. See https://fb.me/react-warning-keys for more information.

那么我现在有两个问题，
1. 为什么需要key？
2. 用arr的index做key行不行？

## 为什么需要key?  
要解释清楚这个问题我们首先要了解react的渲染过程。  
我在上面那段代码中插入一个ID为0的子组件 。 

    <ul>     
        <ChildComponent id={0}></ChildComponent>
        <ChildComponent id={1}></ChildComponent>
        <ChildComponent id={2}></ChildComponent>
    </ul>

理想的情况是react比较出前后两次的不同是在序列的头部插入了一个`<ChildComponent id={0}></ChildComponent>` 组件。  
随后 id为0的组件触发组件挂载,1和2的组件触发更新。由于1，2组件属性没有Update,所以不触发render。但是问题是要比较两个序列的不同时间复杂度为O(n^2)所以react采用了一种暴力更新所有组件的方式。  
即第一个组件视为id从1改变成0触发update,第二个组件从2变为1,第三个组件触发挂载。如果序列有一千个子属性呢，这个就是很大的一个性能问题,key由此因运而生。
使用Key后的代码如下。  

    <ul>     
        <ChildComponent key={1}   id={1}></ChildComponent>
        <ChildComponent key={2}   id={2}></ChildComponent>
    </ul>

此时无论在序列的前面还是后面插入子组件，只要key是唯一的,react就能区别出原有的key为1,2的组件，并不会去改变其props。

## 用arr的index做key行不行？
不能,使用arr的Index依然会触发react.js的warning。以下面代码为例。  
        
        <ul> 
            {
                list.map((item,index)=>{
                      <ChildComponent
                        key={index}
                    >
                }
                )
            }
        </ul>

答案是否定的,试想一下list这个数组没增加一个元素，是不是所有的ChildComponent的key都会改变一次呢？