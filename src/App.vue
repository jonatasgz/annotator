<template>
  <div id="app" class="container-fluid">
    <div class="row">
      <div class="col-md-12 p-4">
        <nav class="navbar navbar-expand-lg bg-primary rounded" data-bs-theme="dark">
          <div class="container-fluid">
            <a class="navbar-brand" href="#">Annotator</a>
            <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNavAltMarkup"
              aria-controls="navbarNavAltMarkup" aria-expanded="false" aria-label="Toggle navigation">
              <span class="navbar-toggler-icon"></span>
            </button>
            <div class="collapse navbar-collapse" id="navbarNavAltMarkup">
              <ul class="navbar-nav ms-auto mb-2 mb-lg-0">
                <li class="nav-item">
                  <a class="nav-link disabled" href="#">Login</a>
                </li>
                <li class="nav-item">
                  <a class="nav-link disabled" href="#">Exit</a>
                </li>
              </ul>
            </div>
          </div>
        </nav>
      </div>
    </div>
    <div class="row">

      <div class="col-md-9 px-4">

        <div class="mb-3">
          <label class="form-label"><h5>Text to annotate:</h5></label>
          <textarea class="form-control" v-model="inputText" rows="6"></textarea>
        </div>

        <div class="mt-4">
          <h5>Highlighted text:</h5>
          <p class="border p-3 bg-light" v-html="highlightedText"></p>
        </div>
      </div>

      <div class="col-md-3 sidebar py-0 px-2 bg-white">
        <h5 class="text-center py-2">Words to highlight</h5>
        <div class="p-2 border-bottom">
          <div class="input-group mb-2">
            <input class="form-control" v-model="newWord" />
          </div>
          <div class="d-flex align-items-center mb-2">
            <input type="color" class="form-control form-control-color me-2" v-model="selectedColor" />
            <button class="btn btn-primary w-100" @click="addHighlight">Add word</button>
          </div>
        </div>

        <ul class="list-group list-group-flush sidebar-list">
          <li v-for="(item, index) in sortedHighlights" :key="index"
            class="list-group-item d-flex justify-content-between align-items-center">
            <span>
              <span class="badge px-2 py-1"
                :style="{ backgroundColor: item.color, color: getContrastColor(item.color) }">
                {{ item.word }}
              </span>
            </span>
            <button class="btn btn-sm btn-danger" @click="removeHighlight(index)">×</button>
          </li>
        </ul>
      </div>

    </div>
  </div>
</template>

<script>
import { ref, computed, onMounted, watch } from "vue";

export default {
  setup() {
    const inputText = ref("");
    const newWord = ref("");
    const selectedColor = ref("#0000ff");
    const highlights = ref([]);

    // Load from LocalStorage when the app starts
    onMounted(() => {
      const savedHighlights = localStorage.getItem("highlightedWords");
      if (savedHighlights) {
        highlights.value = JSON.parse(savedHighlights);
      }
    });

    // Watch for changes and save to LocalStorage
    watch(highlights, (newHighlights) => {
      localStorage.setItem("highlightedWords", JSON.stringify(newHighlights));
    }, { deep: true });

    // Add new word with color
    const addHighlight = () => {
      if (newWord.value.trim() && !highlights.value.some(h => h.word.toLowerCase() === newWord.value.toLowerCase())) {
        highlights.value.push({ word: newWord.value, color: selectedColor.value });
        newWord.value = ""; // Clear input
      }
    };

    // Remove word from highlights
    const removeHighlight = (index) => {
      highlights.value.splice(index, 1);
    };

    // Sort highlighted words by color
    const sortedHighlights = computed(() => {
      return [...highlights.value].sort((a, b) => a.color.localeCompare(b.color));
    });

    // Function to determine contrasting text color
    const getContrastColor = (hex) => {
      const r = parseInt(hex.substring(1, 3), 16);
      const g = parseInt(hex.substring(3, 5), 16);
      const b = parseInt(hex.substring(5, 7), 16);
      const luminance = 0.299 * r + 0.587 * g + 0.114 * b;
      return luminance > 128 ? "#000000" : "#FFFFFF"; // Black text for light colors, white text for dark colors
    };

    // Compute highlighted text with dynamic text color
    const highlightedText = computed(() => {
      if (!inputText.value) return "";

      let text = inputText.value;

      highlights.value.forEach(({ word, color }) => {
        const textColor = getContrastColor(color);
        const regex = new RegExp(`\\b(\\w*${word}\\w*)\\b`, "gi");

        text = text.replace(
          regex,
          (match) => `<span style="background-color:${color}; color:${textColor}; padding: 3px; border-radius: 3px;">${match}</span>`
        );
      });

      return text;
    });

    return {
      inputText,
      newWord,
      selectedColor,
      highlights,
      addHighlight,
      removeHighlight,
      highlightedText,
      sortedHighlights,
      getContrastColor,
    };
  },
};
</script>

<style>
/* Sidebar Styling */
.sidebar {
  background-color: #f8f9fa;
  height: 100vh;
  overflow-y: auto;
  position: sticky;
  top: 0;
  padding: 15px;
  border-left: 1px solid #dee2e6;
}

/* Ensure list does not expand beyond sidebar */
.sidebar-list {
  max-height: calc(100vh - 120px);
  overflow-y: auto;
}

/* Textarea resizing */
textarea {
  resize: none;
}
</style>
