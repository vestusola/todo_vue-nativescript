<template>
  <Page>
    <ActionBar title="Todo" textTransform="uppercase" android:flat="true" />
    <TabView
      tabBackgroundColor="#53ba82"
      tabTextColor="#c4ffdf"
      androidTabsPosition="bottom"
      selectedTabTextColor="#53ba82"
      tabTextFontSize="15"
    >
      <!-- Task -->
      <TabViewItem title="Task" textTransform="uppercase">
        <StackLayout orientation="vertical" width="100%" height="100%">
          <!-- Grid Layout for todo task -->
          <GridLayout columns="2*,*" rows="*" width="100%" height="25%">
            <TextField col="0" row="0" hint="Type new todo" v-model="todo.todo_name" editable="true" @returnPress="onButtonTap" />

            <Button col="1" style="border-radius:5%" class="btn btn-primary" row="0" text="Add" @tap="onButtonTap"></Button>
          </GridLayout>
          <!-- End of Grid Layout for creating todo task -->

          <ListView class="list-group" for="todo in todos" @itemTap="onItemTap" seperatorColor="black" style="height:100%">
            <v-template>
              <Label id="current" :text="todo.todo_name" class="list-group-item-heading" textWrap="true"></Label>
            </v-template>
          </ListView>
        </StackLayout>
      </TabViewItem>
      <!-- End of Task -->

      <!-- Completed Task -->
      <TabViewItem title="Completed Task" textTransform="uppercase">
        <ListView class="list-group" for="done in dones" @itemTap="onDoneTap" style="height:75%" seperatorColor="black">
          <v-template>
            <Label id="completed" :text="done.todo_name" class="list-group-item-heading" textWrap="true" />
          </v-template>
        </ListView>
      </TabViewItem>
      <!-- End of Completed Task -->
    </TabView>
  </Page>
</template>

<script >
export default {
  data() {
    return {
      todo:{},
      todos:[],
      errors:[],
      dones: []
    };
  },
  methods: {
    onButtonTap(){
      if (!!this.todo.todo_name) {
        this.todos.unshift({todo_name: this.todo.todo_name})
        this.todo = {};
      }
      return false;
    },
    onItemTap(args){
      action('What do you like to do?', 'Cancel', ['Mark Completed', 'Delete Forever'])
      .then((res) => {
        switch (res) {
          case 'Mark Completed':
            this.dones.unshift(args.item)
            this.todos.splice(args.index, 1)
            break;
          case 'Delete Forever':
            this.todos.splice(args.index, 1)
          case 'Cancel' || undefined:
            break;
        }
      })
    },
    onDoneTap(args){
      action('What do you like to do with this task?', 'Cancel', ['Mark to do', 'Delete Forever'])
      .then((res) => {
        switch (res) {
          case 'Mark to do':
            this.todos.unshift(args.item)
            this.dones.splice(args.index, 1)
            break;
          case 'Delete Forever':
            this.dones.splice(args.index, 1)
          case 'Cancel' || undefined:
            break;
        }
      })
    }
  },
};
</script>

<style scoped>
ActionBar {
  background-color: #53ba82;
  color: #ffffff;
}

TextField {
  font-size: 14px;
  color: #1f96c5;
  margin-top: 10px;
  margin-bottom: 10px;
  margin-right: 5px;
  margin-left: 5px;
}

#current {
  font-size: 16px;
  font-weight: 700;
  color: #53ba82;
  margin-left: 15px;
  padding-top: 7px;
  padding-bottom: 7px;
}

#completed {
  text-decoration: line-through;
  font-size: 16px;
  color: #0a851a;
  margin-left: 15px;
  padding-top: 7px;
  padding-bottom: 7px;
}

</style>
