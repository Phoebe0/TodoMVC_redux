>**About**
>大家好，我是**且陶陶**，今天跟大家分享一个redux的todoList案例，通过这个案例能够快速掌握redux的基本知识点🌹


~~❤️...❤️...❤️...❤️..❤️..❤️..❤️..❤️..❤️...❤️...❤️...❤️...❤️...❤️...❤️...❤️..❤️...❤️...❤️...❤️...❤️...❤️..❤️...❤️...❤️...~~ 

# 前情回顾 - 什么是redux 🪷
最流行的**状态管理工具**之一。(类似于 vue中的vuex)

Redux和React是两个独立的工具/
## **三个核心概念🌟**

1. action（动作/行为）：【对象格式】描述要做的事（例如：登陆、退出、增删改查等等…)
2. reducer（函数）：【函数格式 function reducer(state = 0，action){ } 】更新状态
3. store（仓库）：整合action(动作)和reduce(函数)
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/144c0f65e7ca48bc8cba0649d8eeb215.png)


store分配要做的事`action`给`reducer`
# 🍬TodoMVC案例
代码地址🍻：
[TodoMvc](https://github.com/Phoebe0/React_Introduction/tree/main/todo_mvc_redux)
欢迎大家批评指正～

# 功能介绍 🌺

🍦 添加事项
🍦 删除事项
🍦 完成or未完成事项
🍦 全选反选
🍦 清空


![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/a687672a81844616a929f1fbae3b4fd5.png)

# 🍿 静态结构


![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/f61a54ccd40d448088f82756f13c0b4b.png)

# 🍰 状态管理 - redux

## 一、创建store📂



![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/41fd72c255e245caaead05601aea87a5.png)
1. 在 `store/reducer/todos.js` 中处理行为
    
    ```js
    const initList = [
      { id: 1, name: '学习日语，备考N1', isDone: true },
      { id: 2, name: '学习英语，备考雅思', isDone: false },
      { id: 3, name: '学习GO，找工作', isDone: false },
    ]
    export default function todosReducer(state = initList, sction) {
      return state
    }
    ```
    
2. 在 `store/reducers/index.js` 中合并单独的reducer并导出
    
    ```js
    // 模块合并 并导出
    import todos from './todo'
    import { combineReducers } from 'redux'
    
    const rootReducer = combineReducers({ todos })
    export default rootReducer
    
    ```
    
3. 在`store/index.js`中挂载 reducer和action
    
    ```js
    // 创建仓库，挂载reducers 并导出
    import { createStore } from 'redux'
    import reducers from './reducers/index'
    // 创建store
    const store = createStore(reducers)
    export default store
    
    ```
    

## 二、引入redux🧊

在`index.jsx`中，引入`redux`和`react-redux`

用Provider包裹根组件，并提供store值

```js
import ReactDOM from 'react-dom/client'
import App from './App'
import store from './store/index'
import { Provider } from 'react-redux'
import './styles/base.css'
import './styles/index.css'

// 渲染UI界面
const root = ReactDOM.createRoot(document.querySelector('#root'))
root.render(
  <Provider store={store}>
    <App></App>
  </Provider>
)

```

## 三、使用仓库状态📉

1. 在 `components/TodoMain.jsx` 【列表内容组件】中，使用 useSelector, useDispatch 这两个hook 操作状态。
    
    ```js
    import React from 'react'
    import TodoItem from './TodoItem'
    import { useSelector, useDispatch } from 'react-redux'
    export default function TodoMain() {
      // 拿到状态
      const todos = useSelector((state) => state.todos)
      **console.log(todos)**
      // 修改状态
      const dispatch = useDispatch()
      ...
      ...
    ```
    
  
    
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/d2ac7f4c9c024f54a2e9cd1fababf4b9.png)

## 更改状态🍥

**步骤**

1. 界面绑定onChange事件，dispatch触发行为。
2. 定义一个action行为，声明actionType
3. 根据行为在todosReducer中处理状态

# 功能实现🍹

## 界面渲染🕸️

### 渲染 事项📋

1. 在`TodoMain.jsx`中。循环渲染`todolist`中的每一项。传递每一项item

```js
 ...
 ...
 
  return (
    <section className="main">
      <input id="toggle-all" className="toggle-all" type="checkbox" />
      <label htmlFor="toggle-all">Mark all as complete</label>
      <ul className="todo-list">
        {/* todolist的每一项 */}
        **{todos.map((item) => {
          return <TodoItem key={item.id} todos={item}></TodoItem>
        })}**
      </ul>
    </section>
  )
```

1. 在`TodoItem.jsx`子组件中接收每一项。并渲染
    1. 划线样式类名：completed
    2. 展示输入框类名：editing
    
    ```js
    export default function TodoItem(**props**) {
      const todoitem = props.todos
      return (
        // completed - 划线，已完成事项
        // editing - 输入事项
        <li className={todoitem.done ? 'completed' : ''}>
          <div className="view">
             {/* 复选框设置选中状态 */}
            <input className="toggle" type="checkbox" checked={todoitem.isDone} />
            <label>{todoitem.name}</label>
            <button className="destroy"></button>
          </div>
          <input className="edit" />
        </li>
      )
    }
    
    ```
    

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/751a14a6428d46789aa9356afd03f23c.png)


做到这里，我们会发现控制台报错：


![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/74f9aa319552426eafc7830e309f213e.png)
意思是我们这里添加了checked属性，但是需要添加一个`change`事件。所以接下来需要添加`change`事件。

## 修改单项🐣

<aside>
💡 选择要修改的项目的复选框，然后改变checked状态。

</aside>

### 添加事件🐥

因为当前是受控组件，无法修改。所以需要给他一个`onChange`事件

`onChange`事件交给store去修改数据。

---

**思路：**

1. 绑定`onChange`事件，在这个事件中用`dispatch`触发`action`行为
2. 定义一个`action`行为
3. 声明`actionTypes`
4. 根据行为在`todosReducer`里面处理状态

---

**代码：**

1. 绑定`onChange`事件
    1. 传递id和当前状态
    
    ```js
    <input
      className="toggle"
      type="checkbox"
      checked={todoitem.isDone}
      onChange={() => {
        dispatch(changeDone(todoitem.id, !todoitem.isDone))
      }}
    />
    ```
    
2. 定义action行为
    
    ```js
    import { CHANGE_STATE } from '../constants/todo'
    
    // 修改单个状态的行为
    export const changeDone = (id) => {
      return {
        type: CHANGE_STATE,
        id,
      }
    }
    
    ```
    
3. 声明`actionType`
    
    ```js
    // 声明 constantTypes
    export const CHANGE_STATE = 'todos/changeDone' // 修改单个复选框状态类型
    
    ```
    
4. `todosReducer`里面处理状态
    
    ```js
    case CHANGE_STATE:
          // 注意：状态不可变
          return state.map((item) => {
            if (item.id === action.id) {
              return {
                ...item,
                isDone: action.isDone,
              }
            } else {
              return item
            }
          })
    ```
    
5. 使用`dispatch`触发`action`
    
    ```js
    import React from 'react'
    import { useDispatch } from 'react-redux'
    ...
    export default function TodoItem(props) {
     ...
      const dispatch = useDispatch()
      return (
    	  ...
            <input
              className="toggle"
              type="checkbox"
              checked={todoitem.isDone}
              onChange={() => {
                **dispatch**(changeDone(todoitem.id, !todoitem.isDone))
              }}
            />
         ...
      )
    }
    
    ```
    

## 删除单项🐤

<aside>
💡 获取要删除的那一项的id,然后过滤掉它。

</aside>

**思路：**

1. 给X绑定点击事件 `onClick`
2. 定义一个`action`行为
3. 声明`actionTypes`
4. 根据行为在`todosReducer`里面处理状态

---

**代码：**

1. 给X绑定点击事件 `onClick`
    
    ```js
    <button
    className="destroy"
    onClick={() => {
      dispatch(delTodo(todoitem.id))
    }}
    ></button>
    ```
    
2. 定义一个`action`行为
    
    ```js
    // 删除单个代办项
    export const delTodo = (id) => {
      return {
        type: DELETE_TODO,
        id,
      }
    }
    
    ```
    
3. 声明`actionTypes`
    
    ```js
    export const DELETE_TODO = 'todos/delTodo' // 删除单个待办
    ```
    
4. 根据行为在`todosReducer`里面处理状态
    
    ```js
      case DELETE_TODO:
          return state.filter((item) => {
            // 过滤掉与选择的这一行相同的id
            return item.id !== action.id
          })
    ```
    
    ## 添加单项🦜
    
    <aside>
    💡 首先对拿到的做非空校验；然后数组添加一项数据。
    
    </aside>
    
    1. 绑定`onChange`事件，得到输入框的输入内容
        
        ```js
        import React, { useState } from 'react'
        import { useDispatch } from 'react-redux'
        import { addTodo } from '../store/actions/todo'
        
        export default function TodoHeader() {
          **const [inputValue, setInputValue] = useState('')
        
          // 添加单项todo
          const addValue = (e) => {
            setInputValue(e.target.value)
          }**
          return (
            <header className="header">
              <h1>todos</h1>
              <input
                className="new-todo"
                placeholder="今天做什么?"
                value={inputValue}
                autoFocus
                **onChange={addValue}**
              />
            </header>
          )
        }
        
        ```
        
    2. 绑定`onKeyDown` 事件，键盘按下时传递输入项`value`
        
        ```js
          <input
                className="new-todo"
                placeholder="今天做什么?"
                value={inputValue}
                autoFocus
                onChange={addValue}
                onKeyDown={(e) => {
                  if (e.key === 'Enter') {
                    console.log('回车', inputValue)
                    dispatch(addTodo(inputValue))
                    setInputValue('') // 清空输入框
                  }
                }}
              />
        ```
        
    3. 定义一个`action`行为
        
        ```js
        // 添加单个待办项
        export const addTodo = (inputValue) => {
          return {
            type: ADD_TODO,
            name: inputValue,
          }
        }
        ```
        
    4. 声明`actionTypes`
        
        ```js
        export const ADD_TODO = 'todos/addTodo' // 添加单个待办项
        ```
        
    5. 根据行为在`todosReducer`里面处理状态
        
        ```js
        case ADD_TODO:
              if (!action.name.trim()) return
              // 状态不可变！！！
              return [
                {
                  id: state.length + 1,
                  name: action.name,
                  isDone: false,
                },
                ...state,
              ]
        ```
        
    
  ## 底部筛选🐩
    
    
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/4f9ddb4bc85d41d48dedf9b7933aa7f0.png)
    
    <aside>
    💡 要实现底部筛选，可以在footer中使用过滤器进行分发。
  
    </aside>
    
   ### 一、列表项绑定筛选后数据
    
   1. 声明`actionTypes`
        
        ```js
        // 筛选栏标题
        export const SHOW_ALL = 'show_all'
        export const SHOW_COMPLETED = 'show_completed'
        export const SHOW_ACTIVE = 'show_active'
        // 筛选行为
        export const SET_VISIBILITY_FILTER = 'todos/setVisibilityFilter'
        ```
        
   2. 定义筛选栏标签的静态数据
        
        ```js
        import { SHOW_ALL,SHOW_ACTIVE,SHOW_COMPLETED } from "./todo";
        
        export  const FILTER_TITLES = {
            [SHOW_ALL]: 'All',
            [SHOW_ACTIVE]: 'Active',
            [SHOW_COMPLETED]: 'Completed'
          }
        ```
        
   3. 定义一个`action`行为
        
        ```j
        // 底部筛选栏 - 用于更新Redux store中的过滤状态
        export const setVisibilityFilter = (filter) => ({
          type: SET_VISIBILITY_FILTER,
          filter
        })
        ```
        
   4. 根据行为在`todosReducer`里面处理状态
        1. 新建一个`reducer/filter.js`
        
        ```js
        import { SET_VISIBILITY_FILTER } from '../constants/todo'
        import { SHOW_ALL } from '../constants/todo'
        // 设置已完成&未完成，并返回参数。
        const visibilityFilter = (state = SHOW_ALL, action) => {
          switch (action.type) {
            case SET_VISIBILITY_FILTER:
              return action.filter
            default:
              return state
          }
        }
        
        export default visibilityFilter
        ```
        
      2. 新建一个`selector/isVisible.js`
        
        ```js
        // todo项是否可见 方法
        import { SHOW_ACTIVE, SHOW_ALL, SHOW_COMPLETED } from '../constants/todo'
        
        export function selectVisible(state = [], filter) {
          switch (filter) {
            case SHOW_ALL:
              return state
            case SHOW_ACTIVE:
              return state.filter((todo) => !todo.isDone)
            case SHOW_COMPLETED:
              return state.filter((todo) => todo.isDone)
            default:
              return state
          }
        }
        ```
        
 5. 在`TodoMain.jsx`中，使用筛选(未完成/已完成/全部)后的状态来循环渲染列表项
 
```javascript
// 筛选出已完成or未完成or全部的项
// 传入两个参数-参数1：所有数据；参数2：过滤条件
  const visibleTodos = useSelector((state) =>
    selectVisible(state.todos, state.visibilityFilter)
  )
```
### 二、底部筛选栏设置过滤条件
    
   1. 在`TodoFooter.jsx`中，循环渲染过滤条件。
   1. 给a链接绑定`onClick`事件，触发`action`行为。实现数据的过滤展示。
        ```js
        <ul className="filters">
          {Object.keys(FILTER_TITLES).map((filterTitle) => (
            <li key={filterTitle}>
              <a
                href="./#"
                className={classNames({ selected: filterTitle === filter })}
                onClick={() => dispatch(setVisibilityFilter(filterTitle))}
              >
                {FILTER_TITLES[filterTitle]}
              </a>
            </li>
          ))}
        </ul>
        ```
        
   
## 删除全部已完成☘️
    
   1. 给按钮绑定点击事件 `onClick`
        
        ```js
        <button
          className="clear-completed"
          onClick={() => dispatch(changeAll(true))}
        >
          Clear completed
        </button>
        ```
        
  2. 定义一个`action`行为
        
        ```js
        // 清除所有已完成
        export const changeAll = (isDone) => {
          return {
            type: CHANGE_ALL,
            isDone,
          }
        }
        ```
        
  3. 声明`actionTypes`
        
        ```js
        export const CHANGE_ALL = 'todos/changeAll' // 清除所有已完成
        ```
        
  4. 根据行为在`todosReducer`里面处理状态
        
        ```js
        case CHANGE_ALL:
        	return state.filter((item) => {
        	  return item.isDone !== action.isDone
        })
        ```
        
    
## 持久化存储 - 本地 🌈
   <aside>
💡 将仓库中的状态存储到localStorage中；2. 从浏览器本地存储中得到状态，如果状态存在，仓库中的数据更新为本地存储的数据。

</aside>

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/d4abacb3071843dcb31aba1ff0543e75.png)


1. 定义一个`action`行为
    
    ```js
    // 本地localstore存储
    export const setLocalToken = (todos) => ({
      type: SET_LOCAL_TOKEN,
      todos,
    })
    ```
    
2. 声明`actionTypes`
    
    ```js
    // 本地localstore存储
    export const SET_LOCAL_TOKEN = 'todos/setLocalToken'
    ```
    
3. 根据行为在`reducer`里面处理状态
    
    ```js
    case SET_LOCAL_TOKEN:
          return action.todos
    ```
    
4. 在`TodoMain.jsx`中触发action
    
    ```js
    const todos = useSelector((state) => state.todos)
    // 触发action，传入本地存储的状态
      useEffect(() => {
        const savedTodos = JSON.parse(localStorage.getItem('todos'))
        if (savedTodos) {
          dispatch(setLocalToken(savedTodos))
        }
    //[dispatch] 作为依赖数组。只有当 dispatch 更新时才重新执行 useEffect 中的逻辑
      }, [dispatch])
    // 状态存储到本地
      useEffect(() => {
        localStorage.setItem('todos', JSON.stringify(todos))
      }, [todos])
    ```
    
