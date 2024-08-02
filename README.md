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
        
