<template>
<div class="mixer-item" :class="{ muted: audioSource.muted}">

  <div class="flex">
    <div class="source-name">{{ audioSource.source.displayName }}</div>
    <div class="db-value">
      <div v-if="audioSource.fader.deflection == 0">-Inf dB</div>
      <div v-if="audioSource.fader.deflection !== 0">{{ audioSource.fader.db.toFixed(1) }} dB</div>
    </div>
  </div>

  <div class="volmeter">
    <div class="volmeter-level" ref="level"></div>
    <div class="volmeter-peak" ref="peak"></div>
  </div>

  <div class="flex">
    <Slider
      :value="audioSource.fader.deflection"
      :min="0"
      :max="1"
      :interval="0.01"
      @input="onSliderChangeHandler"
      tooltip="false"
    />
    <div class="controls">
      <i class="icon-btn fa fa-volume-up"
         title="click to switch off"
         v-if="!audioSource.muted"
         @click="setMuted(true)"
      >
      </i>
      <i
        class="icon-btn fa fa-volume-off"
        title="click to switch on"
        v-if="audioSource.muted"
        @click="setMuted(false)"
      >
      </i>
      <i
        class="icon-btn fa fa-cog"
        @click="showSourceMenu(audioSource.sourceId)"
      >
      </i>
    </div>
  </div>

</div>
</template>

<script lang="ts" src="./MixerItem.vue.ts"></script>

<style lang="less" scoped>
@import "../styles/index";

.mixer-item {
  position: relative;
  padding: 10px 12px;

  .source-name {
    flex: 1
  }

  .db-value {
    width: 60px;
    text-align: right;
  }

  .volmeter {
    position: relative;
    overflow: hidden;
    margin: 10px 0;
    height: 4px;
    border-radius: 4px;
    background-color: @slider-background-color;

    .volmeter-level {
      .absolute(0, auto, 0, 0);
      width: 100%;
      background-color: @teal;
      transition-property: transform;
      transition-duration: 100ms;
      transition-timing-function: linear;
      transform-origin: left center;
    }

    .volmeter-peak {
      .absolute(0, auto, 0, 0);
      width: 2px;
      background-color: @input-border-color;
      transition-property: left;
      transition-duration: 100ms;
      transition-timing-function: linear;
    }
  }

  .slider {
    flex: 1;
  }

  &.muted .slider {
    opacity: 0.4;
  }

  .controls {
    width: 60px;
    text-align: right;
    flex: 0 0 60px;

    .fa-volume-off {
      color: @red;
    }
  }
}

.night-theme {
  .volmeter {
    background-color: @night-slider-bg;
  }
}
</style>
