# Music-player-app

<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Music Player Interface</title>
<link rel="stylesheet" href="styles.css">
</head>
<body>

<div id="music-player" class="player">
  <audio id="audio-element">
      <source src="path-to-your-first-song.mp3" type="audio/mpeg">
      Your browser does not support the audio element.
  </audio>
  <div class="controls">
      <button id="play-pause" onclick="togglePlayPause()">Play</button>
      <button id="prev" onclick="playPreviousTrack()">Prev</button>
      <button id="next" onclick="playNextTrack()">Next</button>
  </div>
  <div class="progress-bar">
      <div id="progress" class="progress"></div>
  </div>
  <div class="playlist">
      <ul id="playlist">
          <li onclick="changeTrack('path-to-your-first-song.mp3', this)">First Song Title</li>
          <li onclick="changeTrack('path-to-your-second-song.mp3', this)">Second Song Title</li>
          <li onclick="changeTrack('path-to-your-third-song.mp3', this)">Third Song Title</li>
          <!-- More songs can be added here -->
      </ul>
  </div>
</div>

<script src="script.js"></script>
</body>
</html>
CSS Source Code:

/* General Styles */
body {
  font-family: 'Arial', sans-serif;
  background: #f8f8f8;
  color: #333;
  line-height: 1.6;
  padding: 20px;
}

/* Music Player Styles */
.player {
  background: #fff;
  border: 1px solid #ddd;
  padding: 20px;
  margin: 20px auto;
  width: 300px;
  box-shadow: 0 5px 15px rgba(0,0,0,0.1);
}

/* Control Button Styles */
.controls button {
  background: #ff9800;
  border: none;
  padding: 10px 20px;
  margin-right: 5px;
  color: #fff;
  cursor: pointer;
  font-size: 16px;
  transition: background 0.3s ease;
}

.controls button:hover {
  background: #e68900;
}

/* Progress Bar Styles */
.progress-bar {
  background: #e0e0e0;
  border-radius: 5px;
  overflow: hidden;
  position: relative;
  height: 10px;
  margin-top: 20px;
}

.progress {
  background: #ff9800;
  height: 10px;
  width: 0%;
}

/* Playlist Styles */
.playlist {
  margin-top: 20px;
  background: #f7f7f7;
  border: 1px solid #ddd;
  padding: 10px;
}

#playlist li {
  padding: 10px;
  background: #fff;
  border-bottom: 1px solid #ddd;
}

#playlist li:last-child {
  border-bottom: none;
}

#playlist li:hover, #playlist li.playing {
  background: #ff9800;
  color: #fff;
  cursor: pointer;
}

/* Responsive Media Queries */
@media (max-width: 600px) {
  .player {
      width: auto;
      padding: 10px;
  }

  .controls button {
      padding: 5px 10px;
      font-size: 14px;
  }
}
JavaScript Source Code:

// JavaScript for Custom Music Player Controls
var audio = document.getElementById('audio-element');
var playlistItems = document.querySelectorAll('#playlist li');
var tracks = [
  'path-to-your-first-song.mp3',
  'path-to-your-second-song.mp3',
  'path-to-your-third-song.mp3',
  // ...add more tracks here
];

function togglePlayPause() {
  if (audio.paused) {
      audio.play();
      updatePlayPauseButton(true);
  } else {
      audio.pause();
      updatePlayPauseButton(false);
  }
}

function updatePlayPauseButton(isPlaying) {
  var playPauseButton = document.getElementById('play-pause');
  playPauseButton.innerText = isPlaying ? 'Pause' : 'Play';
}

function playNextTrack() {
  currentTrackIndex = (currentTrackIndex + 1) % tracks.length;
  changeTrack(tracks[currentTrackIndex], playlistItems[currentTrackIndex]);
}

function playPreviousTrack() {
  currentTrackIndex = (currentTrackIndex - 1 + tracks.length) % tracks.length;
  changeTrack(tracks[currentTrackIndex], playlistItems[currentTrackIndex]);
}

function changeTrack(source, element) {
  audio.src = source;
  audio.play();
  updatePlayPauseButton(true);
  updatePlaylistHighlight(element);
}

function updatePlaylistHighlight(element) {
  if (element) {
      playlistItems.forEach(li => li.classList.remove('playing'));
      element.classList.add('playing');
  }
}

audio.addEventListener('ended', playNextTrack);

audio.addEventListener('timeupdate', function() {
  var progressBar = document.getElementById('progress');
  var percentage = Math.floor((100 / audio.duration) * audio.currentTime);
  progressBar.style.width = percentage + '%';
});

// Adding click event listeners to playlist items
playlistItems.forEach((item, index) => {
  item.addEventListener('click', function() {
      currentTrackIndex = index;
      changeTrack(tracks[currentTrackIndex], this);
  });
});
