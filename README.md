# Safety Planning Android App

## Purpose
This app empowers survivors by helping them assess, plan, and manage their safety before, during, and after an abusive relationship. It is designed to be flexible and personalized so plans can be tailored to each person’s needs.

## Inspiration & Resources
- **Model site:** [The Hotline's Safety Planning Tool](https://www.thehotline.org/)
- **Victim services taxonomy:** Mirror the core categories from the hotline in app modules or tabs.
- **App examples:** Aspire News App, myPlan App, Bright Sky Canada offer reference points for flows and content organization.

## Important Design Principles
- **Privacy & security:** Never collect or store identifying information such as names, addresses, photos, or other personally identifiable uploads.
- **Survivor-controlled data:** Only store what is necessary and defer sensitive uploads.
- **Trauma-informed language:** Use gentle, non-blaming, empowering language. Avoid legal jargon where possible.
- **Ethical considerations:** Clearly state the app is **not a substitute for emergency services** and cannot guarantee prevention of harm.

## Essential Features

### 1. Personalized Safety Plan Builder
- **Dynamic questionnaire**
  - Start by asking about the user’s relationship status (planning to leave, post-separation, etc.).
  - Each status presents a tailored set of questions; only relevant questions appear.
  - Answers update a plan JSON in Firebase Realtime Database.
  - Users can revisit and edit answers; all changes sync back to Realtime DB.
- **Plan generation**
  - Provide relevant tips based on answered questions.
  - Offer a short list of tips in each plan section (e.g., 3 items for shelter planning).
  - Render tips in a scrollable list (RecyclerView) with per-item actions.
  - Persist changes and selections to Realtime DB.
  - Map each plan section to JSON using `question_id` keys.

### 2. Emergency Exit Button
- **Quick-escape control:** A persistent exit icon/button on every screen.
- **Behavior on tap:**
  - Redirects to a neutral website (e.g., a search engine) in a webview or browser.
  - Immediately terminates the app session to clear state.

### 3. Secure Access Options
- **PIN setup & storage:** After the first successful Firebase login, prompt users to create a 4–6 digit PIN. Store it securely with AndroidX Security via `SharedPreferences` or Keystore.
- **Unlock flow on relaunch:** Present the same login screen with two options: enter PIN (validated against encrypted value) or use Firebase login. PIN path should bypass full authentication. PIN does **not** need to match MVP credentials.

### 4. Storage of Emergency Information
- **Data categories:** IDs, court orders, emergency contacts, safe locations, medications (names, dosage), and other critical notes.
- **CRUD operations:** Allow users to add, edit, and delete items in each category.
- **Cloud strategy:**
  - Store metadata (titles, descriptions) in Firebase Realtime DB.
  - Store binary assets (PDFs, photos) in Firebase Firestore, saving only references/URLs in the database.

### 5. Support Connection
- Dedicated page listing direct links to victim services, hotlines, shelters, legal aid, and police.
- Use a predefined directory (JSON/HashMap) with ~5 entries per major Canadian city; city selection comes from the questionnaire.

## Technical Notes
- Build with Android, Firebase Realtime Database, and Firestore integration.
- Use `RecyclerView` for plan tip lists and `ActionView`/`FloatingActionButton` for the emergency exit control.
- Favor JSON or GSON for serializing questionnaire answers and plan sections.

## Safety & Legal Disclaimer
This app cannot guarantee safety or replace emergency assistance. In any immediate danger, users should contact emergency services directly.
