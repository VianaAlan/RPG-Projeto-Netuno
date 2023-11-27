const readlineSync = require('readline-sync');

console.log("---------- Registre-se ----------");

let Nome, Email, Senha, ConfirmarSenha, Idade;
let DiaNascimento, MesNascimento, AnoNascimento;
let Classe, Arma, Genero, CorCabelo, CorPele;
let PontosDisponiveis = 50;
let Forca = 0, Precisao = 0, Destreza = 0, Mana = 0, Vida = 0;
let VelocidadeMontaria, TempoDescansoMontaria, ResistenciaMontaria, Montaria;

function validarNome() {
    Nome = readlineSync.question("Nome completo: ");
    let partesNome = Nome.split(' ');
    return partesNome.length >= 2;
}

function validarDataNascimento() {
    let dataNascimento = readlineSync.question("Data de nascimento (DDMMAAAA): ");
    if (dataNascimento.length !== 8) return false;

    DiaNascimento = parseInt(dataNascimento.substring(0, 2));
    MesNascimento = parseInt(dataNascimento.substring(2, 4));
    AnoNascimento = parseInt(dataNascimento.substring(4, 8));

    if (isNaN(DiaNascimento) || isNaN(MesNascimento) || isNaN(AnoNascimento) || AnoNascimento < 1900 || AnoNascimento > new Date().getFullYear()) {
        return false;
    }

    let dataNascimentoValida = new Date(AnoNascimento, MesNascimento - 1, DiaNascimento);
    return dataNascimentoValida.getMonth() === MesNascimento - 1 && dataNascimentoValida.getDate() === DiaNascimento;
}

function calcularIdade() {
    let hoje = new Date();
    let idade = hoje.getFullYear() - AnoNascimento;
    let mesAtual = hoje.getMonth() + 1;
    if (mesAtual < MesNascimento || (mesAtual === MesNascimento && hoje.getDate() < DiaNascimento)) {
        idade--;
    }
    return idade;
}

function escolherOpcao(opcoes, mensagem) {
    let escolha;
    do {
        console.log(mensagem);
        for (let i = 0; i < opcoes.length; i++) {
            console.log(`${i + 1}) ${opcoes[i]}`);
        }
        escolha = parseInt(readlineSync.question("Escolha (1 a " + opcoes.length + "): "));
    } while (isNaN(escolha) || escolha < 1 || escolha > opcoes.length);
    return escolha;
}

while (!validarNome()) {
    console.log("Por favor, insira seu nome completo!.");
}

while (!validarDataNascimento() || calcularIdade() < 18) {
    console.log("Você deve ter pelo menos 18 anos para se registrar.");
}

do {
    Email = readlineSync.question("Email: ");
    if (!/^\S+@\S+\.\S+$/.test(Email)) {
        console.log("Formato de email inválido. Use um formato como exemplo@email.com.");
    }
} while (!/^\S+@\S+\.\S+$/.test(Email));

do {
    Senha = readlineSync.question("Senha: ", { hideEchoBack: true });
    ConfirmarSenha = readlineSync.question("Confirme sua Senha: ", { hideEchoBack: true });
    if (Senha !== ConfirmarSenha) {
        console.log("As senhas não correspondem. Tente novamente.");
    }
} while (Senha !== ConfirmarSenha);

let podeRegistrar = true;

if (podeRegistrar) {
    console.log("Registro concluído com sucesso!");

    console.log("---------- Login ----------");

    let Tentativas = 0;
    let LoginEmail, LoginSenha;

    do {
        LoginEmail = readlineSync.question("Digite seu e-mail: ");
        LoginSenha = readlineSync.question("Digite sua senha: ", { hideEchoBack: true });

        if (LoginEmail === Email && LoginSenha === Senha) {
            console.log("Login bem sucedido!");
            console.log("---------- Início do Jogo ----------");
            break;
        } else {
            console.log("E-mail ou senha inválidos! Tente novamente.");
            Tentativas += 1;

            if (Tentativas >= 3) {
                console.log("Você excedeu o limite de tentativas. O programa será encerrado.");
                process.exit();
            }
        }
    } while (true);

    let classes = ["Paladino", "Atirador", "Guerreiro", "Bárbaro", "Arqueiro"];
    Classe = classes[escolherOpcao(classes, "Escolha a classe para jogar:") - 1];


    switch (Classe) {
        case "Paladino":
            Arma = escolherOpcao(["Cajado", "Alabardas"], "Escolha a arma para o Paladino:");
            break;
        case "Atirador":
            Arma = escolherOpcao(["Rifle", "Pistolas Duplas"], "Escolha a arma para o Atirador:");
            break;
        case "Guerreiro":
            Arma = escolherOpcao(["Lança", "Espada e Escudo"], "Escolha a arma para o Guerreiro:");
            break;
        case "Bárbaro":
            Arma = escolherOpcao(["Espada Longa", "Machado"], "Escolha a arma para o Bárbaro:");
            break;
        case "Arqueiro":
            Arma = escolherOpcao(["Arco", "Besta"], "Escolha a arma para o Arqueiro:");
            break;
        default:
            console.log("Escolha inválida.");
            process.exit();
    }

    switch (Classe) {
        case "Paladino":
            distribuirPontos(7, 10, 8, 15, 10);
            break;
        case "Atirador":
            distribuirPontos(7, 19, 16, 0, 8);
            break;
        case "Guerreiro":
            distribuirPontos(15, 5, 10, 0, 20);
            break;
        case "Bárbaro":
            distribuirPontos(20, 3, 6, 0, 21);
            break;
        case "Arqueiro":
            distribuirPontos(3, 20, 10, 5, 12);
            break;
        default:
            console.log("Escolha inválida.");
            process.exit();
    }

    let generos = ["Masculino", "Feminino"];
    Genero = generos[escolherOpcao(generos, "Gênero (Masculino, Feminino):") - 1];

    let coresCabelo = ["Preto", "Castanho Escuro", "Castanho Claro", "Loiro", "Ruivo", "Branco"];
    CorCabelo = coresCabelo[escolherOpcao(coresCabelo, "Cor de cabelo (Preto, Castanho Escuro, Castanho Claro, Loiro, Ruivo, Branco):") - 1];

    let coresPele = ["Preto", "Pardo", "Branco", "Indígena", "Amarelo"];
    CorPele = coresPele[escolherOpcao(coresPele, "Cor de pele (Preto, Pardo, Branco, Indígena, Amarelo):") - 1];

    let montarias = ["Panda", "Cavalo", "Tigre", "Elefante", "Lobo"];
    Montaria = montarias[escolherOpcao(montarias, "Escolha sua Montaria:") - 1];

    switch (Montaria) {
        case "Panda":
            distribuirPontosMontaria(3, 20, 10);
            break;
        case "Cavalo":
            distribuirPontosMontaria(7, 10, 6);
            break;
        case "Tigre":
            distribuirPontosMontaria(8, 5, 12);
            break;
        case "Elefante":
            distribuirPontosMontaria(4, 12, 15);
            break;
        case "Lobo":
            distribuirPontosMontaria(9, 5, 11);
            break;
        default:
            console.log("Escolha de montaria inválida.");
            process.exit();
    }

    console.log("---------- Informações do Personagem ----------");
    console.log("Classe: " + Classe);
    console.log("Arma: " + Arma);
    console.log("Gênero: " + Genero);
    console.log("Cor de cabelo: " + CorCabelo);
    console.log("Cor de pele: " + CorPele);

    console.log("---------- Atributos do Personagem ----------");
    console.log("Força: " + Forca);
    console.log("Precisão: " + Precisao);
    console.log("Destreza: " + Destreza);
    console.log("Mana: " + Mana);
    console.log("Vida: " + Vida);

    console.log("---------- Informações da Montaria ----------");
    console.log("Montaria: " + Montaria);
    console.log("Velocidade: " + VelocidadeMontaria);
    console.log("Tempo para descanso: " + TempoDescansoMontaria + "min");
    console.log("Resistência: " + ResistenciaMontaria);

    readlineSync.keyInPause("Pressione Enter para encerrar...");
} else {
    console.log("Registro não concluído. Programa encerrado.");
}

function distribuirPontos(forca, precisao, destreza, mana, vida) {
    if (PontosDisponiveis >= forca + precisao + destreza + mana + vida) {
        PontosDisponiveis -= forca + precisao + destreza + mana + vida;
        Forca += forca;
        Precisao += precisao;
        Destreza += destreza;
        Mana += mana;
        Vida += vida;
    } else {
        console.log("Pontos distribuidos automaticamente.");
        process.exit();
    }
}

function distribuirPontosMontaria(velocidade, tempoDescanso, resistencia) {
    VelocidadeMontaria = velocidade;
    TempoDescansoMontaria = tempoDescanso;
    ResistenciaMontaria = resistencia;
}
