# 🌐 Social Media App

A full-stack **social media application** built with **Django (REST API)** for the backend and **Vue.js** for the frontend.  
It allows users to **connect, share posts, search, send friend requests, and interact socially** in a modern UI.

---

## 🚀 Features

- 🔐 **Authentication**
  - User Registration & Login
  - JWT-based authentication
  - Profile management

- 👥 **Friend System**
  - Send, accept, and decline friend requests
  - View friends list
  - Follow/unfollow functionality

- 📝 **Posts**
  - Create, edit, and delete posts
  - Public and private posts
  - Like and comment on posts
  - Post search functionality

- 🔎 **Search**
  - Search users by name
  - Search posts by content
  - Smart query filtering using Django ORM

- 📩 **Notifications**
  - Get notified on friend requests
  - Post interactions

---

## 🛠 Tech Stack

### Backend
- Django REST Framework (DRF)
- JWT Authentication
- PostgreSQL / MySQL (your choice)

### Frontend
- React.js (with Hooks & Context API / Redux)
- TailwindCSS / Bootstrap (UI Styling)
- Axios for API calls

---

## ⚙️ Installation & Setup
  **Clone the Repository**
```bash


git clone https://github.com/your-username/social-media-app.git
cd social-media-app
```


  **Backend Setup**
```bash
cd backend
pip install -r requirements.txt
python manage.py migrate
python manage.py createsuperuser
python manage.py runserver
```



  **Frontend Setup**
  ```bash
cd frontend
npm install
npm start
```
