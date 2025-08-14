<template>
  <div class="editable-table">
    <div class="controls">
      <button  class="btn" @click="addRow">დამატება</button>
      <button  class="btn" @click="onSave">შენახვა</button>
      <button  class="btn" @click="$emit('close')">დახურვა</button>
    </div>

    <div class="table-scroll" @paste.prevent="onPaste">
      <table>
        <thead>
          <tr>
            <th>სახელი*</th>
            <th>რაოდენობა*</th>
            <th>ფასი*</th>
            <th>ერთეული</th>
            <th>კომენტარი</th>
            <th></th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(row, idx) in localRows" :key="row.id || idx">
            <td>
              <input v-model="row.name" :class="{ invalid: fieldInvalid(idx, 'name') }" />
            </td>
            <td>
              <input v-model.number="row.qty" type="number" min="0" :class="{ invalid: fieldInvalid(idx, 'qty') }" />
            </td>
            <td>
              <input v-model.number="row.price" type="number" step="0.01" min="0" :class="{ invalid: fieldInvalid(idx, 'price') }" />
            </td>
            <td>
              <select v-model="row.unit">
                <option>pcs</option>
                <option>kg</option>
                <option>box</option>
              </select>
            </td>
            <td>
              <input v-model="row.comment" />
            </td>
            <td>
              <button @click="removeRow(idx)">x</button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>

    <div class="validation-note" v-if="hasInvalid">
      რამდენიმე ველი არასწორია — როგორც მინიმუმ ყველა სავალდებულო ველი უნდა იყოს შევსებული და რაოდენობა/ფასი დადებითი რიცხვი უნდა იყოს.
    </div>
  </div>
</template>

<script setup>
import { ref, watch, computed } from 'vue'

const props = defineProps({ rows: { type: Array, default: () => [] } })
const emit = defineEmits(['update:rows', 'close', 'save'])

const localRows = ref(props.rows.map(r => ({ ...r })))

watch(() => props.rows, v => {
  localRows.value = v.map(r => ({ ...r }))
})

function addRow() {
  localRows.value.push({ id: Date.now() + Math.random(), name: '', qty: 0, price: 0, unit: 'pcs', comment: '' })
}

function removeRow(idx) {
  localRows.value.splice(idx, 1)
}

function fieldInvalid(idx, field) {
  const r = localRows.value[idx]
  if (!r) return false
  if (field === 'name') return !r.name || r.name.trim() === ''
  if (field === 'qty') return !(Number.isFinite(r.qty) && r.qty > 0)
  if (field === 'price') return !(Number.isFinite(r.price) && r.price >= 0)
  return false
}

const hasInvalid = computed(() => {
  return localRows.value.some((r, idx) => fieldInvalid(idx, 'name') || fieldInvalid(idx, 'qty') || fieldInvalid(idx, 'price'))
})

function onSave() {
  if (hasInvalid.value) {
    alert('გთხოვთ გამოასწოროთ ვალდებული ველები')
    return
  }
  emit('update:rows', localRows.value)
  emit('save', localRows.value)
}

// Paste handler supports tab/newline separated input (Excel-style)
function onPaste(e) {
  const text = (e.clipboardData || window.clipboardData).getData('text')
  if (!text) return

  // parse into rows by newlines and columns by tabs
  const lines = text.replace(/\r/g, '').split('\n').filter(l => l.trim() !== '')
  if (lines.length === 0) return

  const parsed = lines.map(line => line.split('\t'))

  // We'll try to merge parsed into existing table starting from last row
  let startIdx = localRows.value.length
  parsed.forEach((cols, i) => {
    const targetIdx = startIdx + i
    const row = localRows.value[targetIdx] || { id: Date.now() + Math.random(), name: '', qty: 0, price: 0, unit: 'pcs', comment: '' }
    // map first 5 columns: name, qty, price, unit, comment
    if (cols[0] !== undefined) row.name = cols[0]
    if (cols[1] !== undefined) row.qty = Number(cols[1]) || 0
    if (cols[2] !== undefined) row.price = Number(cols[2]) || 0
    if (cols[3] !== undefined) row.unit = cols[3]
    if (cols[4] !== undefined) row.comment = cols[4]

    if (targetIdx >= localRows.value.length) localRows.value.push(row)
    else localRows.value[targetIdx] = row
  })
}
</script>

<style lang="scss" scoped>

@import  "../styles/_base.scss";

.editable-table {
  padding: 12px;
  border: 1px solid $border;
  background: $surface;

  .table-scroll {
    overflow: auto;
    max-height: 360px;

    table {
      width: 100%;
      border-collapse: collapse;

      th, td {
        padding: 6px 8px;
        border: 1px solid $border;
        text-align: left;
      }
    }
  }

  input.invalid {
    outline: 2px solid $danger;
    outline-style: auto;
  }

  .validation-note {
    color: $danger;
    margin-top: 8px;
  }

  .controls {
    margin-bottom: 8px;
  }
}

</style>
