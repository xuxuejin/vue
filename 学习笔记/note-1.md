## 父子组件是怎么通信

  父子组件的关系可以总结为 props down, events up。父组件通过 props 向下传递数据给子组件，子组件通过 events 给父组件发送消息
  
## 父组件向子组件传值步骤

  1.数据肯定是定义在父组件中的，供所有子组件共享，所以要在父组件的data中定义值

    export default {
        name: 'app',
        data() { // 父组件定义全局数据，供子组件使用
            return { // return一定要有，因为data是一个方法，没有return就返回undefined，找不到数据
                newlists:[
                    {
                        title: "vue学习1",
                        time: "2017/08/15"
                    },
                    {
                        title: "vue学习2",
                        time: "2017/08/15"
                    },
                    {
                        title: "vue学习3",
                        time: "2017/08/15"
                    }
                ],
                titlelist: "学习vue有好处"
            }
        }
    }
    
  2.父组件和子组件要有契合点：父组件引用，调用，注册子组件

    // 导入子组件
    import childComponents from "./xxx.vue";
    
    // 在页面中引用子组件，并且把父组件的值绑定给子组件
    <childComponents v-bind:newlists="newlists" v-bind:titlelist="titlelist" ></childComponents>

    // 注册组件
    export default {
        components: {
            childComponents
        }
    };
    
  3.子组件内部接受父组件传过来的值，通过props来接收

    props: {
        // 必传且是数组
        newlists: {
            type: Array,
            required: true
        },
        // 必传且是字符串
        titlelist: {
            type: String,
            required: true
        },
        // 数字，有默认值
        page: {
            type: Number,
            default: 100
        }
    }
    
    ★props验证，用来指定验证规格，如果传入的数据不符合规格，Vue会发出警告，当组件给其他人使用时，这很有用
    
  4.子组件内部就可以用父组件的值了

    <h3>{{ titlelist }}</h3>

    <li v-for="item in newlists">
        <p>{{ item.title }}</p>
        <span>{{ item.time }}</span>
    </li>

    ★注意：子组件接受的父组件的值分为引用类型和普通类型两种

      普通类型：字符串（String）、数字（Number）、布尔值（Boolean）、空（Null），复制了一个副本，可以随便修改

      引用类型：数组（Array）、对象（Object），复制过来的是个引用，和父组件中都是同一个内存地址

      ★普通类型是可以在子组件中更改，不会影响其他兄弟子组件内同样调用的来自父组件的值

      ★引用类型的值，当在子组件中修改后，父组件的也会修改，那么后果就是，其他同样引用了改值的子组件内部的值也会跟着被修改
      
## 如何给标签属性赋值 src href

  <img v-bind:src="list.src" /> 可简写为 <img :src="list.src" />
  
  如果需要添加参数，使用以下两种写法：
  (1)<a :href="'http://www.xxx.com/' + value.user_id">
  (2)<a :href="`http://www.xxx.com/${value.user_id}`">
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
