<template>
  <div class="max-w-7xl mx-auto grid grid-cols-4 gap-4">
    <!-- Left: Profile -->
    <div class="main-left col-span-1">
      <div class="p-4 bg-white border border-gray-200 text-center rounded-lg">
        <img :src="user.get_avatar" class="mb-6 rounded-full" />

        <p><strong>{{ user.name }}</strong></p>

        <div class="mt-6 flex space-x-8 justify-around">
          <p class="text-xs text-gray-500">{{ user.friends_count }} friends</p>
          <p class="text-xs text-gray-500">{{ user.posts_count }} posts</p>
        </div>

        <!-- Send Friend Request button -->
        <div v-if="showSendRequestButton" class="mt-6">
          <button
            class="inline-block py-2 px-4 bg-purple-600 text-white rounded-lg"
            @click="sendFriendRequest(user.id)"
          >
            Send Friend Request
          </button>
        </div>
      </div>
    </div>

    <!-- Center: Requests & Friends -->
    <div class="main-center col-span-2 space-y-4">
      <!-- Friendship Requests -->
      <div
        class="p-4 bg-white border border-gray-200 rounded-lg"
        v-if="friendshipRequests.length"
      >
        <h2 class="mb-6 text-xl">Friendship requests</h2>

        <div
          class="p-4 text-center bg-gray-100 rounded-lg"
          v-for="friendshipRequest in friendshipRequests"
          :key="friendshipRequest.id"
        >
          <img
            :src="friendshipRequest.created_by.get_avatar"
            class="mb-6 mx-auto rounded-full"
          />

          <p>
            <strong>
              <RouterLink
                :to="{
                  name: 'profile',
                  params: { id: friendshipRequest.created_by.id }
                }"
              >
                {{ friendshipRequest.created_by.name }}
              </RouterLink>
            </strong>
          </p>

          <div class="mt-6 flex space-x-8 justify-around">
            <p class="text-xs text-gray-500">
              {{ friendshipRequest.created_by.friends_count }} friends
            </p>
            <p class="text-xs text-gray-500">
              {{ friendshipRequest.created_by.posts_count }} posts
            </p>
          </div>

          <div class="mt-6 space-x-4">
            <button
              class="inline-block py-2 px-4 bg-purple-600 text-white rounded-lg"
              @click="handleRequest('accepted', friendshipRequest.created_by.id)"
            >
              Accept
            </button>
            <button
              class="inline-block py-2 px-4 bg-red-600 text-white rounded-lg"
              @click="handleRequest('rejected', friendshipRequest.created_by.id)"
            >
              Reject
            </button>
          </div>
        </div>

        <hr />
      </div>

      <!-- Friends list -->
      <div
        class="p-4 bg-white border border-gray-200 rounded-lg grid grid-cols-2 gap-4"
        v-if="friends.length"
      >
        <div
          class="p-4 text-center bg-gray-100 rounded-lg"
          v-for="f in friends"
          :key="f.id"
        >
          <img :src="f.get_avatar" class="mb-6 rounded-full" />

          <p>
            <strong>
              <RouterLink :to="{ name: 'profile', params: { id: f.id } }">
                {{ f.name }}
              </RouterLink>
            </strong>
          </p>

          <div class="mt-6 flex space-x-8 justify-around">
            <p class="text-xs text-gray-500">{{ f.friends_count }} friends</p>
            <p class="text-xs text-gray-500">{{ f.posts_count }} posts</p>
          </div>
        </div>
      </div>
    </div>

    <!-- Right: Suggestions -->
    <div class="main-right col-span-1 space-y-4">
      <PeopleYouMayKnow />
      <Trends />
    </div>
  </div>
</template>

<script>
import axios from "axios";
import PeopleYouMayKnow from "../components/PeopleYouMayKnow.vue";
import Trends from "../components/Trends.vue";
import { useUserStore } from "@/stores/user";

export default {
  name: "FriendsView",

  setup() {
    const userStore = useUserStore();
    return { userStore };
  },

  components: {
    PeopleYouMayKnow,
    Trends
  },

  data() {
    return {
      user: {},
      friendshipRequests: [],
      friends: [],
      isFriendFlag: false,
      hasPendingRequestFlag: false
    };
  },

  computed: {
    isFriend() {
      return this.isFriendFlag;
    },
    hasPendingRequest() {
      return this.hasPendingRequestFlag;
    },
    isOwnProfile() {
      return this.userStore.user.id === this.user.id;
    },
    showSendRequestButton() {
      return !this.isOwnProfile && !this.isFriend && !this.hasPendingRequest;
    }
  },

  mounted() {
    this.getFriends();
  },

  methods: {
    getFriends() {
      axios
        .get(`/api/friends/${this.$route.params.id}/`)
        .then((response) => {
          this.friendshipRequests = response.data.requests;
          this.friends = response.data.friends;
          this.user = response.data.user;
          this.isFriendFlag = response.data.is_friend;
          this.hasPendingRequestFlag = response.data.has_pending_request;
        })
        .catch((error) => {
          console.log("error", error);
        });
    },

    handleRequest(status, pk) {
      axios
        .post(`/api/friends/${pk}/${status}/`)
        .then(() => {
          this.getFriends();
        })
        .catch((error) => {
          console.log("error", error);
        });
    },

    sendFriendRequest(pk) {
      axios
        .post(`/api/friends/${pk}/request/`)
        .then(() => {
          this.getFriends();
        })
        .catch((error) => {
          console.error(error);
        });
    }
  }
};
</script>
