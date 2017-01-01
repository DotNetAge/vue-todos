<template>
    <div id="app">
        <h1>{{title}}</h1>
        <ul class="todos">
            <li>
                <input v-model="newTodo"
                       @keyup.13="addItem"
                       placeholder="快写下您要我记住的事吧"
                       autofocus="true"/>
            </li>
            <li v-for="(todo,index) in todos"
                :class="{'checked':todo.done}">
                <input type="checkbox"
                       @change="saveToStore"
                       v-model="todo.done"
                /><label>{{ index+1 }}.{{ todo.value }}</label>
                <time>{{ todo.created | date }}</time>
                <button @click.prevent="delItem(todo)"></button>
            </li>
        </ul>
    </div>
</template>

<script type="text/ecmascript-6">
    import './assets/site.less'
    import './assets/todos.less'
    import moment from 'moment'
    import 'moment/locale/zh-cn'
    moment.locale('zh-cn')

    export default {
        name: 'app',
        data () {
            return {
                newTodo:'',
                title: 'vue-todos',
                todos: []
            }
        },
        created (){
            if (this.is_initialized) {
                this.todos = JSON.parse(localStorage.getItem('VUE-TODOS'))
            }
        },
        computed: {
            is_initialized (){
                return localStorage.getItem('VUE-TODOS') != null
            }
        },
        filters: {
            date(val){
                return moment(val).calendar()
            }
        },
        methods: {
            addItem() {
                this.todos.push({
                    value: this.newTodo,
                    created: Date.now(),
                    done: false
                });
                this.saveToStore();
                this.newTodo = ''
            },
            delItem (todo) {
                this.todos = this.todos.filter((x) => x !== todo)
                this.saveToStore()
            },
            saveToStore(){
                localStorage.setItem('VUE-TODOS', JSON.stringify(this.todos))
            }
        }
    }
</script>