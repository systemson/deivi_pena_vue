<template>
  <div style="width: 50%">
    <label>Available Options</label>
    <select :id="field.id" :value="selected" multiple style="width: 100%">
      <option
        v-for="option in field.options"
        :value="option.id"
        :key="option.id"
        @click="unselect($event)">
        {{ option.label }}
      </option>
    </select>
  </div>
  <div style="width: 50%">
    <label>Disabled Options</label>
    <select :id="field.id" @change="handleChange;" :value="selected" multiple style="width: 100%">
      <option v-for="option in disabled" :value="option.id" :key="option.id">
        {{ option.label }}
      </option>
    </select>
  </div>
</template>

<script setup>
import fieldMixin from "../FieldMixin";
const emit = defineEmits(["update"]);
const props = defineProps({
  field: {
    type: Object,
    required: true,
  },
  disabled: {
    type: [String, Number, Boolean, Object, Array],
    default: [],
  },
});
let { handleChange, debounce } = fieldMixin.setup(props, { emit });

const unselect = (id) => {
  const found = props.field.options.find((option) => option.id == id);
  disabled = props.field.filter((option) => option.id !== id);
  props.field.options.push(found);

  console.log(props.field.options, props.disabled);
};

const select = (id) => {
  const found = props.disabled.find((option) => option.id == id);
  disabled = props.disabled.filter((option) => option.id !== id);
  props.field.options.push(found);

  console.log(props.field.options, props.disabled);
};
</script>

<style scoped>
/* Add your styles here */
</style>
