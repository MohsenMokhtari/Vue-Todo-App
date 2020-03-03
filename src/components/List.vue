<template>
  <section>
    <link href="https://fonts.googleapis.com/css?family=Gugi" rel="stylesheet">
    <v-layout class="mt-3" align-center justify-center>
      <v-flex xs4>
        <v-text-field
          color="#0378A6"
          prepend-inner-icon="edit"
          v-model="itemName"
          label="Type a new task."
          @keyup.enter="add"
          dark
        ></v-text-field>
      </v-flex>
      <v-btn @click="add" class="ml-5" icon flat color="#0378A6">
        <v-icon large color="#0378A6">add_circle</v-icon>
      </v-btn>
      <v-btn
        v-if="completedItems.length > 0"
        @click="showModal = !showModal"
        class="ml-5"
        icon
        flat
        color="#0378A6"
      >
        <v-icon large color="#0378A6">delete</v-icon>
      </v-btn>

      <v-dialog v-model="showModal" width="250px">
        <v-card class="modal text-xs-center">
          <h2 class="white--text pt-5 pb-5">Clear all completed tasks</h2>
          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn @click="showModal = !showModal" outline icon large color="red">
              <v-icon large>close</v-icon>
            </v-btn>
            <v-btn @click="clearAll" outline icon large color="green">
              <v-icon large>done</v-icon>
            </v-btn>
            <v-spacer></v-spacer>
          </v-card-actions>
        </v-card>
      </v-dialog>
    </v-layout>
    <v-layout class="mt-3" align-center justify-center>
      <v-flex xs6>
        <div v-for="(item,index) in items" :key="item.title">
          <v-card color="rgba(255,255,255,0.15)" class="pa-3 mt-3">
            <v-layout row>
              <v-flex xs12>
                <h3>{{item.title}}</h3>
              </v-flex>
              <v-flex>
                <v-btn @click="remove(index)" class="ml-5" icon flat color="#005778">
                  <v-icon small color="#005778">close</v-icon>
                </v-btn>
              </v-flex>
              <v-flex>
                <v-btn @click="complete(index)" class="ml-1" icon flat color="#005778">
                  <v-icon small color="#005778">done</v-icon>
                </v-btn>
              </v-flex>
            </v-layout>
          </v-card>
        </div>
      </v-flex>
    </v-layout>

    <v-layout class="mt-3" align-center justify-center>
      <v-flex xs6>
        <div v-for="(item, index) in completedItems" :key="item.title">
          <v-card color="rgba(255,255,255,0.01)" class="pa-3 mt-3">
            <v-layout row>
              <v-flex xs12>
                <h3 style="text-decoration: line-through;">{{item.title}}</h3>
              </v-flex>
              <v-flex>
                <v-btn @click="removeCompleted(index)" class="ml-5" icon flat color="#005778">
                  <v-icon small color="#005778">close</v-icon>
                </v-btn>
              </v-flex>
            </v-layout>
          </v-card>
        </div>
      </v-flex>
    </v-layout>

    <v-layout
      v-if="items.length==0 &&completedItems.length==0"
      class="mt-5"
      align-center
      justify-center
    >
      <v-flex xs6>
        <h1 class="noTasks white--text">REALLY? You've got nothing to do !?</h1>
      </v-flex>
    </v-layout>
  </section>
</template>
<script>
export default {
  name: "List",
  data() {
    return {
      itemName: "",
      showModal: false,
      lists: [
        {
          name: "Personal Work",
          items: [{ title: "Go to gym" }, { title: "Buy a suit" }],
          completedItems: []
        },
        {
          name: "Company Stuff",
          items: [
            { title: "Interview with Josh" },
            { title: "Pay taxes" },
            { title: "Fire Mohsen" }
          ],
          completedItems: []
        }
      ],

      items: [
        /* {
          title: "Go to gym"
        },
        {
          title: "Go Shopping"
        },
        {
          title: "Study for Finals"
        } */
      ],
      completedItems: []
    };
  },
  methods: {
    add() {
      if (this.itemName) {
        this.items.push({ title: this.itemName });
      }
      this.itemName = "";
    },
    remove(index) {
      this.items.splice(index, 1);
    },
    removeCompleted(index) {
      this.completedItems.splice(index, 1);
    },
    complete(index) {
      this.completedItems.push(this.items[index]);
      this.items.splice(index, 1);
    },
    clearAll() {
      this.completedItems = [];
      this.showModal = false;
    }
  },
  created() {
    this.items = this.lists[0].items;
    this.completedItems = this.lists[0].completedItems;
  },
  mounted() {
    var vm = this;
    this.$root.$on("toList", index => {
      vm.items = vm.lists[index].items;
      vm.completedItems = vm.lists[index].completedItems;
    });
  }
};
</script>

<style scoped>
h3 {
  color: whitesmoke;
}
.noTasks {
  font-family: "Gugi", cursive;
  text-align: center;
  font-size: 250%;
}
.modal {
  background-color: rgba(0, 0, 0, 0.55);
  position: relative;
  width: 250px;
  height: 250px;
  top: calc(50% - 175px);
  left: calc(50% - 125px);
  overflow: hidden;
}
</style>
