<template>
  <div>
    <div v-for="item in items" :key="item.id" class="checkbox-item">
      <input 
        type="checkbox" 
        :id="item.id" 
        v-model="item.selected" 
        @change="toggleSelection(item)"
      />
      <label :for="item.id">{{ item.label }}</label>
      <div v-if="item.children && item.expanded" class="children">
        <CheckboxHierarchy 
          :items="item.children" 
          @update="saveToLocalStorage"
        />
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'CheckboxHierarchy',
  props: {
    items: {
      type: Array,
      required: true,
    },
  },
  methods: {
    toggleSelection(item) {
      item.expanded = !item.expanded;
      if (!item.expanded) {
        this.deselectChildren(item);
      }
      this.saveToLocalStorage();
    },
    deselectChildren(item) {
      if (item.children) {
        item.children.forEach(child => {
          child.selected = false;
          child.expanded = false;
          this.deselectChildren(child);
        });
      }
    },
    saveToLocalStorage() {
      this.$emit('update', this.items);
    },
    loadFromLocalStorage() {
      const savedItems = localStorage.getItem('checkboxHierarchy');
      if (savedItems) {
        const parseAndMerge = (saved, original) => {
          for (const savedItem of saved) {
            const originalItem = original.find(item => item.id === savedItem.id);
            if (originalItem) {
              originalItem.selected = savedItem.selected;
              originalItem.expanded = savedItem.expanded;
              if (savedItem.children && originalItem.children) {
                parseAndMerge(savedItem.children, originalItem.children);
              }
            }
          }
        };
        parseAndMerge(JSON.parse(savedItems), this.items);
      }
    }
  },
  mounted() {
    this.loadFromLocalStorage();
  }
};
</script>

<style scoped>
.checkbox-item {
  margin-left: 20px;
}
.children {
  margin-left: 20px;
}
</style>
