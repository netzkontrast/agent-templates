# Claude Code Templates - Tools and Logic Documentation

This document outlines the reusable tools and business logic identified in the `claude-code-templates` project.

**Note:** The core tooling for this project is written in **Javascript**, not Python. The primary logic is located in the `cli-tool/src/` directory.

---

## Core Tooling Files

The following Javascript files contain the main logic for discovering, configuring, and installing templates.

### 1. `file-operations.js`

This file handles all file system operations, particularly the downloading and installation of templates from the GitHub repository.

**Key Functions:**

*   **`downloadFileFromGitHub(filePath)`**:
    *   **Functionality**: Downloads a single file from the `claude-code-templates` GitHub repository.
    *   **Input**: `filePath` (string) - The path to the file within the repository's templates directory.
    *   **Output**: (Promise<string>) The content of the downloaded file.

*   **`downloadDirectoryFromGitHub(dirPath)`**:
    *   **Functionality**: Downloads an entire directory from the GitHub repository using the GitHub API.
    *   **Input**: `dirPath` (string) - The path to the directory.
    *   **Output**: (Promise<Object>) An object where keys are filenames and values are the file contents.

*   **`copyTemplateFiles(templateConfig, targetDir, options)`**:
    *   **Functionality**: The main function that orchestrates the entire template installation process. It checks for existing files, prompts the user for overwrite/merge actions, creates backups, and copies all the necessary files based on the user's selections.
    *   **Inputs**:
        *   `templateConfig` (Object): The configuration object for the selected template (from `templates.js`).
        *   `targetDir` (string): The destination directory for the installation.
        *   `options` (Object): Command-line options (e.g., `--yes` for non-interactive mode).
    *   **Output**: (Promise<boolean>) `true` if the installation was successful, `false` otherwise.

### 2. `utils.js`

This file contains utility functions for project analysis and file searching.

**Key Functions:**

*   **`detectProject(targetDir)`**:
    *   **Functionality**: Analyzes the project in the specified directory to detect the programming language (e.g., `javascript-typescript`, `python`) and framework (e.g., `react`, `django`).
    *   **Input**: `targetDir` (string) - The path to the project directory.
    *   **Output**: (Promise<Object>) An object containing the detected language, framework, and other project details.

*   **`findFilesByExtension(dir, extensions)`**:
    *   **Functionality**: Recursively finds all files in a directory that match the given extensions.
    *   **Inputs**:
        *   `dir` (string): The directory to search.
        *   `extensions` (Array<string>): An array of file extensions to look for (e.g., `['.js', '.ts']`).
    *   **Output**: (Promise<Array<string>>) An array of paths to the found files.

### 3. `prompts.js`

This file manages the interactive command-line prompts using the `inquirer` library. It guides the user through the process of selecting a language, framework, and other template options.

**Key Functions:**

*   **`interactivePrompts(projectInfo, options)`**:
    *   **Functionality**: Drives the entire interactive setup flow, presenting the user with a series of questions (steps) to configure their template installation.
    *   **Inputs**:
        *   `projectInfo` (Object): The project information detected by `utils.js`.
        *   `options` (Object): Command-line options to potentially skip some prompts.
    *   **Output**: (Promise<Object>) An object containing all the user's answers.

### 4. `templates.js`

This file defines the available templates and their configurations. It maps languages and frameworks to specific template files.

**Key Variables & Functions:**

*   **`TEMPLATES_CONFIG`**:
    *   **Functionality**: A large constant object that serves as the main configuration for all available templates. It defines the files, frameworks, and metadata for each language.

*   **`getAvailableLanguages()`**:
    *   **Functionality**: Returns a list of all available programming languages from the `TEMPLATES_CONFIG`.
    *   **Output**: (Array<Object>) An array of language objects formatted for `inquirer`.

*   **`getFrameworksForLanguage(language)`**:
    *   **Functionality**: Returns a list of available frameworks for a given language.
    *   **Input**: `language` (string) - The selected programming language.
    *   **Output**: (Array<Object>) An array of framework objects.

*   **`getTemplateConfig(selections)`**:
    *   **Functionality**: Constructs the final template configuration object based on the user's selections from the prompts.
    *   **Input**: `selections` (Object) - The user's answers from `prompts.js`.
    *   **Output**: (Object) The complete configuration object used by `copyTemplateFiles`.
