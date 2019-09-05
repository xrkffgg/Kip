<template>
  <div id="kip">
    <div 
      :class="[
        'kip-input',
        inputSize ? 'kip-input--' + inputSize : '',
        {
          'is-disabled' : inputDisabled,
        }
      ]"
    >
      <template>
        <div class="inner-box" :class="focused ? 'inner-box-focus' : ''">
          <input
            class="inner"
            v-bind="$attrs0"
            :disabled="inputDisabled"
            :readonly="readonly"
            type="text"
            maxlength="3"
            autocomplete="off"
            ref="input1"
            @compositionstart="handleCompositionStart"
            @compositionupdate="handleCompositionUpdate"
            @compositionend="handleCompositionEnd"
            @input="handleInput"
            @focus="handleFocus"
            @blur="handleBlur"
            @change="handleChange"
          >
          <div class="ip-point">.</div>
          <input
            class="inner"
            v-bind="$attrs1"
            :disabled="inputDisabled"
            :readonly="readonly"
            type="text"
            maxlength="3"
            autocomplete="off"
            ref="input2"
            @compositionstart="handleCompositionStart"
            @compositionupdate="handleCompositionUpdate"
            @compositionend="handleCompositionEnd"
            @input="handleInput"
            @focus="handleFocus"
            @blur="handleBlur"
            @change="handleChange"
          >
          <div class="ip-point">.</div>
          <input
            class="inner"
            v-bind="$attrs2"
            :disabled="inputDisabled"
            :readonly="readonly"
            type="text"
            maxlength="3"
            autocomplete="off"
            ref="input3"
            @compositionstart="handleCompositionStart"
            @compositionupdate="handleCompositionUpdate"
            @compositionend="handleCompositionEnd"
            @input="handleInput"
            @focus="handleFocus"
            @blur="handleBlur"
            @change="handleChange"
          >
          <div class="ip-point">.</div>
          <input
            class="inner"
            v-bind="$attrs3"
            :disabled="inputDisabled"
            :readonly="readonly"
            type="text"
            maxlength="3"
            autocomplete="off"
            ref="input4"
            @compositionstart="handleCompositionStart"
            @compositionupdate="handleCompositionUpdate"
            @compositionend="handleCompositionEnd"
            @input="handleInput"
            @focus="handleFocus"
            @blur="handleBlur"
            @change="handleChange"
          >
        </div>
      </template>
    </div>
  </div>
</template>

<script>
import {isKorean} from './utils';
export default {
  name: 'kip',
  data () {
    return {
      focused: false,
    }
  },

  props:{
    size: String,
    disabled: Boolean,
    readonly: Boolean,
    isComposing: false,
  },

  computed:{
    inputDisabled() {
      return this.disabled;
    },
    inputSize() {
      return this.size;
    },
  },

  methods: {
    focus() {
      this.getInput().focus();
    },
    blur() {
      this.getInput().blur();
    },
    select() {
      this.getInput().select();
    },
    getInput() {
      return this.$refs.input;
    },

    handleCompositionStart() {
      this.isComposing = true;
    },
    handleCompositionUpdate(event) {
      const text = event.target.value;
      const lastCharacter = text[text.length - 1] || '';
      this.isComposing = !isKorean(lastCharacter);
    },
    handleCompositionEnd(event) {
      if (this.isComposing) {
        this.isComposing = false;
        this.handleInput(event);
      }
    },

    handleFocus(event) {
      this.focused = true;
      this.$emit('focus', event);
    },
    handleInput(event) {
      // should not emit input during composition
      // see: https://github.com/ElemeFE/element/issues/10516
      if (this.isComposing) return;

      // hack for https://github.com/ElemeFE/element/issues/8548
      // should remove the following line when we don't support IE
      if (event.target.value === this.nativeInputValue) return;

      this.$emit('input', event.target.value);

      // ensure native input value is controlled
      // see: https://github.com/ElemeFE/element/issues/12850
      this.$nextTick(this.setNativeInputValue);
    },
    handleChange(event) {
      this.$emit('change', event.target.value);
    },
    handleBlur(event) {
      this.focused = false;
      this.$emit('blur', event);
      if (this.validateEvent) {
        this.dispatch('ElFormItem', 'el.form.blur', [this.value]);
      }
    },
  },
}
</script>

<style lang="scss">
  .kip-input {
    position: relative;
    font-size: 14px;
    display: inline-block;
    width: 100%;

    .inner-box{
      min-width: 276px;
      -webkit-appearance: none;
      background-color: #fff;
      background-image: none;
      border-radius: 4px;
      border: 1px solid #dcdfe6;
      box-sizing: border-box;

      &::placeholder {
        color: #C0C4CC;
      }

      &:hover {
        border-color: #C0C4CC;
      }
    }

    .inner-box-focus {
      border-color: #409EFF !important;
    }

    .inner {
      -webkit-appearance: none;
      background-color: #fff;
      background-image: none;
      border: 0;
      border-radius: 4px;
      color: #606266;
      display: inline-block;
      font-size: inherit;
      height: 40px;
      line-height: 40px;
      outline: none;
      padding: 0 10px;
      max-width: 40px;
      text-align: center;
    }

    .ip-point{
      color: #666;
      font-weight: bold;
      line-height: 40px;
      width: 3px;
      display: inline-block;
    }
  }
</style>
