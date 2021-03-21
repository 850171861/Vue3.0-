 <template>
  <div>
    <h1>conut的值:{{count}}</h1>
    <h1>监听count值变化:旧值{{countOldV}}----新值{{countNewV}}</h1>
    <h1>compunted算计:{{double}}</h1>
    <button @click="add">改变count值+1</button>

    <h1> {{greetings}}</h1>
    <button @click="updateGreetings">为hello添加 world</button>

    <h1>页面坐标x:{{x}} --- 页面坐标y:{{y}}</h1>

    <span v-if="loaded"><h1>请求数据:</h1>  {{result}}</span>

    <h1>{{error}}</h1>

<h1>vue3 -Suspense新组件</h1>
    <Suspense>
      <template #default>
        <test-axios />
      </template>
      <template #fallback>
        <h1>....loading</h1>
      </template>
    </Suspense>
  </div>

</template>

<script lang="ts">
import {
  ref,
  computed,
  reactive,
  defineComponent,
  toRefs,
  watch,
  onErrorCaptured,
} from "vue";
import useMousePosition from "./hooks/useMousePosition";
import useURLLoader from "./hooks/useURLLoader";
import TestAxios from "./components/testAxios.vue";
interface DogResult {
  code: number;
  data: [];
}
export default defineComponent({
  name: "App",
  components: {
    TestAxios,
  },
  setup() {
    const error = ref(null);
    onErrorCaptured((e: any) => {
      error.value = e;
      return true;
    });
    const data: any = reactive({
      count: 0,
      add: () => {
        data.count++;
      },
      double: computed(() => data.count * 2),
    });
    const { x, y } = useMousePosition();
    const { result, loading, loaded } = useURLLoader<DogResult>("http://www.wudongming.com:3001/api/articlelist?status=1&page=1&limit=1");
    const greetings = ref("hello");
    const updateGreetings = () => {
      greetings.value += "  world";
    };


    // 监听多个
    // watch([greetings, () => data.count], (newV, oldV)
    const countNewV = ref('');
    const countOldV = ref('');
    watch(() => data.count, (newV, oldV) => {
      console.log(newV)
      countNewV.value = newV
      countOldV.value = oldV
      

    });

    const resData = toRefs(data);
    return {
      ...resData,
      greetings,
      updateGreetings,
      x,
      y,
      result,
      loading,
      loaded,
      error,
      countNewV,
      countOldV
    };
  },
});
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
