---
layout: default
---

<div class="about-section">
  <p>
    &emsp;&emsp;> About me:
  <br>
  <br>&emsp;&emsp;- Name: Helen.Z
  <br>&emsp;&emsp;- I'm from Beijing, China and a permanent resident of Canada, living in Regina.
  <br>&emsp;&emsp;- My MBTI is INTJ, and my Constellation is Aquarius.
  <br>&emsp;&emsp;- My interests include music, singing, drawing, traveling, and many more!
  </p>
</div>

<div class="post-section">
  <p><br>&emsp;&emsp;> Welcome to onlymanes Blog! There lists my articles.</p>
  <ul class="post-list">
    {% for post in site.posts %}
      <li class="post-item">
        <a href="{{ post.url }}" class="post-link">{{ post.title }}</a>
      </li>
    {% endfor %}
  </ul>
</div>

<div class="visitor-counter">
  Total Visitors: <span id="visitorCount">Loading...</span>
</div>

<script type="module">
  import { initializeApp } from "https://www.gstatic.com/firebasejs/9.22.0/firebase-app.js";
  import { getDatabase, ref, runTransaction, onValue } from "https://www.gstatic.com/firebasejs/9.22.0/firebase-database.js";

  const firebaseConfig = {
    apiKey: "AIzaSyAETlOOjT2ulQn-JeUrNKdRMaYhR4o7D2k",
    authDomain: "onlymanes-blog.firebaseapp.com",
    databaseURL: "https://onlymanes-blog-default-rtdb.firebaseio.com",
    projectId: "onlymanes-blog",
    storageBucket: "onlymanes-blog.appspot.com",
    messagingSenderId: "888926945739",
    appId: "1:888926945739:web:094a1e33c1f01512bb364e"
  };

  try {
    const app = initializeApp(firebaseConfig);
    const db = getDatabase(app);
    
    document.addEventListener('DOMContentLoaded', () => {
      const counterRef = ref(db, 'visitorCount');
      
      if (!sessionStorage.getItem('counted')) {
        runTransaction(counterRef, (current) => {
          return (current === null ? 1 : current + 1);
        }).catch((error) => {
          console.error('Transaction error:', error);
        });
        sessionStorage.setItem('counted', 'true');
      }

      onValue(counterRef, 
        (snapshot) => {
          document.getElementById('visitorCount').textContent = snapshot.val() || 0;
        }, 
        (error) => {
          console.error('Listener error:', error);
          document.getElementById('visitorCount').textContent = 'Offline';
        }
      );
    });

  } catch (error) {
    console.error('Firebase init error:', error);
    document.getElementById('visitorCount').textContent = 'System Error';
  }
</script>