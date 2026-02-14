# System Design: Sahpathi AI

## 1. Architecture Overview
The system utilizes a Microservices architecture to handle the heavy AI processing (Video/Audio) separate from the lightweight user interactions.

## 2. Tech Stack & APIs
* **Core Brain:** **Google Gemini 1.5 Pro** (Large Context Window) to process entire textbooks/PDFs.
* **Knowledge Engine:** Integration with **NotebookLM logic** (simulated via API) for generating "Audio Overviews" and deep summaries.
* **Voice Layer:**
    * **STT (Speech-to-Text):** Google Cloud Speech-to-Text for capturing student queries in Indian languages.
    * **TTS (Text-to-Speech):** Google Wavenet voices for natural-sounding AI responses.
* **Gamification Database:** **Firebase Realtime Database** to track XP, Levels, and Leaderboards instantly across devices.
* **Auth:** Firebase Authentication (Phone Number + Google Login).

## 3. Data Flow
1.  **Ingestion:** Student selects a standard/board or uploads a PDF.
2.  **Indexing:** The file is stored in a **Google Cloud Storage Bucket** and indexed by Gemini 1.5 Pro.
3.  **Interaction Loop:**
    * Student asks a question (Voice/Text).
    * Gemini retrieves context from the specific book chapter.
    * Response is generated in the student's preferred language.
    * If the concept is abstract, **Imagen 3 (via Gemini)** generates a visual aid.
4.  **Gamification Trigger:** completing a chapter updates the User Profile in Firebase, awarding a "Knowledge Badge."

## 4. Offline/Low-Bandwidth Optimization
* Since the target is "Bharat," the app caches the current chapter's text and audio locally, allowing the student to study even with spotty internet connection.