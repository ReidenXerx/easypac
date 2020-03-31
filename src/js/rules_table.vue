<template>
  <div>

    <table class="sexy-list">
      <caption class="sexy-caption">{{table_name}}</caption>

        <tr class="sexy-head-line">
          <td class="sexy-cell" v-for="cap in table_captions">{{cap}}</td>
        </tr>
        <tr class="sexy-head-line">
          <td class="sexy-cell">Новое правило:</td>
          <td class="sexy-cell"><input type="text" v-model="add.name"/></td>
          <td class="sexy-cell"></td>
          <td class="sexy-cell"></td>
          <td class="sexy-cell"></td>
          <td class="sexy-cell"></td>
          <td class="sexy-cell"><button v-on:click="addRule()">Добавить</button></td>
        </tr>

        <tr v-for="(item, index) in table_data" class="sexy-line">
          <td class="sexy-cell">
            <input type="checkbox" :id="index" v-model="item.checked">
            <label :for="index">{{item.checked}}</label>
          </td>
          <td class="sexy-cell">{{item.name}}</td>
          <td class="sexy-cell">
            <select class="sexy-select" v-model="item.hosts.selected">
              <option disabled value="">not selected</option>
              <option v-for="(host, index) in item.hosts.vals">
                {{host.value}}
              </option>
            </select>
          </td>
          <td class="sexy-cell">
            <select class="sexy-select" v-model="item.actions.selected">
              <option disabled value="">not selected</option>
              <option v-for="(action, index) in item.actions.vals">
                {{action.type}}
              </option>
            </select>
          </td>
          <td class="sexy-cell">
            <select class="sexy-select multiple" v-model="item.ids.selected" multiple v-if="item.actions.selected==='PROXY'">
              <option v-for="(id, index) in proxy_ids">
                {{id}}
              </option>
            </select>
          </td>
          <td class="sexy-cell"><button v-on:click="removeData(index)">Удалить</button></td>
          <td class="sexy-cell"><button v-on:click="addhost.show=true; addhost.id=index">Новый хост</button></td>
        </tr>
    </table>

    <SexyWindow :header="addhost.name" :ismin="false" :ismax="false" :isdelete="false" v-if="addhost.show">
      <div class="sexy-dialog flex f-d-column a-i-center" slot="window-body">
        <input class="sexy-input" type="text" v-model="addhost.host" placeholder="введите хост..." />
        <div class="flex j-c-s-around w100">
          <button class="sexy-button" v-on:click="addHost(addhost.id); addhost.show=false">Добавить</button>
          <button class="sexy-button" v-on:click="addhost.show=false">Отмена</button>
        </div>
      </div>
    </SexyWindow>

  </div>

</template>

<script>
import SexyWindow from './window.vue';

export default {
  name: 'rulesTable',
  components: {
    SexyWindow
  },
  props: {
    table_data: {
      type: Array,
      default: function() { return [{ checked: 'true', name: 'Test', hosts: { selected: '', vals: [
        {value: 'v1'}, {value: 'v2'}, {value: 'v3'}
      ]}, actions: { selected: '', vals: [
        {type: 'DIRECT'}, {type: 'BLOCK'}, {type: 'PROXY'}
      ]}, ids: { selected: [], vals: []}
     }]  }
      },
    table_name: {
      type: String,
      default: 'Rules Table'
    },
    table_captions: {
      type: Array,
      default: function() { return['Включено', 'Наименование', 'Хосты', 'Действия', 'Прокси', '', ''] }
    },
    proxy_ids: {
      type: Array,
      default: function() { return['id_1', 'id_2', 'id_3'] }
    }
  },
  data() {
    return {
      add: { checked: 'false', name: 'new rule', hosts: [], actions: [
        {type: 'DIRECT'}, {type: 'BLOCK'}, {type: 'PROXY'} ]},
      selected: { host: 'not selected', action: 'not selected', id_hosts: [] },
      addhost: {
        host: '',
        show: false,
        name: 'Новый хост',
        id: ''
      }
    }
  },
  methods: {
    addRule: function() {
      this.table_data.push({
        checked: false,
        name: this.add.name,
        hosts: {selected: '', vals: []},
        actions: {selected: '', vals: Object.values(this.add.actions)},
        ids: { selected: [], vals: Object.values(this.proxy_ids)}
      })
    },
    addHost: function(index) {
      this.table_data[index].hosts.vals.push({value: this.addhost.host});
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
