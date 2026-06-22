<script setup>
import {ref} from 'vue'
import {$tp} from '../../platform-i18n'
import PlatformLayout from '../../PlatformLayout.vue'
import AdapterNameSelector from './audiovideo/AdapterNameSelector.vue'
import DisplayOutputSelector from './audiovideo/DisplayOutputSelector.vue'
import DisplayDeviceOptions from "./audiovideo/DisplayDeviceOptions.vue";
import DisplayModesSettings from "./audiovideo/DisplayModesSettings.vue";
import Checkbox from "../../Checkbox.vue";

const props = defineProps([
  'platform',
  'config',
])

const config = ref(props.config)
</script>

<template>
  <div id="audio-video" class="config-page">
    <!-- Audio Sink -->
    <div class="mb-3">
      <label for="audio_sink" class="form-label">{{ $t('config.audio_sink') }}</label>
      <input type="text" class="form-control" id="audio_sink"
             :placeholder="$tp('config.audio_sink_placeholder', 'alsa_output.pci-0000_09_00.3.analog-stereo')"
             v-model="config.audio_sink" />
      <div class="form-text">
        {{ $tp('config.audio_sink_desc') }}<br>
        <PlatformLayout :platform="platform">
          <template #windows>
            <pre>tools\audio-info.exe</pre>
          </template>
          <template #freebsd>
            <pre>pacmd list-sinks | grep "name:"</pre>
            <pre>pactl info | grep Source</pre>
          </template>
          <template #linux>
            <pre>pacmd list-sinks | grep "name:"</pre>
            <pre>pactl info | grep Source</pre>
          </template>
          <template #macos>
            <a href="https://github.com/mattingalls/Soundflower" target="_blank">Soundflower</a><br>
            <a href="https://github.com/ExistentialAudio/BlackHole" target="_blank">BlackHole</a>.
          </template>
        </PlatformLayout>
      </div>
    </div>


    <PlatformLayout :platform="platform">
      <template #windows>
        <!-- Virtual Sink -->
        <div class="mb-3">
          <label for="virtual_sink" class="form-label">{{ $t('config.virtual_sink') }}</label>
          <input type="text" class="form-control" id="virtual_sink" :placeholder="$t('config.virtual_sink_placeholder')"
                 v-model="config.virtual_sink" />
          <div class="form-text">{{ $t('config.virtual_sink_desc') }}</div>
        </div>

        <!-- Install Steam Audio Drivers -->
        <Checkbox class="mb-3"
                  id="install_steam_audio_drivers"
                  locale-prefix="config"
                  v-model="config.install_steam_audio_drivers"
                  default="true"
        ></Checkbox>
      </template>
    </PlatformLayout>

    <!-- Disable Audio -->
    <Checkbox class="mb-3"
              id="stream_audio"
              locale-prefix="config"
              v-model="config.stream_audio"
              default="true"
    ></Checkbox>

    <AdapterNameSelector
        :platform="platform"
        :config="config"
    />

    <DisplayOutputSelector
      :platform="platform"
      :config="config"
    />

    <DisplayDeviceOptions
      :platform="platform"
      :config="config"
    />

    <!-- Display Modes -->
    <DisplayModesSettings
        :platform="platform"
        :config="config"
    />

    <hr class="my-4" />
    <h5 class="mb-3">{{ $t('config.multi_instance_header') }}</h5>

    <!-- Multi-Instance Count -->
    <div class="mb-3">
      <label for="multi_instance_count" class="form-label">{{ $t('config.multi_instance_count') }}</label>
      <input type="number" class="form-control" id="multi_instance_count"
             v-model="config.multi_instance_count" min="1" max="16" />
      <div class="form-text">{{ $t('config.multi_instance_count_desc') }}</div>
    </div>

    <!-- Multi-Instance Mode -->
    <div class="mb-3">
      <label for="multi_instance_mode" class="form-label">{{ $t('config.multi_instance_mode') }}</label>
      <select id="multi_instance_mode" class="form-select" v-model="config.multi_instance_mode">
        <option value="0">{{ $t('config.multi_instance_mode_0') }}</option>
        <option value="1">{{ $t('config.multi_instance_mode_1') }}</option>
      </select>
      <div class="form-text">{{ $t('config.multi_instance_mode_desc') }}</div>
    </div>

    <!-- Cert-Display Map -->
    <div class="mb-3">
      <label for="cert_display_map" class="form-label">{{ $t('config.cert_display_map') }}</label>
      <input type="text" class="form-control" id="cert_display_map"
             v-model="config.cert_display_map"
             :placeholder="$tp('config.cert_display_map_placeholder', 'sha256:abc123=0,sha256:def456=1')" />
      <div class="form-text">{{ $t('config.cert_display_map_desc') }}</div>
    </div>

  </div>
</template>

<style scoped>
</style>
