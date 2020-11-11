<template>
  <div id="app">
    <Suspense>
      <template #default>
        <AsyncShow></AsyncShow>
      </template>
      <template #fallback>
        <h1>loading</h1>
      </template>
    </Suspense>

    <HelloWord msg="test" />
    <h1>{{count}}</h1>
    <h1>{{double}}</h1>
    <h1>X:{{x}}, Y:{{y}}</h1>
    <div v-if="loading">loading</div>
    <div v-if="loaded"><img :src="result.message" /></div>
    <button @click="onModalOpen">打开窗口</button>
    <button @click="increase">+1</button>
    <button @click="updateGreeting">click</button>
    <Modal :isOpen="modalIsOpen" @close-modal="onModalClose">my modal...</Modal>

  </div>

</template>

<script>
import HelloWord from './components/HelloWorld'
import Modal from './components/modal'
import AsyncShow from './components/AsyncShow'
import { computed, reactive, toRefs, ref, watch } from "vue";
import useMousePosition from './hooks/useMousePosition.ts'
import useURLLoader from './hooks/useURLLoader.ts'


export default {
  name: "App",
  props: {
    msg: String
  },
  setup() {
    // console.log(props, context)
    const { x, y } = useMousePosition()

    const { result, loading, loaded, error } = useURLLoader('https://dog.ceo/api/breeds/image/random')

    const data = reactive({
      count: 0,
      increase: () => {
        //这里不需要value了
        data.count++;
      },
      double: computed(() => {
        return data.count * 2;
      })
    });

    const greetings = ref('')
    const updateGreeting = () => {
      greetings.value += 'Hello!'
    }
    // watch(greetings, (newV, oldV) => {
    //   document.title = 'update' + greetings.value
    // })

    //观察多个对象 打印的 newV 和 oldV 分别是数组  一个代表第greetings，第二个是count
    //注意 如果直接观察data 打印出个proxy 想精确观察data.count 需要函数返回，否则
    //watch中不接受data.count 因为他表示一个数字 没法watch
    watch([greetings, () => data.count], (newV, oldV) => {
      console.log(newV, oldV)
      document.title = 'update' + greetings.value + data.count;
    })

    //直接返回data 也没发现什么问题
    // return data

    //如果要返回对象，用展开运算符 需要用 toRefs包装一下，让展开后都变成响应式对象
    const refData = toRefs(data)


    const modalIsOpen = ref(false)

    const onModalOpen = () => {
      modalIsOpen.value = true;
    }

    const onModalClose = () => {
      modalIsOpen.value = false;
    }




    return {
      ...refData,
      updateGreeting,
      greetings,
      x, y,
      result,
      loading,
      error,
      loaded,
      modalIsOpen,
      onModalOpen,
      onModalClose
    };
  },
  components: {
    HelloWord, Modal, AsyncShow
  }
};
</script>


