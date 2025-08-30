# task-3
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Responsive Interactive Web Page</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      margin: 0;
      padding: 0;
      background: #f4f4f4;
    }

    header, section {
      padding: 20px;
    }

    header {
      background: #333;
      color: white;
      text-align: center;
    }

    .container {
      display: flex;
      gap: 20px;
      flex-wrap: wrap;
      justify-content: center;
    }

    .box {
      background: white;
      padding: 20px;
      flex: 1 1 300px;
      max-width: 500px;
      box-shadow: 0 2px 5px rgba(0,0,0,0.1);
    }

    h2 {
      margin-top: 0;
    }

    img {
      width: 100%;
      height: auto;
    }

    button {
      padding: 10px 15px;
      margin: 10px 5px 0 0;
      cursor: pointer;
    }

    @media (max-width: 768px) {
      body {
        font-size: 16px;
      }

      .container {
        flex-direction: column;
        align-items: center;
      }
    }

    @media (max-width: 480px) {
      body {
        font-size: 14px;
      }

      button {
        width: 100%;
      }
    }
  </style>
</head>
<body>

  <header>
    <h1>Responsive Interactive Web Page</h1>
  </header>

  <section class="container">

    <!-- Quiz Section -->
    <div class="box">
      <h2>Interactive Quiz</h2>
      <div id="quiz"></div>
      <button onclick="nextQuestion()">Next Question</button>
    </div>

    <!-- Image Carousel Section -->
    <div class="box">
      <h2>Image Carousel</h2>
      <img id="carousel" src="https://picsum.photos/id/1015/600/300" alt="Carousel Image">
      <br>
      <button onclick="prevImage()">Previous</button>
      <button onclick="nextImage()">Next</button>
    </div>

    <!-- API Fetch Section -->
    <div class="box">
      <h2>Random Joke (API)</h2>
      <p id="joke">Loading joke...</p>
      <button onclick="loadJoke()">New Joke</button>
    </div>

  </section>

  <script>
    // === Quiz Logic ===
    const quizData = [
      { question: "Capital of France?", options: ["Paris", "Rome"], answer: 0 },
      { question: "2 + 2 = ?", options: ["3", "4"], answer: 1 },
      { question: "Which language runs in browser?", options: ["PHP", "JavaScript"], answer: 1 }
    ];

    let quizIndex = 0;

    function renderQuiz() {
      const q = quizData[quizIndex];
      const html = <p><strong>${q.question}</strong></p> +
        q.options.map((opt, i) =>
          <label><input type="radio" name="option" value="${i}"> ${opt}</label><br>
        ).join('');
      document.getElementById("quiz").innerHTML = html;
    }

    function nextQuestion() {
      const selected = document.querySelector('input[name="option"]:checked');
      if (!selected) return alert("Please select an option.");
      const correct = Number(selected.value) === quizData[quizIndex].answer;
      alert(correct ? "Correct!" : "Incorrect.");
      quizIndex++;
      if (quizIndex < quizData.length) {
        renderQuiz();
      } else {
        document.getElementById("quiz").innerHTML = "<strong>Quiz completed!</strong>";
      }
    }

    renderQuiz();

    // === Carousel Logic ===
    const images = [
      "https://picsum.photos/id/1015/600/300",
      "https://picsum.photos/id/1025/600/300",
      "https://picsum.photos/id/1043/600/300"
    ];
    let imageIndex = 0;

    function updateCarousel() {
      document.getElementById("carousel").src = images[imageIndex];
    }

    function nextImage() {
      imageIndex = (imageIndex + 1) % images.length;
      updateCarousel();
    }

    function prevImage() {
      imageIndex = (imageIndex - 1 + images.length) % images.length;
      updateCarousel();
    }

    // === Joke API Logic ===
    function loadJoke() {
      fetch("https://api.chucknorris.io/jokes/random")
        .then(res => res.json())
        .then(data => {
          document.getElementById("joke").innerText = data.value;
        })
        .catch(() => {
          document.getElementById("joke").innerText = "Could not load a joke.";
        });
    }

    loadJoke(); // Load joke on page load
  </script>
</body>
</html>
