---
layout: post
title: Mood Matcher 
search_exclude: true
description: Match song with mood
hide: true
menu: nav/home.html
---
 <!-- Background style -->
<style> 
  @import url('https://fonts.googleapis.com/css2?family=Poppins:wght@400;600&display=swap');

  body {
    font-family: 'Poppins', sans-serif;
    background: linear-gradient(135deg, #f9c5d1, #9795ef);
    background-size: 400% 400%;
    animation: gradientShift 10s ease infinite;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
    margin: 0;
  }

  @keyframes gradientShift {
    0% { background-position: 0% 50%; }
    50% { background-position: 100% 50%; }
    100% { background-position: 0% 50%; }
  }

  .container {
    background: white;
    padding: 2rem;
    border-radius: 1.5rem;
    box-shadow: 0 8px 20px rgba(0, 0, 0, 0.2);
    text-align: center;
    width: 95%;
    max-width: 420px;
  }

  h2 {
    color: #5c2a9d;
    margin-bottom: 1rem;
  }

  select, button {
    margin-top: 1rem;
    padding: 0.75rem;
    width: 100%;
    border-radius: 0.75rem;
    border: none;
    font-size: 1rem;
    background-color: #f1effa;
    color: #333;
    transition: all 0.3s ease;
  }

  select:hover, button:hover {
    background-color: #ddd1f5;
  }

  button {
    background-color: #8a4fff;
    color: white;
    font-weight: 600;
  }

  button:hover {
    background-color: #6e2cbf;
  }

  .result {
    margin-top: 2rem;
    font-weight: bold;
    color: #444;
    background-color: #f0ecfb;
    padding: 1rem;
    border-radius: 1rem;
    box-shadow: inset 0 0 5px rgba(0, 0, 0, 0.1);
  }
</style>
<!-- Container div to hold the Mood Matcher content -->
<div class="container">
  <h2>Mood Matcher 🎵</h2>
  <label for="mood">Mood:</label>   <!-- Label for the mood dropdown selection -->
  <select id="mood">  <!-- Dropdown menu for users to select their current mood -->
    <option value="happy">Happy</option>  <!-- Each option represents a different mood -->
    <option value="sad">Sad</option>
    <option value="motivated">Motivated</option>
    <option value="tired">Tired</option>
    <option value="nostalgic">Nostalgic</option>
    <option value="romantic">Romantic</option>
    <option value="anxious">Anxious</option>
    <option value="angry">Angry</option>
    <option value="peaceful">Peaceful</option>
    <option value="inspired">Inspired</option>
    <option value="lonely">Lonely</option>
    <option value="energetic">Energetic</option>
    <option value="chill">Chill</option>
  </select>

  <label for="time">Time of Day:</label> <!-- Label for the time of day dropdown selection -->
  <select id="time"> <!-- Dropdown menu for users to select their current time -->
    <option value="early_morning">Early Morning</option>  <!-- Each option represents a different mood -->
    <option value="morning">Morning</option>
    <option value="late_morning">Late Morning</option>
    <option value="noon">Noon</option>
    <option value="afternoon">Afternoon</option>
    <option value="evening">Evening</option>
    <option value="late_evening">Late Evening</option>
    <option value="night">Night</option>
    <option value="midnight">Midnight</option>
  </select>

  <button onclick="matchMood()">🎧 Get Recommendation</button>

  <div class="result" id="result"></div>
</div>

<script>    // All the music that will displayed
  const musicData = {
  happy: {
    early_morning: ["Walking on Sunshine - Katrina & The Waves", "Send Me On My Way - Rusted Root"],
    morning: ["Sunshine - OneRepublic", "Good Morning - Max Frost"],
    late_morning: ["Happy - Pharrell Williams", "Sugar - Maroon 5"],
    noon: ["Best Day of My Life - American Authors", "Pocketful of Sunshine - Natasha Bedingfield"],
    afternoon: ["Uptown Funk - Bruno Mars", "Can't Stop The Feeling! - Justin Timberlake"],
    evening: ["Fireflies - Owl City", "Good Time - Owl City & Carly Rae Jepsen"],
    late_evening: ["Safe and Sound - Capital Cities", "Walking By - Something Corporate"],
    night: ["Blinding Lights - The Weeknd", "Electric Love - BORNS"],
    midnight: ["Tongue Tied - Grouplove", "Shut Up and Dance - Walk the Moon"]
  },
  sad: {
    early_morning: ["Skinny Love - Bon Iver", "All I Want - Kodaline"],
    morning: ["The Scientist - Coldplay", "Tears Dry On Their Own - Amy Winehouse"],
    late_morning: ["Creep - Radiohead", "Let Her Go - Passenger"],
    noon: ["Lost - Frank Ocean", "Say Something - A Great Big World"],
    afternoon: ["Someone Like You - Adele", "Happier - Ed Sheeran"],
    evening: ["The Night We Met - Lord Huron", "I Know The End - Phoebe Bridgers"],
    late_evening: ["Forever - Lewis Capaldi", "Space Song - Beach House"],
    night: ["Jealous - Labrinth", "Liability - Lorde"],
    midnight: ["The Archer - Taylor Swift", "From the Dining Table - Harry Styles"]
  },
  motivated: {
    early_morning: ["Run the World - Beyoncé", "On the Rise - Lindsey Stirling"],
    morning: ["Eye of the Tiger - Survivor", "Stronger - Kanye West"],
    late_morning: ["Hall of Fame - The Script", "On Top of the World - Imagine Dragons"],
    noon: ["Don't Stop Believin' - Journey", "High Hopes - Panic! At The Disco"],
    afternoon: ["Believer - Imagine Dragons", "Remember the Name - Fort Minor"],
    evening: ["Till I Collapse - Eminem", "Whatever It Takes - Imagine Dragons"],
    late_evening: ["Unstoppable - Sia", "Glorious - Macklemore"],
    night: ["Power - Kanye West", "Lose Yourself - Eminem"],
    midnight: ["Can't Hold Us - Macklemore & Ryan Lewis", "Stronger Than I've Ever Been - Kaleena Zanders"]
  },
  tired: {
    early_morning: ["Breathe Me - Sia", "Motion Picture Soundtrack - Radiohead"],
    morning: ["Holocene - Bon Iver", "Banana Pancakes - Jack Johnson"],
    late_morning: ["Gravity - John Mayer", "Cherry Wine - Hozier"],
    noon: ["The Night We Met - Lord Huron", "Sleep - Azure Ray"],
    afternoon: ["Lost Cause - Beck", "Rivers and Roads - The Head and the Heart"],
    evening: ["No Surprises - Radiohead", "Bloom - The Paper Kites"],
    late_evening: ["To Build A Home - Cinematic Orchestra", "Holocene - Bon Iver"],
    night: ["Moon River - Frank Ocean", "Weightless - Marconi Union"],
    midnight: ["Motion - Tycho", "Night Owl - Galimatias"]
  },
  nostalgic: {
    early_morning: ["The Only Living Boy in New York - Simon & Garfunkel", "Vienna - Billy Joel"],
    morning: ["Somewhere Over the Rainbow - Israel Kamakawiwoʻole", "Hey There Delilah - Plain White T’s"],
    late_morning: ["First Day of My Life - Bright Eyes", "Iris - Goo Goo Dolls"],
    noon: ["Heroes - David Bowie", "Home - Edward Sharpe & The Magnetic Zeros"],
    afternoon: ["Mr. Brightside - The Killers", "Stolen Dance - Milky Chance"],
    evening: ["Yellow - Coldplay", "Chasing Cars - Snow Patrol"],
    late_evening: ["Breathe Me - Sia", "Landslide - Fleetwood Mac"],
    night: ["Somewhere Only We Know - Keane", "Yesterday - The Beatles"],
    midnight: ["Fix You - Coldplay", "The Night We Met - Lord Huron"]
  },
  romantic: {
    early_morning: ["Kiss Me - Sixpence None the Richer", "Your Song - Elton John"],
    morning: ["Lover - Taylor Swift", "I Choose You - Sara Bareilles"],
    late_morning: ["Lucky - Jason Mraz ft. Colbie Caillat", "You and I - Ingrid Michaelson"],
    noon: ["Can’t Help Falling In Love - Elvis Presley", "Better Together - Jack Johnson"],
    afternoon: ["Love on Top - Beyoncé", "Just the Way You Are - Bruno Mars"],
    evening: ["Thinking Out Loud - Ed Sheeran", "Like I'm Gonna Lose You - Meghan Trainor & John Legend"],
    late_evening: ["Say You Won't Let Go - James Arthur", "Make You Feel My Love - Adele"],
    night: ["Perfect - Ed Sheeran", "All of Me - John Legend"],
    midnight: ["Speechless - Dan + Shay", "You Are The Reason - Calum Scott"]
  },
  anxious: {
    early_morning: ["Intro - The xx", "Beach Baby - Bon Iver"],
    morning: ["This Is Me Trying - Taylor Swift", "Numb - Linkin Park"],
    late_morning: ["Dark Paradise - Lana Del Rey", "Let It Happen - Tame Impala"],
    noon: ["Midnight City - M83", "Breathe - Télépopmusik"],
    afternoon: ["Crystals - Of Monsters and Men", "Electric Feel - MGMT"],
    evening: ["Let Go - Frou Frou", "Swim - Jack's Mannequin"],
    late_evening: ["Youth - Daughter", "Holocene - Bon Iver"],
    night: ["Runaway - AURORA", "The Way It Was - The Killers"],
    midnight: ["Retrograde - James Blake", "Fade Into You - Mazzy Star"]
  },
  angry: {
    early_morning: ["Stressed Out - Twenty One Pilots", "Misery Business - Paramore"],
    morning: ["In The End - Linkin Park", "The Pretender - Foo Fighters"],
    late_morning: ["Numb - Linkin Park", "DNA. - Kendrick Lamar"],
    noon: ["Welcome to the Jungle - Guns N’ Roses", "Centuries - Fall Out Boy"],
    afternoon: ["Break Stuff - Limp Bizkit", "Killing in the Name - Rage Against the Machine"],
    evening: ["Smells Like Teen Spirit - Nirvana", "Bodies - Drowning Pool"],
    late_evening: ["The Way I Am - Eminem", "Headstrong - Trapt"],
    night: ["Monster - Skillet", "Duality - Slipknot"],
    midnight: ["Bleed It Out - Linkin Park", "My Songs Know What You Did In the Dark - Fall Out Boy"]
  },
  peaceful: {
    early_morning: ["Weightless - Natasha Bedingfield", "Sun It Rises - Fleet Foxes"],
    morning: ["Banana Pancakes - Jack Johnson", "Holocene - Bon Iver"],
    late_morning: ["River Flows In You - Yiruma", "Ophelia - The Lumineers"],
    noon: ["Bloom - The Paper Kites", "Saturn - Sleeping at Last"],
    afternoon: ["Sunset Lover - Petit Biscuit", "Home - Phillip Phillips"],
    evening: ["The Ocean - Mike Perry", "Lost in the Moment - NF"],
    late_evening: ["Night Trouble - Petit Biscuit", "Intro - M83"],
    night: ["Weightless - Marconi Union", "Asleep - The Smiths"],
    midnight: ["Night Owl - Galimatias", "Weightless - Marconi Union"]
  },
  inspired: {
    early_morning: ["A Sky Full of Stars - Coldplay", "Brand New - Ben Rector"],
    morning: ["Rise Up - Andra Day", "Wake Me Up - Avicii"],
    late_morning: ["Hall of Fame - The Script", "On Top of the World - Imagine Dragons"],
    noon: ["Unwritten - Natasha Bedingfield", "Good Life - OneRepublic"],
    afternoon: ["Brave - Sara Bareilles", "Pompeii - Bastille"],
    evening: ["Whatever It Takes - Imagine Dragons", "Fight Song - Rachel Platten"],
    late_evening: ["Born This Way - Lady Gaga", "Roar - Katy Perry"],
    night: ["Wavin’ Flag - K’naan", "The Climb - Miley Cyrus"],
    midnight: ["Dreams - Fleetwood Mac", "I Lived - OneRepublic"]
  },
  lonely: {
    early_morning: ["Holocene - Bon Iver", "River - Leon Bridges"],
    morning: ["Lost Boy - Ruth B", "From Afar - Vance Joy"],
    late_morning: ["All I Want - Kodaline", "Skinny Love - Bon Iver"],
    noon: ["Let Her Go - Passenger", "Breathe Me - Sia"],
    afternoon: ["Unsteady - X Ambassadors", "Say You Love Me - Jessie Ware"],
    evening: ["Another Love - Tom Odell", "Cigarette Daydreams - Cage The Elephant"],
    late_evening: ["Roslyn - Bon Iver & St. Vincent", "Cherry Wine - Hozier"],
    night: ["Talking to the Moon - Bruno Mars", "From the Dining Table - Harry Styles"],
    midnight: ["The Night We Met - Lord Huron", "I’m Not The Only One - Sam Smith"]
  },
  energetic: {
    early_morning: ["Walking on Sunshine - Katrina & The Waves", "Dance Monkey - Tones and I"],
    morning: ["Shut Up and Dance - Walk the Moon", "Can’t Stop - Red Hot Chili Peppers"],
    late_morning: ["Can’t Hold Us - Macklemore", "Good As Hell - Lizzo"],
    noon: ["Levitating - Dua Lipa", "Boom Clap - Charli XCX"],
    afternoon: ["Don't Stop Me Now - Queen", "Titanium - David Guetta ft. Sia"],
    evening: ["Take On Me - a-ha", "We Found Love - Rihanna ft. Calvin Harris"],
    late_evening: ["Feel It Still - Portugal. The Man", "Classic - MKTO"],
    night: ["Party Rock Anthem - LMFAO", "Yeah! - Usher"],
    midnight: ["Midnight City - M83", "We Are Young - fun."]
  },
  chill: {
    early_morning: ["Sunset Lover - Petit Biscuit", "Lost - Frank Ocean"],
    morning: ["Budapest - George Ezra", "Better Together - Jack Johnson"],
    late_morning: ["Electric Feel - MGMT", "Put Your Records On - Corinne Bailey Rae"],
    noon: ["Breezeblocks - Alt-J", "ilomilo - Billie Eilish"],
    afternoon: ["Cherry - Harry Styles", "Moon - Kanye West"],
    evening: ["The Less I Know The Better - Tame Impala", "Heartbeats - José González"],
    late_evening: ["Coffee - Beabadoobee", "Lost in Japan - Shawn Mendes"],
    night: ["Come Through - Jeremy Zucker", "Riptide - Vance Joy"],
    midnight: ["Night Owl - Galimatias", "I Wanna Be Yours - Arctic Monkeys"]
  }
};
/**
 * ✅ PROCEDURE: matchMood()
 * - Takes input (mood, time)
 * - Uses SEQUENCING: Step-by-step operations
 * - Uses SELECTION: if-else conditions to handle logic
 * - Uses ITERATION: loops through matching songs
 * - Outputs text based on selected song (visual output)
 */
// Procedure to match the mood and time with an appropriate song
// This function takes two parameters: mood and time, which are selected by the user.
function matchMood() {
  // 📥 Step 1: INPUT — Get user selections
  // 🧩 Step 1: Get user input (Sequencing Step 1)
  const mood = document.getElementById("mood").value;
  const time = document.getElementById("time").value;

  // 📦 Step 2: Access output elements
  const resultDiv = document.getElementById("result");
  const listOutput = document.getElementById("listOutput");

  // 📚 Step 3: Look up song list based on mood and time
  // Check if the mood and time exist in musicData before accessing
  if (musicData[mood] && musicData[mood][time]) {
    const options = musicData[mood][time];

    // 🔁 ITERATION: Loop to display all possible songs
    let songList = "🎶 Matching Songs:<br>";
    for (let i = 0; i < options.length; i++) {
      songList += `• ${options[i]}<br>`;
    }
    listOutput.innerHTML = musicData;

    // 🎯 SELECTION + RANDOM CHOICE
    const randomIndex = Math.floor(Math.random() * options.length);
    const selectedSong = options[randomIndex];

    // 📤 OUTPUT: Display suggested song
    resultDiv.textContent = `🎧 Try this: ${selectedSong}`;
  } else {
    // ❌ No matches found
    // if no matches found this error message is shown
    listOutput.innerHTML = "";
    resultDiv.textContent = `😔 Sorry, no songs found for that mood/time.`;
  }
}

</script>
