<!-- ModalForm.svelte -->
<script>
    import { createEventDispatcher } from 'svelte';

  const dispatch = createEventDispatcher();
  let values = {
    task: '',
    startTime: '',
    repeat:'',
    repeatTimes:'',
  };


  function handleSubmit() {
    const formValues = { ...values };
    dispatch('submit', formValues); // Emitting the submit event with form values
    values = { task: '',  startTime: '', repeat: '', repeatTimes: ''  }; // Resetting form values
  }
  
</script>

<div class="modal-form">
  <form on:submit|preventDefault={handleSubmit}>
    <h3>New Task</h3>
    <label for="task">Task</label><br>
    <input type="text" name="task" bind:value={values.task}><br><br>
    <label for="repeat">Repeat:</label>

    <label for="OnlyThisTime">
      <input type="radio" name="repeat" value="OnlyThisTime" bind:group={values.repeat}>
      Only this time
    </label>

    <label for="Daily">
      <input type="radio" name="repeat" value="Daily" bind:group={values.repeat}>
      Daily
    </label>

    <label for="Weekly">
      <input type="radio" name="repeat" value="Weekly" bind:group={values.repeat}>
      Weekly
    </label><br><br>

    <label for="StartTime">Start Time</label>
    <input type="datetime-local" name="Start Time" bind:value={values.startTime}>

    <label for="RepeatTimes">Repeat Times</label>
    <input type="number" min="0" name="RepeatTimes" bind:value={values.repeatTimes}>

    <button type="submit">Submit</button><br>
  </form>
</div>

<!-- ModalForm.svelte -->
<style>
  .modal-form {
    padding: 20px;
    background-color:  #f2f2f2;
    border-radius: 8px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
    font-family: Arial, sans-serif;
  }

  .modal-form h3 {
    margin-top: 0;
    font-size: 24px;
    color: #0a0a0a;
  }

  .modal-form label {
    display: inline-block;
    margin-bottom: 5px;
    font-weight: bold;
    color: #0a0a0a;
  }

  .modal-form input[type="text"],
  .modal-form input[type="datetime-local"],
  .modal-form input[type="number"] {
    width: 100%;
    padding: 8px;
    margin-bottom: 10px;
    border: 1px solid #ccc;
    border-radius: 4px;
    font-size: 16px;
    box-sizing: border-box;
  }

  .modal-form input[type="radio"] {
    margin-right: 10px;
    display: flex;
    justify-content:space-around;
  }

  .modal-form button[type="submit"] {
    display: block;
    width: 100%;
    padding: 10px;
    border: none;
    border-radius: 4px;
    background-color: #007bff;
    color: #fff;
    font-size: 16px;
    cursor: pointer;
  }

  .modal-form button[type="submit"]:hover {
    background-color: #0056b3;
  }

  /* Additional CSS for radio buttons in a single line */
  .modal-form label input[type="radio"] {
    display: inline-block;
    vertical-align: baseline;
  }
</style>

