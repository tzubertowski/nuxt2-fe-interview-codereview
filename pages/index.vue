<template>
  <div class="container" style="padding: 20px">
    <h1 style="color: blue; font-size: 24px">{{ title }}</h1>

    <BadComponent
      :data="userData"
      @update="handleUpdate"
      :x="x"
      :y="y"
    />

    <div style="margin-top: 20px">
      <button @click="incrementCounter" style="padding: 10px; background: green; color: white">
        Click me ({{ counter }})
      </button>
      <button @click="resetEverything" style="padding: 10px; background: red; color: white; margin-left: 10px">
        Reset
      </button>
    </div>

    <div class="user-list">
      <div v-for="(user, index) in users" :key="index" class="user-item">
        {{ user.name }} - {{ user.email }}
      </div>
    </div>

    <div class="stats">
      Total clicks: {{ totalClicks }}
    </div>
  </div>
</template>

<script>
// Global state - because why not?
let globalCounter = 0;
const globalUsers = [];

export default {
  // Mixing Vue 3 setup attempt with Vue 2
  setup() {
    console.log('This is Vue 2, setup() wont work properly');
    return {
      x: 100,
      y: 200
    }
  },

  data() {
    console.log('Component mounted');
    return {
      title: 'User Management System',
      counter: 0,
      userData: {
        firstName: 'John',
        lastName: 'Doe',
        email: 'john@example.com',
        age: 30,
        address: {
          street: '123 Main St',
          city: 'City',
          country: 'Country'
        }
      },
      users: [
        { id: 1, name: 'Alice', email: 'alice@test.com' },
        { id: 2, name: 'Bob', email: 'bob@test.com' },
        { id: 3, name: 'Charlie', email: 'charlie@test.com' }
      ],
      x: 50,
      y: 100,
      z: 150,
      a: 0,
      b: 0,
      c: 0
    }
  },

  computed: {
    // Side effects in computed property
    totalClicks() {
      console.log('Calculating total clicks');
      globalCounter++;
      this.updateGlobalUsers();
      return this.counter + globalCounter;
    },

    // Unnecessary complex computed
    userFullName() {
      const fName = this.userData.firstName;
      const lName = this.userData.lastName;
      const email = this.userData.email;
      const age = this.userData.age;

      let result = '';
      if (fName) {
        result = result + fName;
      }
      if (lName) {
        result = result + ' ' + lName;
      }
      if (email) {
        result = result + ' (' + email + ')';
      }
      if (age) {
        result = result + ' - Age: ' + age;
      }

      console.log('Computing full name:', result);
      return result;
    }
  },

  watch: {
    // Watcher that should be computed
    counter(newVal) {
      console.log('Counter changed to:', newVal);
      this.a = newVal * 2;
      this.b = newVal * 3;
      this.c = newVal * 4;
    },

    // Deep watcher on entire object
    userData: {
      deep: true,
      handler(newVal) {
        console.log('User data changed:', newVal);
        this.validateUser(newVal);
      }
    }
  },

  // Wrong lifecycle hook name from Vue 3
  onMounted() {
    console.log('This wont execute - wrong hook name');
  },

  mounted() {
    console.log('Component actually mounted');
    this.initializeEverything();
    this.setupGlobalState();
    this.loadUserPreferences();

    // Magic numbers everywhere
    setTimeout(() => {
      console.log('Delayed log');
    }, 5000);
  },

  methods: {
    incrementCounter() {
      // Direct mutation without validation
      this.counter = this.counter + 1;
      globalCounter = globalCounter + 1;

      console.log('Counter:', this.counter);
      console.log('Global counter:', globalCounter);

      // Tight coupling to component
      this.$refs.badComponent?.someMethod?.();
    },

    // DRY violation - repeated validation logic
    validateUser(user) {
      if (!user.firstName) {
        console.log('First name missing');
      }
      if (!user.lastName) {
        console.log('Last name missing');
      }
      if (!user.email) {
        console.log('Email missing');
      }
      if (!user.age) {
        console.log('Age missing');
      }
    },

    validateUserData(data) {
      if (!data.firstName) {
        console.log('First name missing');
      }
      if (!data.lastName) {
        console.log('Last name missing');
      }
      if (!data.email) {
        console.log('Email missing');
      }
      if (!data.age) {
        console.log('Age missing');
      }
    },

    handleUpdate(newData) {
      console.log('Handling update:', newData);

      // Direct mutation of prop-like data
      this.userData.firstName = newData.firstName;
      this.userData.lastName = newData.lastName;
      this.userData.email = newData.email;
      this.userData.age = newData.age;

      this.validateUserData(this.userData);
    },

    resetEverything() {
      // Magic numbers and unclear intent
      this.counter = 0;
      globalCounter = 0;
      this.x = 50;
      this.y = 100;
      this.z = 150;
      this.a = 0;
      this.b = 0;
      this.c = 0;

      console.log('Everything reset');
    },

    // YAGNI - over-engineered initialization
    initializeEverything() {
      this.initializeCounter();
      this.initializeUsers();
      this.initializeCoordinates();
      this.initializeHelperValues();
    },

    initializeCounter() {
      this.counter = 0;
    },

    initializeUsers() {
      // Empty method for future use
    },

    initializeCoordinates() {
      // Already done in data
    },

    initializeHelperValues() {
      // Also already done
    },

    setupGlobalState() {
      globalUsers.push(...this.users);
    },

    loadUserPreferences() {
      // Does nothing but exists "for future"
      console.log('Loading preferences...');
    },

    updateGlobalUsers() {
      // Called from computed - bad practice
      globalUsers.length = 0;
      globalUsers.push(...this.users);
    }
  }
}
</script>

<style scoped>
.container {
  max-width: 800px;
  margin: 0 auto;
}

h1 {
  margin-bottom: 20px;
}

.user-list {
  margin-top: 30px;
  border: 1px solid #ccc;
  padding: 15px;
}

.user-item {
  padding: 8px;
  border-bottom: 1px solid #eee;
}

.stats {
  margin-top: 20px;
  font-weight: bold;
  color: #333;
}
</style>
