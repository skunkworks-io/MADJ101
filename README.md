### Step-by-Step Guide to Create and Deploy a Mobile App Development Portal

#### 1. Set Up Your Development Environment

1. **Install Node.js and npm**:
   - Download and install Node.js from [nodejs.org](https://nodejs.org/).
   - Verify the installation by running the following commands in your terminal:
     ```bash
     node -v
     npm -v
     ```

2. **Install Firebase CLI**:
   - Run the following command to install Firebase CLI globally:
     ```bash
     npm install -g firebase-tools
     ```

#### 2. Clone the Repository

1. **Clone Your Repository**:
   - Clone the `MADJ101` repository to your local machine:
     ```bash
     git clone https://github.com/skunkworksza/MADJ101.git
     cd MADJ101
     ```

#### 3. Initialize Firebase in Your Project

1. **Create a New Firebase Project**:
   - Go to the [Firebase Console](https://console.firebase.google.com/).
   - Click on "Add project" and follow the instructions to create a new project.

2. **Initialize Firebase in Your Local Project**:
   - Run the following command to set up Firebase Hosting:
     ```bash
     firebase init hosting
     ```
   - Follow the prompts to configure Firebase Hosting for your project.

#### 4. Create the Project Structure

1. **Set Up Directory Structure**:
   - Create the following directories:
     ```bash
     mkdir -p course-materials/lectures
     mkdir -p course-materials/exercises
     mkdir -p course-materials/resources
     mkdir -p practical-lab-work
     mkdir -p web-portal/css
     mkdir -p web-portal/js
     mkdir -p web-portal/assets
     ```

2. **Create Initial Files**:
   - Create `README.md` files in `course-materials/lectures`, `course-materials/exercises`, `course-materials/resources`, and `practical-lab-work`:
     ```bash
     echo "# Lectures" > course-materials/lectures/README.md
     echo "# Exercises" > course-materials/exercises/README.md
     echo "# Resources" > course-materials/resources/README.md
     echo "# Practical Lab Work" > practical-lab-work/README.md
     ```
   - Create `index.html` in `web-portal`:
     ```html
     <!DOCTYPE html>
     <html lang="en">
     <head>
         <meta charset="UTF-8">
         <meta name="viewport" content="width=device-width, initial-scale=1.0">
         <title>Mobile App Development for Juniors</title>
         <link href="https://fonts.googleapis.com/css2?family=IBM+Plex+Sans:wght@400;700&display=swap" rel="stylesheet">
         <link rel="stylesheet" href="css/styles.css">
         <script src="https://www.gstatic.com/firebasejs/9.6.1/firebase-app.js"></script>
         <script src="https://www.gstatic.com/firebasejs/9.6.1/firebase-auth.js"></script>
         <script src="https://www.gstatic.com/firebasejs/9.6.1/firebase-firestore.js"></script>
     </head>
     <body>
         <header>
             <h1>Mobile App Development for Juniors</h1>
             <h2>Course Code: MADJ101</h2>
             <h3>Building Foundational Skills for Future App Developers</h3>
         </header>
         <nav>
             <ul>
                 <li><a href="lectures.html">Lectures</a></li>
                 <li><a href="exercises.html">Exercises</a></li>
                 <li><a href="resources.html">Resources</a></li>
                 <li><a href="practical-lab.html">Practical Lab Work</a></li>
             </ul>
         </nav>
         <main>
             <section id="content">
                 <h2>Welcome to the Course Portal</h2>
                 <p>Navigate through the sections to access course materials and lab exercises.</p>
                 <div id="auth-container">
                     <div id="login-form">
                         <h3>Login</h3>
                         <input type="email" id="login-email" placeholder="Email">
                         <input type="password" id="login-password" placeholder="Password">
                         <button onclick="login()">Login</button>
                     </div>
                     <div id="register-form">
                         <h3>Register</h3>
                         <input type="email" id="register-email" placeholder="Email">
                         <input type="password" id="register-password" placeholder="Password">
                         <button onclick="register()">Register</button>
                     </div>
                 </div>
             </section>
         </main>
         <footer>
             <p>&copy; 2024 Skunkworks (Pty) Ltd. All rights reserved.</p>
         </footer>
         <script src="js/scripts.js"></script>
     </body>
     </html>
     ```

   - Create `styles.css` in `web-portal/css`:
     ```css
     /* CSS styles for the course portal */
     body {
         font-family: "IBM Plex Sans", sans-serif;
         background-color: #f4f4f9;
         color: #333;
         margin: 0;
         padding: 0;
     }
     header {
         background-color: #1a73e8;
         color: white;
         text-align: center;
         padding: 20px 0;
         box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
     }
     header h1 {
         font-size: 2.5em;
         margin: 0.2em 0;
         font-weight: 700;
     }
     header h2 {
         font-size: 1.2em;
         margin: 0.2em 0;
         font-weight: 400;
     }
     header h3 {
         font-size: 1em;
         margin: 0.2em 0;
         font-weight: 400;
     }
     nav ul {
         list-style: none;
         padding: 0;
         display: flex;
         justify-content: center;
         background-color: #1a73e8;
         margin: 0;
     }
     nav ul li {
         margin: 0 15px;
     }
     nav ul li a {
         color: white;
         text-decoration: none;
         font-weight: 700;
     }
     nav ul li a:hover {
         text-decoration: underline;
     }
     main {
         padding: 20px;
     }
     footer {
         text-align: center;
         padding: 10px 0;
         background-color: #1a73e8;
         color: white;
         position: fixed;
         width: 100%;
         bottom: 0;
     }
     ```

   - Create `scripts.js` in `web-portal/js`:
     ```javascript
     // Firebase configuration
     const firebaseConfig = {
         apiKey: "YOUR_API_KEY",
         authDomain: "course-madj101.firebaseapp.com",
         projectId: "course-madj101",
         storageBucket: "course-madj101.appspot.com",
         messagingSenderId: "491294238145",
         appId: "1:491294238145:web:66bcaff49ae8f7faed3b40",
         measurementId: "G-GDQCV0XQVJ"
     };

     // Initialize Firebase
     firebase.initializeApp(firebaseConfig);
     const auth = firebase.auth();
     const db = firebase.firestore();

     // Login function
     function login() {
         const email = document.getElementById("login-email").value;
         const password = document.getElementById("login-password").value;
         auth.signInWithEmailAndPassword(email, password)
             .then((userCredential) => {
                 const user = userCredential.user;
                 alert("Login successful!");
                 // Redirect to course materials page or update UI accordingly
             })
             .catch((error) => {
                 alert("Login failed: " + error.message);
             });
     }

     // Register function
     function register() {
         const email = document.getElementById("register-email").value;
         const password = document.getElementById("register-password").value;
         auth.createUserWithEmailAndPassword(email, password)
             .then((userCredential) => {
                 const user = userCredential.user;
                 alert("Registration successful!");
                 // Save user data to Firestore
                 db.collection("users").doc(user.uid).set({
                     email: email,
                     progress: {
                         lectures: [],
                         exercises: [],
                         labWork: []
                     }
                 });
                 // Redirect to course materials page or update UI accordingly
             })
             .catch((error) => {
                 alert("Registration failed: " + error.message);
             });
     }

     // Function to update progress
     function updateProgress(section, item) {
         const user = auth.currentUser;
         if (user) {
             const userDocRef = db.collection("users").doc(user.uid);
             userDocRef.update({
                 ["progress." + section]: firebase.firestore.FieldValue.arrayUnion(item)
             }).then(() => {
                 alert("Progress updated!");
             }).catch((error) => {
                 alert("Error

 updating progress: " + error.message);
             });
         } else {
             alert("No user logged in!");
         }
     }

     // Function to submit quiz
     function submitQuiz() {
         const user = auth.currentUser;
         if (user) {
             const form = document.getElementById("quiz-form");
             const answers = {
                 question1: form.elements["question1"].value,
                 question2: form.elements["question2"].value
             };
             db.collection("users").doc(user.uid).collection("assessments").add({
                 quiz: answers,
                 timestamp: firebase.firestore.FieldValue.serverTimestamp()
             }).then(() => {
                 alert("Quiz submitted!");
             }).catch((error) => {
                 alert("Error submitting quiz: " + error.message);
             });
         } else {
             alert("No user logged in!");
         }
     }
     ```

#### 5. Set Up GitHub Actions for Automated Deployments

1. **Create GitHub Actions Workflows**:

   - Create `.github/workflows/deploy-preview.yml` for preview deployments:
     ```yaml
     name: Deploy to Preview Channel

     on:
       pull_request:

     jobs:
       build_and_preview:
         runs-on: ubuntu-latest
         steps:
           - uses: actions/checkout@v4
           - name: Set up Node.js
             uses: actions/setup-node@v3
             with:
               node-version: '20'
           - name: Install dependencies
             run: |
               if [ -f package-lock.json ]; then
                 npm ci
               else
                 npm install
               fi
           - name: Build project
             run: npm run build
           - uses: FirebaseExtended/action-hosting-deploy@v0
             with:
               repoToken: "${{ secrets.GITHUB_TOKEN }}"
               firebaseServiceAccount: "${{ secrets.FIREBASE_SERVICE_ACCOUNT }}"
               expires: 30d
               projectId: course-madj101
     ```

   - Create `.github/workflows/deploy-prod.yml` for live deployments:
     ```yaml
     name: Deploy to Live Channel

     on:
       push:
         branches:
           - main

     jobs:
       deploy_live_website:
         runs-on: ubuntu-latest
         steps:
           - uses: actions/checkout@v4
           - name: Set up Node.js
             uses: actions/setup-node@v3
             with:
               node-version: '20'
           - name: Install dependencies
             run: |
               if [ -f package-lock.json ]; then
                 npm ci
               else
                 npm install
               fi
           - name: Build project
             run: npm run build
           - uses: FirebaseExtended/action-hosting-deploy@v0
             with:
               firebaseServiceAccount: "${{ secrets.FIREBASE_SERVICE_ACCOUNT }}"
               projectId: course-madj101
               channelId: live
     ```

2. **Add Secrets to Your GitHub Repository**:
   - Go to your repository on GitHub.
   - Navigate to "Settings" > "Secrets" > "Actions".
   - Add the following secrets:
     - `FIREBASE_SERVICE_ACCOUNT`: The JSON key of your Firebase service account.
     - `GITHUB_TOKEN`: This is automatically available in GitHub Actions and doesn't need to be added manually.

#### 6. Final Steps

1. **Commit and Push Changes**:
   - Add and commit all your changes:
     ```bash
     git add .
     git commit -m "Initial commit with project setup and GitHub Actions"
     ```
   - Push to your repository:
     ```bash
     git push origin main
     ```

2. **Open a Pull Request**:
   - Create a new branch for your feature or fix.
   - Push the branch and open a pull request.
   - Verify that the GitHub Action deploys to the preview channel.

3. **Merge and Deploy to Live**:
   - Once the pull request is reviewed and approved, merge it into the `main` branch.
   - The GitHub Action will automatically deploy the changes to the live channel.

This guide covers the setup and deployment of your Mobile App Development Portal using Firebase Hosting and GitHub Actions. If you have any questions or need further assistance, feel free to ask!
