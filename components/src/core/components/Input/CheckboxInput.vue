<!--
/*
 * This file is part of OrangeHRM Inc
 *
 * Copyright (C) 2020 onwards OrangeHRM Inc
 *
 * This program is free software; you can redistribute it and/or modify
 * it under the terms of the GNU General Public License as published by
 * the Free Software Foundation; either version 3 of the License, or
 * (at your option) any later version.
 *
 * This program is distributed in the hope that it will be useful,
 * but WITHOUT ANY WARRANTY; without even the implied warranty of
 * MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
 * GNU General Public License for more details.
 *
 * You should have received a copy of the GNU General Public License
 * along with this program.  If not, see  http://www.gnu.org/licenses
 */
-->

<template>
  <div class="oxd-checkbox-wrapper">
    <label :class="{'--disabled': disabled}">
      <template v-if="labelPosition === 'left'">
        {{ optionLabel }}
      </template>
      <input
        type="checkbox"
        @focus="onFocus"
        @blur="onBlur"
        @change="onChange"
        :disabled="disabled"
        v-bind="$attrs"
        v-model="checked"
      />
      <span :class="classes" :style="style" class="oxd-checkbox-input">
        <oxd-icon class="oxd-checkbox-input-icon" :name="checkIcon" />
      </span>
      <template v-if="labelPosition === 'right'">
        {{ optionLabel }}
      </template>
    </label>
  </div>
</template>

<script lang="ts">
import {defineComponent} from 'vue';
import {Position, LABEL_POSITIONS, RIGHT} from './types';
import Icon from '@ohrm/oxd/core/components/Icon/Icon.vue';

export interface State {
  focused: boolean;
  // eslint-disable-next-line @typescript-eslint/no-explicit-any
  checkedProxy: any;
}

export default defineComponent({
  name: 'oxd-checkbox-input',
  inheritAttrs: false,
  props: {
    modelValue: {},
    style: {
      type: Object,
    },
    hasError: {
      type: Boolean,
      default: false,
    },
    disabled: {
      type: Boolean,
      default: false,
    },
    checkIcon: {
      type: String,
      default: 'check',
    },
    optionLabel: {
      type: String,
      default: '',
    },
    labelPosition: {
      type: String,
      default: RIGHT,
      validator: function(value: Position) {
        return LABEL_POSITIONS.indexOf(value) !== -1;
      },
    },
  },

  emits: ['update:modelValue', 'blur', 'focus', 'change'],

  components: {
    'oxd-icon': Icon,
  },

  data(): State {
    return {
      focused: false,
      checkedProxy: {},
    };
  },

  computed: {
    classes(): object {
      return {
        'oxd-checkbox-input': true,
        'oxd-checkbox-input--active': !this.focused,
        'oxd-checkbox-input--focus': this.focused,
        'oxd-checkbox-input--error': this.hasError,
        [`--label-${this.labelPosition}`]: true,
      };
    },
    checked: {
      get() {
        return this.modelValue;
      },
      set(value) {
        this.checkedProxy = value;
      },
    },
  },

  methods: {
    onFocus(e: Event) {
      this.focused = true;
      this.$emit('focus', e);
    },
    onBlur(e: Event) {
      this.focused = false;
      this.$emit('blur', e);
    },
    onChange(e: Event) {
      this.$emit('update:modelValue', this.checkedProxy);
      this.$emit('change', e);
    },
  },
});
</script>

<style src="./checkbox-input.scss" lang="scss" scoped></style>
