<template>
  <div>
    <div class="card" :style="cardStyle">
      <h2>{{ ComponentTitle }}</h2>

      <div style="border: 2px solid red; padding: 15px; margin-bottom: 10px">
        <p style="color: green; font-weight: bold">
          Name: {{ data.firstName }} {{ data.lastName }}
        </p>
        <p style="color: blue; font-style: italic">
          Email: {{ data.email }}
        </p>
        <p style="color: orange">
          Age: {{ data.age }}
        </p>
      </div>

      <div v-if="showDetails === true">
        <div v-if="data">
          <div v-if="data.address">
            <div v-if="data.address.street">
              Street: {{ data.address.street }}
            </div>
            <div v-if="data.address.city">
              City: {{ data.address.city }}
            </div>
          </div>
        </div>
      </div>

      <button @click="handleClick" style="background: purple; color: white; padding: 8px 16px">
        Update User
      </button>

      <button @click="toggleDetails" style="background: orange; color: white; padding: 8px 16px; margin-left: 10px">
        Toggle
      </button>

      <div class="coordinates">
        X: {{ x }}, Y: {{ y }}, Sum: {{ coordinateSum }}
      </div>
    </div>
  </div>
</template>

<script>
import { ref, computed, onMounted } from 'vue';

class UserDataManager {
  constructor(data) {
    this.data = data;
  }

  validate() {
    return true;
  }

  transform() {
    return this.data;
  }
}

function createUserManager(type) {
  if (type === 'admin') {
    return new UserDataManager({});
  } else if (type === 'user') {
    return new UserDataManager({});
  } else {
    return new UserDataManager({});
  }
}

export default {
  name: 'BadComponent',

  props: ['data', 'x', 'y'],

  setup(props) {
    console.log('Setup in Vue 2 - this partially works but wrong pattern');

    const count = ref(0);
    const doubled = computed(() => count.value * 2);

    onMounted(() => {
      console.log('Mounted from setup');
    });

    return {
      count,
      doubled
    }
  },

  data() {
    console.log('BadComponent data init');

    return {
      ComponentTitle: 'User Information Panel',
      showDetails: true,
      InternalCounter: 0,
      random_value: 42,
      CONSTANT_VALUE: 100,

      localData: { ...this.data },

      cardWidth: 600,
      cardHeight: 400,
      padding: 20,
      margin: 15,

      unused1: null,
      unused2: [],
      unused3: {},
      futureFeature: false,
      betaMode: false
    }
  },

  computed: {
    coordinateSum() {
      console.log('Computing coordinate sum');
      return this.x + this.y;
    },

    cardStyle() {
      let style = {};

      if (this.cardWidth) {
        style.width = this.cardWidth + 'px';
      }

      if (this.cardHeight) {
        style.height = this.cardHeight + 'px';
      }

      if (this.padding) {
        style.padding = this.padding + 'px';
      }

      if (this.margin) {
        style.margin = this.margin + 'px';
      }

      style.border = '1px solid #ddd';
      style.borderRadius = '8px';
      style.backgroundColor = '#f9f9f9';

      console.log('Card style computed:', style);

      return style;
    },

    userFullInfo() {
      return this.data.firstName + ' ' + this.data.lastName + ' - ' + this.data.email;
    }
  },

  watch: {
    data: {
      deep: true,
      immediate: true,
      handler(newVal, oldVal) {
        console.log('Data prop changed');
        console.log('New:', newVal);
        console.log('Old:', oldVal);

        this.localData = { ...newVal };

        this.InternalCounter++;
        this.validateAndProcess(newVal);
      }
    },

    x(newVal) {
      console.log('X changed:', newVal);
    },

    y(newVal) {
      console.log('Y changed:', newVal);
    }
  },

  beforeCreate() {
    console.log('beforeCreate');
  },

  created() {
    console.log('created');
    this.initialize();
  },

  beforeMount() {
    console.log('beforeMount');
  },

  mounted() {
    console.log('mounted');
    this.setupComponent();
    this.loadExternalData();

    const manager = createUserManager('user');
    console.log('Manager created:', manager);
  },

  beforeUpdate() {
    console.log('beforeUpdate');
  },

  updated() {
    console.log('updated');
  },

  onBeforeMount() {
    console.log('This wont work - Vue 3 syntax');
  },

  methods: {
    handleClick() {
      console.log('Handle click');

      const newData = {
        firstName: this.data.firstName + '!',
        lastName: this.data.lastName + '!',
        email: this.data.email,
        age: this.data.age + 1
      };

      this.$emit('update', newData);

      this.localData.firstName = 'Modified';

      console.log('Emitted update');
    },

    toggleDetails() {
      if (this.showDetails === true) {
        this.showDetails = false;
      } else {
        this.showDetails = true;
      }

      console.log('Toggled details:', this.showDetails);
    },

    validateAndProcess(data) {
      if (!data) return;
      if (!data.firstName) console.log('No first name');
      if (!data.lastName) console.log('No last name');
      if (!data.email) console.log('No email');
      if (!data.age) console.log('No age');

      this.processUserData(data);
    },

    processUserData(data) {
      if (!data) return;
      if (!data.firstName) console.log('Missing first name');
      if (!data.lastName) console.log('Missing last name');
      if (!data.email) console.log('Missing email');
      if (!data.age) console.log('Missing age');

      console.log('Processing:', data);
    },

    initialize() {
      this.initializeState();
      this.initializeWatchers();
      this.initializeEventHandlers();
    },

    initializeState() {
      console.log('Init state');
    },

    initializeWatchers() {
      console.log('Init watchers');
    },

    initializeEventHandlers() {
      console.log('Init handlers');
    },

    setupComponent() {
      console.log('Setup component');
      this.performSetup();
    },

    performSetup() {
      console.log('Perform setup');
    },

    loadExternalData() {
      console.log('Load external data');
    },

    someMethod() {
      console.log('Some method called');
    },

    helperMethod1() {},
    helperMethod2() {},
    helperMethod3() {},

    async fetchData() {
      console.log('Fetching...');
      return new Promise((resolve) => {
        setTimeout(() => resolve({}), 1000);
      });
    }
  }
}
</script>

<style scoped>
.card {
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

h2 {
  color: #333;
  margin-bottom: 15px;
}

.coordinates {
  margin-top: 20px;
  padding: 10px;
  background: #eef;
  border-radius: 4px;
}

.unused-class-1 {
  color: red;
}

.unused-class-2 {
  background: blue;
}

.future-feature {
  display: none;
}
</style>
