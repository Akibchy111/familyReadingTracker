<template>
  <div class="leftbar">
    <no-family-summary
      v-if="!createFamilyToggle && !isInFamily"
      @toggleCreateFamily="toggleCreateFamily"
    />
    <create-family
      v-if="createFamilyToggle && !isInFamily"
      @toggleCreateFamily="toggleCreateFamily"
      @toggleFamilySummary="toggleFamilySummary"
    />
    <family-summary
      v-if="familySummaryToggle || isInFamily"
      @toggleUserList="toggleUserList"
      @toggleMyFamily="toggleMyFamily"
    />
    <users-list
      v-if="userListToggle"
      @AddedUser="markUserUnavailable"
      @toggleUserList="toggleUserList"
      :users="users"
    />
    <family-list :users="users" v-if="myFamilyToggle" />
  </div>
</template>

<script>
import familiesService from "@/services/FamiliesService";
import CreateFamily from "./CreateFamily.vue";
import NoFamilySummary from "./NoFamilySummary.vue";
import FamilySummary from "./FamilySummary.vue";
import UsersList from "./UsersList.vue";
import FamilyList from "./FamilyList.vue";
export default {
  components: {
    CreateFamily,
    NoFamilySummary,
    FamilySummary,
    UsersList,
    FamilyList,
  },
  props: ['bookSectionToggle'],
  data() {
    return {
      createFamilyToggle: false,
      familySummaryToggle: false,
      userListToggle: false,
      myFamilyToggle: false,
      users: [],
    };
  },
  computed: {
    isInFamily() {
      return this.$store.state.family.name != null ? true : false;
    },
  },
  methods: {
    toggleMyFamily() {
      this.refreshUsers();
      if (this.bookSectionToggle == this.myFamilyToggle) {
        this.$emit("toggleBookSection")
      }
    },
    refreshUsers() {
      let i = 0;
      this.users.forEach((user) => {
        familiesService.getFamilyByUser(user.id).then((response) => {
          if (response.status === 200) {
            user.familyId = response.data.familyId;
            i++;
            if (i == this.users.length) {
              this.myFamilyToggle = !this.myFamilyToggle;
            }
          }
        });
      });
    },
    markUserUnavailable(userId, familyId) {
      this.users.forEach((user) => {
        if (userId === user.id) {
          user.familyId = familyId;
        }
      });
    },
    toggleCreateFamily() {
      this.createFamilyToggle = !this.createFamilyToggle;
    },
    toggleFamilySummary() {
      this.familySummaryToggle = true;
      this.createFamilyToggle = false;
    },
    toggleUserList() {
      this.userListToggle = !this.userListToggle;
      this.$emit("toggleBookSection");
    },
  },
  created() {
    familiesService.getUsers().then((response) => {
      if (response.status === 200) {
        let tempUsers = response.data;
        tempUsers.forEach((user) => {
          familiesService.getFamilyByUser(user.id).then((response) => {
            if (response.status === 200) {
              user.familyId = response.data.familyId;
            }
          });
        });
        this.users = tempUsers;
      }
    });
  },
  beforeMount() {
    familiesService
      .getFamilyByUser(this.$store.state.user.id)
      .then((response) => {
        if (response.status === 200) {
          if (response.data.familyName != null) {
            this.$store.commit("ADD_FAMILY", response.data.familyName);
            this.$store.commit("ADD_FAMILY_ROLE", response.data.familyRole);
            this.$store.commit("ADD_FAMILY_ID", response.data.familyId);
          }
        }
      });
  },
};
</script>

<style scoped>
</style>