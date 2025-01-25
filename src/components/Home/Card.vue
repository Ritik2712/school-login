<script setup lang="ts">
import { ref, onMounted, watch } from "vue";
import axios from "axios";

const apiUrl =
  "https://api.devharlemwizardsinabox.com/campaign/campaign_school_list/?search=";

// State variables
const searchQuery = ref("");
const schoolList = ref([]);
const isLoading = ref(false);
const errorMessage = ref("");

// Function to fetch schools from the API
const fetchSchools = async (query = "") => {
  isLoading.value = true;
  errorMessage.value = "";
  try {
    const response = await axios.get(apiUrl + query);
    const data = response.data.school_list;
    schoolList.value = data;

    if (!data.length) {
      errorMessage.value = "No schools found.";
    }
  } catch (error) {
    errorMessage.value = "Failed to fetch schools. Please try again.";
  } finally {
    isLoading.value = false;
  }
};

watch(
  searchQuery,
  (newQuery) => {
    fetchSchools(newQuery);
  },
  { immediate: true }
);

// Fetch all schools on component mount
onMounted(() => {
  fetchSchools();
});
</script>

<template>
  <div class="center card-container">
    <img class="card-img" alt="Player" src="../../assets/home/player.png" />
    <div class="card">
      <h2>Are you ready to take the challenge?</h2>

      <!-- App Download Section -->
      <div class="download-container">
        <p>Download the Harlem Wizard App</p>
        <div class="download">
          <a href="#" aria-label="Download from App Store">
            <img
              class="btn-image"
              alt="App Store"
              src="../../assets/home/apple-store.png"
            />
          </a>
          <a href="#" aria-label="Download from Google Play">
            <img
              class="btn-image"
              alt="Google Play"
              src="../../assets/home/goolge-store.png"
            />
          </a>
        </div>
      </div>

      <div class="line-container">
        <div class="line"></div>
        <div class="text">or you can sign up now</div>
        <div class="line"></div>
      </div>

      <!-- School Search Section -->
      <div class="container">
        <div class="search-bar">
          <input
            type="text"
            v-model="searchQuery"
            placeholder="Search schools..."
            class="search-input"
            aria-label="Search schools"
          />
        </div>

        <!-- Loading State -->
        <div v-if="isLoading" class="loading" role="status">Loading...</div>

        <!-- Error State -->
        <div v-else-if="errorMessage" class="error" role="alert">
          {{ errorMessage }}
        </div>

        <!-- School List -->
        <div v-else-if="schoolList.length" class="school-list">
          <div
            v-for="school in schoolList"
            :key="school.id"
            class="school-card"
          >
            <div class="center school-info">
              <img
                :src="school.logo"
                :alt="`${
                  school.school_name.length > 18
                    ? school.school_name.slice(0, 12)
                    : school.school_name
                }
              logo`"
                class="school-logo"
                loading="lazy"
              />
              <h3 class="school-name">
                {{
                  school.school_name.length > 18
                    ? school.school_name.slice(0, 12) + "..."
                    : school.school_name
                }}
              </h3>
            </div>
            <a
              :href="school.public_page_url"
              target="_blank"
              rel="noopener noreferrer"
              class="login-button"
            >
              Join Campaign
            </a>
          </div>
        </div>
        <div v-else class="error">Something unexpected happen</div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.card-container {
  flex-direction: column;
  padding: 1rem;
  margin-top: 1rem;
}

.card {
  text-align: center;
  border: 1px solid #ccc;
  padding: 2rem 1rem;
  border-radius: 10px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  margin-bottom: 20px;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  max-width: 800px;
  width: 100%;
}

.card-img {
  width: 25%;
}

h2 {
  color: #d1224e;
  margin-bottom: 1.5rem;
}

.download-container {
  margin-bottom: 2rem;
}

.download {
  display: flex;
  gap: 1rem;
  justify-content: center;
  margin-top: 1rem;
}

.btn-image {
  width: 150px;
  height: auto;
  transition: transform 0.2s;
}

.line-container {
  display: flex;
  align-items: center;
  gap: 10px;
  color: #555;
  margin-bottom: 10px;
}

.line {
  flex: 1;
  height: 2px;
  width: 80px;
  background-color: #121212;
}

.text {
  white-space: nowrap;
  font-weight: bold;
}

.btn-image:hover {
  transform: scale(1.05);
}

.container {
  width: 100%;
  max-width: 600px;
}

.search-input {
  padding: 0.75rem 1rem;
  width: 100%;
  max-width: 400px;
  border: 1px solid #ccc;
  border-radius: 5px;
  font-size: 1rem;
  transition: border-color 0.2s;
}

.search-input:focus {
  outline: none;
  border-color: #d1224e;
  box-shadow: 0 0 0 2px rgba(209, 34, 78, 0.1);
}

.school-list {
  display: flex;
  flex-direction: column;
  gap: 1rem;
  margin-top: 1.5rem;
  max-block-size: 400px;
}

.school-card {
  border: 1px solid #eee;
  padding: 1rem;
  border-radius: 10px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  transition: transform 0.2s, box-shadow 0.2s;
}

.school-card:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 12px rgba(0, 0, 0, 0.1);
}

.school-logo {
  width: 60px;
  height: 60px;
  object-fit: cover;
  border-radius: 50%;
  margin-right: 1rem;
}

.school-name {
  font-size: 1rem;
  color: #2c3e50;
  margin: 0;
}

.login-button {
  padding: 0.5rem 1rem;
  font-size: 0.875rem;
  color: #d1224e;
  text-decoration: none;
  border-radius: 20px;
  border: 1px solid #d1224e;
  transition: all 0.2s;
  white-space: nowrap;
}

.login-button:hover {
  background-color: #d1224e;
  color: white;
}

.loading {
  font-size: 1.125rem;
  color: #666;
  margin: 2rem 0;
}

.error {
  color: #d1224e;
  margin: 2rem 0;
}

@media (max-width: 900px) {
  .card-img {
    display: none;
  }
}

@media (max-width: 640px) {
  .card {
    padding: 1rem;
    margin: 100px;
    border: none;
  }

  .line {
    width: 50px;
  }

  .search-input {
    width: 90%;
  }

  .school-card {
    flex-direction: column;
    gap: 1rem;
    text-align: center;
  }

  .school-info {
    flex-direction: column;
  }

  .school-logo {
    margin-right: 0;
    margin-bottom: 0.5rem;
  }

  .btn-image {
    width: 120px;
  }
}
</style>
