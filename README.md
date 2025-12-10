# **MyContent MVP – Article Recommendation System (Content-Based)**

This project implements an article recommendation system based on **content similarity**, using **vector embeddings stored on Azure Blob Storage**.

---

## Features

* Personalized recommendation of 5 articles using embedding similarity
* Content-based approach with precomputed article vectors
* Embeddings stored in Azure Blob Storage and dynamically loaded
* Local user interface built with Streamlit
* User and article datasets based on public data sources

---

## Repository Structure

```
my_content_mvp/
│
├── azure_function/                  # Azure Function backend
│   ├── function_app.py              # Main script containing the recommendation logic
│   ├── requirements.txt             # Dependencies required for Azure execution
│   ├── host.json                    # Azure Functions configuration file
│   ├── .funcignore                  # Files ignored during Azure deployment
│   └── data/
│       ├── clicks_sample.csv        # User/article interaction data
│       └── articles_metadata.csv    # Article metadata
│
├── streamlit_app/                   # Streamlit app for testing the API
│   ├── app.py                       # User interface calling the Azure API
│   └── data/
│       └── users.csv                # List of user IDs used in the interface
│
├── notebook.ipynb                   # Exploratory notebook with tests and model comparisons
├── deploy_function.sh               # CLI script for quick Azure Function deployment
├── .gitignore                       # Files/folders excluded from versioning
└── README.md                        # Project documentation
```

---

## 🔧 Installation & Execution

### 1. Clone the project

```bash
git clone https://github.com/rafiksiala/my_content_mvp.git
cd my_content_mvp
```

---

### 2. Launch the Streamlit interface

```bash
cd streamlit_app
pip install -r requirements.txt
streamlit run app.py
```

---

### 3. Deploy the Azure Function

The Azure Function is triggered by an HTTP request. It:

* downloads embeddings dynamically from Azure Blob Storage,
* computes similarity between articles read by a user,
* returns the 5 most relevant articles.

#### Method 1 — Deployment via Visual Studio Code (recommended when working alone)

1. Open the project in VSCode
2. Right-click on the folder `azure_function`
3. Select **"Deploy to Function App"**
4. Choose the application `my-content-func`

> This fast, visual method is ideal for local use without complex setup.

#### Method 2 — Automated deployment via terminal

Run the following script from the project root:

```bash
./deploy_function.sh
```

This script:

* checks if you're logged in (`az account show`)
* triggers `az login --use-device-code` if necessary
* publishes the function via `func azure functionapp publish my-content-func`

> Perfect for automation or CI/CD pipelines.

---

### 4. Test the API

```http
GET https://my-content-func.azurewebsites.net/api/recommend_articles?user_id=0
```

---

## Technical Architecture

* **Backend:** Azure Functions (Python)
* **Frontend:** Streamlit (Python)
* **Storage:** Azure Blob Storage for embeddings
* **Model:** Content-based approach using cosine similarity

---

## Exploratory Notebook

An **exploratory notebook** is included in the repository.
It contains comparative tests of several recommendation approaches:

* Popularity-based (baseline)
* Collaborative filtering (SVD, NMF, KNN, ALS)
* Content-based using vector embeddings

The code is structured in clear, reusable functions to improve readability.

> This notebook allows you to **visualize comparative performance** (Recall, MAP, MRR, Coverage) and **justifies the choice of the content-based model** adopted for the MVP.

---

## Possible Improvements

* Full REST API (POST, logs, user creation)
* Dynamic embedding updates
* Real-time database
* Combination of content-based + collaborative filtering
* Mobile interface or progressive web app (PWA)

