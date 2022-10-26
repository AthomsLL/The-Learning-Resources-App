<template>
  <BaseCard>
    <BaseButton 
      @click="setSelectedTab('StoredResources')" 
      :mode="storedResourcesButtonMode"
    >Stored Resources</BaseButton>
    <BaseButton 
      @click="setSelectedTab('AddResource')"
      :mode="addResourceButtonMode"  
    >Add Resource</BaseButton>
  </BaseCard>
  <keep-alive>
    <component :is="selectedTab"></component>
  </keep-alive>
</template>

<script>
import StoredResources from './StoredResources.vue'
import AddResource from './AddResource.vue'

export default {
  components: {
    StoredResources,
    AddResource
  },
  data() {
    return {
      selectedTab: 'StoredResources',
      storedResources: [
        { id: 'official-guide', 
          title: 'Official Guide',
          description: 'The official Vue.js documentation.',
          link: 'https://vuejs.org'
        }, 
        { id: 'google', 
          title: 'Google',
          description: 'Learn to google...',
          link: 'https://google.com'
        }
      ]
    }
  },
  provide() {
    return {
      resources: this.storedResources,
      addResource: this.addResource,
      removeResource: this.removeResource,
    }
  },
  computed: {
    storedResourcesButtonMode() {
      return this.selectedTab === 'StoredResources' ? null : 'flat'
    },
    addResourceButtonMode() {
      return this.selectedTab === 'AddResource' ? null : 'flat'
    }
  },
  methods: {
    setSelectedTab(tab) {
      this.selectedTab = tab
    },
    addResource(title, description, url) {
      const newResource = {
        id: new Date().toISOString(),
        title: title,
        description: description,
        link : url
      };
      this.storedResources.unshift(newResource);
      this.selectedTab = 'StoredResources'
    },
    removeResource(resId) {
      const resIndex = this.storedResources.findIndex(res => res.id === resId);
      this.storedResources.splice(resIndex, 1);
    }
  }
}
</script>