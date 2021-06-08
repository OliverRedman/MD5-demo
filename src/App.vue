<template>
  <div id="app">
    <div class="form__group field">
      <input
        type="input"
        class="form__field"
        placeholder="Name"
        name="name"
        id="name"
        v-model="input"
        required
      />
      <label for="name" class="form__label">MD5</label>
      <p class="md5" v-if="input.length > 0">{{ input | md5 | shorten }}</p>
      <p class="md5" v-else>...</p>
    </div>
  </div>
</template>
<script>
// test
import md5 from "./components/md5.js";
export default {
  data: () => ({
    input: "",
    timer: null,
  }),
  methods: {
    convertString: function(str) {
      this.input = [...str]
        .map((char) =>
          char === char.toUpperCase() ? char.toLowerCase() : char.toUpperCase()
        )
        .join("");
    },
  },
  filters: {
    md5,
    shorten: (str) => str.slice(0, 6),
  },
  created: function() {
    this.timer = setInterval(() => {
      this.convertString(this.input);
    }, 60000);
  },
  beforeDestroy() {
    clearInterval(this.timer);
  },
};
</script>

<style lang="scss">
.md5 {
  align-self: center;
  letter-spacing: 4px;
}
* {
  color: white;
  font-family: "Inter", sans-serif;
}
$primary: #11998e;
$secondary: #38ef7d;
$white: #fff;
$gray: #9b9b9b;
.form__group {
  position: relative;
  padding: 15px 0 0;
  margin-top: 10px;
  width: 50%;
}

.form__field {
  width: 12rem;
  border: 0;
  border-bottom: 2px solid $gray;
  outline: 0;
  font-size: 1.3rem;
  color: $white;
  padding: 7px 0;
  background: transparent;
  transition: border-color 0.2s;

  &::placeholder {
    color: transparent;
  }

  &:placeholder-shown ~ .form__label {
    font-size: 1.3rem;
    cursor: text;
    top: 20px;
  }
}

.form__label {
  position: absolute;
  top: 0;
  display: block;
  transition: 0.2s;
  font-size: 1rem;
  color: $gray;
}

.form__field:focus {
  ~ .form__label {
    position: absolute;
    top: 0;
    display: block;
    transition: 0.2s;
    font-size: 1rem;
    color: $primary;
    font-weight: 700;
  }
  padding-bottom: 6px;
  font-weight: 700;
  border-image: linear-gradient(to right, $primary, $secondary);
  border-image-slice: 1;
}
/* reset input */
.form__field {
  &:required,
  &:invalid {
    box-shadow: none;
  }
}
body {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  font-size: 1.5rem;
  background-color: #222222;
}
</style>
