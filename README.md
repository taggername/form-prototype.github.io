
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Student Career Questionnaire</title>
  <style>
    body { font-family: Arial, sans-serif; margin: 40px; background: #f4f6f9; }
    .container { max-width: 1200px; margin: auto; }
    form, .results { background: #fff; padding: 25px; border-radius: 12px; box-shadow: 0 4px 8px rgba(0,0,0,0.1); }
    section { margin-bottom: 40px; padding: 20px; border-left: 4px solid #0077cc; background: #fafafa; border-radius: 6px; }
    h2, h3 { color: #2c3e50; }
    label { display: block; margin-top: 12px; font-weight: bold; }
    .options { margin-left: 20px; }
    button { margin-top: 20px; padding: 12px 18px; border: none; background: #0077cc; color: #fff; border-radius: 6px; cursor: pointer; font-size: 16px; }
    button:hover { background: #005fa3; }
    .hidden { display: none; }
    .cards { display: grid; grid-template-columns: repeat(auto-fit, minmax(350px, 1fr)); gap: 20px; }
    .card { background: #fff; padding: 20px; border-radius: 12px; box-shadow: 0 4px 8px rgba(0,0,0,0.1); }
    .card h3 { margin-top: 0; color: #0077cc; }
    ul { padding-left: 20px; margin: 0; }
  </style>
</head>
<body>
  <div class="container">
    <!-- FORM -->
    <div id="formPage">
      <h2>Comprehensive Student Career & Skills Questionnaire</h2>
      <form id="careerForm">
        <!-- Sections here (your 6 sections with 40–45 questions exactly as before) -->
        <!-- Section 1: Cognitive Abilities -->
        <section>
          <h3>Section 1: Cognitive Abilities</h3>
          <label>1. Logical reasoning</label>
          <div class="options">
            <input type="radio" name="logic" value="logical_strong"> Strong  
            <input type="radio" name="logic" value="logical_average"> Average  
            <input type="radio" name="logic" value="logical_weak"> Weak  
          </div>
          <!-- keep adding rest of Section 1 → Section 6 same as your form -->
        </section>

        <!-- Section 2–6 as before -->
        <!-- ... -->

        <button type="submit">Analyze My Profile</button>
      </form>
    </div>

    <!-- RESULTS -->
    <div id="resultsPage" class="hidden">
      <div class="results">
        <h2>Your Career Suggestions</h2>
        <div class="cards" id="resultsCards"></div>
      </div>
    </div>
  </div>

  <script>
    document.getElementById("careerForm").addEventListener("submit", function(event) {
      event.preventDefault();

      // Collect answers
      const formData = new FormData(event.target);
      let answers = {};
      for (let [key, val] of formData.entries()) {
        if (!answers[key]) answers[key] = [];
        answers[key].push(val);
      }

      // Scoring system
      let scores = { arts:0, business:0, biology:0, technology:0, mathematics:0 };
      let map = {
        logical_strong:"mathematics", logical_average:"business", logical_weak:"arts",
        creativity_high:"arts", creativity_medium:"business", creativity_low:"mathematics",
        analysis_excellent:"mathematics", analysis_good:"business", analysis_poor:"arts",
        abstract_strong:"mathematics", abstract_okay:"biology", abstract_struggle:"arts",
        decision_strong:"business", decision_average:"biology", decision_weak:"arts",
        grasp_fast:"technology", grasp_average:"business", grasp_slow:"arts",
        motivation_high:"business", motivation_medium:"biology", motivation_low:"arts",
        learning_visual:"arts", learning_auditory:"arts", learning_text:"mathematics", learning_kinesthetic:"biology",
        consistency_high:"business", consistency_medium:"biology", consistency_low:"arts",
        challenges_yes:"technology", challenges_sometimes:"business", challenges_no:"arts",
        adaptability_high:"biology", adaptability_medium:"business", adaptability_low:"arts"
        // Add the rest mappings for other questions as needed
      };

      for (let key in answers) {
        answers[key].forEach(val => {
          if (map[val]) scores[map[val]] += 2;
        });
      }

      // Get top 3 domains
      let topDomains = Object.entries(scores).sort((a,b)=>b[1]-a[1]).slice(0,3);

      // Career lists (dummy placeholders — replace with your 50 roles each)
      let careerMap = {
        arts: ["Graphic Designer","Animator","Art Director","Musician", "Actor" /* … up to 50 */],
        business: ["Business Analyst","Entrepreneur","Marketing Manager","Sales Executive","Finance Manager" /* … */],
        biology: ["Biotechnologist","Geneticist","Microbiologist","Pharmacologist","Medical Researcher" /* … */],
        technology: ["Software Engineer","AI Engineer","Cybersecurity Analyst","Cloud Architect","Data Scientist" /* … */],
        mathematics: ["Statistician","Actuary","Data Analyst","Operations Researcher","Cryptographer" /* … */]
      };

      // Build results
      let cards = "";
      topDomains.forEach(([domain, score]) => {
        cards += `<div class="card">
          <h3>${domain.charAt(0).toUpperCase() + domain.slice(1)} (Score: ${score})</h3>
          <ul>${careerMap[domain].map(job=>`<li>${job}</li>`).join("")}</ul>
        </div>`;
      });
      document.getElementById("resultsCards").innerHTML = cards;

      // Switch view
      document.getElementById("formPage").classList.add("hidden");
      document.getElementById("resultsPage").classList.remove("hidden");
    });
  </script>
</body>
</html>
