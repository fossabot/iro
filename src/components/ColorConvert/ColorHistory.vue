<template>
  <div class="history">
    <div class="history-title">
      <div class="history-title--content">
        <h1>History</h1>
        <span>Last color you generate: </span>
      </div>

      <div
        class="history-title--close icon"
        :class="`icon-close--${contrast}`"
        @click="closePage"
      ></div>
    </div>
    <div class="history-items">
      <div
        class="history-item"
        v-for="(history, index) in histories"
        :key="index"
      >
        <div
          class="history-item--color"
          :style="`background-color: ${history.colorStyle}`"
          @click="openHistory(history)"
        ></div>

        <div class="history-item--content">
          <span class="title">{{ history.title }}</span>
          <span class="subtitle">{{ history.colorText }}</span>
        </div>

        <div class="history-item--action">
          <div
            class="history-item--action-item bookmarked icon"
            :class="`icon-bookmark--${contrast}`"
            title="Add to bookmark"
            @click="addToBookmark(history)"
          ></div>
          <div
            class="history-item--action-item copy icon"
            :class="`icon-copy--${contrast}`"
            title="Copy to clipboard"
            @click="addToClipboard(history)"
          ></div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { mapState, mapActions } from "pinia";

import { copyToClipboard } from "@src/services/utils";
import { useAppStore, useDataStore } from "@src/store";

export default {
  name: "ColorHistory",
  props: {
    contrast: String,
  },
  inject: ["notyf"],
  computed: {
    ...mapState(useAppStore, [ "historyPage" ]),
    ...mapState(useDataStore, [ "histories" ])
  },
  methods: {
    ...mapActions(useAppStore, [ "toggleHistoryPage" ]),
    ...mapActions(useDataStore, [ "addBookmarks" ]),
    openHistory(history) {
      this.$emit("colorChanged", [history.type, history.value]);
      this.toggleHistoryPage();
    },
    addToBookmark(history) {
      const clean = JSON.parse(JSON.stringify(history));

      this.addBookmarks(clean);
      this.notyf.success("Color successfuly bookmarked!");
    },
    addToClipboard(history) {
      copyToClipboard(history.colorText);
      this.notyf.success("Copied!");
    },
    closePage() {
      this.toggleHistoryPage();
    },
  },
};
</script>

<style lang="scss">
.history {
  left: 0;
  bottom: -16px;
  opacity: 1;

  position: fixed;
  background-color: var(--dark-transparent-color);
  width: calc(100vw - 32px);
  height: calc(100vh - 92px);

  box-shadow: 0 -3px 4px rgba(0, 0, 0, 0.1);
  padding: 0 16px;
  border-top-left-radius: 16px;
  border-top-right-radius: 16px;

  .history-title {
    padding-bottom: 16px;
    margin-bottom: 16px;
    display: flex;
    justify-content: space-between;
    align-items: center;

    border-top-left-radius: 16px;
    border-top-right-radius: 16px;

    .history-title--content {
      h1 {
        font-size: 1.2em;
        font-weight: bold;
        margin-bottom: 0;
      }

      span {
        font-size: 0.7rem;
      }
    }
    .history-title--close {
      cursor: pointer;
    }
  }

  .history-items {
    overflow-y: scroll;
    height: 100%;

    .history-item:last-child {
      margin-bottom: 120px;
    }
  }
  .history-item {
    padding: 8px;
    display: flex;
    justify-content: space-between;
    align-items: center;

    .history-item--color {
      width: 32px;
      height: 32px;
      border-radius: 100%;
      box-shadow: 0 1px 1px 0 rgba(0, 0, 0, 0.14),
        0 2px 1px -1px rgba(0, 0, 0, 0.12), 0 1px 3px 0 rgba(0, 0, 0, 0.2);

      margin-right: 16px;
      cursor: pointer;
    }

    .history-item--content {
      display: flex;
      flex: 1;
      flex-direction: column;

      .title {
        font-weight: bold;
      }
      .subtitle {
        font-size: 12.5px;
      }
    }

    .history-item--action {
      display: flex;
      flex-direction: row;

      .history-item--action-item {
        padding: 0 8px;
        cursor: pointer;
      }
    }
  }
}

.history-enter-active,
.history-leave-active {
  transition: all 0.3s cubic-bezier(0.65, 0.05, 0.36, 1);
  -moz-transition: all 0.3s cubic-bezier(0.65, 0.05, 0.36, 1);
}
.history-enter-from,
.history-leave-to {
  opacity: 0;
  height: 0;
}
</style>
