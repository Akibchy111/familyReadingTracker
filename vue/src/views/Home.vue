
<template>
  <div class="home homeNav">
    <div id="leftBar">
      <family-well @toggleBookSection="toggleBookSection"/>
      <div id="bookSection" v-if="bookSectionToggle">
        <div class="bookForm">
          <book-view />
          <book-list />
        </div>
      </div>
    </div>
    <div id="reading">
      <h2>Reading Activity</h2><br/>
      <h3 id="greeting">Welcome back, {{$store.state.user.username}}!</h3>
      <reading-activity-view/>
    </div>
      
    <div id="prizes">
      <h2 class="prizeTitle">Prizes</h2>

      <!-- <router-link :to="{ name: 'AddNewPrize' }">Create New Prize</router-link> -->

      <div class="prizeSection">
        <prize-view />
        <prize-list />
      </div>
    </div>
  </div>
</template>

<script>
//import familiesService from '@/services/FamiliesService.js'
import bookService from "@/services/BookService.js";
import BookList from "../components/BookList.vue";
import FamilyWell from "@/components/FamilyWell.vue";
import BookView from "@/components/BookView.vue";
import PrizeList from "../components/PrizeList.vue";
import PrizeView from "../components/PrizeView.vue";
import ReadingActivityView from '../components/ReadingActivityView.vue';

export default {
  components: {
    BookList,
    FamilyWell,
    BookView,
    PrizeList,
    PrizeView,
    ReadingActivityView,
  },
  name: "home",
  data() {
    return {
      bookSectionToggle: true,
    }
  },
  methods: {
    addBook() {
      const dummyBook = {
        title: "newBook",
        author: "genius",
        isbn: "laskjdf",
      };
      bookService.addBook(dummyBook).then((response) => {
        console.log(response.data);
      });
    },
    toggleBookSection() {
      this.bookSectionToggle = !this.bookSectionToggle
    }
  },
  beforeMount() {
    this.$store.commit("CLEAR_FAMILY");
  },
};
</script>
<style scoped>
@import url("https://fonts.googleapis.com/css2?family=Nunito:wght@400;500&display=swap");

a {
  text-decoration: none;
  color: white;
}

a:hover {
  color: #6939c3;
}

h2 {
  font-family: "Nunito", sans-serif;
  text-align: center;
  margin: 20px;
}

#greeting {
  font-family: "Nunito", sans-serif;
  margin: 20px;
  font-size: 2vw;
}

.prizeTitle {
  background-color: #f1d78f;
  border-radius: 20px 0px 0px 20px;
  margin: 10px 0px 5px 10px;
  padding: 24px;
}

.prizeSection {
  flex-grow: 1;
  margin: 5px 0px 10px 10px;
  border-radius: 20px 0px 0px 20px;
  background-color: #f1d78f;
  /* color: #6939c3; */
}

#bookSection {
  flex-grow: 1;
  margin: 5px 10px 10px 0px;
  border-radius: 0px 20px 20px 0px;
  background-color: #f1d78f;
  /* background-image: url("../assets/booksection.jpg");
  background-repeat: no-repeat;
  background-size: cover;
  border-radius: 20px; */
}

.home {
  font-family: "Nunito", sans-serif;
}

.homeNav {
  display: grid;
  column-gap: 5px;
  height: 100vh;
  grid-template-columns: 1fr 1fr 1fr 1fr;
  grid-template-areas: "leftNav readingNav readingNav prizeNav";
}

#leftBar {
  grid-area: leftNav;
  display: flex;
  border: none;
  border-radius: 0px 20px 0px 0px;
  background-color: #6939c3;
  font-size: 35px;
  justify-content: flex-start;
  flex-direction: column;
}

#reading {
  grid-area: readingNav;
  display: flex;
  border: none;
  border-radius: 20px;
  justify-content: flex-start;
  background-color: #53a6d9;
  font-size: 35px;
  flex-direction: column;

  grid-template-areas: "h2 h2"
                       ". ."
                       "h3 h3"
                       ". ."
                      "readingBankBar readingBankBar"
                      ". logReadings"
                      ". logReadings";

}

#reading > h3 {
  display: grid;
  text-align: center;

  grid-template-columns: 1fr 1fr;
  grid-template-areas: "greeting logActivity";
}

#prizes {
  grid-area: prizeNav;
  display: flex;
  border: none;
  border-radius: 20px 0px 0px 0px;
  background-color: #6939c3;
  font-size: 35px;
  justify-content: flex-start;
  flex-direction: column;
}

#createPrizeButton {
  color: white;
  background-color: #6939c3;
  border: 1px solid #6939c3;
  border-radius: 5px;
  padding: 2px 7px;
  margin: 2px 5px 5px 0px;
  font-size: 16px;
}
/* .bookForm{
  background-image: url("../assets/booksection.jpg");
  background-repeat: no-repeat;
  background-size: cover;
  border-radius: 20px;
} */
</style>
