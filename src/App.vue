<template>
  <div id="app">
    <ion-app>
      <ion-header translucent>
        <ion-toolbar>
          <ion-title>Ionic VueJS Picker Sample</ion-title>
        </ion-toolbar>
      </ion-header>
      <ion-content fullscreen class="ion-padding">
        <ion-picker-controller></ion-picker-controller>
        <ion-button @click="openSimplePicker">Open Single Column Picker</ion-button>
        <ion-button @click="openComplexPicker">Open Multi Column Picker</ion-button>
        <ion-item>
          <ion-label>
            <pre> {{results}} </pre>
          </ion-label>
        </ion-item>
      </ion-content>
    </ion-app>
  </div>
</template>

<style src='@ionic/core/css/core.css'></style>
<style src='@ionic/core/css/ionic.bundle.css'></style>

<script>
export default {
  name: "App",
  components: {},
  data() {
    return {
      results: ""
    };
  },
  methods: {
    async openSimplePicker() {
      // get the picker controller this way for now
      const pickerController = document.querySelector("ion-picker-controller");
      await pickerController.componentOnReady();

      // set the arry for the column information, you can set the name of the column
      // and the array of data that should be rendered in the column
      let colOptions = [
        { name: "animal", data: ["Dog", "Cat", "Bird", "Lizard", "Chinchilla"] }
      ];

      // now create the picker, but first you need to create the columns
      // in the proper format for ionic vue to deal with them
      const picker = await pickerController.create({
        columns: this.getColumns(1, colOptions),
        buttons: [
          {
            text: "Cancel",
            role: "cancel"
          },
          {
            text: "Confirm",
            role: "confirm",
            handler: value => {
              console.log("Got Value", value);
              this.results = value;
              picker.dismiss(value, "confirm");
            }
          }
        ]
      });

      picker.onDidDismiss().then(v => {
        console.log(v);
      });

      // present the picker
      await picker.present();
    },

    /**
     * this is a multi colmn picker
     */
    async openComplexPicker() {
      // get the picker controller this way for now
      const pickerController = document.querySelector("ion-picker-controller");
      await pickerController.componentOnReady();

      // set the array for the column information, you can set the name of the column
      // and the array of data that should be rendered in the column. In the array of data
      // to add to the column you can set the text that will be rendered and the value
      // to be associated with the text when selected
      //
      // in this example, ["Monday", "Mon"], the picker will render the full text "Monday",
      // but the value selected will be "Mon". If there is not a value included, then it
      // will default to the index of the row
      //
      let colOptions = [
        {
          name: "day",
          data: [
            ["Monday", "Mon"],
            ["Tuesday", "Tues"],
            ["Wednesday", "Wed"],
            ["Thursday", "Thurs"],
            ["Friday", "Fri"]
          ]
        },
        {
          name: "time",
          data: ["9a", "12p", "3p", "6p"]
        }
      ];

      const picker = await pickerController.create({
        columns: this.getColumns(2, colOptions),
        buttons: [
          {
            text: "Cancel",
            role: "cancel"
          },
          {
            text: "Confirm",
            handler: value => {
              console.log("Got Value", value);
              this.results = value;
            }
          }
        ]
      });

      await picker.present();
    },

    // HELPER FUNCTIONS FOR PICKERS
    getColumns(numColumns, columnOptions) {
      let columns = [];
      for (let i = 0; i < numColumns; i++) {
        let len = columnOptions[i].data.length;

        // if a na,e is provided then set the object to the
        // the name provided
        let colName = columnOptions[i].name || `col-${i}`;
        let col = {
          name: colName,
          options: this.getColumnOptions(i, len, columnOptions)
        };
        columns.push(col);
      }

      return columns;
    },
    getColumnOptions(columnIndex, numOptions, columnOptions) {
      let options = [];
      for (let i = 0; i < numOptions; i++) {
        // if there is no value property provided in the column data
        // then set value to the row index
        if (typeof columnOptions[columnIndex].data[i] == "object") {
          options.push({
            text: columnOptions[columnIndex].data[i % numOptions][0],
            value: columnOptions[columnIndex].data[i % numOptions][1]
          });
        } else {
          options.push({
            text: columnOptions[columnIndex].data[i % numOptions],
            value: i
          });
        }
      }

      return options;
    }
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
