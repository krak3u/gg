// server.js (Node.js + Express)
const express = require('express');
const mongoose = require('mongoose');
const app = express();
app.use(express.json());

// Подключение к MongoDB
mongoose.connect('mongodb://localhost:27017/hackathon_league', { useNewUrlParser: true, useUnifiedTopology: true });

// Модель участника
const Participant = mongoose.model('Participant', {
    name: String,
    team: String,
    rating: Number,
    role: String
});

// Получение всех участников
app.get('/participants', async (req, res) => {
    const participants = await Participant.find();
    res.json(participants);
});

// Создание нового участника
app.post('/participants', async (req, res) => {
    const participant = new Participant(req.body);
    await participant.save();
    res.json(participant);
});

// Получение всех команд
app.get('/teams', async (req, res) => {
    const teams = await Participant.aggregate([
        { $group: { _id: "$team", avgRating: { $avg: "$rating" } } },
        { $sort: { avgRating: -1 } }
    ]);
    res.json(teams);
});

// Запуск сервера
app.listen(5000, () => {
    console.log('Server is running on port 5000');
});
