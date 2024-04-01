<template>
  <div class="window-height">
    <q-layout view="hHh Lpr lff" container class="shadow-2 rounded-borders">
      <q-header class="bg-white">
        <q-toolbar class="toolbar">
          <q-btn flat @click="drawer = !drawer" round dense icon="menu"
            class="q-mr-lg mobile-hide text-black"/>
          <q-img src="/YoutubeLogo.png" class="logo" style="height: 30px; max-width: 150px" />
          <div class="search-container mobile-hide">
            <q-input placeholder="검색" rounded outlined v-model="text" class="search-input">
              <template v-slot:append>
                <q-btn flat round color="grey-7" class="q-mb-md keyboard-btn" icon="keyboard" />
                <q-btn outline rounded color="grey-10 bg-grey-12" class="q-mb-md search-btn"
                  icon="search" />
                </template>
              </q-input>
              <q-btn flat round color="grey-10" class="keyboard-btn" icon="mic" />
            </div>
            <div class="spacer"></div>
            <q-btn outline rounded color="grey-10 bg-grey-12"
              class="mobile-only" icon="search" />
          <div class="right-content mobile-hide">
            <q-btn flat round color="grey-10" class="keyboard-btn" icon="more_vert" />
            <q-btn outline rounded color="primary" icon="account_circle" label="로그인" />
            <q-btn flat round color="grey-10" class="keyboard-btn" icon="invert_colors" />
          </div>
        </q-toolbar>
      </q-header>

      <q-drawer
        v-model="drawer"
        show-if-above

        :mini="!drawer || miniState"
        @click.capture="drawerClick"

        :width="300"
        :breakpoint="500"
        bordered
      >
        <template v-slot:mini>
          <q-scroll-area class="fit mini-slot cursor-pointer">
            <div class="q-py-lg">
              <div class="column items-start">
                <q-icon name="home" class="mini-icon self-center" />
                <span class="self-center q-pb-md">홈</span>
                <q-icon name="music_video" class="mini-icon self-center" />
                <span class="self-center q-pb-md">shorts</span>
                <q-icon name="ondemand_video" class="mini-icon self-center" />
                <span class="self-center q-pb-md">구독</span>
                <q-icon name="video_library" class="mini-icon self-center" />
                <span class="self-center q-pb-md">시청기록</span>
                <q-icon name="history" class="mini-icon self-center" />
                <span class="self-center q-pb-md">시청 기록</span>
              </div>
            </div>
          </q-scroll-area>
        </template>

        <div class="q-mini-drawer-hide absolute" style="top: 15px; right: -17px">
          <q-btn
            dense
            round
            unelevated
            color="accent"
            icon="chevron_left"
            @click="miniState = true"
          />
        </div>
      </q-drawer>

      <q-page-container>
        <q-page class="q-px-lg q-py-md">
          <div class="q-pa-md row items-start q-gutter-md justify-center">
            <template v-for="(card, index) in videoCards" :key="index">
              <q-card class="my-card q-mr-lg" flat bordered>
                <q-img class="yt-img" :src="card.imgSrc" />
                <q-card-section class="row">
                  <q-avatar>
                    <img :src="card.avatarSrc">
                  </q-avatar>
                  <div class="q-pl-md">
                    <div class="text-h5 q-mb-xs">{{ card.title }}</div>
                    <div class="text-body2 text-grey-8">{{ card.channel }}</div>
                    <div class="text-body2 text-grey-8">{{ card.views }}</div>
                  </div>
                </q-card-section>
              </q-card>
            </template>
          </div>
        </q-page>
      </q-page-container>
    </q-layout>
  </div>
</template>

<script>
import { ref } from 'vue';

export default {
  setup() {
    const miniState = ref(true);
    const videoCards = [
      {
        imgSrc: 'https://cdn.quasar.dev/img/mountains.jpg',
        avatarSrc: 'https://cdn.quasar.dev/img/avatar.png',
        title: '뉴스1',
        channel: '뉴스채널1',
        views: '100회',
      },
      {
        imgSrc: 'https://cdn.quasar.dev/img/mountains.jpg',
        avatarSrc: 'https://cdn.quasar.dev/img/avatar.png',
        title: '뉴스2',
        channel: '뉴스채널2',
        views: '321회',
      },
      {
        imgSrc: 'https://cdn.quasar.dev/img/mountains.jpg',
        avatarSrc: 'https://cdn.quasar.dev/img/avatar.png',
        title: '뉴스3',
        channel: '뉴스채널3',
        views: '231회',
      },
      {
        imgSrc: 'https://cdn.quasar.dev/img/mountains.jpg',
        avatarSrc: 'https://cdn.quasar.dev/img/avatar.png',
        title: '뉴스4',
        channel: '뉴스채널4',
        views: '111회',
      },
      {
        imgSrc: 'https://cdn.quasar.dev/img/mountains.jpg',
        avatarSrc: 'https://cdn.quasar.dev/img/avatar.png',
        title: '뉴스4',
        channel: '뉴스채널4',
        views: '111회',
      },
      {
        imgSrc: 'https://cdn.quasar.dev/img/parallax2.jpg',
        avatarSrc: 'https://cdn.quasar.dev/img/avatar.png',
        title: '노래1',
        channel: '노래채널1',
        views: '22회',
      },
      {
        imgSrc: 'https://cdn.quasar.dev/img/parallax2.jpg',
        avatarSrc: 'https://cdn.quasar.dev/img/avatar.png',
        title: '노래2',
        channel: '노래채널2',
        views: '12회',
      },
      {
        imgSrc: 'https://cdn.quasar.dev/img/parallax1.jpg',
        avatarSrc: 'https://cdn.quasar.dev/img/avatar.png',
        title: '게임1',
        channel: '게임채널2',
        views: '131회',
      },
    ];

    return {
      drawer: ref(false),
      miniState,
      videoCards,

      drawerClick(e) {
        // if in "mini" state and user
        // click on drawer, we switch it to "normal" mode
        if (miniState.value) {
          miniState.value = false;

          // notice we have registered an event with capture flag;
          // we need to stop further propagation as this click is
          // intended for switching drawer to "normal" mode only
          e.stopPropagation();
        }
      },
    };
  },
};
</script>

<style lang="scss">
.q-drawer {
  width: 65px !important;
  border-right: none !important;
}

.q-field__control {
  height: 38px;
}

.toolbar {
  display: flex;
  justify-content: space-between;
}

.search-container {
  display: flex;
  align-items: center; /* Center vertically */
  position: relative;
  left: 10%;
}

.search-input {
  width: 600px;
}

.right-content {
  display: flex;
  align-items: center; /* Center vertically */
}

.spacer {
  flex-grow: 1; /* Takes up remaining space */
}

.keyboard-btn {
  margin-right: 11px;
}

.search-btn {
  position: relative;
  left: 11px;
  bottom: .5px;
}

.search-btn::before {
  border: 1px solid #dbdbdb;
}

.my-card {
  width: 100%;
  max-width: 350px;
}

.yt-img {
  border-radius: 10px !important;
  width: 105% !important;
  height: 210px !important;
}

.q-card__section--vert {
  padding-left: 0 !important;
}

.q-card--bordered {
  border: none !important;
}

.mini-slot {
  transition: background-color .28s;
  &:hover {
    background-color: rgba(0, 0, 0, .04);
  }
}

.mini-icon {
  font-size: 1.718em;
  padding: 2px 16px;
  & + & {
    margin-top: 18px;
  }
}

.logo .q-img__container > img {
  object-fit: inherit !important;
}
</style>
