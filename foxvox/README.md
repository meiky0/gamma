# FoxVox: One Click to Alter Reality

FoxVox is an open-source Chrome extension powered by GPT-4o. It demonstrates how
AI can be used to subtly manipulate the content you consume. It can manipulate
how we see political figures, view controversial policies, and even slant entire
news websites to reflect different biases.

## Table of Contents

- [Installation](#installation)
- [Development](#development)
- [Usage](#usage)
- [Technical Summary](#technical-summary)
- [Potential Improvements](#potential-improvements)
- [License](#license)

![image](https://github.com/PalisadeResearch/foxvox/assets/167763910/803448a8-d928-4c21-9613-a5f885f4266f)

## Installation From Source

1. Clone the repository:

```bash
git clone https://github.com/PalisadeResearch/foxvox.git
cd foxvox
```

2. Install dependencies:

```bash
npm install
```

3. Build the project:

```bash
npm run build
```

4. Load the extension in Chrome:
   - Open Chrome and navigate to `chrome://extensions/`
   - Enable "Developer mode"
   - Click "Load unpacked" and select the `foxvox` directory

## Development

This project uses TypeScript, ESLint, Prettier, and pre-commit hooks for a
modern development experience.

### Available Scripts

- `npm run build` - Build the extension for production
- `npm run dev` - Build in development mode with watch
- `npm run type-check` - Run TypeScript type checking
- `npm run lint` - Run ESLint with auto-fix
- `npm run lint:check` - Run ESLint without auto-fix
- `npm run format` - Format code with Prettier
- `npm run format:check` - Check code formatting

### Pre-commit Hooks

The project uses Husky and lint-staged for automated code quality checks:

- **Pre-commit**: Automatically formats and lints staged files
- **Pre-push**: Runs type checking, linting, and format checks

### Code Quality

- **TypeScript**: Full type safety with strict configuration
- **ESLint**: Comprehensive linting rules for TypeScript and Chrome extensions
- **Prettier**: Consistent code formatting
- **Chrome Extension Types**: Proper typing for Chrome extension APIs

## Usage

1. Set your OpenAI API key in the `config.json` file or use the provided key.
2. Visit your favorite website and see the content manipulation in action.

## Technical Summary

### Overview

FoxVox is built using TypeScript, HTML, and CSS. It leverages the GPT-4o model
to dynamically manipulate web content.

### Key Files and Directories

- **background.ts**:

  - Listens for tab updates and sends requests to the GPT-4o API to manipulate
    content.
  - Handles API responses and updates the web page content accordingly.
  - Manages communication between the content scripts and the GPT-4o API.

- **popup.ts**:

  - Provides a user interface for the extension.
  - Allows users to input their OpenAI API key and configure settings.
  - Communicates with `background.ts` to store and retrieve settings.

- **generation.ts**:

  - Contains functions to generate manipulated content.
  - Uses the GPT-4o API to process and alter the text on web pages.
  - Handles the logic for sending content to the API and processing the
    response.

- **database.ts**:

  - Manages local storage for caching manipulated content.
  - Ensures that repeated requests for the same content are minimized.

- **types.ts**:

  - TypeScript type definitions for the entire project.
  - Ensures type safety across all modules.

- **manifest.json**:
  - Specifies the extension's permissions, such as access to web pages and
    storage.
  - Defines the background scripts and popup interface.

### Workflow

1. **Initialization**:

   - When the extension is loaded, `background.ts` initializes and sets up
     listeners for tab updates.
   - `popup.ts` initializes the user interface and retrieves stored settings.

2. **Content Fetching and Manipulation**:

   - When a user navigates to a new page, `background.ts` captures the content
     tree via an injected script.
   - An algorithm finds optimal chunks to divide the content tree into that find
     optimum between minimising html code and maximising visible text while
     retaining the text size large enough for the model to be able to grasp
     context and do good generation.
   - The content is sent to `generation.ts`, which processes it and sends a
     request to the GPT-4o API.
   - The API response is received and processed by `generation.ts`, which then
     updates the web page content.

3. **Communication Between Background and Popup**:

   - `popup.ts` allows users to input their OpenAI API key and other settings.
   - These settings are sent to `background.ts` for storage and use in API
     requests.
   - `background.ts` and `popup.ts` communicate via Chrome's messaging API, with
     the state divided between them.

4. **Content Generation**:

   - `generation.ts` handles the logic for sending content to the GPT-4o API.
   - It constructs the API request, sends the content, and processes the
     response. We use two requests per chunk of text, one for initial generation
     and one for improvement and polish.
   - The manipulated content is then injected back into the web page using the
     associated xpaths.

5. **Caching**:
   - `database.ts` manages local storage to cache manipulated content.
   - This reduces the number of API requests by storing previously manipulated
     content and reusing it when possible.

## Potential Improvements to this Project

- [ ] Make a **stateless popup** and assert UI from background. This will fix
      persistancy of the UI and leverage issues from the popup data being lost
      on closing it.
- [ ] Improve generation through **better prompting**, advanced **generation
      techniques** like CoT, RAG, ToT etc and better **context utilisation** --
      including clustering of the text chunks that utilises semantic similarity
      and spatial locality.
- [ ] Improve UX through a **dedicated server** and **google account
      authorisation** / **phone number authorisation** to remove community key
      shenanigans.
- [ ] Add comprehensive **testing suite** with unit and integration tests.
- [ ] Implement **error boundaries** and better error handling.
- [ ] Add **performance monitoring** and optimization.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file
for details.

The MIT License is a permissive license that allows you to use, modify, and
distribute this software freely, with the only requirement being that you
include the original copyright notice and license text in any copies or
substantial portions of the software.
