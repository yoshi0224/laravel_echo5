<template>
    <div class="container">
        <div class="row justify-content-center">
            <div class="col-md-8">
                <h1>Todoリスト</h1>
                <input type="text" v-model="newTodo" @blur="addTodo">
                <ul>
                <li v-for="todo in todos">{{ todo['name'] }}</li>
                </ul>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
    data(){
        return {
            todos : [],
            newTodo : ''
        }
    },
    mounted() {
              this.getMessages();
                Echo.channel('todo-added-channel')
                    .listen('TodoAdded', (e) => {
                        this.getMessages(); // 全メッセージを再読込
                    });
          },
    methods:{
         getMessages() {
                  const url = '/api/todos';
                  axios.get(url)
                      .then((response) => {
                          this.todos = response.data;
                      });
              },
        addTodo(){
            axios.post('/api/todos',{
                name : this.newTodo
            })
            .then(response => this.todos.push(response.data));         
        this.newTodo = '';
        }
    }
}
</script>