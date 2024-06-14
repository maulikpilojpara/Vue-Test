<template>
  <div id="app">
    <CheckboxHierarchy :items="items" @update="updateItems"/>
  </div>
</template>

<script>
import CheckboxHierarchy from './components/CheckboxHierarchy.vue';

export default {
  name: 'App',
  components: {
    CheckboxHierarchy,
  },
  data() {
    return {
      items: [
        {
          id: 'parent1',
          label: 'Parent 1',
          selected: false,
          expanded: false,
          children: [
            {
              id: 'child1.1',
              label: 'Child 1.1',
              selected: false,
              expanded: false,
              children: [
                { id: 'subchild1.1.1', label: 'Sub Child 1.1.1', selected: false, expanded: false },
                { id: 'subchild1.1.2', label: 'Sub Child 1.1.2', selected: false, expanded: false }
              ]
            },
            {
              id: 'child1.2',
              label: 'Child 1.2',
              selected: false,
              expanded: false,
              children: [
                { id: 'subchild1.2.1', label: 'Sub Child 1.2.1', selected: false, expanded: false },
                { id: 'subchild1.2.2', label: 'Sub Child 1.2.2', selected: false, expanded: false }
              ]
            }
          ]
        },
        {
          id: 'parent2',
          label: 'Parent 2',
          selected: false,
          expanded: false,
          children: [
            {
              id: 'child2.1',
              label: 'Child 2.1',
              selected: false,
              expanded: false,
              children: [
                { id: 'subchild2.1.1', label: 'Sub Child 2.1.1', selected: false, expanded: false },
                { id: 'subchild2.1.2', label: 'Sub Child 2.1.2', selected: false, expanded: false }
              ]
            },
            {
              id: 'child2.2',
              label: 'Child 2.2',
              selected: false,
              expanded: false,
              children: [
                { id: 'subchild2.2.1', label: 'Sub Child 2.2.1', selected: false, expanded: false },
                { id: 'subchild2.2.2', label: 'Sub Child 2.2.2', selected: false, expanded: false }
              ]
            }
          ]
        }
      ]
    };
  },
  methods: {
    updateItems(updatedItems) {
      this.items = updatedItems;
      localStorage.setItem('checkboxHierarchy', JSON.stringify(this.items));
    }
  },
  mounted() {
    const savedItems = localStorage.getItem('checkboxHierarchy');
    if (savedItems) {
      this.items = JSON.parse(savedItems);
    }
  }
};
</script>
