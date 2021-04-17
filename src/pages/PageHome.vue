<template>
  <!--<q-page class="flex flex-center">-->
  <q-page class="relative-position">
    <q-scroll-area class="absolute fullscreen">
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
                <transition-group
          appear
          enter-active-class="animated fadeIn slower"
          leave-active-class="animated fadeOut slower"
        >
          <q-item v-for="tweet in tweets" :key="tweet.date" class="tweet q-py-md">
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
                <span class="text-grey-7"
                  >@reazzondj<br class="lt-md"/>&bull;
                  {{ tweet.date | relativeDate }}</span
                >
              </q-item-label>
              <q-item-label class="tweet-content text-body1"
                >{{ tweet.content }}
              </q-item-label>
              <div class="tweet-icons row justify-between q-mt-sm">
                <q-btn
                  flat
                  round
                  color="grey"
                  size="sm"
                  icon="far fa-comment"
                />
                <q-btn
                  flat
                  round
                  color="grey"
                  size="sm"
                  icon="fas fa-retweet"
                />
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
          </q-item>
        </transition-group>
      </q-list>
    </q-scroll-area>
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
          content: "fds lá os tweets",
          date: 1618617219617
        },
        {
          content: "Conteúdo de tweet bastaten grande para ogrigar a abrir div",
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
      this.newTweetContent = "";
    },
    deleteTweet(tweet) {
      console.log("Delete tweet", tweet);
      let dateToDelete = tweet.date;
      let index = this.tweets.findIndex(tweet => tweet.date === dateToDelete);
      console.log("index: ", index);
      this.tweets.splice(index, 1);
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

.tweet:not(:first-child)
  border-top: 1px solid rgba(0,0,0, 0.12)

.tweet-content
  white-space: pre-line

.tweet-icons
  margin-left: -5px
</style>
