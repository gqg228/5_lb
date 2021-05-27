<template>
    <Page>
        <ActionBar class="header" title="bruh"/>
        <StackLayout>
            <TextField class="input" v-model="todoValue" hint="Давай,заполни меня" />
            <Button class="add" text='Add' @tap="todoNew()"/>
            <ScrollView orientation="horizontal">
                <ListView class="task" for="el in arr">
                    <v-template>
                        <GridLayout columns="220%, 70%, 70%">
                              <Label class="text done"  v-if="el.done" textWrap="true" col="0">{{el.title}}</Label>
                              <Label class="text"  v-else textWrap="true" col="0">{{el.title}}</Label>
                              <Button class="checkbox"  @tap="hideTodo(el.id)" col="1"/>
                              <Button class="btn remove-btn" text="Del" @tap="deleteTodo(el.id)" col="2"/> 
                        </GridLayout>
                    </v-template>
                </ListView>
            </ScrollView>
        </StackLayout>
    </Page>
</template>

<script>
import * as ApplicationSettings from "@nativescript/core/application-settings";
export default {
    data: function() {
        return {
            todoValue: '',
            arr: []
        }
    },
    methods: {

        todoNew(){
        if(this.todoValue != ''){
                this.arr.push({
                    id: Math.random(),
                    title: this.todoValue,
                    done: false
                });
                this.todoValue = '';
            }
          this.save();
        },
        upTodo(){
          // alert(){

          // }
        },
        mounted() {
        if(ApplicationSettings.getString('arr'))
            this.arr = Object.values(JSON.parse(ApplicationSettings.getString('arr')));
         },
        hideTodo(id){
            this.arr = this.arr.map(el => {
                if (el.id == id) 
                    el.done = !el.done;
                return el;
            })
            this.save();
        },
        deleteTodo(id){
            this.arr = this.arr.filter(el => el.id !== id);
            this.save();
        },
        save(){
            let saveData = Object.assign({}, this.arr);
            ApplicationSettings.setString('arr', JSON.stringify(saveData));
        }
    }
    
}
</script>

<style>
    .done {
        text-decoration: line-through;   
    } 
</style>




