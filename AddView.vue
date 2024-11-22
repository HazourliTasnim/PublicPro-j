<template>
    <div class="container">
      <!-- Box pour "Nouvelle tâche" -->
      <div class="title-box">
        <h1 class="title">Nouvelle tâche</h1>
        <button class="close-btn" @click="closeBox">X</button>
      </div>
  
      <!-- Box du formulaire -->
      <div class="form-box">
        <form @submit.prevent="handleSubmit">
          <!-- Champ Nom de la tâche -->
          <div class="form-group">
            <label for="taskName">Nom de la tâche</label>
            <input type="text" id="taskName" v-model="task.name" placeholder="Entrez le nom de la tâche" required />
          </div>
  
          <!-- Sélection de la catégorie avec boîtes cliquables -->
          <div class="form-group">
            <label>Catégorie</label>
            <div class="category-box">
              <div
                v-for="category in categories"
                :key="category"
                :class="['category', { selected: task.category === category }]"
                @click="task.category = category"
              >
                {{ category }}
              </div>
            </div>
          </div>
  
          <!-- Sélection du niveau d'importance avec boîtes cliquables -->
          <div class="form-group">
            <label>Niveau d'importance</label>
            <div class="importance-boxes">
              <div 
                class="importance-box"
                :class="{ selected: task.importance === 'less' }"
                @click="selectImportance('less')"
              >
                Moins important
              </div>
              <div 
                class="importance-box"
                :class="{ selected: task.importance === 'normal' }"
                @click="selectImportance('normal')"
              >
                Important
              </div>
              <div 
                class="importance-box"
                :class="{ selected: task.importance === 'urgent' }"
                @click="selectImportance('urgent')"
              >
                Urgent
              </div>
            </div>
  
            <!-- Barre de progression qui change en fonction de l'importance -->
            <div class="progress-bar">
              <div
                class="progress"
                :style="{ width: progressBarWidth + '%' }"
              >
                <span class="progress-label">
                  {{ importanceLabel }}
                </span>
              </div>
            </div>
          </div>
  
          <!-- Boutons Ajouter et Annuler -->
          <div class="button-group">
            <button type="button" @click="cancel" class="btn btn-cancel">Annuler</button>
            <button type="submit" class="btn btn-add">Ajouter</button>
          </div>
        </form>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        task: {
          name: '',
          category: '',
          importance: 'less', // Valeur initiale de l'importance (Moins important)
        },
        categories: ['Travail', 'Personnel', 'Etude', 'Maison', 'Autre'],
        progressBarWidth: 33, // Initialement à 33% (Moins important)
        importanceLabel: 'Moins important', // Label initial
      };
    },
    methods: {
      handleSubmit() {
        console.log('Tâche ajoutée:', this.task);
        this.task.name = '';
        this.task.category = '';
        this.task.importance = 'less'; // Réinitialiser la valeur d'importance
        this.progressBarWidth = 33; // Réinitialiser la largeur de la barre
        this.importanceLabel = 'Moins important'; // Réinitialiser le label
      },
      cancel() {
        this.task.name = '';
        this.task.category = '';
        this.task.importance = 'less';
        this.progressBarWidth = 33;
        this.importanceLabel = 'Moins important';
      },
      selectImportance(importance) {
        // Change la valeur de l'importance et la barre de progression
        this.task.importance = importance;
        if (importance === 'less') {
          this.progressBarWidth = 33;
          this.importanceLabel = 'Moins important';
        } else if (importance === 'normal') {
          this.progressBarWidth = 66;
          this.importanceLabel = 'Important';
        } else if (importance === 'urgent') {
          this.progressBarWidth = 100;
          this.importanceLabel = 'Urgent';
        }
      },
      closeBox() {
        // Logic to close the "Nouvelle tâche" box
        console.log('Fermer la boîte "Nouvelle tâche"');
      }
    },
  };
  </script>
  
  <style scoped>
  /* Container pour centrer le formulaire */
  .container {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    height: 100vh;
    background-color: #f5f5f5;
    width: 100%;
    padding: 20px;
  }
  
  /* Box pour "Nouvelle tâche" */
  .title-box {
    background-color: #C0C0C0; /* Violet clair */
    width: 600px; /* Largeur égale à celle du formulaire */
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 15px;
    border-radius: 8px;
    margin-bottom: 20px;
  }
  
  .title {
    font-size: 24px;
    color: #6a1b9a;
  }
  
  .close-btn {
    background-color: transparent;
    border: none;
    font-size: 20px;
    cursor: pointer;
    color: #6a1b9a;
  }
  
  /* Box du formulaire */
  .form-box {
    background-color: #C0C0C0; /* Violet clair */
    padding: 30px;
    border-radius: 8px;
    width: 600px; /* Largeur ajustée */
    box-shadow: 0px 4px 8px rgba(0, 0, 0, 0.1);
    text-align: center;
  }
  
  /* Style des champs de formulaire */
  .form-group {
    margin-bottom: 20px;
    text-align: left;
  }
  
  .form-group label {
    display: block;
    font-weight: bold;
    color: #6a1b9a;
  }
  
  .form-group input,
  .form-group select {
    width: 100%;
    padding: 8px;
    margin-top: 5px;
    border-radius: 4px;
    border: 1px solid #ccc;
    box-sizing: border-box;
  }
  
  /* Sélection des catégories : espace entre les boîtes */
  .category-box {
    display: flex;
    justify-content: space-evenly;
    margin-top: 10px;
    flex-wrap: wrap;
    gap: 10px;
  }
  
  .category {
    padding: 10px 20px;
    border-radius: 5px;
    cursor: pointer;
    font-weight: bold;
    color: #6a1b9a; /* Couleur du texte violet */
    border: 2px solid #6a1b9a; /* Bordure violette */
    transition: background-color 0.3s ease;
    flex: 1 1 20%; /* Flex pour garder les catégories bien dans leur box */
    text-align: center;
    max-width: 120px; /* Limite la largeur */
  }
  
  .category:hover {
    background-color: #8e99f3;
  }
  
  .category.selected {
    background-color: #6a1b9a;
    color: white;
  }
  
  /* Sélection du niveau d'importance : espacement ajusté */
  .importance-boxes {
    display: flex;
    justify-content: space-evenly;
    margin-top: 10px;
    gap: 10px;
  }
  
  .importance-box {
    padding: 8px 12px;
    border-radius: 5px;
    cursor: pointer;
    font-weight: bold;
    color: #6a1b9a; /* Couleur du texte violet */
    border: 2px solid #6a1b9a; /* Bordure violette */
    transition: background-color 0.3s ease;
    width: 30%;
    text-align: center;
  }
  
  .importance-box:hover {
    background-color: #8e99f3;
  }
  
  .importance-box.selected {
    background-color: #6a1b9a;
    color: white;
  }
  
  /* Barre de progression - largeur augmentée */
  .progress-bar {
    background-color: #ddd;
    border-radius: 5px;
    height: 20px; /* Hauteur augmentée pour la barre */
    margin-top: 10px;
    width: 100%;
  }
  
  .progress {
    background-color: #6a1b9a;
    height: 100%;
    border-radius: 5px;
    position: relative;
  }
  
  .progress-label {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    color: white;
    font-size: 14px;
    font-weight: bold;
  }
  
  /* Boutons */
  .button-group {
    display: flex;
    justify-content: space-between;
    margin-top: 20px;
  }
  
  /* Ordre des boutons inversé */
  .button-group .btn-add {
    order: 2; /* Ajouter à droite */
  }
  
  .button-group .btn-cancel {
    order: 1; /* Annuler à gauche */
  }
  
  .btn {
    padding: 10px 20px;
    border: none;
    border-radius: 5px;
    cursor: pointer;
  }
  
  .btn-add {
    background-color: #6a1b9a;
    color: white;
  }
  
  .btn-cancel {
    background-color: #6a1b9a;
    color: white;
  }
  
  .btn:hover {
    opacity: 0.9;
  }
  </style>
  