# Mobile AI Demo with Free Resources

## Overview

This project demonstrates a **mobile AI demo app** that shows how to **send text input from an Android device to an AI service**, receive embeddings, and display results.

**Key highlights:**

- Designed as a **demo**: easy to swap or test different AI models.
- Mobile app only sends requests → **no heavy computation on device**.
- **Full-screen loading** ensures smooth user experience.
- Uses **free resources**: FastAPI, ngrok free tunnels, open-source AI models.

<video height="600" controls>
  <source src="assets/test.mov" type="video/quicktime">
  Your browser does not support the video tag.
</video>


---

## 1️⃣ Features

- **POST /embed API** → returns embeddings for input text.
- **Android App**: sends text, receives embeddings, displays shape via Toast.
- **Full-screen loading overlay** while waiting for AI response.
- Supports **any AI model** that can produce embeddings (OpenCLIP, CLIP variants, or custom models).

---

## 2️⃣ How It Works

### 2.1 Architecture

```plaintext
Android App

│

▼

[LinearLayout with EditText + Button + ProgressBar]

│

▼

Ngrok Tunnel (public URL)

│

▼

FastAPI Server (Kaggle / Colab)

│

▼

AI Model (OpenCLIP / any model)

│

▼

Embeddings JSON

```

Embeddings JSON

* Android sends POST request with text:

```json
{"texts": ["your text here"], "normalize": true}
```

* Server processes text with AI model → returns embeddings:

```json
{"embeddings": [[0.12, 0.01,...]]}
```

* App parses JSON, hides **ProgressBar**, shows Toast with shape **[num_texts, dim]**.

### **2.2 Using Free Resources**

1. Run AI model on free GPU: Kaggle or Colab.
2. Expose FastAPI server with **ngrok free** public URL.
3. Android app calls API using this public URL.
4. Optionally, set **ngrok authtoken** for persistent tunnels:

```python
from pyngrok import ngrok
ngrok.set_auth_token("YOUR_AUTHTOKEN")
public_url = ngrok.connect(8000)
```

* Replace URL in Android code:

```java
URL url = new URL("https://xyz.ngrok-free.app/embed");
```

---

## **3️⃣ Setup FastAPI Server**

*(Same as before – install dependencies and run notebook/server)*

---

## **4️⃣ Android Setup**

* **Layout: **NestedScrollView + LinearLayout + EditText + Button + ProgressBar**.**
* Fragment code: AsyncTask background request → shows full-screen ProgressBar → parses JSON → shows Toast.

---

## **5️⃣ Usage**

1. Run server + ngrok → get public URL.
2. **Replace URL in ** **FirstFragment.java** **.**
3. Build & run Android app.
4. Enter text → press Send → loading appears.
5. When server responds → loading hides, Toast shows embedding shape.

---

## **6️⃣ Notes & Tips**

* Ngrok free URL changes on each restart → update app URL.
* Demo supports **any AI model** for generating embeddings.
* Optional: add Authorization header if ngrok auth is enabled.

---

## **Conclusion**

This repo is a **demo of using AI on mobile devices**, showcasing how to integrate AI models with Android apps using free resources. It is **model-agnostic**, so you can test with different models without changing app code significantly.
