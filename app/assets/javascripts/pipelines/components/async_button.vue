<script>
  /* eslint-disable no-alert */

  import eventHub from '../event_hub';
  import loadingIcon from '../../vue_shared/components/loading_icon.vue';
  import icon from '../../vue_shared/components/icon.vue';
  import tooltip from '../../vue_shared/directives/tooltip';

  export default {
    directives: {
      tooltip,
    },
    components: {
      loadingIcon,
      icon,
    },
    props: {
      endpoint: {
        type: String,
        required: true,
      },
      title: {
        type: String,
        required: true,
      },
      icon: {
        type: String,
        required: true,
      },
      cssClass: {
        type: String,
        required: true,
      },
      id: {
        type: Number,
        required: true,
      },
    },
    data() {
      return {
        isLoading: false,
      };
    },
    computed: {
      buttonClass() {
        return `btn ${this.cssClass}`;
      },
    },
    methods: {
      onClick() {
        eventHub.$emit('actionConfirmationModal', {
          id: this.id,
          callback: this.makeRequest,
        });
      },
      makeRequest() {
        this.isLoading = true;

        eventHub.$emit('postAction', this.endpoint);
      },
    },
  };
</script>

<template>
  <button
    v-tooltip
    type="button"
    @click="onClick"
    :class="buttonClass"
    :title="title"
    :aria-label="title"
    data-container="body"
    data-placement="top"
    :disabled="isLoading">
    <icon
      :name="icon"
    />
    <loading-icon v-if="isLoading" />
  </button>
</template>
