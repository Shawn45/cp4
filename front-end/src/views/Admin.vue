<template>

  <div class="admin">

    <h1>Journal Manager</h1>

    <div class="add-area">
      <div class="heading">
        <h2>Add new Event</h2>
      </div>
      <div class="add">
        <div class="form">
          <input v-model="title" placeholder="Title">
          <p></p>
          <input v-model="location" placeholder="Location">
          <p></p>
          <input v-model="date" placeholder="Date">
          <p></p>
          <input v-model="people" placeholder="People">
          <p></p>
          <textarea rows="5" cols="30" v-model="description" placeholder="Description"></textarea>
          <p></p>
          <input type="file" name="photo" @change="fileChanged">
          <button @click="upload">Upload</button>
        </div>
        <div class="upload" v-if="addItem">
          <h2>{{addItem.title}}</h2>
          <h2>{{addItem.location}}</h2>
          <h2>{{addItem.date}}</h2>
          <h2>{{addItem.people}}</h2>
          <img :src="addItem.path" />
          <p>{{addItem.description}}</p>
        </div>
      </div>
    </div>


    <div class="edit-area">
      <div class="heading">
        <h2>Edit/Delete an Event</h2>
      </div>
      <div class="edit">
        <div class="form">
          <input v-model="findTitle" placeholder="Search">
          <div class="suggestions" v-if="suggestions.length > 0">
            <div class="suggestion" v-for="s in suggestions" :key="s.id" @click="selectItem(s)">{{s.title}}</div>
          </div>
        </div>
        <div class="upload" v-if="findItem">
          <input v-model="findItem.title">
          <p></p>
          <input v-model="findItem.location">
          <p></p>
          <input v-model="findItem.date">
          <p></p>
          <input v-model="findItem.people">
          <p></p>
          <img :src="findItem.path" />
          <p></p>
          <textarea rows="5" cols="30" v-model="findItem.description"></textarea>
          <p></p>
        </div>
        <div class="actions" v-if="findItem">
          <button @click="deleteItem(findItem)">Delete</button>
          <button @click="editItem(findItem)">Edit</button>
        </div>
      </div>

    </div>



  </div>
</template>

<script>
import axios from 'axios';
export default {
  name: 'Admin',
  data() {
    return {
      title: "",
      file: null,
      description:"",
      location:"",
      date:"",
      people:"",
      addItem: null,
      items: [],
      findTitle: "",
      findItem: null,
      findDescription:"",
      findLocation: "",
      findDate: "",
      findPeople: "",
    }
  },

  computed: {
    suggestions() {
      let items = this.items.filter(item => item.title.toLowerCase().startsWith(this.findTitle.toLowerCase()));
      return items.sort((a, b) => a.title > b.title);
    }
  },

  created() {
    this.getItems();
  },

  methods: {
    fileChanged(event) {
      this.file = event.target.files[0]
    },

    async getItems() {
      try {
        let response = await axios.get("/api/items");
        this.items = response.data;
        return true;
      } catch (error) {
        console.log(error);
      }
    },

    selectItem(item) {
      this.findTitle = "";
      this.findItem = item;
      this.findDescription ="";
      this.findLocation="";
      this.findDate="";
      this.findPeople="";
    },

    async deleteItem(item) {
      try {
        await axios.delete("/api/items/" + item._id);
        this.findItem = null;
        this.getItems();
        return true;
      } catch (error) {
        console.log(error);
      }
    },

    async upload() {
        try {
          const formData = new FormData();
          formData.append('photo', this.file, this.file.name)
          let r1 = await axios.post('/api/photos', formData);
          let r2 = await axios.post('/api/items', {
            title: this.title,
            path: r1.data.path,
            description: this.description,
            location: this.location,
            date: this.date,
            people: this.people,
          });
          this.addItem = r2.data;

        } catch (error) {
          console.log(error);
        }

        window.location.reload();
      },


    async editItem(item) {
        try {
          await axios.put("/api/items/" + item._id, {
            title: this.findItem.title,
            description: this.findItem.description,
            location: this.findItem.location,
            date: this.findItem.date,
            people: this.findItem.people,
          });
          this.findItem = null;
          this.getItems();
          return true;
        } catch (error) {
          console.log(error);
        }
      },
  },
}
</script>



<style scoped>
.add-area {
  margin-bottom: 40px;
}

.image h2 {
  font-style: italic;
  font-size: 1em;
}

.heading {
  display: flex;
  margin-bottom: 10px;
  margin-top: 20px;
}

.heading h2 {
  margin-top: 8px;
  margin-left: 10px;
}

.add,
.edit {
  display: flex;
}

.circle {
  border-radius: 50%;
  width: 18px;
  height: 18px;
  padding: 8px;
  background: #333;
  color: #fff;
  text-align: center
}

/* Form */
input,
textarea,
select,
button {
  font-family: 'Montserrat', sans-serif;
  font-size: 1em;
}

.form {
  margin-right: 50px;
}

/* Uploaded images */
.upload h2 {
  margin: 0px;
}

.upload img {
  max-width: 300px;
}

/* Suggestions */
.suggestions {
  width: 200px;
  border: 1px solid #ccc;
}

.suggestion {
  min-height: 20px;
}

.suggestion:hover {
  background-color: #5BDEFF;
  color: #fff;
}


</style>
