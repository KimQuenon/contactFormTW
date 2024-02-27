<template>
    <form @submit.prevent="submitForm">
      <div>
        <label>Nom</label>  
        <input
          type="text"
          v-model="name"
          class="form-control"
          placeholder="Votre nom...*"
          required="required"
        >
      </div>
      <div>
        <label>Prénom</label>  
        <input
          type="text"
          v-model="surname"
          class="form-control"
          placeholder="Votre prénom...*"
          required="required"
        >
      </div>
      <div>
        <label>Email</label>
        <input
          type="email"  
          v-model="email"
          class="form-control"
          placeholder="Votre adresse mail...*"
          required="required"
        >
      </div>
      <div>
        <label>Téléphone</label>
        <input
          type="tel"  
          v-model="email"
          class="form-control"
          placeholder="Votre numéro de téléphone..."
          required="required"
        >
      </div>
      <div>
        <label>Message</label>
        <textarea
          type="text"
          v-model="message"
          class="form-control"
          placeholder="Commencez à rédiger..."
          required="required"
        ></textarea>
      </div>
      <div>
        <button type="submit" class="btn">Envoyer</button>
      </div>
    </form>
  </template>