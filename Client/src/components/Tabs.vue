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
          <div class="error">{{error}}</div>
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
import Ajv from "ajv"

export default {
  data() {
    return {
      tabs: ['Json', 'Table'],
      selectedTab: 'Json',
      text: "",
      jsonData: {},
      error: "",
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
      ],
    }
  },
  methods: {
    onSubmit() {
      if (this.text.trim()) {
        this.sendData(this.text)
      }
    },
    async sendData() {
      if (!this.validateJson()) {
        this.error = "This text is not a valid JSON"
        return
      }
      try {
        await axios
            .post('http://172.20.10.3:5000/api/json/add', JSON.parse(this.text), {
              headers: {
                'Content-Type': 'application/json',
              },
            })
            .then((json) => (this.saved = json.data))
      } catch (error) {
        console.log(error)
      }
    },
    validateJson() {
      try {
        const data = JSON.parse(this.text)
        const ajv = new Ajv()
        const schema = {
          type: "array",
          minItems: 1,
          items : {
            type: "object",
            properties: {
              id: {type: "integer"},
              val: {type: "number"}
            },
            required: ["id", "val"],
            additionalProperties: false,
          }
        }
        const validate = ajv.compile(schema)
        const valid = validate(data)
        if (!valid) console.log(validate.errors)
        else this.error = ""
        return valid
      } catch (error) {
        console.log(error)
      }
    }
  }
}
</script>

<style scoped>
.error {
  color: red;
  margin-top: 1rem;
}
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