<script>
  import ModalForm from './Modal.svelte';

  let formModal = false;
  // @ts-ignore
  let tasks = [];
  // @ts-ignore
  let editIndex = null; // Track the index of the task being edited
  


  // @ts-ignore
  function handleFormSubmit(event) {
     const { task, startTime, repeat, repeatTimes } = event.detail;
      // Assuming timeDate is '2023-12-18T12:13'
      const formattedDateTime = new Date(startTime).toLocaleString('en-US', {
        dateStyle: 'short', // Adjust the date style as needed (long, medium, short, full)
        timeStyle: 'short', // Adjust the time style as needed (long, medium, short)
      });


   const updatedTask = {
    task,
    startTime: formattedDateTime,
    repeat,
    repeatTimes,
    selectedJob: '/job',
    selectedatask:'/task',
  };

    // @ts-ignore
    if (editIndex !== null) {
      // If editIndex is not null, update existing task
      // @ts-ignore
      tasks[editIndex] = updatedTask;
      editIndex = null; // Reset editIndex after editing
      // @ts-ignore
      console.log("Task Edited:", tasks);
    } else {
      // If editIndex is null, add a new task
      // @ts-ignore
      tasks = [...tasks, updatedTask];
      console.log("New Task Added:", tasks);
    }
    formModal = false; // Close the modal after submission
  }

  // @ts-ignore
  function deleteTask(index) {
    // @ts-ignore
    tasks = tasks.filter((_, i) => i !== index);
    console.log("Task deleted:", tasks);
  }

  // @ts-ignore
  function editTask(index) {
    // Set the editIndex and populate form with existing task details
   
    editIndex = index;
    // @ts-ignore
    const taskToEdit = tasks[index];
    // Open modal for editing
    formModal = true;
    // Update form values with task details for editing
    // @ts-ignore
    values = {
    task: taskToEdit.task,
    startTime: taskToEdit.startTime,
    repeat: taskToEdit.repeat,
    repeatTimes: taskToEdit.repeatTimes,
  };
  }
  

   // @ts-ignore
   function handleSelectedTask(index, event) {
    const selectedTaskValue = event.target.value;
    // @ts-ignore
    if (tasks[index]) {
      // @ts-ignore
      tasks[index].selectedTask = selectedTaskValue;
      console.log('Selected Task:', selectedTaskValue);
    }
  }

  // @ts-ignore
  function handleSelectedJob(index, event) {
    const selectedJobValue = event.target.value;
    // @ts-ignore
    if (tasks[index]) {
      // @ts-ignore
      tasks[index].selectedJob = selectedJobValue;
      console.log('Selected Job:', selectedJobValue);
    }
  }
</script>


<div class="task-manager">
    <div class="header">
      <h1><i class="fa-solid fa-calendar-days"></i> Task Lists</h1>
      <button on:click={() => (formModal = true)} class="add-task-button">Add New Task</button>
      <div class="modal">
        {#if formModal}
          <ModalForm on:submit={handleFormSubmit} />
        {/if}
      </div>
    </div>
        <div class="task-manager-header">
          <h2>Task</h2>
          <h2>Start Time</h2>
          <h2>Repeat</h2>
          <!-- <h2>Repeat Times</h2> -->
          <!-- <h2>Task</h
          <!-- <h2>Task</h2> -->
          <h2>Job</h2>
          
          <h2>Edit</h2>
           <h2>Action</h2>
         
      </div>
      <div class="task-manager-list">
            <!-- Displaying Tasks -->
            {#each tasks as task, index (task)}
             <!-- Use a unique variable for each task's selection -->
             
                <div class="task-manager-list-item" key={task.task}>
                  <h2>{task.task}</h2>
                  <h2>{task.startTime}</h2>
                  <h2>{task.repeat}</h2>
                  
                 

                  <div class="job-dropdown">
                      <select bind:value={task.selectedJob} id={`jobDropdown_${index}`} on:change={(event) => handleSelectedJob(index, event)} >
                        <option value="/job">Job</option>
                        <option value="/create">Create</option>
                        <option value="/search">Search</option>
                        <option value="/fetch">Fetch</option>
                        <option value="/delete">Delete</option>
                        <option value="/save">Save</option>
                        <option value="/cancel">Cancel</option>
                        <option value="/activate">Activate</option>
                      
                        <option value="/executenow">Executenow</option>
                        <option value="/setdependency">Setdependency</option>
                        <option value="/removedependency">Removedependency</option>
                        
                      </select> 
                  </div>
                  
                  <div class="edit">
                    <button on:click={() => deleteTask(index)} class="delete-task-button"><i class="fa-solid fa-trash-can"></i></button>
                    <button on:click={() => editTask(index)} class="edit-task-button"><i class="fa-solid fa-pen-to-square"></i></button>
                  </div>

                 <div class="run-task">
                  <button>Run Task</button>
                 </div>

                </div>
                
            {/each}
        </div>
         
</div>


 
<style>
  .task-manager {
    display: flex;
    flex-direction: column;
    min-height: 100vh;
    overflow: hidden;
  }

  .header {
    display: flex;
    align-items: center;
    justify-content: space-between;
    margin-bottom: 20px;
    margin-right: 20px;
    margin-left: 20px;
  }

  .header h1 {
    font-weight: bold;
    font-size: 24px;
    display: flex;
    align-items: center;
  }

  .header h1 i {
    margin-right: 5px;
  }

  .add-task-button {
    background-color: black;
    color: red;
    border: 1px solid red;
    padding: 8px 16px;
    border-radius: 4px;
    cursor: pointer;
    width: fit-content;
  }

  .task-manager-header {
      display: grid; /* Use grid for more precise control */
  grid-template-columns: repeat(6, 0.5fr); /* Create 4 equally spaced columns */
  background-color: rgb(190, 196, 203);
  margin-left: 10px;
  margin-right: 10px;
  padding: 8px; /* Add padding for spacing */
  font-weight: bold;
  font-size: 16px;
  }

  

  .task-manager-list-item {
    display: grid; 
  grid-template-columns: repeat(6, 0.5fr); 
  align-items: center;
  padding: 8px;
  font-weight: normal;
  border-bottom: 1px solid #ccc;
  font-size: 14px;
  

  }

 .delete-task-button {
    background-color: #ff6347;
  color: white;
  border: none;
 
  border-radius: 4px;
  cursor: pointer;
  width: fit-content;
  transition: background-color 0.3s ease; 
  padding: 10px;
  margin-right: 5px;
  
} 
.edit-task-button {
    background-color: #ff6347;
    color: white;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    padding: 10px;
    
    transition: background-color 0.3s ease;  /* Smooth transition on hover */
}
.edit {
    position: relative;
    display: inline-block;
  }

 

  .delete-task-button:hover {
    background-color: #ff7f50;
  } 
  .edit-task-button:hover {
    background-color: #ff7f50;
  } 

  .modal {
    position: fixed;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    width: 40%;
    height: 40%;
    
  }
  
  
.job-dropdown {
    /* Include any other necessary styles */
    position: relative;
    display: inline-block;
  }

  select {
    font-weight: bold;
    padding: 8px;
    border-radius: 4px;
    border: 2px solid #ccc;
   
  }
.run-task {
    display: flex;
    justify-content:flex-start;
    align-items: flex-start;
  }
  .run-task button {
    /* Adjust button styles as needed */
    background-color: #008CBA;
    color: white;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    padding: 8px 16px;
    transition: background-color 0.3s ease; /* Smooth transition on hover */
  }

  .run-task button:hover {
    background-color: #005f73;
  }

  

  /* Additional styles as needed */
</style>
