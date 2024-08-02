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
        
