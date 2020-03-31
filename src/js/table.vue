<template>
  <table class="sexy-list">
    <caption class="sexy-caption">{{table_name}}</caption>
      <tr class="sexy-line">
        <td class="sexy-cell"><input type="text" v-model="cells.cell1"/></td>
        <td class="sexy-cell"><input type="text" v-model="cells.cell2"/></td>
        <td class="sexy-cell"><button v-on:click="addData">Добавить</button></td>
      </tr>
      <tr v-for="(item, index) in table_data" class="sexy-line">
        <td class="sexy-cell">{{item.cell1}}</td>
        <td class="sexy-cell">{{item.cell2}}</td>
        <td class="sexy-cell"><button v-on:click="removeData(index)">Удалить</button></td>
      </tr>
  </table>

</template>

<script>
  export default {
    name: 'sexyTable',
    props: {
      table_data: {
        type: Array,
        default: function() { return [{ cell1: 'd1', cell2: 'd2' }]  }
        },
      table_name: {
        type: String,
        default: 'Empty table'
      }
    },
    data() {
      return {
        cells: {cell1: '', cell2: ''}
      }
    },
    methods: {
      addData: function() {
        this.table_data.push({ cell1: this.cells.cell1, cell2: this.cells.cell2 })
      },
      removeData: function(i) {
        this.table_data.splice(i, 1);
      },
      renameTable: function(name) {
        this.table_name = name;
      }
    }
  }
</script>
