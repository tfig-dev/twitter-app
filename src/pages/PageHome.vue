<template>
  <!--<q-page class="flex flex-center">-->
  <q-page>
    <div class="q-py-lg q-px-md row items-end q-col-gutter-sm">
      <div class="col">
        <q-input
          bottom-slots
          v-model="newTweetContent"
          class="new-tweet"
          placeholder="What's happening?"
          counter
          maxlength="1500"
          autogrow
        >
          <template v-slot:before>
            <q-avatar size="xl">
              <img src="https://cdn.quasar.dev/img/avatar5.jpg" />
            </q-avatar>
          </template>
        </q-input>
      </div>
      <div class="col col-shrink">
        <q-btn
          @click="addnewTweet"
          :disable="!newTweetContent"
          class="q-mb-lg"
          unelevated
          rounded
          color="primary"
          label="Tweet"
          no-caps
        />
      </div>
    </div>
    <q-separator class="divider" size="10px" color="grey-2" />
    <!-- Tweet timeline -->

    <q-list separator>
      <q-item v-for="tweet in tweets" :key="tweet.date" class="q-py-md">
        <q-item-section avatar top>
          <q-avatar size="xl">
            <img src="https://cdn.quasar.dev/img/avatar5.jpg" />
          </q-avatar>
        </q-item-section>

        <q-item-section>
          <q-item-label class="text-subtitle1">
            <strong>
              Tiago Figueiredo
            </strong>
            <span class="text-grey-7">@reazzondj</span>
          </q-item-label>
          <q-item-label class="tweet-content text-body1">{{ tweet.content }}
          </q-item-label>
          <div class="tweet-icons row justify-between q-mt-sm">
            <q-btn flat round color="grey" size="sm" icon="far fa-comment" />
            <q-btn flat round color="grey" size="sm" icon="fas fa-retweet" />
            <q-btn flat round color="grey" size="sm" icon="far fa-heart" />
            <q-btn
              flat
              round
              color="grey"
              size="sm"
              icon="fas fa-trash"
              @click="deleteTweet(tweet)"
            />
          </div>
        </q-item-section>

        <q-item-section side top>
          {{ tweet.date | relativeDate }}
        </q-item-section>
      </q-item>
    </q-list>
  </q-page>
</template>

<script>
import { formatDistance } from "date-fns";

export default {
  name: "PageIndex",
  data() {
    return {
      newTweetContent: "",
      tweets: [
        {
          content: "tweet random",
          date: 1618617230770
        },
        {
          content: "Conteúdo de teste",
          date: 1618617219617
        },
        {
          content: "Conteúdo de teste",
          date: 1618617172943
        }
      ]
    };
  },
  methods: {
    addnewTweet() {
      let newTweet = {
        content: this.newTweetContent,
        date: Date.now()
      };
      this.tweets.unshift(newTweet);
    },
    deleteTweet(tweet) {
      console.log("Delete tweet", tweet);
      let dateToDelete = tweet.date
      let index = this.tweets.findIndex(tweet => tweet.date === dateToDelete)
      console.log("index: ", index)
      this.tweets.splice(index, 1)
    }
  },
  filters: {
    relativeDate(value) {
      return formatDistance(value, new Date());
    }
  }
};
</script>
<style lang="sass">
.new-tweet
  textarea
    font-size: 19px
    line-height: 1.4 !important

.divider
  border-top: 1 px solid
  border-bottom: 1 px solid
  border-color: $grey-4

.tweet-content
  white-space: pre-line

.tweet-icons
  margin-left: -5px
</style>
