<template>
  <div class="row">
    <div class="col s6 offset-s3">
      <h1>Create task</h1>
      <form @submit.prevent="submitHandler">
        <div class="input-field">
          <input
            id="title"
            type="text"
            class="validate"
            required
            v-model="title"
          />
          <label for="tite">Title</label>
          <span class="helper-text" data-error="Title is required"></span>
        </div>
        <div class="chips" ref="chips"></div>
        <div class="input-field">
          <textarea
            id="description"
            class="materialize-textarea"
            v-model="description"
          ></textarea>
          <label for="description">Description</label>
          <span class="character-counter" style="float: right; font-size: 12px;"
            >{{ description.length }}/2048</span
          >
        </div>
        <input type="text" class="datepicker" ref="datepicker" />
        <button class="btn" type="submit">Create Task</button>
      </form>
    </div>
  </div>
</template>

<script>
export default {
  name: "Create",
  data() {
    return {
      description: "",
      title: "",
      chips: null,
      date: null,
    };
  },
  mounted() {
    this.chips = M.Chips.init(this.$refs.chips, {
      placeholder: "Task tags",
    });
    this.date = M.Datepicker.init(this.$refs.datepicker, {
      format: "dd.mm.yyyy",
      defaultDate: new Date(),
      setDefaultDate: true,
    });
  },
  methods: {
    submitHandler() {
      const task = {
        title: this.title,
        description: this.description,
        id: Date.now(),
        status: "active",
        tags: this.chips.chipsData,
        date: this.date.date,
      };
      this.$store.dispatch("createTask", task);
      this.$router.push("/list");
    },
  },
  destroyed() {
    if (this.date && this.date.destroy) {
      this.date.destroy();
    }
    if (this.chips && this.chips.destroy) {
      this.chips.destroy();
    }
  },
};
</script>
