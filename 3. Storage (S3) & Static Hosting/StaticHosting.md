## 6️⃣ Hands-on: Hosting a Static Website on S3

### What is a Static Website?
A static website contains files like:
- HTML
- CSS
- JavaScript

No server-side processing is required.

---

### 🛠 Step-by-Step Process

#### Step 1: Create an S3 Bucket
- Enable public access (only required permissions)
- Choose region

#### Step 2: Upload Website Files 

<img width="1575" height="396" alt="Screenshot 2026-01-26 131730" src="https://github.com/user-attachments/assets/6b9b72c1-ebe6-45b7-b06f-8fccb4a54461" />

<img width="1575" height="396" alt="image" src="https://github.com/user-attachments/assets/839c5872-9a9b-46c0-a2b9-916bab037801" />

---

#### Step 3: Enable Static Website Hosting
- Go to bucket → Properties
- Enable **Static website hosting**
- Set:
  - Index document: `index.html`
  - Error document: `error.html`

---

#### Step 4: Configure Bucket Policy

Allow public read access:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::noor-static-website-bucket/*"
    }
  ]
}
