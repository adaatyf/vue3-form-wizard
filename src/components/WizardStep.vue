<template>
  <li :class="{ active: tab.active, disable: !tab.enable }" class="wizard-step-li">
    <a href="javascript:void(0)"
      :class="{ disabled: !tab.checked, 'anchor-active': tab.active, 'inactive-checked': tab.checked && !tab.active }"
      :style="cursorStyle" class="icon-title-wrap">
      <div class="wizard-icon-circle md" role="tab" :tabindex="tab.checked ? 0 : ''" :id="`step-${tab.tabId}`"
        :aria-controls="tab.tabId" :aria-disabled="tab.active" :aria-selected="tab.active" :class="{
          checked: tab.checked,
          square_shape: isStepSquare,
          tab_shape: isTabShape,
        }" :style="[
  tab.checked ? stepCheckedStyle : {},
  tab.validationError ? errorStyle : {},
  tab.checked && !tab.active ? inactiveCheckedStyle : {}
]">
        <div v-if="tab.active" class="wizard-icon-container"
          :class="{ square_shape: isStepSquare, tab_shape: isTabShape }"
          :style="[iconActiveStyleBg, tab.validationError ? errorStyle : {}]">
          <slot name="active-step">
            <span class="wizard-icon" v-if="tab.customIcon" v-html="tab.customIcon"></span>
            <i v-else :class="tab.icon ? tab.icon : ''" class="wizard-icon" :style="tab.checked ? iconActiveStyle : ''">
              {{ tab.icon ? null : index + 1 }}
            </i>
          </slot>
        </div>
        <slot v-else>
          <span class="wizard-icon" v-if="tab.customIcon" v-html="tab.customIcon"></span>
          <i v-else :class="tab.checked ? 'fa fa-check' : ''" class="wizard-icon"
            :style="tab.checked ? iconActiveStyle : ''">
            {{ tab.checked ? null : index + 1 }}
          </i>
        </slot>
      </div>
      <slot name="title">
        <span class="stepTitle" :class="{ active: tab.active, has_error: tab.validationError }"
          :style="tab.active || tab.checked ? stepTitleStyle : {}">
          {{ tab.title }}
        </span>
      </slot>
      <slot name="customIcon"> </slot>
    </a>
    <div class="wizard-horizontal-bar" v-if="!isLastTab" :style="[tab.checked && !tab.active ? horizontalBarStyle : {}]">
    </div>
  </li>
</template>
<script>
export default {
  name: "wizard-step",
  props: {
    tab: {
      type: Object,
      default: () => { },
    },
    transition: {
      type: String,
      default: "",
    },
    index: {
      type: Number,
      default: 0,
    },
    disableBackOnClickStep: {
      type: Boolean,
      default: false,
    },
    isLastTab: {
      type: Boolean,
      required: true
    }
  },

  computed: {
    iconActiveStyleBg() {
      return {
        backgroundColor: this.tab.color,
      };
    },
    iconActiveStyle() {
      if (!this.tab.active) {
        return { color: this.tab.color };
      }
    },
    stepCheckedStyle() {
      return {
        borderColor: this.tab.color,
      };
    },
    inactiveCheckedStyle() {
      return {
        borderColor: 'transparent',
        backgroundColor: 'transparent'
      }
    },
    errorStyle() {
      return {
        borderColor: this.tab.errorColor,
        backgroundColor: this.tab.errorColor,
      };
    },
    stepTitleStyle() {
      let isError = this.tab.validationError;
      return {
        color: isError ? this.tab.errorColor : 'rgb(33, 37, 41)',
      };
    },
    isStepSquare() {
      return this.tab.shape === "square";
    },
    isTabShape() {
      return this.tab.shape === "tab";
    },
    cursorStyle() {
      return this.disableBackOnClickStep ? "cursor: default" : "";
    },
    horizontalBarStyle() {
      return {
        backgroundColor: this.tab.color
      }
    }
  },
};
</script>
<style scoped>
li.disable {
  pointer-events: none;
}

.anchor-active {
  border-bottom: 3px solid v-bind('tab.color');
  border-top: 3px solid transparent;
}

.stepTitle {
  white-space: nowrap;
  font-weight: 500;
}

.wizard-horizontal-bar {
  position: absolute;
  width: 12px;
  height: 1px;
  background: rgb(215, 216, 217);
  right: -19px;
}
</style>
