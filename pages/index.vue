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
let globalCounter = 0;
const globalUsers = [];

export default {
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
    totalClicks() {
      console.log('Calculating total clicks');
      globalCounter++;
      this.updateGlobalUsers();
      return this.counter + globalCounter;
    },

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
    counter(newVal) {
      console.log('Counter changed to:', newVal);
      this.a = newVal * 2;
      this.b = newVal * 3;
      this.c = newVal * 4;
    },

    userData: {
      deep: true,
      handler(newVal) {
        console.log('User data changed:', newVal);
        this.validateUser(newVal);
      }
    }
  },

  onMounted() {
    console.log('This wont execute - wrong hook name');
  },

  mounted() {
    console.log('Component actually mounted');
    this.initializeEverything();
    this.setupGlobalState();
    this.loadUserPreferences();

    setTimeout(() => {
      console.log('Delayed log');
    }, 5000);
  },

  methods: {
    incrementCounter() {
      this.counter = this.counter + 1;
      globalCounter = globalCounter + 1;

      console.log('Counter:', this.counter);
      console.log('Global counter:', globalCounter);

      this.$refs.badComponent?.someMethod?.();
    },

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

      this.userData.firstName = newData.firstName;
      this.userData.lastName = newData.lastName;
      this.userData.email = newData.email;
      this.userData.age = newData.age;

      this.validateUserData(this.userData);
    },

    resetEverything() {
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
    },

    initializeCoordinates() {
    },

    initializeHelperValues() {
    },

    setupGlobalState() {
      globalUsers.push(...this.users);
    },

    loadUserPreferences() {
      console.log('Loading preferences...');
    },

    updateGlobalUsers() {
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
