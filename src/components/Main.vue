<template>
  <main class="main">
    <button class="main__button" @click="getData">
      {{ isShow ? 'Hide data' : 'Get Data'}}
    </button>
    <List
        v-show="isShow && this.data"
        :data="this.data"
        :load="this.isLoading"
        :error="this.isError"
        :display="this.isDisplay"
    ></List>
  </main>
</template>

<script>
import List from "@/components/List";
import simulateAsyncReq from "@/plugins/getDataFunc";
import {payload} from "@/mocData";

export default {
  name: "Main",
  components: {List},
  data() {
    return {
      isShow: false,
      isLoading: true,
      isError: false,
      isDisplay: false,
      data: [],
    };
  },
  methods: {
    async getData() {
      this.isShow = !this.isShow;

      simulateAsyncReq(payload)
        .then((result) => {
          if (this.data.length === 0) {
            for (let i = 0; i < result.stocks.length; i++) {
              this.data.push({
                name: result.stocks[i],
                current: result.current[i],
                change: (result.start[i] - result.current[i]).toFixed(2)
              });
            }

            this.data.forEach(element => {
              if (element.change >= 0) {
                element.change = `+${+element.change}`;
              }
            });

            this.data.sort(( a, b ) =>  {
              if (a.name > b.name) {
                return 1;
              }
              if (a.name < b.name) {
                return -1;
              }

              return 0;
            });

          }

          this.isLoading = false;
          this.isDisplay = true;
          this.isError = false;
        })
          .catch(() => {
            this.isLoading = false;
            this.isError = true;
          })
    },
  },
}
</script>

<style scoped lang="scss">
  .main {
    padding: 40px;
    width: 400px;
    margin: 0 auto;
    box-sizing: border-box;
    border: 2px solid #f8f9fa;

    &__button {
      padding: 7px 30px;
      background: #f5f5f5;
      border: 1px solid #eee;
      margin-bottom: 30px;
      font-weight: 600;
    }
  }
</style>
