<!DOCTYPE html>
<html lang="fr">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>CinéList</title>

  <style>
      body {
        font-family: "Arial", sans-serif;
        margin: 0;
        padding: 0;
        background-color: #f4f4f9;
        height: 100vh;
        overflow-x: hidden;
        overflow-y: auto;
        box-sizing: border-box;
      }
      #homePage {
        display: flex;
        height: 100vh;
        justify-content: space-evenly;
        align-items: center;
      }
      .half-page {
        width: 40%;
        height: 60%;
        display: flex;
        justify-content: center;
        align-items: center;
        cursor: pointer;
        transition: transform 0.3s ease, box-shadow 0.3s ease;
        border-radius: 10px;
        position: relative;
        overflow: hidden;
      }
      .half-page:hover {
        transform: scale(1.05);
        box-shadow: 0 10px 30px rgba(0, 0, 0, 0.3);
      }
      .films-section {
        background-color: #ffd700;
        color: black;
      }
      .series-section {
        background-color: #e50914;
        color: white;
      }
      .expand {
        position: absolute;
        width: 0;
        height: 0;
        border-radius: 50%;
        background-color: inherit;
        transform: translate(-50%, -50%);
        transition: all 0.8s ease;
      }
      .expand-active {
        width: 200vw;
        height: 200vh;
        border-radius: 0;
        transition: all 0.8s ease;
      }
      .container {
        min-height: 100vh;
        width: 100%;
        padding: 20px;
        box-sizing: border-box;
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        text-align: center;
      }
      button,
      .add-button {
        background-color: #007bff;
        color: white;
        border: none;
        padding: 10px 20px;
        margin: 5px;
        border-radius: 5px;
        cursor: pointer;
        font-size: 1rem;
      }
      button:hover,
      .add-button:hover {
        background-color: #0056b3;
      }
      @keyframes slideUp {
        from {
          transform: translateY(20px);
          opacity: 0;
        }
        to {
          transform: translateY(0);
          opacity: 1;
        }
      }

      .slide-up {
        animation: slideUp 0.5s ease-out;
      }
      .films-slide-up {
        animation: slideUp 0.5s ease-out;
        background-color: #ffd700; /* Jaune pour films */
      }

      .series-slide-up {
        animation: slideUp 0.5s ease-out;
        background-color: #e50914; /* Rouge pour séries */
      }

      .home-container {
        position: fixed; /* Change de 'absolute' à 'fixed' */
        top: 10px; /* Toujours à 10px du haut */
        left: 10px; /* Toujours à 10px de la gauche */
        z-index: 100; /* S'assure qu'il reste au-dessus des autres éléments */
      }

      .home-button {
        background-color: transparent; /* Fond transparent */
        color: inherit; /* Couleur héritée pour respecter la DA */
        border: none; /* Pas de bordure */
        font-size: 2rem; /* Taille de l'icône */
        cursor: pointer; /* Curseur pointeur */
        display: flex; /* Aligne le contenu */
        align-items: center;
        justify-content: center;
        transition: transform 0.3s ease; /* Animation au survol */
      }
      .home-button:hover {
        transform: scale(1.1);
      }
      input[type="text"],
      input[type="number"] {
        padding: 10px;
        margin: 10px 0;
        border: 1px solid #ccc;
        border-radius: 5px;
        font-size: 1rem;
        width: 80%;
        max-width: 400px;
      }
      input[type="number"] {
        width: 80px;
      }
      .list-container {
        width: 100%; /* Largeur à 100% */
        max-width: 500px; /* Largeur maximale */
        margin: auto; /* Centre horizontalement */
        padding: 15px;
        background: #fff;
        border-radius: 10px;
        box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);

        /* Ajout pour centrer verticalement et horizontalement */
        display: flex;
        flex-direction: column; /* Organisation en colonne */
        justify-content: center; /* Centre verticalement */
        align-items: center; /* Centre horizontalement */
        min-height: 50vh; /* Minimum 50% de la hauteur de la page */
      }
      .list-item {
        width: 100%;
        display: flex;
        align-items: center;
        background-color: #f9f9f9;
        margin: 5px 0;
        padding: 10px;
        border-radius: 5px;
        cursor: grab;
        position: relative;
        color: inherit; /* Assurez-vous que la couleur du texte est héritée */
      }
      .list-item .rank {
        font-weight: bold;
        font-size: 1.2rem;
        color: #555; /* Vous pouvez ajuster cette couleur si nécessaire */
        margin-right: 10px;
        width: 30px;
        text-align: center;
      }
      .list-item img {
        width: 80px;
        height: 120px;
        border-radius: 5px;
        object-fit: cover;
        margin-right: 15px;
        flex-shrink: 0; /* Empêche l'image de se réduire lorsqu'il y a trop de texte */
      }
      .list-item span {
        color: inherit; /* Assurez-vous que le texte hérite de la couleur de son parent */
        white-space: normal; /* Permet au texte de se briser sur plusieurs lignes */
        word-wrap: break-word; /* Permet de couper les mots longs qui dépassent */
        word-break: break-word; /* Assure que même les mots très longs se cassent */
        overflow-wrap: break-word; /* Assure le retour à la ligne en cas de mot trop long */
        text-align: left; /* Aligne le texte à gauche */
      }

      .films-section .list-item,
      .series-section .list-item {
        color: #000; /* Couleur par défaut du texte */
      }

      .films-section .delete-icon,
      .series-section .delete-icon {
        color: #000; /* La croix est noire par défaut */
      }
      .delete-icon {
        position: absolute;
        right: 5px;
        font-size: 1.2rem;
        cursor: pointer;
        transition: color 0.3s ease;
        color: inherit; /* Assurez-vous que la couleur de la croix de suppression est aussi héritée */
      }
      .delete-icon:hover {
        color: red;
      }
      #mediaDetailPage {
        display: none;
        height: 100vh;
        padding: 20px;
        box-sizing: border-box;
      }

      .detail-section {
        display: flex;
        align-items: flex-start;
        gap: 20px;
        max-width: 1000px;
        margin: 0 auto;
        padding: 20px;
        background: #fff;
        border-radius: 10px;
        box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
      }
      .detail-image {
        width: 300px;
        height: auto;
        border-radius: 10px;
      }

      .detail-info {
        flex-grow: 1;
      }

      .detail-info p {
        margin: 10px 0;
      }

      .trailer {
        margin-top: 20px;
        width: 100%;
        aspect-ratio: 16 / 9; /* Format d'écran large */
      }

      .date-visionnage {
        margin-top: 20px;
        text-align: center;
      }
      .inline-info {
        display: flex;
        align-items: center; /* Alignement vertical */
        gap: 5px; /* Espacement entre les champs */
        flex-wrap: nowrap; /* Empêche les sauts de ligne */
        white-space: nowrap; /* Texte sur une seule ligne */
      }

      .editable-field {
        padding: 5px;
        border: 1px solid #ccc;
        border-radius: 5px;
        font-size: 1rem;
        width: auto; /* Taille adaptative */
        min-width: 30px; /* Largeur minimale */
      }

      .title-field {
        font-size: 1.5rem;
        margin-bottom: 10px;
        width: 100%; /* Pleine largeur pour le titre */
      }

      .detail-info h1 {
        margin-bottom: 10px;
        font-size: 2rem;
      }

      .critique {
        margin-top: 20px;
        padding: 10px;
        border: 1px solid #ddd;
        border-radius: 5px;
        height: 150px;
        width: 100%;
      }

      textarea,
      select,
      input[type="date"] {
        width: 100%;
        padding: 10px;
        margin: 10px 0;
        border-radius: 5px;
        border: 1px solid #ccc;
      }

      img {
        max-width: 100%;
        height: auto;
        border-radius: 10px;
        margin-bottom: 20px;
      }

      h1 {
        margin-bottom: 20px;
      }
      .delete-video {
        position: absolute;
        top: 5px;
        right: 5px;
        background-color: red;
        color: white;
        border: none;
        padding: 5px 10px;
        font-size: 1rem;
        border-radius: 50%;
        cursor: pointer;
        z-index: 10;
      }
      .delete-video:hover {
        background-color: darkred;
      }
      .trailer iframe,
      .trailer-container iframe {
        width: 100%;
        aspect-ratio: 16 / 9;
        border: none;
        margin-top: 10px;
      }

      #watch-location {
        position: fixed;
        top: 20px;
        right: 20px;
        background-color: #333;
        color: white;
        border-radius: 10px;
        padding: 10px;
        box-shadow: 0 4px 10px rgba(0, 0, 0, 0.3);
        font-size: 1rem;
        z-index: 100;
        display: flex;
        flex-direction: row;
        align-items: flex-start;
        gap: 10px;
        display: none; /* Caché par défaut */
      }

      .platform-option {
        gap: 8px;
        cursor: pointer;
        padding: 8px;
        border-radius: 5px;
        background-color: #444;
        transition: background-color 0.3s ease;
        width: 40px;
        height: 40px;
      }

      .platform-option:hover {
        background-color: #555;
      }
    </style>
  </head>
  <body>
    <div id="mediaDetailPage">
      <div class="home-container">
        <button class="home-button" onclick="goBack()">⬅️</button>
      </div>

   <div class="detail-section">
        <!-- Image -->
        <img id="detailImage" class="detail-image" />

        <!-- Informations détaillées -->
  <div class="detail-info">
          <!-- Titre -->
          <h1 id="detailTitle" class="title-field">Titre</h1>

          <!-- Durée -->
<p class="inline-info">
            <label for="detailDuration">Durée :</label>
            <input
              id="detailDuration"
              type="text"
              placeholder=""
              class="editable-field"
              size="1"
            />
          </p>

          <!-- Date de sortie -->
  <p class="inline-info">
            <label for="detailReleaseYear">Année de sortie :</label>
            <input
              id="detailReleaseYear"
              type="text"
              class="editable-field"
              size="1"
            />
          </p>

          <!-- Réalisateur -->
  <p class="inline-info">
            <label for="detailDirector">Réalisateur :</label>
            <input
              id="detailDirector"
              type="text"
              class="editable-field"
              size="1"
            />
          </p>

          <!-- Casting -->
   <p class="inline-info">
            <label for="detailCast">Casting :</label>
            <input
              id="detailCast"
              type="text"
              class="editable-field"
              size="1"
            />
          </p>
          <p class="date-visionnage">
            <label for="watchDate">Vu le:</label>
            <input type="date" id="watchDate" size="1" />
          </p>

          <!-- Critique -->
   <textarea
            id="critique"
            class="critique"
            placeholder="Écrire une critique..."
          ></textarea>

          <!-- Zone pour le lien YouTube -->
   <input
            id="trailerLinkInput"
            type="text"
            placeholder="Collez le lien YouTube ici"
            class="editable-field"
          />
          <!-- Conteneur pour la vidéo -->
          <div id="trailerContainer" class="trailer"></div>
        </div>
      </div>
    </div>

    <!-- Page d'accueil -->
  <div id="homePage">
      <div class="half-page films-section" id="filmsPageButton">
        <div class="section-title">🎬 Films</div>
      </div>

  <div class="half-page series-section" id="seriesPageButton">
        <div class="section-title">📺Séries</div>
      </div>
    </div>

    <!-- Page Films -->
<div id="filmsPage" class="container films-section" style="display: none">
      <h2>🎬 Films</h2>
      <input type="text" id="filmInput" placeholder="Nom du film" />
      <input type="number" id="filmPosition" placeholder="#" />
      <button class="add-button" onclick="addItem('films')">Ajouter</button>
      <div id="filmsList" class="list-container"></div>
      <div class="home-container">
        <button class="home-button" onclick="goHome()">⬅️</button>
      </div>
    </div>

    <!-- Page Séries -->
 <div id="seriesPage" class="container series-section" style="display: none">
      <h2>📺 Séries</h2>
      <input type="text" id="serieInput" placeholder="Nom de la série" />
      <input type="number" id="seriePosition" placeholder="#" />
      <button class="add-button" onclick="addItem('series')">Ajouter</button>
      <div id="seriesList" class="list-container"></div>
      <div class="home-container">
        <button class="home-button" onclick="goHome()">⬅️</button>
      </div>
    </div>

    <!-- Section pour indiquer où le média a été vu -->
<div id="watch-location" class="watch-location">
      <div id="cinemaButton" class="platform-option">
        <img
          src="https://play-lh.googleusercontent.com/N0aZP4IjTnaUwR4JfDEdrmMdY7SEAIPpaz4arLwptPwQxRNu42H_HnTIsHXtOZaSFchq=w240-h480-rw"
          alt="Cinéma"
        />
      </div>

  <div id="primeButton" class="platform-option">
        <img
          src="https://play-lh.googleusercontent.com/mZ6pRo5-NnrO9GMwFNrK5kShF0UrN5UOARVAw64_5aFG6NgEHSlq-BX5I8TEXtTOk9s=w240-h480-rw"
          alt="Prime Vidéo"
        />
      </div>

  <div id="netflixButton" class="platform-option">
        <img
          src="https://play-lh.googleusercontent.com/TBRwjS_qfJCSj1m7zZB93FnpJM5fSpMA_wUlFDLxWAb45T9RmwBvQd5cWR5viJJOhkI=w240-h480-rw"
          alt="Netflix"
        />
      </div>

  <div id="disneyButton" class="platform-option">
        <img
          src="https://m.media-amazon.com/images/I/719t3jd2NeL.png"
          alt="Disney+"
        />
      </div>

  <div id="appleButton" class="platform-option">
        <img
          src="https://play-lh.googleusercontent.com/zovfDsfyegE7SF3hCrN_hWPiQ2VLSh_Hreg20YsgQD5d9rfeq_HLA1fdq3q9zn-QNg=w240-h480-rw"
          alt="Apple TV+"
        />
      </div>

   <div id="autreButton" class="platform-option">
        <img
          src="https://www.svgrepo.com/show/479050/question-mark.svg"
          alt="Autre"
        />
      </div>
    </div>

   <script>
      //---Variables globales---\\
      const detailImage = document.getElementById("detailImage");
      let scrollInterval;
      let isDragging = false;
      let draggedItem = null;

      ///--1-FONCTIONS: Navigation entre les pages---\\\

      function navigateToPage(pageId, section, event) {
        // Sauvegarde de la page active dans sessionStorage
        sessionStorage.setItem("activePage", section);

        const homePage = document.getElementById("homePage");
        const targetPage = document.getElementById(pageId);
        const body = document.body;

        // Supprimer les anciennes classes de couleur
        body.classList.remove("films-page", "series-page");
        targetPage.classList.remove("films-slide-up", "series-slide-up");

        // Appliquer l'animation avec un délai
        if (section === "films-section") {
          targetPage.classList.add("films-slide-up");
        } else if (section === "series-section") {
          targetPage.classList.add("series-slide-up");
        }
        // Masquer la page d'accueil avec un fondu
        homePage.style.transition = "opacity 0.5s ease";
        homePage.style.opacity = 0;

        // Délai pour afficher la page cible avec animation
        setTimeout(() => {
          homePage.style.display = "none";

          // Rendre visible la nouvelle page et appliquer l'effet
          targetPage.style.display = "flex"; // Utiliser flex pour conserver la structure
          targetPage.style.opacity = 1;
        }, 500); // Temps d'attente pour la transition

        // Ajouter la classe en fonction de la section choisie
        if (section === "films-section") {
          body.classList.add("films-page");
        } else if (section === "series-section") {
          body.classList.add("series-page");
        }

        // Crée un cercle pour l'animation
        const circle = document.createElement("div");
        circle.classList.add("expand");

        // Définir la couleur du cercle en fonction de la section
        if (section === "films-section") {
          circle.style.backgroundColor = "#ffd700"; // Jaune pour Films
        } else if (section === "series-section") {
          circle.style.backgroundColor = "#e50914"; // Rouge pour Séries
        }

        // Ajouter le cercle au corps
        document.body.appendChild(circle);

        // Positionner le cercle au point de clic
        const x = event.clientX;
        const y = event.clientY;
        circle.style.top = `${y}px`;
        circle.style.left = `${x}px`;

        // Déclencher l'animation
        setTimeout(() => {
          circle.classList.add("expand-active");
        }, 10);

        // Supprimer le cercle après l'animation
        setTimeout(() => {
          circle.remove();
        }, 800);

        // Masquer la page d'accueil et afficher la nouvelle page
        homePage.style.transition = "opacity 0.5s ease";
        homePage.style.opacity = 0;

        setTimeout(() => {
          homePage.style.display = "none";
          targetPage.style.display = "block";
        }, 800);
      }

      function goHome() {
        const homePage = document.getElementById("homePage");
        const pages = [
          document.getElementById("filmsPage"),
          document.getElementById("seriesPage"),
        ];
        const body = document.body;

        body.classList.remove("films-page", "series-page");

        pages.forEach((page) => page.classList.remove("active"));

        setTimeout(() => {
          pages.forEach((page) => (page.style.display = "none"));
          homePage.style.display = "flex";
          homePage.style.opacity = 1;
        }, 500);
      }

      function goBack() {
        // Masquer la page de détails
        document.getElementById("mediaDetailPage").style.display = "none";

        // Récupérer la page active depuis sessionStorage
        const activePage = sessionStorage.getItem("activePage");

        // Masquer la section "Where to watch"
        document.getElementById("watch-location").style.display = "none"; // Cacher la section

        if (activePage === "films-section") {
          document.getElementById("filmsPage").style.display = "block"; // Affiche la page Films
        } else if (activePage === "series-section") {
          document.getElementById("seriesPage").style.display = "block"; // Affiche la page Séries
        }

        // Recharge la vidéo lors du retour
        updateTrailer();
      }

      //-Écouteurs d'événements-\\

      document.addEventListener("DOMContentLoaded", () => {
        document
          .getElementById("filmsPageButton")
          .addEventListener("click", (e) => {
            navigateToPage("filmsPage", "films-section", e);
          });

        document
          .getElementById("seriesPageButton")
          .addEventListener("click", (e) => {
            navigateToPage("seriesPage", "series-section", e);
          });
      });

      ///--2-FONCTIONS: Gestion des détails des médias---\\\

      function showMediaDetails(
        id,
        title = "Titre",
        imgSrc = "https://via.placeholder.com/300x450",
        releaseYear = "",
        duration = "",
        director = "",
        cast = ""
      ) {
        if (!id) {
          console.error("L'ID du média est manquant.");
          return;
        }

        // Enregistrer l'ID spécifique au média
        sessionStorage.setItem("currentMediaId", id);

        // Masquer les autres pages
        document.getElementById("filmsPage").style.display = "none";
        document.getElementById("seriesPage").style.display = "none";
        document.getElementById("homePage").style.display = "none";

        const detailPage = document.getElementById("mediaDetailPage");
        detailPage.style.display = "block";

        // Afficher la section "Where to watch"
        document.getElementById("watch-location").style.display = "flex"; // Affiche la section

        // Charger les informations sauvegardées
        const savedDetails = JSON.parse(
          sessionStorage.getItem(`mediaDetails_${id}`)
        );

        // Réinitialiser la zone de texte
        const input = document.getElementById("trailerLinkInput");
        input.style.display = "block"; // Toujours réafficher l'entrée au début
        input.value = ""; // Réinitialiser la valeur de l'entrée

        const trailerContainer = document.getElementById("trailerContainer");
        trailerContainer.innerHTML = ""; // Effacer l'iframe existante

        if (savedDetails) {
          document.getElementById("detailImage").src = savedDetails.image || "";
          document.getElementById("detailTitle").textContent =
            savedDetails.title || title;
          document.getElementById("detailDuration").value =
            savedDetails.duration || duration;
          document.getElementById("detailReleaseYear").value =
            savedDetails.releaseYear || releaseYear;
          document.getElementById("detailDirector").value =
            savedDetails.director || director;
          document.getElementById("detailCast").value =
            savedDetails.cast || cast;
          document.getElementById("critique").value =
            savedDetails.critique || "";
          document.getElementById("watchDate").value =
            savedDetails.watchDate || "";

          // Si un trailer est enregistré, charger la vidéo
          if (savedDetails.trailerLink) {
            input.value = savedDetails.trailerLink;
            updateTrailer(id); // Mise à jour avec l'ID spécifique
          }
        } else {
          // Réinitialiser les valeurs par défaut si aucune donnée n'est enregistrée
          document.getElementById("detailImage").src = imgSrc;
          document.getElementById("detailTitle").textContent = title;
          document.getElementById("detailDuration").value = duration;
          document.getElementById("detailReleaseYear").value = releaseYear;
          document.getElementById("detailDirector").value = director;
          document.getElementById("detailCast").value = cast;
          document.getElementById("critique").value = "";
          document.getElementById("watchDate").value = "";
          input.style.display = "block"; // Rendre la zone visible pour un nouveau lien
        }
      }

      function saveMediaDetails() {
        const currentMediaId = sessionStorage.getItem("currentMediaId");
        if (!currentMediaId) return;

        const details = {
          title: document.getElementById("detailTitle").textContent,
          duration: document.getElementById("detailDuration").value,
          releaseYear: document.getElementById("detailReleaseYear").value,
          director: document.getElementById("detailDirector").value,
          cast: document.getElementById("detailCast").value,
          critique: document.getElementById("critique").value,
          trailerLink: document.getElementById("trailerLinkInput").value,
          watchDate: document.getElementById("watchDate").value,
          image: document.getElementById("detailImage").src, // Assure que l'image est sauvegardée
        };

        sessionStorage.setItem(
          `mediaDetails_${currentMediaId}`,
          JSON.stringify(details)
        );

        updateTrailer(currentMediaId); // Mise à jour immédiate de la vidéo
      }

      function loadMediaDetails() {
        // Récupérer les détails sauvegardés
        const savedDetails = JSON.parse(sessionStorage.getItem("mediaDetails"));

        if (savedDetails) {
          console.log("Chargement des détails :", savedDetails); // Pour vérifier les données récupérées

          // Mettre à jour les champs avec les détails sauvegardés
          document.getElementById("detailTitle").textContent =
            savedDetails.title || "Titre";
          document.getElementById("detailDuration").value =
            savedDetails.duration || "";
          document.getElementById("detailReleaseYear").value =
            savedDetails.releaseYear || "";
          document.getElementById("detailDirector").value =
            savedDetails.director || "";
          document.getElementById("detailCast").value = savedDetails.cast || "";
          document.getElementById("critique").value =
            savedDetails.critique || "";
          document.getElementById("trailerLinkInput").value =
            savedDetails.trailerLink || "";
          document.getElementById("watchDate").value =
            savedDetails.watchDate || "";
          document.getElementById("detailImage").src =
            savedDetails.image || "https://via.placeholder.com/300x450";

          // Si un lien YouTube est présent, mettre à jour la vidéo
          if (savedDetails.trailerLink) {
            updateTrailer();
          }
        } else {
          console.warn("Aucun détail sauvegardé !");
        }
      }

      function updateTrailer(id) {
        const trailerContainer = document.getElementById("trailerContainer");
        const input = document.getElementById("trailerLinkInput");

        // Charger les détails spécifiques au média
        const savedDetails = JSON.parse(
          sessionStorage.getItem(`mediaDetails_${id}`)
        );
        const trailerLink =
          savedDetails && savedDetails.trailerLink
            ? savedDetails.trailerLink
            : input.value;

        // Vérifier si le lien est valide
        const youtubeRegex =
          /(?:https?:\/\/)?(?:www\.)?(?:youtube\.com\/(?:embed\/|v\/|watch\?v=)|youtu\.be\/)([a-zA-Z0-9_-]{11})/;
        const match = trailerLink.match(youtubeRegex);

        if (match && match[1]) {
          const videoId = match[1];
          const newSrc = `https://www.youtube.com/embed/${videoId}`;

          trailerContainer.innerHTML = `
            <div style="position: relative;">
                <button class="delete-video" onclick="removeTrailer('${id}')">✖</button>
                <iframe src="${newSrc}" frameborder="0" allowfullscreen></iframe>
            </div>
        `;
          input.style.display = "none"; // Cache l'entrée après ajout
        } else {
          // Réinitialiser si aucun lien valide
          console.warn("Lien YouTube invalide !");
          trailerContainer.innerHTML = ""; // Vide l'affichage
          input.style.display = "block"; // Rendre l'entrée visible
        }
      }

      function removeTrailer(id) {
        const trailerContainer = document.getElementById("trailerContainer");
        trailerContainer.innerHTML = ""; // Efface l'iframe
        const input = document.getElementById("trailerLinkInput");
        input.style.display = "block"; // Réaffiche l'entrée
        input.value = ""; // Réinitialise l'entrée

        // Supprimer le lien sauvegardé
        const savedDetails =
          JSON.parse(sessionStorage.getItem(`mediaDetails_${id}`)) || {};
        savedDetails.trailerLink = ""; // Vide le lien
        sessionStorage.setItem(
          `mediaDetails_${id}`,
          JSON.stringify(savedDetails)
        );

        saveMediaDetails();
      }

      //-Écouteurs d'événements-\\

      document
        .getElementById("trailerLinkInput")
        .addEventListener("input", () => {
          const input = document.getElementById("trailerLinkInput").value;

          // Vérifie si l'entrée correspond à un lien YouTube valide
          const youtubeRegex =
            /(?:https?:\/\/)?(?:www\.)?(?:youtube\.com\/(?:embed\/|v\/|watch\?v=)|youtu\.be\/)([a-zA-Z0-9_-]{11})/;

          const match = input.match(youtubeRegex);

          if (match && match[1]) {
            const videoId = match[1];
            const newSrc = `https://www.youtube.com/embed/${videoId}`;

            // Met à jour dynamiquement la vidéo
            const trailerContainer =
              document.getElementById("trailerContainer");
            trailerContainer.innerHTML = `<iframe src="${newSrc}" frameborder="0" allowfullscreen></iframe>`;
          }

          updateTrailer(); // Mise à jour avec l'ID fixe
          saveMediaDetails(); // Sauvegarde
        });

      document
        .getElementById("watchDate")
        .addEventListener("change", saveMediaDetails);

      document.addEventListener("DOMContentLoaded", () => {
        loadMediaDetails(); // Recharge les détails sauvegardés
        // Recharge toujours la vidéo sauvegardée, même si l'input est vide
        updateTrailer();
      });

      detailImage.addEventListener("dragover", (e) => e.preventDefault());

      detailImage.addEventListener("drop", (e) => {
        e.preventDefault();
        const file = e.dataTransfer.files[0]; // Récupérer le fichier

        if (file && file.type.startsWith("image/")) {
          const reader = new FileReader();

          reader.onload = (event) => {
            const currentMediaId = sessionStorage.getItem("currentMediaId");
            if (!currentMediaId) return;

            // Mise à jour de l'image dans la page de détail
            detailImage.src = event.target.result;

            // Mise à jour dans sessionStorage
            const savedDetails =
              JSON.parse(
                sessionStorage.getItem(`mediaDetails_${currentMediaId}`)
              ) || {};
            savedDetails.image = event.target.result;
            sessionStorage.setItem(
              `mediaDetails_${currentMediaId}`,
              JSON.stringify(savedDetails)
            );

            // Mise à jour dans la liste
            const listItem = document.querySelector(
              `[data-id="${currentMediaId}"] img`
            );
            if (listItem) listItem.src = event.target.result; // Mise à jour de l'image dans la liste
          };
          reader.readAsDataURL(file); // Lecture du fichier
        } else {
          alert("Veuillez déposer une image valide !");
        }
      });

      ///--3-FONCTIONS: Gestion des listes---\\\

      function addItem(type) {
        const inputId = type === "films" ? "filmInput" : "serieInput";
        const positionId = type === "films" ? "filmPosition" : "seriePosition";
        const listId = type === "films" ? "filmsList" : "seriesList";

        const input = document.getElementById(inputId);
        const positionInput = document.getElementById(positionId);
        const list = document.getElementById(listId);

        const value = input.value.trim();
        const position = parseInt(positionInput.value);

        if (!value) return alert("Veuillez entrer un nom.");

        const id = `${type}_${Date.now()}`; // ID unique basé sur l'heure

        const item = document.createElement("div");
        item.className = "list-item slide-up"; // Ajout de l'effet slide-up
        item.draggable = true;
        item.setAttribute("data-id", id); // Ajouter l'ID dans l'attribut personnalisé

        item.addEventListener("dragstart", () => startDrag(item));
        item.addEventListener("dragover", dragOver);
        item.addEventListener("drop", dropItem);

        // Création d'une zone de dépôt d'image
        const img = document.createElement("img");
        img.src = "https://via.placeholder.com/120x180";
        img.alt = value;
        img.onclick = () => showMediaDetails(id, value, img.src);

        img.onclick = () => {
          const id = item.getAttribute("data-id"); // Récupérer l'ID depuis l'attribut
          showMediaDetails(id, value, img.src);
        };

        const dropZone = document.createElement("div");
        dropZone.className = "drop-zone";
        dropZone.appendChild(img);

        dropZone.addEventListener("dragover", handleDragOver);
        dropZone.addEventListener("dragleave", handleDragLeave);
        dropZone.addEventListener("drop", (e) => handleImageDrop(e, img));

        const text = document.createElement("span");
        text.textContent = value;

        const deleteIcon = document.createElement("span");
        deleteIcon.className = "delete-icon";
        deleteIcon.textContent = "✖";
        deleteIcon.onclick = () => {
          item.remove();
          updateListNumbers(list);
        };

        const rank = document.createElement("div");
        rank.className = "rank";
        rank.textContent = list.children.length + 1;

        item.append(rank, dropZone, text, deleteIcon);

        if (
          !isNaN(position) &&
          position > 0 &&
          position <= list.children.length + 1
        ) {
          list.insertBefore(item, list.children[position - 1]);
        } else {
          list.appendChild(item);
        }

        input.value = "";
        positionInput.value = "";
        updateListNumbers(list);
      }

      function updateListNumbers(list) {
        list.querySelectorAll(".list-item").forEach((item, index) => {
          item.querySelector(".rank").textContent = index + 1;
        });
      }

      function startDrag(item) {
        // Début du drag
        draggedItem = item;
        isDragging = true; // Active le mode "dragging"
        setTimeout(() => item.classList.add("dragging"), 0);

        // Ajoute l'écouteur de mouvement de la souris pour déclencher le scroll
        document.addEventListener("mousemove", autoScroll);
      }

      function dragOver(e) {
        e.preventDefault();

        const list = e.currentTarget.parentElement;
        const draggingItem = list.querySelector(".dragging");

        if (!draggingItem) return;

        const items = Array.from(list.children);
        const currentIndex = items.indexOf(draggedItem);
        const targetIndex = items.indexOf(e.currentTarget);

        if (currentIndex > targetIndex) {
          list.insertBefore(draggedItem, e.currentTarget);
        } else {
          list.insertBefore(draggedItem, e.currentTarget.nextSibling);
        }

        // Met à jour les positions des IDs dans la liste
        updateListNumbers(list);
        saveMediaDetails(); // Mise à jour immédiate des détails après déplacement
      }

      function dropItem() {
        if (draggedItem) {
          draggedItem.classList.remove("dragging");
        }
        draggedItem = null; // Réinitialise après utilisation
        isDragging = false;

        // Supprime les événements liés au défilement
        document.removeEventListener("mousemove", autoScroll);
        stopAutoScroll();
      }

      function handleDragOver(e) {
        e.preventDefault(); // Empêche le comportement par défaut pour autoriser le drop
        e.currentTarget.classList.add("highlight"); // Ajoute une classe pour surligner la zone
      }

      function handleDragLeave(e) {
        e.currentTarget.classList.remove("highlight"); // Supprime la classe surlignée
      }

      function autoScroll(e) {
        const scrollSpeed = 10; // Vitesse de défilement
        const scrollZone = 350; // Zone d'activation élargie à 350px

        clearInterval(scrollInterval); // Arrête tout défilement précédent

        // Détermine si l'utilisateur est proche du haut
        if (e.clientY < scrollZone) {
          scrollInterval = setInterval(() => {
            window.scrollBy(0, -scrollSpeed);
          }, 10);
        }
        // Détermine si l'utilisateur est proche du bas
        else if (window.innerHeight - e.clientY < scrollZone) {
          scrollInterval = setInterval(() => {
            window.scrollBy(0, scrollSpeed);
          }, 10);
        }
        // Sinon, arrête le défilement
        else {
          clearInterval(scrollInterval);
        }
      }

      function stopAutoScroll() {
        // Arrête le défilement automatique
        clearInterval(scrollInterval);
      }

      //-Écouteurs d'événements-\\

      ["filmInput", "filmPosition"].forEach((id) => {
        document
          .getElementById(id)
          .addEventListener("keypress", (e) => handleEnter(e, "films"));
      });

      ["serieInput", "seriePosition"].forEach((id) => {
        document
          .getElementById(id)
          .addEventListener("keypress", (e) => handleEnter(e, "series"));
      });

      document
        .getElementById("cinemaButton")
        .addEventListener("click", function () {
          document.getElementById("cinemaButton").style.display = "block"; // Affiche seulement Cinéma
          document.getElementById("netflixButton").style.display = "none";
          document.getElementById("autreButton").style.display = "none";
          document.getElementById("disneyButton").style.display = "none";
          document.getElementById("primeButton").style.display = "none";
          document.getElementById("appleButton").style.display = "none";
        });

      document
        .getElementById("netflixButton")
        .addEventListener("click", function () {
          document.getElementById("netflixButton").style.display = "block"; // Affiche seulement Netflix
          document.getElementById("autreButton").style.display = "none";
          document.getElementById("disneyButton").style.display = "none";
          document.getElementById("primeButton").style.display = "none";
          document.getElementById("appleButton").style.display = "none";
          document.getElementById("cinemaButton").style.display = "none";
        });

      document
        .getElementById("autreButton")
        .addEventListener("click", function () {
          document.getElementById("netflixButton").style.display = "none";
          document.getElementById("autreButton").style.display = "block"; // Affiche seulement autre
          document.getElementById("disneyButton").style.display = "none";
          document.getElementById("primeButton").style.display = "none";
          document.getElementById("appleButton").style.display = "none";
          document.getElementById("cinemaButton").style.display = "none";
        });

      document
        .getElementById("disneyButton")
        .addEventListener("click", function () {
          document.getElementById("netflixButton").style.display = "none";
          document.getElementById("autreButton").style.display = "none";
          document.getElementById("disneyButton").style.display = "block"; // Affiche seulement Disney+
          document.getElementById("primeButton").style.display = "none";
          document.getElementById("appleButton").style.display = "none";
          document.getElementById("cinemaButton").style.display = "none";
        });

      document
        .getElementById("primeButton")
        .addEventListener("click", function () {
          document.getElementById("netflixButton").style.display = "none";
          document.getElementById("autreButton").style.display = "none";
          document.getElementById("disneyButton").style.display = "none";
          document.getElementById("primeButton").style.display = "block"; // Affiche seulement Prime Video
          document.getElementById("appleButton").style.display = "none";
          document.getElementById("cinemaButton").style.display = "none";
        });

      document
        .getElementById("appleButton")
        .addEventListener("click", function () {
          document.getElementById("netflixButton").style.display = "none";
          document.getElementById("autreButton").style.display = "none";
          document.getElementById("disneyButton").style.display = "none";
          document.getElementById("primeButton").style.display = "none";
          document.getElementById("appleButton").style.display = "block"; // Affiche seulement Apple TV+
          document.getElementById("cinemaButton").style.display = "none";
        });

      ///--4-FONCTIONS: Interaction avec les champs de texte et images---\\\

      function adjustFieldWidth(input) {
        input.style.width = input.value.length + 1 + "ch";
      }

      function autoResizeInput(input) {
        input.style.width = input.value.length + 1 + "ch"; // Largeur dynamique
      }

      function handleImageDrop(e, imgElement) {
        e.preventDefault();
        const file = e.dataTransfer.files[0];

        if (file && file.type.startsWith("image/")) {
          const reader = new FileReader();
          reader.onload = (event) => {
            imgElement.src = event.target.result;

            const id = imgElement.closest(".list-item").getAttribute("data-id");
            const savedDetails =
              JSON.parse(sessionStorage.getItem(`mediaDetails_${id}`)) || {};

            savedDetails.image = event.target.result; // Mise à jour de l'image
            sessionStorage.setItem(
              `mediaDetails_${id}`,
              JSON.stringify(savedDetails)
            );
          };
          reader.readAsDataURL(file);
        } else {
          alert("Veuillez déposer une image valide !");
        }
      }

      function handleEnter(event, type) {
        if (event.key === "Enter") addItem(type);
      }

      //-Écouteurs d'événements-\\

      document
        .querySelectorAll(".editable-field, textarea")
        .forEach((input) => {
          input.addEventListener("input", () => {
            saveMediaDetails(); // Sauvegarde après modification
            console.log(
              "Sauvegarde en cours :",
              sessionStorage.getItem("mediaDetails")
            ); // Debug
          });
        });

      document.querySelectorAll(".editable-field").forEach((input) => {
        input.addEventListener("input", () => adjustFieldWidth(input));
      });

      document.querySelectorAll(".editable-field").forEach((input) => {
        input.addEventListener("input", function () {
          autoResizeInput(this); // Ajuster la largeur pendant la saisie
        });

        // Ajuster à la taille initiale au chargement
        autoResizeInput(input);
      });

      ///---TEST---\\\
    </script>
  </body>
</html>
