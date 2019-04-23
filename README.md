# ionic-vue-picker-sample


### Sample code showing how to create a [picker using Ionic Framework v4 web components](https://ionicframework.com/docs/api/picker) in a Vue JS project

#### this is a temporary workaround until the picker controller in integrated into the library the same way the other controllers are. For now I have retrieved the comntroller using the following approach

```javascript
      // get the picker controller this way for now
      const pickerController = document.querySelector("ion-picker-controller");
      await pickerController.componentOnReady();
```
>the picker documentation - https://ionicframework.com/docs/api/picker

<div style="display: block;
  margin-left: auto;
  margin-right: auto;
  width: 50%;">
  <div>
 <img src="https://raw.githubusercontent.com/aaronksaunders/ionic-vue-picker-sample/master/src/assets/Screen%20Shot-1.png" height="800" style="padding:10px">

 <img src="https://raw.githubusercontent.com/aaronksaunders/ionic-vue-picker-sample/master/src/assets/Screen%20Shot-2.png" height="800"  style="padding:10px">
 </div>
 </div>
