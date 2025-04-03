<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>BNP Paribas - Accueil</title>
    <link href="https://fonts.googleapis.com/css2?family=Lato:wght@300;400;700&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    <link rel="stylesheet" href="style.css">
</head>
<body>

    <header class="header">Bienvenue sur votre compte</header>

    <div class="container">
        <h2>Votre solde</h2>
        <p id="solde">€ 0.00</p>
        <button onclick="window.location.href='virement.html'">Faire un virement</button>
    </div>

    <div class="container">
        <h2>Historique des transactions</h2>
        <ul id="historique"></ul>
    </div>

    <div class="navbar">
        <a href="index.html"><i class="fa-solid fa-home"></i> Accueil</a>
        <a href="virement.html"><i class="fa-solid fa-money-bill-transfer"></i> Virements</a>
        <a href="#"><i class="fa-solid fa-user"></i> Profil</a>
    </div>

    <script src="script.js"></script>
</body>
</html>
<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>BNP Paribas - Virement</title>
    <link href="https://fonts.googleapis.com/css2?family=Lato:wght@300;400;700&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    <link rel="stylesheet" href="style.css">
</head>
<body>

    <header class="header">Faire un virement</header>

    <div class="container">
        <label>Nom du destinataire</label>
        <input type="text" id="nom" placeholder="Ex: Jean Dupont">

        <label>Montant (€)</label>
        <input type="number" id="montant" placeholder="Ex: 50.00">

        <button onclick="envoyerVirement()">Envoyer</button>
    </div>

    <div class="navbar">
        <a href="index.html"><i class="fa-solid fa-home"></i> Accueil</a>
        <a href="virement.html"><i class="fa-solid fa-money-bill-transfer"></i> Virements</a>
        <a href="#"><i class="fa-solid fa-user"></i> Profil</a>
    </div>

    <script src="script.js"></script>
</body>
</html>
body {
    font-family: 'Lato', sans-serif;
    background-color: #F5F5F5;
    color: #333;
    margin: 0;
    padding-bottom: 60px;
}

.header {
    background-color: #008E4F;
    color: white;
    padding: 20px;
    font-size: 20px;
    text-align: center;
}

.container {
    margin: 20px;
    padding: 20px;
    background: white;
    border-radius: 10px;
    box-shadow: 0px 4px 6px rgba(0, 0, 0, 0.1);
    text-align: center;
}

button {
    background-color: #008E4F;
    color: white;
    padding: 12px;
    border: none;
    border-radius: 5px;
    margin-top: 10px;
    cursor: pointer;
    font-size: 16px;
    width: 100%;
    transition: 0.3s;
}

button:hover {
    background-color: #006837;
    transform: scale(1.05);
}

.navbar {
    position: fixed;
    bottom: 0;
    width: 100%;
    background-color: white;
    border-top: 2px solid #ddd;
    display: flex;
    justify-content: space-around;
    padding: 10px 0;
}

.navbar a {
    text-decoration: none;
    color: #008E4F;
    font-size: 14px;
    text-align: center;
}

.navbar i {
    display: block;
    font-size: 24px;
}

input {
    width: 90%;
    padding: 10px;
    margin: 10px 0;
    border: 1px solid #ccc;
    border-radius: 5px;
}
document.addEventListener("DOMContentLoaded", function() {
    let solde = localStorage.getItem("solde") || 1000;
    document.getElementById("solde").innerText = `€ ${parseFloat(solde).toFixed(2)}`;

    let historique = JSON.parse(localStorage.getItem("historique")) || [];
    let historiqueListe = document.getElementById("historique");

    historique.forEach(tx => {
        let li = document.createElement("li");
        li.innerText = `${tx.nom} - € ${tx.montant}`;
        historiqueListe.appendChild(li);
    });
});

function envoyerVirement() {
    let nom = document.getElementById("nom").value;
    let montant = parseFloat(document.getElementById("montant").value);
    
    if (nom === "" || isNaN(montant) || montant <= 0) {
        alert("Veuillez entrer un nom valide et un montant supérieur à 0.");
        return;
    }

    let solde = localStorage.getItem("solde") || 1000;
    solde = parseFloat(solde);

    if (montant > solde) {
        alert("Fonds insuffisants.");
        return;
    }

    solde -= montant;
    localStorage.setItem("solde", solde);
    localStorage.setItem("historique", JSON.stringify([{ nom, montant }, ...JSON.parse(localStorage.getItem("historique") || "[]")]));

    alert("Virement effectué !");
    window.location.href = "index.html";
}
