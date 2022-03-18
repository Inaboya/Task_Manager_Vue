<template>
    <form @submit="onSubmit" class="add-task">
        <div class="form-control">
            <label>Task</label>
            <input type="text" v-model="text" name="text" placeholder="Add Task">
        </div>

        <div class="form-control">
            <label>Day and Time</label>
            <input type="day" v-model="day" name="day" placeholder="Add Time">
        </div>

        <div class="form-control-check">
            <label>Reminder</label>
            <input type="checkbox" v-model="reminder" name="reminder">

        </div>

        <div class="form-control-check">
            <label>Completed</label>
            <input type="checkbox" v-model="completed" name="completed">
        </div>

        <input type="submit" class="btn" value="Save Task">
    </form>
</template>


<script>
export default {
    name: "AddTask",
    data() {
        return {
            text: "",
            day: "",
            reminder: false,
            completed: false
        }
    },
    methods: {
        onSubmit(e) {
          e.preventDefault();

          if (!this.text) {
            alert("Please add a task");
            return;
          }

          const newTask = {
            // id: Math.floor(Math.random() * 1000000),
            text: this.text,
            day: this.day,
            reminder: this.reminder,
            completed: this.completed
          }
          this.$emit("add-task", newTask);
          // console.log(newTask);
          this.text = '';
          this.day = '';
          this.reminder = false;
          this.completed = false;

        }
    }
}
</script>


<style scoped>
.add-task {
  margin-bottom: 40px;
}
.form-control {
  margin: 20px 0;
}
.form-control label {
  display: block;
}
.form-control input {
  width: 100%;
  height: 40px;
  margin: 5px;
  padding: 3px 7px;
  font-size: 17px;
}
.form-control-check {
  display: flex;
  align-items: center;
  justify-content: space-between;
}
.form-control-check label {
  flex: 1;
}
.form-control-check input {
  flex: 2;
  height: 20px;
}

</style>