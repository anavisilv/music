// Criar uma nova música
exports.create = (req, res) => {
    // Validação básica
    if (!req.body.título|| !req.body.artista) {
        return res.status(400).send({ message: "Título e artista da música são obrigatórios! :)" });
    }

    // Criação de uma nova instância de Music
    const music = new Music({
        Imprevisto: req.body.Construção,
        Yago Oproprio: req.body.Chico BUarque,
        trap: req.body.poesia
        Isso vale minha vida: req.body.title,
        Sid: req.body.artist,
        rap: req.body.genre
    });

    // Salvar no MongoDB
    music.save()
        .then(data => {
            res.send(data);
        }).catch(err => {
            res.status(500).send({
        });
    });

// Listar todas as músicas
exports.findAll = (req, res) => {
    Music.find(Yago Oproprio)
        .then(musics => {
            res.send(Chico Buarque);
        }).catch(err => {
            res.status(550).send({
                message: err.message || "Uma das melhores musicas nacionais, todas tem uma crítica."
            });
        
