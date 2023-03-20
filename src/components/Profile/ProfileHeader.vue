<template>
  <header v-if="me.id">
    <div class="profile-cover-pic">
      <img
        src="https://pbs.twimg.com/profile_banners/1276256128758759429/1676957167/600x200"
      />
    </div>
    <div class="profile-header">
      <div class="profile-actions">
        <div class="profile-actions-image">
          <img
            src="https://scontent.frjh1-1.fna.fbcdn.net/v/t39.30808-6/328890854_6049339775125967_4957679846861641276_n.jpg?_nc_cat=102&ccb=1-7&_nc_sid=09cbfe&_nc_eui2=AeEsEQMGABUL1QCAWvQYGi_1veZIDFRML2C95kgMVEwvYPFgzNY-WuJXHuSh_neYNCPizwJdAuvXfhItdWXbsiED&_nc_ohc=l5XtSvk99Q0AX_CXCpk&_nc_ht=scontent.frjh1-1.fna&oh=00_AfD6dLB0oODbpYkip_xovqirR7ezdp4MZ85UbkilHQBxaA&oe=641DCB5B"
          />
        </div>
        <div class="profile-actions-edit">
          <div
            class="edit-button"
            @click="$store.commit('setEditProfileStatus')"
          >
            Edit profile
          </div>
        </div>
      </div>
      <div class="profile-info">
        <p class="profile-info-name">
          Md Shiblu Ahmed
        </p>
        <span class="profile-info-username">
          shibluahmed01
        </span>
      </div>
      <div class="profile-description">
        I am a font-end developer
      </div>
      <div class="profile-created-at">
        <span>
          <base-icon icon="link" />
          <a href="https://twitter.com/shibluahmed01">Main account</a>
        </span>
        <span>
          <base-icon icon="calendar" />
          Joined 2022
        </span>
      </div>
      <div class="profile-follower-counts">
        <p>
          {{ me.account.followingCount }}
          <span>Following</span>
        </p>
        <p>
          {{ me.account.followerCount }}
          <span>Followers</span>
        </p>
      </div>
    </div>
  </header>
</template>

<script>
import { mapGetters } from "vuex";
import moment from "moment";
import { getMe } from "@/services/api";
import BaseIcon from "@/components/BaseIcon";

export default {
  name: "ProfileHeader",
  components: {
    BaseIcon,
  },
  computed: {
    ...mapGetters({
      getMyProfileId: "getMyProfileId",
      me: "getMe",
    }),
    profileWebsite() {
      return {
        website: new URL(new URL(this.me.profile.website)).host,
        full_website: this.me.profile.website,
      };
    },
    joinedAtDate() {
      return `${moment(this.me.createdAt).format("MMM YYYY")}`;
    },
  },
  async mounted() {
    try {
      const response = await getMe({ id: this.getMyProfileId });
      this.$store.commit("setMe", response.data);
      return;
    } catch (err) {
      this.$notification({
        type: "error",
        message: "Error when fetching user data",
      });
    }
  },
  methods: {
    moment,
  },
};
</script>

<style lang="scss">
@import "@/assets/theme/colors.scss";
.profile {
  &-cover-pic {
    border-bottom: $border-dark;
    img {
      vertical-align: middle;
    }
  }
  &-actions {
    display: flex;
    align-items: center;
    justify-content: space-between;
    &-image {
      width: 130px;
      height: 130px;
      margin-top: -80px;
      img {
        border: 1px solid $color-dark-gray;
        border-radius: 999px;
        width: 100%;
      }
    }
    &-edit {
      .edit-button {
        border-radius: 999px;
        border: 1px solid $color-blue;
        color: $color-blue;
        font-weight: bold;
        font-size: 1rem;
        padding: 1rem;
        cursor: pointer;
        transition: background-color 80ms ease;
        &:hover {
          background-color: rgba($color: $color-blue, $alpha: 0.1);
        }
      }
    }
  }
  &-info {
    margin-top: 1rem;
    &-name {
      color: #fff;
      margin: 0;
      font-weight: bold;
      font-size: 1.5rem;
    }
    &-username {
      font-size: 1.2rem;
      color: $color-dark-gray;
    }
  }
  &-description {
    margin-top: 1rem;
    color: #fff;
  }
  &-created-at {
    margin-top: 1rem;
    display: flex;
    align-items: center;
    color: $color-dark-gray;
    a {
      color: $color-blue;
      &:hover {
        text-decoration: underline;
      }
    }
    span {
      display: flex;
      align-items: center;
      & + span {
        margin-left: 2rem;
      }
      svg {
        fill: $color-dark-gray;
        margin-right: 0.5rem;
        width: 1.2rem;
        height: 1.2rem;
      }
    }
  }
  &-follower-counts {
    display: flex;
    color: #fff;
    cursor: pointer;
    margin-top: 1rem;
    p {
      margin: 0;
      & + p {
        margin-left: 1rem;
      }
      span {
        color: $color-dark-gray;
      }
      &:hover {
        text-decoration: underline;
      }
    }
  }
}
</style>
