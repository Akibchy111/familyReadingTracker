<template>
  <div >
    <form id="activityForm">
      <label class="form-Label" for="name-input">Reader:</label>
      <input
        class="form-input"
        type="text"
        placeholder="Username"
        v-model="readingActivity.accountForName"
      />
      <br />

      <label class="form-Label" for="book-name-input">Name of book:</label>
      <input
        class="form-input"
        type="text"
        placeholder="Book title"
        v-model="readingActivity.bookNameToAdd"
      />
      <!-- need to switch to radio with options of all books that user added -->
      <br />

      <label class="form-Label" for="reading-format-input">Format:</label>
      <select
        class="form-input"
        id="format"
        name="format"
        size="1"
        placeholder="--Please choose format--"
        v-model="readingActivity.format"
      >
        <option value="Select A Format" selected>
          --Please choose format--
        </option>
        <option value="Paper">Paper</option>
        <option value="Digital">Digital</option>
        <option value="Audiobook">Audiobook</option>
        <option value="Read-Aloud(Reader)">Read-Aloud(Reader)</option>
        <option value="Read-Aloud(Listener)">Read-Aloud(Listener)</option>
        <option value="Other">Other</option>
      </select>
      <!-- need to switch to radio type which gives choices (maybe) -->
      <br />

      <label class="form-Label" for="time-read-input"
        >Time Spent Reading(in minutes):</label
      >
      <input
        class="form-input"
        type="number"
        placeholder="Time Spent Reading"
        v-model="readingActivity.timeRead"
      />
      <label class="form-Label" for="notes">Notes:</label>
      <textarea
        rows="5"
        cols="30"
        class="notes"
        type="text"
        placeholder=""
        v-model="readingActivity.note"
      /><br />

      <button
        class="saveActivity"
        type="submit"
        id="submit-button"
        v-on:click.prevent="createActivity()"
      >
        Save Activity</button
      ><br />
      <!-- <button class="backButton" type="button" @click="toggleCreateActivity">
        Back
      </button> -->
    </form>
  </div>
</template>

<script>
import ReadingActivityService from "@/services/ReadingActivityService";

export default {
  name: "logActivity",
  data() {
    return {
      readingActivity: {
        accountForName: "",
        userId: null,
        format: "",
        timeRead: "",
        bookNameToAdd: "",
        note: "",
      },
    };
  },

  methods: {
    createActivity() {
      this.readingActivity.userId= this.$store.state.user.id;
      ReadingActivityService.createActivity(this.readingActivity).then(
        (response) => {
          if (response.status === 201) {
            this.$store.commit("SET_ACTIVITY", this.readingActivity);
          }
        }
      );
    },
    resetForm() {
      this.readingActivity = {};
      this.showForm = false;
    },
    toggleCreateActivity() {
      this.$emit("toggleCreateActivity");
    },
  },
};
</script>

<style scoped>
@import url("https://fonts.googleapis.com/css2?family=Nunito:wght@400;500&display=swap");

.form-Label {
  font-family: "Nunito", sans-serif;
  font-weight: bold;
}

#activityForm {
  display: flex;
  flex-direction: column;
  justify-content: space-evenly;
  align-items: center;
  font-size:30px;

  font-family: "Nunito", sans-serif;
  font-weight: bold;
}


.form-input {
  font-family: 'Times New Roman', Times, serif;
}

.saveActivity,
.backButton {
  color: white;
  background-color: #6939c3;
  border: 1px solid #6939c3;
  border-radius: 3px;
  padding: 2px 7px;
  margin: 2px 5px 5px 0px;
  font-size: 15px;
}

.saveActivity:hover,
.backButton:hover {
  color: #6930c3;
  background-color: white;
}
.notes {
  max-width: 250px;
  max-height: 150px;
  min-height: 17px;
}
</style>