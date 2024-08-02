/music-manager-api
│
├── /config
│   └── db.config.js      # Configuração do MongoDB
│
├── /controllers
│   └── musicController.js # Controladores das rotas da API
│
├── /models
│   └── Music.js           # Modelo do Mongoose para as músicas
│
├── /routes
│   └── musicRoutes.js     # Definição das rotas da API
│
├── /tests                 # Testes (opcional)
│   └── music.test.js
│
├── .gitignore
├── package.json
├── server.js              # Arquivo principal do servidor Express
└── README.md              # Documentação do projeto
const Music = require('../models/Music');

// Criar uma nova música
exports.create = (req, res) => {
    // Validação básica
    if (!req.body.title || !req.body.artist) {
        return res.status(400).send({ message: "Título e artista da música são obrigatórios!" });
    }

    // Criação de uma nova instância de Music
    const music = new Music({
        title: req.body.title,
        artist: req.body.artist,
        genre: req.body.genre
    });

    // Salvar no MongoDB
    music.save()
        .then(data => {
            res.send(data);
        }).catch(err => {
            res.status(500).send({
                message: err.message || "Ocorreu algum erro ao criar a música."
            });
        });
};

// Listar todas as músicas
exports.findAll = (req, res) => {
    Music.find()
        .then(musics => {
            res.send(musics);
        }).catch(err => {
            res.status(500).send({
                message: err.message || "Ocorreu algum erro ao recuperar as músicas."
            });
        });
};
const express = require('express');
const router = express.Router();
const musicController = require('../controllers/musicController');

// Rota para criar uma nova música
router.post('/music', musicController.create);

// Rota para listar todas as músicas
router.get('/music', musicController.findAll);

module.exports = router;
const express = require('express');
const bodyParser = require('body-parser');
const mongoose = require('mongoose');
const dbConfig = require('./config/db.config');

const app = express();

// Parse requests de content-type - application/x-www-form-urlencoded
app.use(bodyParser.urlencoded({ extended: true }));

// Parse requests de content-type - application/json
app.use(bodyParser.json());

// Conexão com o banco de dados MongoDB
mongoose.connect(dbConfig.url, {
    useNewUrlParser: true,
    useUnifiedTopology: true
}).then(() => {
    console.log("Conectado ao banco de dados MongoDB.");
}).catch(err => {
    console.log('Erro ao conectar ao banco de dados MongoDB:', err);
    process.exit();
});

// Definindo rotas
const musicRoutes = require('./routes/musicRoutes');
app.use('/api', musicRoutes);

// Porta do servidor
const PORT = process.env.PORT || 3000;
app.listen(PORT, () => {
    console.log(`Servidor está rodando na porta ${PORT}`);
});
