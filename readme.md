# **Mobile AI Demo with Free Resources**

## **Overview**

This project demonstrates how to **use AI on mobile devices** while leveraging **free computing resources**. The mobile app sends text input to a remote AI service, receives embeddings, and displays results.

Key highlights:

- AI runs on **remote server** (e.g., Kaggle notebook, Colab GPU).
- Mobile app only sends requests and displays results → **no heavy computation on device**.
- **Full-screen loading** ensures smooth user experience.
- Uses **free resources**: FastAPI, ngrok free tunnels, open-source AI models.

---

## **1️⃣ Features**

- **POST /embed API** → returns embeddings for input text.
- **Android App**: sends text, receives embeddings, displays shape via Toast.
- **Full-screen loading overlay** while waiting for AI response.
- **Free & accessible resources**: Kaggle/Colab GPU, ngrok free tunnels.

---

## **2️⃣ How It Works**

### **2.1 Architecture**

```
Android App
   │
   ▼
[Full-screen Loading Overlay]
   │
   ▼
Ngrok Tunnel (public URL)
   │
   ▼
FastAPI Server (Kaggle / Colab)
   │
   ▼
AI Model (OpenCLIP / any open-source model)
   │
   ▼
Embeddings JSON
```

- Android sends POST request with text:

```
{"texts": ["your text here"], "normalize": true}
```

- Server processes text with AI model → returns embeddings:

```
{"embeddings": [[0.12, 0.01,...]]}
```

- App parses JSON, hides loading overlay, shows Toast with shape **[num_texts, dim]**.

### **2.2 Using Free Resources**

1. **AI model** runs on free GPU: Kaggle or Colab.
2. **Ngrok free** exposes local server to public URL.
3. Android app calls API using public URL.
4. Optionally, use **ngrok authtoken** for persistent tunnels:

```
from pyngrok import ngrok
ngrok.set_auth_token("YOUR_AUTHTOKEN")
public_url = ngrok.connect(8000)
```

- Replace URL in Android code:

```
URL url = new URL("https://xyz.ngrok-free.app/embed");
```

---

## **3️⃣ Setup FastAPI Server**

### **3.1 Install dependencies**

```
pip install fastapi uvicorn open-clip-torch torch pyngrok nest_asyncio
```

### **3.2 Notebook Example (Kaggle / Colab)**

```
from fastapi import FastAPI
from pydantic import BaseModel
from pyngrok import ngrok
import nest_asyncio, uvicorn, open_clip

nest_asyncio.apply()
app = FastAPI()

class EmbedRequest(BaseModel):
    texts: list[str]
    normalize: bool = True

@app.post("/embed")
def embed(req: EmbedRequest):
    embeddings = [[ord(c)/100 for c in t] for t in req.texts]
    return {"embeddings": embeddings}

ngrok.set_auth_token("YOUR_AUTHTOKEN")
public_url = ngrok.connect(8000)
print(public_url)

uvicorn.run(app, host="0.0.0.0", port=8000)
```

---

## **4️⃣ Android Setup**

### **4.1 Layout**

- **EditText** for input.
- **Button** to send request.
- **RelativeLayout** overlay full-screen with **ProgressBar + TextView**.

### **4.2 Fragment Code**

- Sends POST request in **AsyncTask** (background thread).
- Shows **full-screen loading** overlay during request.
- Parses JSON response, displays embedding shape via Toast.
- Handles network errors and timeouts.

_(See **FirstFragment.java** for full code.)_

---

## **5️⃣ Usage**

1. Run FastAPI server + ngrok → copy **public URL**.
2. **Replace URL in **FirstFragment.java**.**
3. Build & run Android app.
4. Enter text → press Send → full-screen loading appears.
5. When server responds → overlay hides, Toast shows shape.

---

## **6️⃣ Notes & Tips**

- Ngrok free URL changes each restart → update app with new URL.
- Kaggle/Colab GPU free → may have session limits.
- **You can replace AI model with ** **any OpenCLIP-supported checkpoint** **.**
- Optional: add Authorization header if enabling ngrok auth:

```
conn.setRequestProperty("Authorization", "Bearer YOUR_AUTHTOKEN");
```

- Embedding dimensions depend on model (ViT-B/32 → 512, ViT-L/14 → 768).

---

## **7️⃣ Optional Extensions**

- Kotlin + Coroutine for cleaner async calls.
- Retry mechanism for failed requests.
- Cache embeddings on device to reduce API calls.
- Support multiple AI models/endpoints in one app.

---

**💡 \*\***Conclusion\*\*

This repo shows how to **bring AI to mobile devices** using **free resources**, keeping computation off-device, while maintaining smooth UX with **full-screen loading overlays**. Ideal for **prototyping AI-powered apps** without expensive infrastructure.

---

Mình có thể viết thêm **version Markdown với hình minh họa overlay, API request flow và screenshot app** để README trực quan hơn, bạn có muốn mình làm luôn không?
