# [🎥 Watch Me Complete This Lab Here!](https://www.loom.com/share/49f6c80efb1944478346b1ef128702e9)

# Host A Static Website in Azure

This lab walks you through deploying a public-facing resource in Azure. Instead of building a complex server to host a simple website, you can use Azure Blob Storage. 

---

## 🏗️ Architecture Diagram

```
                        ┌─────────────────────────────────────────────┐
                        │              AZURE SUBSCRIPTION              │
                        │                                              │
                        │   ┌──────────────────────────────────────┐  │
                        │   │     Resource Group: rg-lab01-[name]  │  │
                        │   │                                      │  │
                        │   │   ┌──────────────────────────────┐   │  │
                        │   │   │  Storage Account             │   │  │
                        │   │   │  stlab01[name]               │   │  │
                        │   │   │                              │   │  │
                        │   │   │  ┌────────────────────────┐  │   │  │
                        │   │   │  │  Container: $web       │  │   │  │
                        │   │   │  │  ┌──────────────────┐  │  │   │  │
                        │   │   │  │  │   index.html     │  │  │   │  │
                        │   │   │  │  └──────────────────┘  │  │   │  │
                        │   │   │  └────────────────────────┘  │   │  │
                        │   │   │                              │   │  │
                        │   │   │  Static Website: ENABLED     │   │  │
                        │   │   │  Primary Endpoint (HTTPS) ───┼───┼──┼──┐
                        │   │   └──────────────────────────────┘   │  │  │
                        │   └──────────────────────────────────────┘  │  │
                        └─────────────────────────────────────────────┘  │
                                                                          │
            ┌─────────────────────────────────────────────────────────────┘
            │         Public HTTPS Endpoint
            │         https://stlab01[name].z13.web.core.windows.net/
            ▼
     ┌──────────────┐
     │   End User   │
     │  (Browser)   │
     │   Internet   │
     └──────────────┘
```

**Traffic Flow:**
1. User navigates to the **Primary Endpoint URL** in their browser
2. Request routes over the public internet to **Azure's CDN edge**
3. Azure serves `index.html` directly from the **`$web` container**
4. Page renders in the user's browser — **no server involved**

---

## 💡 Why Azure Blob Storage Over A Traditional Web Server
Azure Blob Storage with static website hosting is a PaaS solution — Microsoft manages all underlying infrastructure. There is no server to provision, patch, or maintain. For a static website serving HTML, CSS, and JavaScript files there is no need for a compute layer. This reduces cost, eliminates operational overhead, and scales automatically to handle any amount of traffic.

## ☁️ AWS Equivalent
The AWS equivalent is S3 static website hosting. Both services store files in object storage and serve them over a public HTTP/HTTPS endpoint without a web server. The Azure-specific difference is that Azure Blob Storage uses a dedicated $web container for static hosting while AWS S3 uses bucket-level website configuration.

---

## Phase 1 — Create the Storage Account

The **Storage Account** is where your website files will live.
 
<img width="966" height="959" alt="Create Storage Account" src="https://github.com/user-attachments/assets/9c0f27d0-2ea9-4715-ae75-376e08f9960e" />

1. In the search bar, type `Storage accounts` and select it
2. Click **`+ Create`**
3. Fill in the **Basics** tab:
   - **Subscription:** Select your active subscription.
   - **Resource group:** Create a new resource group or select an existing one.
   - **Storage account name:** Enter a globally unique name.
   - **Region:** Choose a region closest to you.
   - **Performance:** Select **Standard**
   - **Redundancy:** Select **Locally-redundant storage**


4. Click **`Review + create`**
5. Wait for **validation to pass**, then click **`Create`**
6. After deployment (~30 seconds), click **`Go to resource`**

---

## Phase 2 — Enable Static Website Hosting

Now we turn your storage account into a web server — with a single toggle.

<img width="1103" height="597" alt="Endpoint URL-Index Document Name-Error Document Name" src="https://github.com/user-attachments/assets/5ce19344-f138-446b-b657-01523fe79917" />

1. On the **left-hand menu** of your Storage Account, scroll to the **Data management** section
2. Click **`Static website`**
3. Toggle **Static website** from `Disabled` → **`Enabled`**
4. Fill in the following fields:
   - **Index document name:** `index.html`
   - **Error document path:** `404.html` 

5. Click **`Save`**

> 🔴 **CRITICAL STEP:** After saving, a **Primary endpoint URL** will appear.
>
>  **Copy this URL now.** This is your website's public address.

---

## Phase 3 — Create Your Website File

Time to create the content your website will serve.

1. Open your **text editor**
2. Create a file named `index.html`
3. Add your HTML content
4. Save the file to your Desktop as **`index.html`**
   
---

## Phase 4 — Upload Content

Now we push your file to Azure's `$web` container.

<img width="1103" height="403" alt="Containers" src="https://github.com/user-attachments/assets/c536379a-85db-414f-ac37-3c296b994007" />

<img width="1334" height="433" alt="Uploaded index html file" src="https://github.com/user-attachments/assets/2f269d2d-9216-434d-838b-2c8f15c95dbf" />

1. Return to the **Azure Portal** on your Storage Account
2. On the left menu, click **`Containers`** (under *Data storage*)
3. You will see a container named **`$web`** — this was auto-created when you enabled static hosting. Click to open it
4. Click **`Upload`** at the top of the container blade
5. Browse and select your `index.html` file from your Desktop
6. Click **`Upload`**

---

## Phase 5 — Validation

Let's confirm your website is live.

1. Open a **new browser tab**
2. Paste the **Primary endpoint URL** you copied in Phase 2
3. Press **Enter**

**Expected Result:**

<img width="591" height="278" alt="Endpoint Validation" src="https://github.com/user-attachments/assets/0f82201f-0450-41f6-b9d5-cdf1d8e7f9ab" />


🎉 **Congratulations! You have successfully deployed a serverless website on Azure.**
