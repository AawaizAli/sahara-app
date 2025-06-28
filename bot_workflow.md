### **Proposed Flow for Sahara (Voice-Based Healthcare Triage System)**

Here’s a detailed explanation of the **workflow** for **Sahara**, with specific focus on **inputs** and **outputs** at each stage. We’ll consider both **technical aspects** (integration of models and backend) and **user experience** (voice interaction).

---

### **1. User Initiates Interaction (Voice Input)**

**Input:**

* **User Voice Input** (spoken symptoms or inquiry): The user presses a **microphone button** and speaks into the app (e.g., "I have a headache and fever").

**Tools Involved:**

* **Speech-to-Text (STT)**: Models like **Whisper** or **Vosk** will transcribe the audio to text.
* **Language Detection**: Automatic detection of the spoken language (Urdu, Sindhi, or English) using models like **langdetect** or **fastText**.

**Output:**

* **Transcribed Text**: The voice input is converted into text and passed to the next stage (e.g., "I have a headache and fever").

---

### **2. Intent Detection and Symptom Extraction**

**Input:**

* **User Text** (from STT output): The transcribed text (e.g., "I have a headache and fever").

**Tools Involved:**

* **NLP (Natural Language Processing)**: Use **Rasa**, **Haystack**, or **SciSpacy** to process the text.

  * **Intent Detection**: Identify the general intent (e.g., "symptom reporting").
  * **Entity Recognition**: Extract medical entities (e.g., "headache", "fever").

**Output:**

* **Intent**: The system identifies the intent as "symptom reporting".
* **Entities**: The model extracts symptoms (e.g., headache, fever) from the text and stores them in structured form (e.g., `{"headache": true, "fever": true}`).

---

### **3. Follow-Up Question Generation (Dynamic Questioning)**

**Input:**

* **Extracted Symptoms**: The identified symptoms (e.g., headache and fever) are used to generate follow-up questions.

**Tools Involved:**

* **Dialogue System**: **Sesame** or **Rasa** will use the extracted symptoms to generate dynamic follow-up questions.

  * For example, for "headache" the bot might ask, "Where exactly do you feel the headache? Is it a sharp pain or dull?"
  * For "fever", it might ask, "How high is your fever? Have you felt chills?"

**Output:**

* **Follow-Up Question**: The system generates contextually appropriate follow-up questions based on the initial symptoms.

  * Example: "Where exactly do you feel the headache?"

---

### **4. Continuous Dialogue Management (Handling Multiple Turns)**

**Input:**

* **User Response to Follow-Up Question**: User speaks (e.g., "The headache is on the left side of my head, and it’s dull").

**Tools Involved:**

* **NLP**: Use **Sesame** or **Rasa** for processing user responses.

  * **Symptom Expansion**: The system must track multiple turns of the conversation to refine the diagnosis.
  * **Context Management**: Maintain context of previous responses (e.g., left-side headache, dull pain) for follow-up questioning.

**Output:**

* **Updated Symptoms**: The system updates its understanding of the symptoms based on new responses (e.g., `{"headache": {"location": "left side", "type": "dull"}`).

* **Follow-Up Question**: Based on the ongoing conversation, the system continues asking dynamic follow-up questions.

---

### **5. Diagnosis & Symptom Categorization**

**Input:**

* **Extracted Symptoms** (multiple turns): At this point, the bot has a refined set of symptoms after several rounds of questioning (e.g., "left-side dull headache", "fever of 101°F", "no chills").

**Tools Involved:**

* **Symptom Mapping**: Use **MedCAT** or **SciSpacy** to map symptoms to medical conditions (e.g., "headache" → "migraine", "fever" → "flu").

  * The system may use a **symptom-disease matching algorithm** to diagnose or suggest potential conditions based on the extracted symptoms.

**Output:**

* **Diagnosis Suggestion**: The system outputs a list of possible diagnoses (e.g., "Migraine", "Flu").

* **Next Steps**: The system may ask further clarification or suggest that the user consults a doctor, depending on the severity.

---

### **6. Doctor Matching and Appointment Scheduling**

**Input:**

* **Diagnosis or Suggested Condition**: The system uses the diagnosis (e.g., "migraine") to match the user with a relevant doctor.

**Tools Involved:**

* **Doctor Matching**: Use a **doctor database** (PostgreSQL) to match the user to a doctor based on specialty (e.g., a neurologist for migraines).

* **Appointment System**: Integration with **Google Calendar API**, **FullCalendar**, or **Cal.com** to check doctor availability and schedule appointments.

**Output:**

* **Doctor Match**: The system suggests a doctor (e.g., Dr. John Smith, Neurologist).

* **Appointment Confirmation**: The system schedules an appointment (e.g., "Your appointment with Dr. John Smith is confirmed for tomorrow at 10 AM").

---

### **7. Final Summary and Feedback**

**Input:**

* **Appointment Confirmation**: Once the doctor is matched, and the appointment is set, the system will confirm the details.

**Tools Involved:**

* **TTS (Text-to-Speech)**: The system will use **Coqui TTS** or **Piper** to read the confirmation message aloud.

**Output:**

* **Final Confirmation**: The bot confirms the details to the user, including the appointment, and asks for feedback on the experience.

---

### **Overall Flow Summary with Inputs and Outputs:**

1. **User Input (Voice)** → **STT Model** → **Transcribed Text**
2. **Text Processing (NLP)** → **Intent & Entity Recognition** → **Symptom Data** (e.g., `{"headache": true, "fever": true}`)
3. **Follow-Up Question Generation** → **Bot Asks Follow-Up** (e.g., "Where do you feel the headache?")
4. **User Response (Voice)** → **STT Model** → **NLP** → **Updated Symptoms**
5. **Diagnosis** → **Symptom Mapping** → **Possible Diagnosis** (e.g., "Migraine")
6. **Doctor Matching** → **Doctor Database** → **Appointment Scheduling** → **Appointment Confirmation**
7. **Final Summary (Voice)** → **TTS** → **Final Confirmation to User**

---

### **Key Considerations for the Flow:**

* **Real-time processing**: Ensure **low-latency** between speech recognition and bot responses to keep the user engaged.
* **Multilingual Support**: Implement robust multilingual support in **STT**, **NLP**, and **TTS** for Urdu, Sindhi, and English.
* **Scalability**: Use containerization (e.g., Docker) for deploying individual components like the voice models, backend services, and appointment scheduler.
* **User Privacy**: Ensure that sensitive medical data is securely handled and stored, following regulations (e.g., HIPAA or local data protection laws).

---

Would you like assistance in defining specific technical implementations for any of these steps, such as **doctor matching** logic or **symptom extraction** models?
