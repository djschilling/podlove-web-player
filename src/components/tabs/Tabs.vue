<template>
  <div class="tabs" :style="containerStyle" v-if="hasTabs" id="tabs">
    <tab-header-component>
      <tab-header-item-component v-if="isVisibleTab.info" name="info" :active="tabs.info" :click="toggleTab('info')">
        <info-icon slot="icon"></info-icon>
        <span slot="title">{{ $t('INFO.TITLE') }}</span>
      </tab-header-item-component>
      <tab-header-item-component v-if="isVisibleTab.chapters" name="chapters" :active="tabs.chapters" :click="toggleTab('chapters')">
        <chapters-icon slot="icon"></chapters-icon>
        <span slot="title">{{ $t('CHAPTERS.TITLE') }}</span>
      </tab-header-item-component>
      <tab-header-item-component v-if="isVisibleTab.transcripts" name="transcripts" :active="tabs.transcripts" :click="toggleTab('transcripts')">
        <transcripts-icon slot="icon"></transcripts-icon>
        <span slot="title">{{ $t('TRANSCRIPTS.TITLE') }}</span>
      </tab-header-item-component>
      <tab-header-item-component v-if="isVisibleTab.share" name="share" :active="tabs.share" :click="toggleTab('share')">
        <share-icon slot="icon"></share-icon>
        <span slot="title">{{ $t('SHARE.TITLE') }}</span>
      </tab-header-item-component>
      <tab-header-item-component v-if="isVisibleTab.download" name="download" :active="tabs.download" :click="toggleTab('download')">
        <download-icon slot="icon"></download-icon>
        <span slot="title">{{ $t('DOWNLOAD.TITLE') }}</span>
      </tab-header-item-component>
      <tab-header-item-component v-if="isVisibleTab.audio" name="audio" :active="tabs.audio" :click="toggleTab('audio')">
        <audio-icon slot="icon"></audio-icon>
        <span slot="title">{{ $t('AUDIO.TITLE') }}</span>
      </tab-header-item-component>
    </tab-header-component>

    <tab-body-component :active="tabs.info" name="info" :aria-selected="tabs.info" ref="info">
      <info-tab v-if="isVisibleTab.info && tabs.info"></info-tab>
    </tab-body-component>
    <tab-body-component :active="tabs.chapters" name="chapters" :aria-selected="tabs.chapters" ref="chapters">
      <chapters-tab v-if="isVisibleTab.chapters && tabs.chapters"></chapters-tab>
    </tab-body-component>
    <tab-body-component :active="tabs.transcripts" class="fixed" name="transcripts" :aria-selected="tabs.transcripts" ref="transcripts">
      <transcripts-tab v-if="isVisibleTab.transcripts && tabs.transcripts"></transcripts-tab>
    </tab-body-component>
    <tab-body-component :active="tabs.share" name="share" :aria-selected="tabs.share" ref="share">
      <share-tab v-if="isVisibleTab.share && tabs.share"></share-tab>
    </tab-body-component>
    <tab-body-component :active="tabs.download" name="download" :aria-selected="tabs.download" ref="download">
      <download-tab v-if="isVisibleTab.download && tabs.download"></download-tab>
    </tab-body-component>
    <tab-body-component :active="tabs.audio" name="audio" :aria-selected="tabs.audio" ref="audio">
      <audio-tab v-if="isVisibleTab.audio && tabs.audio"></audio-tab>
    </tab-body-component>
  </div>
</template>

<script>
import store from 'store'

import TabHeaderComponent from 'shared/TabHeader'
import TabHeaderItemComponent from 'shared/TabHeaderItem'
import TabBodyComponent from 'shared/TabBody'

import ChaptersIcon from 'icons/ChaptersIcon'
import ShareIcon from 'icons/ShareIcon'
import DownloadIcon from 'icons/DownloadIcon'
import InfoIcon from 'icons/InfoIcon'
import AudioIcon from 'icons/AudioIcon'
import TranscriptsIcon from 'icons/TranscriptsIcon'

import InfoTab from './info/Info'
import ShareTab from './share/Share'
import ChaptersTab from './chapters/Chapters'
import TranscriptsTab from './transcripts/Transcripts'
import DownloadTab from './download/Download'
import AudioTab from './audio/Audio'

export default {
  data () {
    return {
      theme: this.$select('theme'),
      tabs: this.$select('tabs'),
      chapters: this.$select('chapters'),
      reference: this.$select('reference'),
      visibleComponents: this.$select('visibleComponents'),
      components: this.$select('components')
    }
  },
  computed: {
    containerStyle () {
      return {
        'background-color': this.theme.tabs.body.background
      }
    },
    isVisibleTab () {
      return {
        info: this.components.tabs.info && this.visibleComponents.tabInfo,
        chapters: this.components.tabs.chapters && this.visibleComponents.tabChapters,
        share: this.components.tabs.share && this.visibleComponents.tabShare,
        download: this.components.tabs.download && this.visibleComponents.tabDownload,
        transcripts: this.components.tabs.transcripts && this.visibleComponents.tabTranscripts,
        audio: this.components.tabs.audio && this.visibleComponents.tabAudio
      }
    },
    hasTabs () {
      return Object.keys(this.isVisibleTab)
        .reduce((result, tab) => {
          if (result) {
            return true
          }

          return this.isVisibleTab[tab]
        }, false)
    }
  },
  methods: {
    toggleTab (tab) {
      return () => {
        store.dispatch(store.actions.toggleTab(tab))
      }
    }
  },
  components: {
    TabHeaderComponent,
    TabHeaderItemComponent,
    TabBodyComponent,

    ChaptersIcon,
    ShareIcon,
    DownloadIcon,
    InfoIcon,
    AudioIcon,
    TranscriptsIcon,

    InfoTab,
    ShareTab,
    ChaptersTab,
    TranscriptsTab,
    DownloadTab,
    AudioTab
  }
}
</script>

<style lang="scss">
  @import '~styles/variables';

  .tabs {
    width: 100%;
    background: $background-color;
  }
</style>
