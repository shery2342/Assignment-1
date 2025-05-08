# Assignment-1 {
  const express = require('express');
const app = express();
const port = 3000;

app.use(express.json());

let tasks = [];
let nextId = 1;

// POST /addTask → Add a new task
app.post('/addTask', (req, res) => {
  const { taskName } = req.body;
  if (!taskName) {
    return res.status(400).json({ error: 'taskName is required' });
  }

  const newTask = { id: nextId++, taskName };
  tasks.push(newTask);
  res.status(201).json(newTask);
});

// GET /tasks → Get all tasks
app.get('/tasks', (req, res) => {
  res.json(tasks);
});

// DELETE /task/:id → Delete a task by ID
app.delete('/task/:id', (req, res) => {
  const taskId = parseInt(req.params.id);
  const index = tasks.findIndex(task => task.id === taskId);

  if (index === -1) {
    return res.status(404).json({ error: 'Task not found' });
  }

  const deletedTask = tasks.splice(index, 1)[0];
  res.json({ message: 'Task deleted', task: deletedTask });
});

app.listen(port, () => {
  console.log(`Server running on http://localhost:${port}`);
});
