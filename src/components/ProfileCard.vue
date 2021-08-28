<script setup>
import { computed } from 'vue'
const props = defineProps({
  profile: Object
})

const dateFormatter = new Intl.DateTimeFormat("en-US", {
  year: 'numeric',
  month: 'short',
  day: 'numeric',
});
const joinedAt = computed(() => {
  if (!props.profile.created_at) {
    return ''
  }
  const date = new Date(props.profile.created_at)

  return dateFormatter.format(date)
})
</script>

<template>
  <div class="profile-card">
    <div class="avatar">
      <img :src="props.profile.avatar_url" alt="" />
    </div>

    <div class="infos">
      <h2 class="name" v-if="props.profile.name">{{ props.profile.name }}</h2>
      <h2 class="name" v-else>@{{ props.profile.login }}</h2>
      <p class="username"><a :href="props.profile.html_url">@{{ props.profile.login }}</a></p>
      <p class="joined_at">Joined {{ joinedAt }}</p>
    </div>

    <div class="bio">
      <p v-if="props.profile.bio">{{ props.profile.bio }}</p>
      <p v-else class="disabled">This profile has no bio</p>
    </div>

    <div class="stats">
      <div class="stat">
        <p class="label">Repos</p>
        <p class="value">{{ props.profile.public_repos }}</p>
      </div>

      <div class="stat">
        <p class="label">Followers</p>
        <p class="value">{{ props.profile.followers }}</p>
      </div>

      <div class="stat">
        <p class="label">Following</p>
        <p class="value">{{ props.profile.following }}</p>
      </div>
    </div>

    <div class="contacts">
      <div class="contact location" :class="{ disabled: !props.profile.location}">
        <div class="icon">
          <img src="../assets/images/icon-location.svg" alt="" />
        </div>
        <p>{{ props.profile.location ?? 'Not Available' }}</p>
      </div>

      <div class="contact blog" :class="{ disabled: !props.profile.blog}">
        <div class="icon">
          <img src="../assets/images/icon-website.svg" alt="" />
        </div>
        <p>
          <a :href="props.profile.blog" 
            v-if="props.profile.blog" 
            target="_blank">
            {{ props.profile.blog }}
          </a>
          <span v-else>Not Available</span>
        </p>
      </div>

      <div class="contact twitter" :class="{ disabled: !props.profile.twitter_username}">
        <div class="icon">
          <img src="../assets/images/icon-twitter.svg" alt="" />
        </div>
        <p>
          <a :href="'https://twitter.com/' + props.profile.twitter_username" 
            v-if="props.profile.twitter_username" 
            target="_blank">
            {{ props.profile.twitter_username }}
          </a>
          <span v-else>Not Available</span>
        </p>
      </div>

      <div class="contact company" :class="{ disabled: !props.profile.company}">
        <div class="icon">
          <img src="../assets/images/icon-company.svg" alt="" />
        </div>
        <p>{{ props.profile.company ?? 'Not Avalaible' }}</p>
      </div>
    </div>

  </div>
</template>

<style>
.profile-card {
  display: grid;
  grid-template-columns: 70px 1fr;
  background: var(--bg-component);
  border-radius: 15px;
  padding: 32px 24px;
  gap: 32px 24px;
}
.profile-card img {
  display: block;
  width: 100%;
  height: auto;
  border-radius: 50%;
}
.profile-card .avatar {
  grid-column: 1;
  grid-row: 1;
  overflow: hidden;
}
.profile-card .infos {
  grid-column: 2;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  justify-content: space-between;
}
.profile-card .infos .name {
  font-size: 1.2em;
  line-height: 1.8em;
}
.profile-card .infos .joined_at {
  color: #697C9A; 
}
body.dark-mode .profile-card .infos .joined_at {
  color: inherit; 
}
.profile-card .infos a,
.profile-card .infos a:hover {
  color: #0079FF;
  text-decoration: none;
}
.profile-card .bio {
  grid-column: 1 / span 2;
}
.profile-card .stats {
  grid-column: 1 / span 2;
  padding: 16px;
  background: var(--bg-component-2);
  display: flex;
  justify-content: space-around;
  align-items: center;
  border-radius: 10px;
}
.profile-card .stat {
  text-align: center;
  flex: 1;
}
.profile-card .stat .label {
  font-size: .9em;
  line-height: 1.2em;
  color: #4B6A9B; 
}
body.dark-mode .profile-card .stat .label {
  color: inherit;
}
.profile-card .stat .value {
  font-size: 1.2em;
  line-height: 1.9em;
  margin-top: .5em;
  font-weight: 700;
}
.profile-card .contacts {
  grid-column: 1 / span 2;
  color: #4B6A9B;
}
body.dark-mode .profile-card .contacts {
  color: inherit;
}
.profile-card  .contact {
  display: flex;
  align-items: center;
  margin-bottom: 16px;
}
.profile-card .contact .icon {
  width: 20px;
  text-align: center;
}
.profile-card .contact img {
  width: auto;
  max-width: 100%;
  max-height: 20px;
  vertical-align: middle;
  color: #4b6a9b; 
}
.profile-card .contact p {
  margin-left: 20px;
}

@media screen and (min-width: 768px) {
  .profile-card {
    padding: 2.6em;
    grid-template-columns: 120px 1fr;
    gap: 2.3em 2.6em;
  }
  .profile-card .infos {
    justify-content: space-around;
  }
  .profile-card .infos .name {
    font-size: 1.7em;
    line-height: 1;
    margin-bottom: .5em;
  }
  .profile-card .stats {
    padding: 1em 2.1em;
  }
  .profile-card .stat {
    text-align: left;
  }
  .profile-card .contacts {
    display: flex;
    flex-flow: row wrap;
  }
  .profile-card .location {
    flex-basis: 50%;
    order: 1;
  }
  .profile-card .twitter {
    flex-basis: 50%;
    order: 2;
  }
  .profile-card .blog {
    flex-basis: 50%;
    order: 3;
  }
  .profile-card .company {
    flex-basis: 50%;
    order: 4;
  }
}

@media screen and (min-width: 1440px) {
  .profile-card {
    padding: 3.2em;
    gap: 2.3em 2.6em;
  }
  .profile-card .avatar {
    grid-row: 1 / span 4;
  }
  .profile-card .bio,
  .profile-card .stats,
  .profile-card .contacts {
    grid-column: 2;
  }
  .profile-card .infos {
    width: 100%;
    flex-flow: row wrap;
    justify-content: flex-start;
    align-items: flex-start;
  }
  .profile-card .infos .name {
    flex-basis: 50%;
    order: 1;
  }
  .profile-card .infos .joined_at {
    flex-basis: 50%;
    order: 2;
    text-align: right;
  }
  .profile-card .infos .username {
    flex-basis: 50%;
    order: 3;
  }
}
</style>