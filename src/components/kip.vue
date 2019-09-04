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
        <div class="inner-box">
          <input
            class="inner"
            v-bind="$attrs"
            :disabled="inputDisabled"
            :readonly="readonly"
            type="text"
            maxlength="3"
            autocomplete="off"
            ref="input"
            @compositionstart="handleCompositionStart"
            @compositionupdate="handleCompositionUpdate"
            @compositionend="handleCompositionEnd"
            @input="handleInput"
            @focus="handleFocus"
            @blur="handleBlur"
            @change="handleChange"
          >
          <input
            class="inner"
            v-bind="$attrs"
            :disabled="inputDisabled"
            :readonly="readonly"
            type="text"
            maxlength="3"
            autocomplete="off"
            ref="input"
            @compositionstart="handleCompositionStart"
            @compositionupdate="handleCompositionUpdate"
            @compositionend="handleCompositionEnd"
            @input="handleInput"
            @focus="handleFocus"
            @blur="handleBlur"
            @change="handleChange"
          >
          <input
            class="inner"
            v-bind="$attrs"
            :disabled="inputDisabled"
            :readonly="readonly"
            type="text"
            maxlength="3"
            autocomplete="off"
            ref="input"
            @compositionstart="handleCompositionStart"
            @compositionupdate="handleCompositionUpdate"
            @compositionend="handleCompositionEnd"
            @input="handleInput"
            @focus="handleFocus"
            @blur="handleBlur"
            @change="handleChange"
          >
          <input
            class="inner"
            v-bind="$attrs"
            :disabled="inputDisabled"
            :readonly="readonly"
            type="text"
            maxlength="3"
            autocomplete="off"
            ref="input"
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
      min-width: 260px;
      -webkit-appearance: none;
      background-color: #fff;
      background-image: none;
      border-radius: 4px;
      border: 1px solid #dcdfe6;
      box-sizing: border-box;
    }

    .inner {
      -webkit-appearance: none;
      background-color: #fff;
      background-image: none;
      border-radius: 4px;
      border: 1px solid #dcdfe6;
      box-sizing: border-box;
      color: #606266;
      display: inline-block;
      font-size: inherit;
      height: 40px;
      line-height: 40px;
      outline: none;
      padding: 0 15px;
      transition: border-color .2s cubic-bezier(.645,.045,.355,1);
      min-width: 60px;

      &::placeholder {
        color: #C0C4CC;
      }

      &:hover {
        border-color: #C0C4CC;
      }

      &:focus {
        outline: none;
        border-color: #409EFF;
      }
    }
  }
</style>
