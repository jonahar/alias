# ALIAS: Wedding Edition Generator

A standalone, single-file web application designed to generate custom, print-ready ALIAS card decks and a matching mathematical board game. It runs entirely client-side with no build step or backend required.

## Features

*   **Tabbed Interface:** Seamlessly switch between generating Card Fronts, the Game Board, and Card Backs.
*   **Procedural Board Generation:** Automatically calculates and draws a winding "superellipse" (squircle) SVG path that perfectly hugs the edges of an A4 layout, adjusting node placement dynamically.
*   **Client-Side Asset Management:** 
    *   Upload `.txt` files to instantly populate the card deck with custom words.
    *   Upload local images for the Start space, Finish space, and a full-bleed board background using the HTML5 `FileReader` API (converting images to Base64 in memory).
*   **High-Resolution Export:** 
    *   **PDF:** Dynamically rewrites CSS `@page` rules to ensure the browser's native print engine outputs the exact physical dimensions required (2.5"x3.5" for cards, A4 Landscape for the board).
    *   **ZIP/PNG:** Uses external libraries to snapshot DOM elements into high-res images and bundles them into a single archive.

## Technical Architecture

The entire application is contained within a single `index.html` file to ensure maximum portability. 

### Dependencies (Loaded via CDN)
*   **`html2canvas`**: Renders DOM elements into high-resolution `<canvas>` objects for PNG export.
*   **`JSZip`**: Packages the generated canvas images into a `.zip` file for batch downloading.
*   **Google Fonts**: Uses *Frank Ruhl Libre* for elegant front-facing text and *Varela Round* for the bubbly card-back logo.

## File Structure Breakdown

Inside the HTML file, the code is organized into discrete blocks:

1.  **`<style>` (CSS):** 
    *   Contains the UI styling for the control panel.
    *   Defines the strict physical dimensions for `.card` and `.board-wrapper`.
    *   Houses `@media print` rules that strip away the UI and drop shadows when exporting to PDF.
2.  **UI & Containers (HTML):** 
    *   `#control-panel`: The interactive top bar containing tabs, file inputs, and export triggers.
    *   `#deck-container` & `#card-backs-container`: Empty flexbox wrappers where the JS dynamically injects the generated card elements.
    *   `#board-container`: Houses the base `<svg>` element where the JavaScript mathematically plots the game path, nodes, and clipping masks.
3.  **Data Storage:** 
    *   `<script type="text/plain" id="raw-words">`: Acts as a fallback data store for the default word list.
4.  **Logic (`<script>`):** 
    *   **State Management:** Handles tab switching and toggling the visibility of specific tools (like the opacity slider).
    *   **Generator Functions:** `generateCards()`, `generateCardBacks()`, and `generateBoard()` parse the current data state and rebuild the DOM elements.
    *   **Math Engine:** Uses trigonometric functions to calculate the coordinates for the superellipse spiral path on the board.
    *   **Export Handlers:** Manages the async promises required to render the DOM to canvas and bundle the ZIP files.

## Usage Instructions

1.  **Run the App:** Double-click the `index.html` file to open it in any modern web browser.
2.  **Customize Words:** On the "Cards" tab, click **Upload Words (.txt)** to load a plain text file (one word per line). The deck will instantly regenerate.
3.  **Customize the Board:** Switch to the "Board" tab. Use the upload buttons to set custom images for the Start node, Finish node, and the background. Adjust the slider to fade the background image.
4.  **Exporting:** 
    *   Click **Export as PDF** to trigger the browser's print dialog (Set margins to "None" and ensure "Background graphics" are enabled).
    *   Click **Export as ZIP / PNG** to download individual image files suitable for professional print shops.