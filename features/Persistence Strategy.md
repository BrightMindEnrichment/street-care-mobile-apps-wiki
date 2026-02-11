## Offline Strategy: Single-State Persistence

To ensure data safety without over-engineering the UI, we utilize a **Single-State Persistence** model (Auto-Save).

### 1. The "Auto-Save" Pattern
* **Behavior:** The app maintains a **single active session**. Every user input is immediately serialized to JSON and saved to the device's persistent Key-Value storage.
* **Restoration:** On app launch, the system checks for existing persistence data. If found, the form is **pre-filled** to the exact state where the user left off.
* **Completion:** Once the user successfully submits ([Atomic Batch to Firestore](https://firebase.google.com/docs/firestore/manage-data/transactions#batched-writes)), the local persistence is cleared.

### 2. Implementation
We avoid heavy SQL databases in favor of lightweight, platform-native Key-Value stores for simplicity:
* **Android:** `Jetpack DataStore` (storing the Log+Requests object as a JSON string).
* **iOS:** `UserDefaults` (storing the JSON blob).

### 3. Benefits
* **Zero Config:** No complex database schemas or migrations required.
* **Crash Proof:** Data is saved on every navigation, ensuring 0% data loss on crashes.
* **Simplified UX:** The user never has to "Save Draft" or "Open Draft"; it just works.
