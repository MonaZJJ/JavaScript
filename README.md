 按照PromiseA+规范写Promise的简单实现原理
 //第一步：Promise构造函数接受一个按数作为参数，该函数的两个参数分别是resolve和reject。它是两个函数，由JavaScript引擎提供，不用自己部署;
 function Promise(task){
     let that = this;   //缓存this
     that.status = "pending";  //进行中的状态
     that.value = undefined;   //初始值
     
     that.onResolvedCallbacks = []; //存放成功后要执行的回调函数的序列
     that.RejectedCallnacks = [];   //存放失败后要执行的回调函数序列
     
     //该方法是将promise由pending变成fullfilled
     function resolve（value）{
         if（that.status == "pending"）{
            that.status = "fullfilled";
            that.value = value;
            that.onResolveCallback.forEach(cb=>cd(that.value))
         }
     }
     //该方法是将promise由pending变成rejected
     function reject(reason){
         if(that.status = "pending"){
            that.status = "rejected";
            that.value = reason;
            that.onRejectedCallback.forEach(cb=>cd(that.value));
         } 
     }
     
     try{
        //每一个Promise在new一个实例的时候，接受函数都是立即执行的
        task(resolve,reject)
     }catch(e){reject(e)}  
 }
 
 //第二步：写then方法，接收两个函数onFullfilled onRejected,状态是成功状态时调用onFullfilled传入成功后的值，失败时执行onRejected,传入失败的原因，
 peding状态时将成功和失败后的这两个方法缓存到对应的数组中，当成功或失败后，依次再执行调用
 Promise.prototype.then = function(onFullfilled,onRejected){
     let that = this;
     if(that.status == 'fullfilled'){onFullfilled(that.value);}
     if(that.status == 'rejected'){onRejected(that.value);}
     if(that.status == 'pending'){that.onResolvedCallbacks.push(onFullfilled);that.onRejectedCallbacks.push(onRejected);} 
 }
