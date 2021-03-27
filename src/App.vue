<template>
  <ion-app class="ion-padding">
    <ion-content>
      <ion-button @click="openSimplePicker"
        >Open Single Column Picker</ion-button
      >
      <ion-button @click="openComplexPicker"
        >Open Multi Column Picker</ion-button
      >
      <div style="margin: 10px">
        <pre>{{ JSON.stringify(pickerResult, null, 2) }}</pre>
      </div>
    </ion-content>
  </ion-app>
</template>

<script lang="ts">
import { defineComponent, ref } from "vue";
import { IonApp, pickerController, IonButton, IonContent } from "@ionic/vue";
export default defineComponent({
  name: "App",
  components: {
    IonApp,
    IonContent,
    IonButton,
  },
  setup() {
    const pickerResult = ref<any>();

    // HELPER FUNCTIONS FOR PICKERS

    const getColumnOptions = (
      columnIndex: any,
      numOptions: any,
      columnOptions: any
    ) => {
      const options = [];
      for (let i = 0; i < numOptions; i++) {
        // if there is no value property provided in the column data
        // then set value to the row index
        if (typeof columnOptions[columnIndex].data[i] == "object") {
          options.push({
            text: columnOptions[columnIndex].data[i % numOptions][0],
            value: columnOptions[columnIndex].data[i % numOptions][1],
          });
        } else {
          options.push({
            text: columnOptions[columnIndex].data[i % numOptions],
            value: i,
          });
        }
      }
      return options;
    };

    const getColumns = (numColumns: any, columnOptions: any) => {
      const columns = [];
      for (let i = 0; i < numColumns; i++) {
        const len = columnOptions[i].data.length;
        // if a na,e is provided then set the object to the
        // the name provided
        const colName = columnOptions[i].name || `col-${i}`;
        const col = {
          name: colName,
          options: getColumnOptions(i, len, columnOptions),
        };
        columns.push(col);
      }
      return columns;
    };

    const openSimplePicker = async () => {
      // set the arry for the column information, you can set the name of the column
      // and the array of data that should be rendered in the column
      const colOptions = [
        {
          name: "animal",
          data: ["Dog", "Cat", "Bird", "Lizard", "Chinchilla"],
        },
      ];
      // now create the picker, but first you need to create the columns
      // in the proper format for ionic vue to deal with them
      const picker = await pickerController.create({
        columns: getColumns(1, colOptions),
        buttons: [
          {
            text: "Cancel",
            role: "cancel",
          },
          {
            text: "Confirm",
            role: "confirm",
            handler: (value) => {
              console.log("Got Value", value);
              console.log(value);
              picker.dismiss(value, "confirm");
            },
          },
        ],
      });
      picker.onDidDismiss().then((v) => {
        console.log("onDidDismiss", v);
        pickerResult.value = v;
      });
      // present the picker
      await picker.present();
    };

    /**
     * this is a multi colmn picker
     */
    const openComplexPicker = async () => {
      // set the array for the column information, you can set the name of the column
      // and the array of data that should be rendered in the column. In the array of data
      // to add to the column you can set the text that will be rendered and the value
      // to be associated with the text when selected
      //
      // in this example, ["Monday", "Mon"], the picker will render the full text "Monday",
      // but the value selected will be "Mon". If there is not a value included, then it
      // will default to the index of the row
      //
      const colOptions = [
        {
          name: "day",
          data: [
            ["Monday", "Mon"],
            ["Tuesday", "Tues"],
            ["Wednesday", "Wed"],
            ["Thursday", "Thurs"],
            ["Friday", "Fri"],
          ],
        },
        {
          name: "time",
          data: ["9a", "12p", "3p", "6p"],
        },
      ];
      const picker = await pickerController.create({
        columns: getColumns(2, colOptions),
        buttons: [
          {
            text: "Cancel",
            role: "cancel",
          },
          {
            text: "Confirm",
            handler: (value) => {
              console.log("Got Value", value);
              console.log(value);
              picker.dismiss(value, "confirm");
            },
          },
        ],
      });

      picker.onDidDismiss().then((v) => {
        console.log("onDidDismiss", v);
        pickerResult.value = v;
      });

      await picker.present();
    };

    return {
      openSimplePicker,
      openComplexPicker,
      pickerResult,
    };
  },
});
</script>

<style>
</style>
