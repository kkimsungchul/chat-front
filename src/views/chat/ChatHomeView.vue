<template>
  <div>
    <input id="customFile" type="file" @change="handleFileChange" />
    <button @click="chat">분석</button>
    <br />
    <br />
    <div>
      <div>
        대화한 사용자 수 :
        {{ memberCount }}
        <br />
        총 대화 횟수
        {{ totalLine }}
      </div>
      <br />
      <div :style="styleDisplay">
        <div style="width: 30%; display: block">
          <b>대화한 사용자</b>
          <br />
          <table>
            <tr>
              <th>순번</th>
              <th>사용자명</th>
            </tr>
            <tr :key="i" v-for="(mList, i) in memberList">
              <td>{{ i }}</td>
              <td>
                {{ mList }}
              </td>
            </tr>
          </table>
        </div>

        <div style="width: 30%; display: block">
          <b>대화한 횟수</b>
          <br />
          <table>
            <tr>
              <th>순위</th>
              <th>사용자명</th>
              <th>대화횟수 (대화지분율)</th>
            </tr>
            <tr :key="i" v-for="(tList, i) in tokeList">
              <td>{{ i }}</td>
              <td>{{ Object.keys(tList)[0] }}</td>
              <td>
                {{ tList[Object.keys(tList)[0]] }} (
                {{
                  ((tList[Object.keys(tList)[0]] / totalLine) * 100).toFixed(3)
                }}%)
              </td>
            </tr>
          </table>
        </div>
        <div style="width: 30%; display: block">
          <b>쫓아낸 사용자 수</b>
          <br />
          <table>
            <tr>
              <th>순위</th>
              <th>사용자명</th>
              <th>쫓아낸 횟수</th>
            </tr>
            <tr :key="i" v-for="(kList, i) in killList">
              <td>{{ i }}</td>
              <td>{{ Object.keys(kList)[0] }}</td>
              <td>{{ kList[Object.keys(kList)[0]] }}</td>
            </tr>
          </table>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import axios from "axios";
export default {
  components: {},
  data() {
    return {
      resultData: {},
      file: "",
      fileName: "",
      memberCount: 0,
      totalLine: 0,
      memberList: [],
      killList: [],
      tokeList: [],
      obj: {
        name: "김성철",
        age: "18",
      },
      styleDisplay: {
        display: "none",
        justifyContent: "center",
      },
    };
  },

  methods: {
    async chat() {
      var submitData = new FormData();
      submitData.append("file", this.file);
      this.resultData = await (
        await axios.post("http://3.39.177.150:8000/chat", submitData, {
          headers: {
            "Content-Type": "multipart/form-data",
          },
        })
      ).data.data;

      this.memberCount = this.resultData.memberCount;
      this.totalLine = this.resultData.totalLine;
      this.memberList = this.resultData.memberList;
      this.killList = this.resultData.killList;
      this.tokeList = this.resultData.tokeList;
      this.styleDisplay.display = "flex";
    },

    handleFileChange(e) {
      let file = e.target.files[0];
      let name = file.name;
      this.fileName = file.name;
      this.file = file;
    },
  },
  setup() {},

  created() {},

  mounted() {},

  unmounted() {},
};
</script>
<style scoped>
table {
  border: 1px solid #444444;
  margin: auto;
}
th,
td {
  border: 1px solid #444444;
}
</style>
