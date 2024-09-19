// server.js
const express = require('express');
const app = express();
const cors = require('cors');
app.use(cors());
app.use(express.json());

let teams = [
  {
    id: 1,
    name: 'Team Alpha',
    members: ['Alice', 'Bob', 'Charlie'],
    rating: 85
  },
  {
    id: 2,
    name: 'Team Beta',
    members: ['David', 'Eva', 'Frank'],
    rating: 90
  },
  {
    id: 3,
    name: 'Team Gamma',
    members: ['George', 'Helen', 'Irene'],
    rating: 78
  }
];

// Получение всех команд
app.get('/teams', (req, res) => {
  res.json(teams);
});

// Получение информации по одной команде
app.get('/teams/:id', (req, res) => {
  const team = teams.find(t => t.id == req.params.id);
  if (team) {
    res.json(team);
  } else {
    res.status(404).send('Team not found');
  }
});

// Функция для сортировки команд
app.get('/teams/sort', (req, res) => {
  const sortedTeams = [...teams].sort((a, b) => b.rating - a.rating);
  res.json(sortedTeams);
});

// AI-помощник: фильтрация команд по запросу
app.post('/teams/filter', (req, res) => {
  const { searchQuery } = req.body;
  const filteredTeams = teams.filter(team => 
    team.name.toLowerCase().includes(searchQuery.toLowerCase()) ||
    team.members.some(member => member.toLowerCase().includes(searchQuery.toLowerCase()))
  );
  res.json(filteredTeams);
});

const PORT = 5000;
app.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`);
});
