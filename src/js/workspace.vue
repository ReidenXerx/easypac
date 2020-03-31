<template>
  <div class="workspace-body flex">
    <aside class="navigate-menu flex f-d-column a-i-center">

      <div class="navigate-item flex f-d-column a-i-center" v-for="(item, index) in items" v-on:click="toggle(index)"  v-bind:class="{selected: visualiseSelection(index)}">
        <i class="navigate-icon fas" :class="item.icon"></i>
        <p>{{item.text}}</p>
      </div>

    </aside>

    <div class="workspace-items-wrap flex j-c-center">
      <div v-if="visible == 0" class="workspace-item">
        <SexyWindow :header="test" />
      </div>
      <div v-if="visible == 1" class="workspace-item">
        <SexyDTable :table_data="proxy_data.hosts"
                    :table_name="proxy_data.name"
                    :table_captions="proxy_data.captions"/>
      </div>
      <div v-if="visible == 2" class="workspace-item"><RulesTable /></div>
    </div>

  </div>
</template>

<script>
import SexyDTable from './dynamic_table.vue';
import SexyWindow from './window.vue';
import RulesTable from './rules_table.vue';

export default {
  name: 'workspace',
  props: {
    user: {
      default: 'Test'
    }
  },
  components: {
    SexyDTable,
    SexyWindow,
    RulesTable
  },
  methods: {
    toggle: function(i) {

        if(this.visible == i) this.visible = -1;
        else this.visible = i;
    },

    visualiseSelection: function(i) {
      if(this.visible == i) {
        return true;
      }
      else return false;
    }
  },

  data () {
    return {
      visible: -1,
      items: [
        {icon: 'fa-user', text: this.user, data: 1},
        {icon: 'fa-project-diagram', text: 'прокси', data: 2},
        {icon: 'fa-book', text: 'правила', data: 3}
      ],
      proxy_data: {
        hosts: [
          {host_ip: '192.168.0.1', port: '8080', proxy_type: 'type1'},
          {host_ip: '192.168.0.2', port: '8080', proxy_type: 'type2'},
          {host_ip: '192.168.0.3', port: '8080', proxy_type: 'type1'}
        ],
        captions: ['Адрес','Порт','Тип',''],
        name: 'Прокси'
      },
      f_visible: false,
      test: 'Test Item'
    }
  }
}
</script>
