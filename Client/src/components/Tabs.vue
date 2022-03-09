<template>
  <div>
    <ul>
      <span class="tab"
            v-for="(tab, index) in tabs"
            @click="selectedTab = tab"
            :class="{ activeTab: selectedTab === tab }"
      >{{ tab }}</span>
      <div v-show="selectedTab === 'Json'">
        <form @submit.prevent="onSubmit">
          <textarea v-model="text">Json here</textarea>
          <button type="submit">Create</button>
        </form>
      </div>
      <div class="table" v-show="selectedTab === 'Table'">
        <table>
          <tr><th>Id</th><th>Value</th></tr>
          <tr v-for="save in saved"><td>{{save.id}}</td><td>{{save.val}}</td></tr> <!--ряд с ячейками тела таблицы-->
        </table>
      </div>
    </ul>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      tabs: ['Json', 'Table'],
      selectedTab: 'Json',
      text: "",
      saved: [
        {
          "id": 1,
          "val": "10"
        },
        {
          "id": 2,
          "val": "15"
        },
        {
          "id": 3,
          "val": "20"
        }
      ]
    }
  },
  methods: {
    onSubmit() {
      if (this.text.trim()) {
        this.sendJson(this.text)
        //this.text = ""
      }
    },
    async sendData() {
      axios.post('http://172.20.10.3:5000/api/json/add', {
        text: "Your legend",
        link: "link",
      })
          .then(function (response) {
            console.log(response)
          })
    },
    async sendJson() {
      try {
        console.log(this.text)
        await fetch('/api/json/add', {
          method: 'POST',
          body: JSON.stringify(this.text),
          headers: {
            'Content-Type': 'application/json',
          },
        })
            .then(response => response.json())
            .then(json => this.saved = json)
      } catch (error) {
        console.log(error)
      }
    }

  }
}
</script>

<style scoped>
.activeTab {
  color: #16C0B0;
  text-decoration: underline;
}
.tab {
  margin-left: 20px;
  cursor: pointer;
}
textarea {
  width: 15rem;
  height: 10rem;
}
form {
  display: flex;
  flex-direction: column;
  align-items: center;
}
button {
  width: 5rem;
  margin-top:2rem;
}
</style>