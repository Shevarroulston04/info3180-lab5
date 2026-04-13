<template>
  <div class="container mt-4">
    <form
      id="movieForm"
      @submit.prevent="saveMovie"
      enctype="multipart/form-data"
    >
      <div class="form-group mb-3">
        <label for="title" class="form-label">Movie Title</label>
        <input
          type="text"
          id="title"
          name="title"
          class="form-control"
        />
      </div>

      <div class="form-group mb-3">
        <label for="description" class="form-label">Description</label>
        <textarea
          id="description"
          name="description"
          class="form-control"
          rows="4"
        ></textarea>
      </div>

      <div class="form-group mb-3">
        <label for="poster" class="form-label">Movie Poster</label>
        <input
          type="file"
          id="poster"
          name="poster"
          class="form-control"
          accept=".jpg,.jpeg,.png"
        />
      </div>

      <button type="submit" class="btn btn-primary">
        Save Movie
      </button>
    </form>
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";

let csrf_token = ref("");

function getCsrfToken() {
  fetch("/api/v1/csrf-token")
    .then((response) => response.json())
    .then((data) => {
      console.log(data);
      csrf_token.value = data.csrf_token;
    })
    .catch((error) => {
      console.log(error);
    });
}

function saveMovie() {
  let movieForm = document.getElementById("movieForm");
  let form_data = new FormData(movieForm);

  fetch("/api/v1/movies", {
    method: "POST",
    body: form_data,
    headers: {
      "X-CSRFToken": csrf_token.value
    }
  })
    .then((response) => response.json())
    .then((data) => {
      console.log(data);
    })
    .catch((error) => {
      console.log(error);
    });
}

onMounted(() => {
  getCsrfToken();
});
</script>