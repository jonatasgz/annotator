# Annotator

## Overview

Annotator is a simple Vue 3 application that allows users to input text, select words to highlight, and apply custom highlight colors.

## Features

- ✅ **Live Text Highlighting** - Words in the input text are dynamically highlighted.
- 🎨 **Custom Highlight Colors** - Users can select colors for each word.
- 🔄 **Persistence with LocalStorage** - Highlighted words remain after page refresh.
- 📋 **Sortable Highlight List** - Words are sorted by color for easy identification.

## Installation

### 1️⃣ Clone the Repository

```sh
git clone https://github.com/jonatasgz/annotator.git
cd annotator
```

### 2️⃣ Install Dependencies

```sh
npm install
```

### 3️⃣ Run the Development Server

```sh
npm run dev
```

Then open `http://localhost:5173/` in your browser.

If you need to specify a different port use:

```sh
npm run dev -- --port different-port
```

## Usage

1. Enter text in the input area.
2. Type a word to highlight and pick a color.
3. Click **"Add Highlight"** to save it.
4. The word will be highlighted **everywhere it appears** in the text.
5. Words are stored in **LocalStorage**, so they persist after refresh.
6. Click **"×"** to remove a word from the list.

## Automatic Text Color Adjustment

The app ensures the text remains visible by adjusting the text color based on background brightness:

- **Light colors** → Dark text (`#000000`)
- **Dark colors** → Light text (`#FFFFFF`)

## Technologies Used

- Vue 3 (Composition API)
- Bootstrap 5 (Responsive UI)
- JavaScript (ES6+)
- LocalStorage (Data persistence)

## Live demo

You can try a livedemo [here](https://annotator.publica-me.com)

##  Future Improvements

- 🧠 **Text Classification Models** for advanced text analysis
- 👤 **User System** for personalized highlights
- 🤝 **Collaboration Features** to share projects with other users

## Contributing

1. Fork the repo
2. Create a new branch (`feature-name`)
3. Commit changes (`git commit -m "Added new feature"`)
4. Push to the branch (`git push origin feature-name`)
5. Open a Pull Request

## License

This software is licensed under GNU General Public License v3.0 - check the LICENSE file for details.
