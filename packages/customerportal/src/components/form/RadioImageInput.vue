<template>
  <div class="RadioImageInput" :class="fieldClasses">
    <div class="RadioImageInput__Wrapper">
      <div v-for="(option, index) in options" :key="id + ' ' + index" class="RadioImageInput__Field">
        <input
          :id="id + ' ' + index"
          type="radio"
          :name="id"
          :value="option.value"
          :disabled="isDisabled"
          :checked="isChecked(option.value)"
          @input="handleInput"
          @blur="handleBlur"
        />

        <label :for="id + ' ' + index" class="RadioImageInput__Label">
          <SvgIcon :icon="option.image" />
          <SvgIcon class="SvgIcon--selected" icon="icon_selected" />
          <span>{{ option.label }}</span>
        </label>
      </div>
    </div>
    <div v-if="error" class="RadioImageInput__Feedback">{{ error }}</div>
  </div>
</template>

<script lang="ts">
import { Prop, Component } from "vue-property-decorator";
import { FormField, SvgIcon } from "@fundermaps/common";

@Component({
  components: {
    SvgIcon
  }
})
export default class RadioImageInput extends FormField {
  /**
   * The type of form field
   */
  @Prop({ default: "radio" }) readonly type!: string;

  private isChecked(value: string | boolean | number): boolean {
    return this.value === value.toString() || this.value === value;
  }

  /**
   * List of css classes
   */
  get fieldClasses(): Record<string, boolean> {
    return {
      "RadioImageInput--disabled": this.isDisabled,
      "RadioImageInput--busy": this.isBusy,
      "RadioImageInput--valid": this.hasBeenValidated ? this.isValid : false,
      "RadioImageInput--invalid": this.hasBeenValidated ? !this.isValid : false
    };
  }
}
</script>

<style lang="scss">
$unselected: adjust-color($VENDOR_PRIMARY_COLOR, $red: 81, $green: 41, $blue: -114, $alpha: -0.7);
$unselectedText: adjust-color($VENDOR_PRIMARY_COLOR, $red: 81, $green: 41, $blue: -114);
$unselectedSvg: adjust-color($VENDOR_PRIMARY_COLOR, $red: 176, $green: 131, $blue: -15);

.RadioImageInput {
  &__Wrapper {
    display: flex;
    justify-content: center;
  }
  &__Field {
    margin-right: 20px;

    &:last-child {
      margin-right: 0;
    }
  }

  &__Label {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-content: center;
    width: 160px;
    height: 160px;

    @media only screen and (min-width: $BREAKPOINT) {
      width: 180px;
      height: 180px;
    }

    position: relative;

    display: flex;
    flex-direction: column;
    align-items: center;

    font-size: 18px;
    line-height: 19px;
    letter-spacing: -0.3px;

    color: $unselectedText;
    border: 2px solid $unselected;
    border-radius: 4px;
    background: white;

    cursor: pointer;
    user-select: none;

    transition: all 0.3s ease-in-out;

    &:hover {
      border-color: $VENDOR_PRIMARY_COLOR;
    }

    .SvgIcon:not(.SvgIcon--selected) {
      @media only screen and (min-width: $BREAKPOINT) {
        font-size: 106px;
      }
      font-size: 86px;
      padding-top: 20px;
      padding-bottom: 13px;
      color: $unselectedSvg;
    }

    .SvgIcon.SvgIcon--selected {
      position: absolute;
      top: -2px;
      right: -2px;
      font-size: 40px;
      color: $VENDOR_PRIMARY_COLOR;
      opacity: 0;
      transition: all 0.3s ease-in-out;
    }
  }

  input {
    display: none;
  }
  input[type="radio"]:checked + &__Label {
    color: $VENDOR_PRIMARY_COLOR;
    border-color: $VENDOR_PRIMARY_COLOR;

    .SvgIcon--selected {
      opacity: 1;
    }
    .SvgIcon:not(.SvgIcon--selected) {
      color: $VENDOR_PRIMARY_COLOR;
    }
  }
}
</style>
