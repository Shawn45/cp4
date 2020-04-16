<template>
<div class="home">
  <section class="image-gallery">
    <div class="image" v-for="item in items" :key="item.id">
      <h1>{{item.title}}</h1>
      <img :src="item.path" />
      <div class="location-date-people_box">
        <p>{{item.location}}</p>
        <p>{{item.date}}</p>
        <p>{{item.people}}</p>
      </div>

      <div class="description_box">
        <p>{{item.description}}</p>
      </div>

    </div>
  </section>
</div>
</template>



<script>
// @ is an alias to /src
import axios from 'axios';
export default {
  name: 'Home',
  data() {
    return {
     items: [],
    }
  },

  created() {
    this.getItems();
  },

  methods: {
    async getItems() {
      try {
        let response = await axios.get("/api/items");
        this.items = response.data;
        return true;
      } catch (error) {
        console.log(error);
      }
    },
  }

}
</script>

<style scoped>
.image h4 {
  font-style: italic;
}

h1 {
  text-align: center;
}

/* Masonry */
*,
*:before,
*:after {
  box-sizing: inherit;
}

.image-gallery {
  column-gap: 1.5em;
}

.image {
  margin: auto;
  display: block;
  max-width: 50%;
  padding-bottom: 1em;
}

.image img {
  width: 100%;
}

/* Masonry on large screens */
@media only screen and (min-width: 1024px) {
  .image-gallery {
    column-count: 1;
  }
}

/* Masonry on medium-sized screens */
@media only screen and (max-width: 1023px) and (min-width: 768px) {
  .image-gallery {
    column-count: 1;
  }
}

/* Masonry on small screens */
@media only screen and (max-width: 767px) and (min-width: 540px) {
  .image-gallery {
    column-count: 1;
  }
}

.location-date-people_box {
  display: block;
  justify-content: flex-start | space-between;
  font-weight: bold;
  color: 	#606060;
}

.description_box {
  color: 	#606060;
}
</style>
