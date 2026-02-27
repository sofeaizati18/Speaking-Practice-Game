<html lang="en" class="h-full">
 <head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Speaking Proficiency Game</title>
  <script src="https://cdn.tailwindcss.com/3.4.17"></script>
  <script src="/_sdk/element_sdk.js"></script>
  <link href="https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@400;500;600;700&amp;family=Inter:wght@400;500;600&amp;display=swap" rel="stylesheet">
  <style>
    * { font-family: 'Inter', sans-serif; }
    h1, h2, h3, .title-font { font-family: 'Space Grotesk', sans-serif; }
    
    @keyframes pulse-ring {
      0% { transform: scale(0.8); opacity: 1; }
      100% { transform: scale(1.4); opacity: 0; }
    }
    
    @keyframes float {
      0%, 100% { transform: translateY(0); }
      50% { transform: translateY(-10px); }
    }
    
    .recording-pulse::before {
      content: '';
      position: absolute;
      inset: -8px;
      border-radius: 50%;
      background: rgba(239, 68, 68, 0.4);
      animation: pulse-ring 1.5s ease-out infinite;
    }
    
    .float-animation { animation: float 3s ease-in-out infinite; }
    
    .level-badge {
      background: linear-gradient(135deg, var(--badge-start), var(--badge-end));
    }
    
    @keyframes slideUp {
      from { opacity: 0; transform: translateY(20px); }
      to { opacity: 1; transform: translateY(0); }
    }
    
    .slide-up { animation: slideUp 0.5s ease-out forwards; }
    
    @keyframes waveform {
      0%, 100% { height: 8px; }
      50% { height: 32px; }
    }
    
    .waveform-bar {
      animation: waveform 0.8s ease-in-out infinite;
    }
  </style>
  <style>body { box-sizing: border-box; }</style>
  <script src="/_sdk/data_sdk.js" type="text/javascript"></script>
 </head>
 <body class="h-full">
  <div id="app-wrapper" class="h-full w-full overflow-auto" style="background: linear-gradient(135deg, #1e1b4b 0%, #312e81 50%, #4338ca 100%);">
   <div class="min-h-full w-full px-4 py-8">
    <div class="max-w-2xl mx-auto"><!-- Header -->
     <div class="text-center mb-8 slide-up">
      <div class="inline-flex items-center gap-3 mb-4">
       <div class="w-12 h-12 rounded-2xl bg-gradient-to-br from-amber-400 to-orange-500 flex items-center justify-center float-animation">
        <svg class="w-7 h-7 text-white" fill="none" stroke="currentColor" viewbox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 11a7 7 0 01-7 7m0 0a7 7 0 01-7-7m7 7v4m0 0H8m4 0h4m-4-8a3 3 0 01-3-3V5a3 3 0 116 0v6a3 3 0 01-3 3z" />
        </svg>
       </div>
      </div>
      <h1 id="game-title" class="title-font text-3xl md:text-4xl font-bold text-white mb-3">Speaking Proficiency Game</h1>
      <p id="instruction-text" class="text-indigo-200 text-lg">Record yourself speaking and discover your CEFR level!</p>
     </div><!-- Main Card -->
     <div class="bg-white/10 backdrop-blur-lg rounded-3xl p-6 md:p-8 border border-white/20 shadow-2xl"><!-- Prompt Section -->
      <div class="bg-indigo-900/50 rounded-2xl p-5 mb-6 border border-indigo-400/30">
       <div class="flex items-start gap-3">
        <div class="w-10 h-10 rounded-xl bg-indigo-500 flex items-center justify-center flex-shrink-0">
         <svg class="w-5 h-5 text-white" fill="none" stroke="currentColor" viewbox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8.228 9c.549-1.165 2.03-2 3.772-2 2.21 0 4 1.343 4 3 0 1.4-1.278 2.575-3.006 2.907-.542.104-.994.54-.994 1.093m0 3h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z" />
         </svg>
        </div>
        <div>
         <h3 class="text-amber-300 font-semibold mb-1">Speaking Prompt</h3>
         <p class="text-white/90 leading-relaxed">Describe your favorite hobby or activity. Talk about what you enjoy doing, how often you do it, and why it makes you happy. Try to speak for at least 30 seconds.</p>
        </div>
       </div>
      </div><!-- Recording Section -->
      <div id="recording-section" class="text-center mb-6"><button id="record-btn" class="relative w-24 h-24 rounded-full bg-gradient-to-br from-red-500 to-rose-600 hover:from-red-400 hover:to-rose-500 transition-all duration-300 shadow-lg hover:shadow-red-500/30 focus:outline-none focus:ring-4 focus:ring-red-500/50 mx-auto flex items-center justify-center">
        <svg id="mic-icon" class="w-10 h-10 text-white" fill="none" stroke="currentColor" viewbox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 11a7 7 0 01-7 7m0 0a7 7 0 01-7-7m7 7v4m0 0H8m4 0h4m-4-8a3 3 0 01-3-3V5a3 3 0 116 0v6a3 3 0 01-3 3z" />
        </svg>
        <svg id="stop-icon" class="w-10 h-10 text-white hidden" fill="currentColor" viewbox="0 0 24 24"><rect x="6" y="6" width="12" height="12" rx="2" />
        </svg></button>
       <p id="record-status" class="text-indigo-200 mt-4 text-sm">Click the microphone to start recording</p>
       <div id="timer" class="text-2xl font-mono text-amber-300 mt-2 hidden">
        00:00
       </div><!-- Waveform visualization -->
       <div id="waveform" class="hidden justify-center items-end gap-1 h-10 mt-4">
        <div class="w-1 bg-amber-400 rounded-full waveform-bar" style="animation-delay: 0s;"></div>
        <div class="w-1 bg-amber-400 rounded-full waveform-bar" style="animation-delay: 0.1s;"></div>
        <div class="w-1 bg-amber-400 rounded-full waveform-bar" style="animation-delay: 0.2s;"></div>
        <div class="w-1 bg-amber-400 rounded-full waveform-bar" style="animation-delay: 0.3s;"></div>
        <div class="w-1 bg-amber-400 rounded-full waveform-bar" style="animation-delay: 0.4s;"></div>
        <div class="w-1 bg-amber-400 rounded-full waveform-bar" style="animation-delay: 0.5s;"></div>
        <div class="w-1 bg-amber-400 rounded-full waveform-bar" style="animation-delay: 0.6s;"></div>
        <div class="w-1 bg-amber-400 rounded-full waveform-bar" style="animation-delay: 0.7s;"></div>
       </div>
      </div><!-- Error Message -->
      <div id="error-message" class="hidden bg-red-500/20 border border-red-400/50 rounded-xl p-4 mb-6 text-red-200 text-center"></div><!-- Submit Button --> <button id="submit-btn" class="w-full py-4 rounded-xl bg-gradient-to-r from-amber-400 to-orange-500 hover:from-amber-300 hover:to-orange-400 text-indigo-900 font-bold text-lg transition-all duration-300 shadow-lg hover:shadow-amber-500/30 disabled:opacity-50 disabled:cursor-not-allowed disabled:hover:shadow-none focus:outline-none focus:ring-4 focus:ring-amber-400/50" disabled> <span id="submit-text">Record your voice first</span> <span id="submit-loading" class="hidden">
        <svg class="animate-spin h-5 w-5 inline mr-2" viewbox="0 0 24 24"><circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4" fill="none" /> <path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z" />
        </svg> Analyzing... </span> </button>
     </div><!-- Results Section (hidden initially) -->
     <div id="results-section" class="hidden mt-6"><!-- Transcription Card -->
      <div class="bg-white/10 backdrop-blur-lg rounded-3xl p-6 border border-white/20 shadow-2xl mb-6 slide-up">
       <div class="flex items-center gap-3 mb-4">
        <div class="w-10 h-10 rounded-xl bg-emerald-500 flex items-center justify-center">
         <svg class="w-5 h-5 text-white" fill="none" stroke="currentColor" viewbox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 12h6m-6 4h6m2 5H7a2 2 0 01-2-2V5a2 2 0 012-2h5.586a1 1 0 01.707.293l5.414 5.414a1 1 0 01.293.707V19a2 2 0 01-2 2z" />
         </svg>
        </div>
        <h2 class="title-font text-xl font-bold text-white">Your Transcription</h2>
       </div>
       <div id="transcription-text" class="bg-indigo-900/50 rounded-xl p-4 border border-indigo-400/30 text-white/90 leading-relaxed italic"><!-- Transcription will appear here -->
       </div>
      </div><!-- CEFR Result Card -->
      <div class="bg-white/10 backdrop-blur-lg rounded-3xl p-6 border border-white/20 shadow-2xl slide-up" style="animation-delay: 0.2s;">
       <div class="flex items-center gap-3 mb-4">
        <div class="w-10 h-10 rounded-xl bg-violet-500 flex items-center justify-center">
         <svg class="w-5 h-5 text-white" fill="none" stroke="currentColor" viewbox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 19v-6a2 2 0 00-2-2H5a2 2 0 00-2 2v6a2 2 0 002 2h2a2 2 0 002-2zm0 0V9a2 2 0 012-2h2a2 2 0 012 2v10m-6 0a2 2 0 002 2h2a2 2 0 002-2m0 0V5a2 2 0 012-2h2a2 2 0 012 2v14a2 2 0 01-2 2h-2a2 2 0 01-2-2z" />
         </svg>
        </div>
        <h2 class="title-font text-xl font-bold text-white">Your CEFR Level</h2>
       </div>
       <div class="text-center py-6">
        <div id="cefr-badge" class="inline-flex items-center justify-center w-24 h-24 rounded-2xl level-badge text-white text-4xl font-bold title-font shadow-xl mb-4" style="--badge-start: #8b5cf6; --badge-end: #6366f1;"><!-- Level will appear here -->
        </div>
        <h3 id="cefr-title" class="text-xl font-semibold text-white mb-2"><!-- Level name --></h3>
        <p id="cefr-description" class="text-indigo-200 max-w-md mx-auto"><!-- Description --></p>
       </div><!-- Stats -->
       <div class="grid grid-cols-3 gap-3 mt-4">
        <div class="bg-indigo-900/50 rounded-xl p-3 text-center border border-indigo-400/30">
         <p class="text-2xl font-bold text-amber-300" id="word-count">0</p>
         <p class="text-xs text-indigo-200">Words</p>
        </div>
        <div class="bg-indigo-900/50 rounded-xl p-3 text-center border border-indigo-400/30">
         <p class="text-2xl font-bold text-amber-300" id="sentence-count">0</p>
         <p class="text-xs text-indigo-200">Sentences</p>
        </div>
        <div class="bg-indigo-900/50 rounded-xl p-3 text-center border border-indigo-400/30">
         <p class="text-2xl font-bold text-amber-300" id="duration-display">0s</p>
         <p class="text-xs text-indigo-200">Duration</p>
        </div>
       </div>
      </div><!-- Try Again Button --> <button id="try-again-btn" class="w-full mt-6 py-4 rounded-xl bg-white/10 hover:bg-white/20 border border-white/30 text-white font-semibold transition-all duration-300 focus:outline-none focus:ring-4 focus:ring-white/30"> 🎤 Try Again </button>
     </div><!-- CEFR Info -->
     <div class="mt-8 text-center">
      <details class="bg-white/5 rounded-2xl border border-white/10"><summary class="px-5 py-3 cursor-pointer text-indigo-200 hover:text-white transition-colors">What is CEFR?</summary>
       <div class="px-5 pb-4 text-left text-sm text-indigo-200/80 space-y-2">
        <p><span class="font-semibold text-amber-300">A1 - Beginner:</span> Can use basic phrases and introduce themselves.</p>
        <p><span class="font-semibold text-amber-300">A2 - Elementary:</span> Can communicate in simple, routine tasks.</p>
        <p><span class="font-semibold text-amber-300">B1 - Intermediate:</span> Can deal with most situations while traveling.</p>
        <p><span class="font-semibold text-amber-300">B2 - Upper Intermediate:</span> Can interact fluently with native speakers.</p>
        <p><span class="font-semibold text-amber-300">C1 - Advanced:</span> Can express ideas fluently and spontaneously.</p>
        <p><span class="font-semibold text-amber-300">C2 - Proficient:</span> Can understand virtually everything with ease.</p>
       </div>
      </details>
     </div>
    </div>
   </div>
  </div>
  <script>
    // Default configuration
    const defaultConfig = {
      game_title: 'Speaking Proficiency Game',
      instruction_text: 'Record yourself speaking and discover your CEFR level!',
      background_color: '#1e1b4b',
      accent_color: '#f59e0b',
      text_color: '#ffffff',
      font_family: 'Space Grotesk'
    };

    let config = { ...defaultConfig };

    // Recording state
    let mediaRecorder = null;
    let audioChunks = [];
    let recordingStartTime = null;
    let timerInterval = null;
    let recordedDuration = 0;
    let transcribedText = '';

    // DOM Elements
    const recordBtn = document.getElementById('record-btn');
    const micIcon = document.getElementById('mic-icon');
    const stopIcon = document.getElementById('stop-icon');
    const recordStatus = document.getElementById('record-status');
    const timer = document.getElementById('timer');
    const waveform = document.getElementById('waveform');
    const submitBtn = document.getElementById('submit-btn');
    const submitText = document.getElementById('submit-text');
    const submitLoading = document.getElementById('submit-loading');
    const errorMessage = document.getElementById('error-message');
    const resultsSection = document.getElementById('results-section');

    // Speech Recognition setup
    const SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition;
    let recognition = null;

    if (SpeechRecognition) {
      recognition = new SpeechRecognition();
      recognition.continuous = true;
      recognition.interimResults = true;
      recognition.lang = 'en-US';
    }

    // Initialize Element SDK
    if (window.elementSdk) {
      window.elementSdk.init({
        defaultConfig,
        onConfigChange: async (newConfig) => {
          config = { ...defaultConfig, ...newConfig };
          updateUI();
        },
        mapToCapabilities: (cfg) => ({
          recolorables: [
            {
              get: () => cfg.background_color || defaultConfig.background_color,
              set: (v) => { cfg.background_color = v; window.elementSdk.setConfig({ background_color: v }); }
            },
            {
              get: () => cfg.accent_color || defaultConfig.accent_color,
              set: (v) => { cfg.accent_color = v; window.elementSdk.setConfig({ accent_color: v }); }
            },
            {
              get: () => cfg.text_color || defaultConfig.text_color,
              set: (v) => { cfg.text_color = v; window.elementSdk.setConfig({ text_color: v }); }
            }
          ],
          borderables: [],
          fontEditable: {
            get: () => cfg.font_family || defaultConfig.font_family,
            set: (v) => { cfg.font_family = v; window.elementSdk.setConfig({ font_family: v }); }
          },
          fontSizeable: undefined
        }),
        mapToEditPanelValues: (cfg) => new Map([
          ['game_title', cfg.game_title || defaultConfig.game_title],
          ['instruction_text', cfg.instruction_text || defaultConfig.instruction_text]
        ])
      });
    }

    function updateUI() {
      document.getElementById('game-title').textContent = config.game_title || defaultConfig.game_title;
      document.getElementById('instruction-text').textContent = config.instruction_text || defaultConfig.instruction_text;
      
      const customFont = config.font_family || defaultConfig.font_family;
      document.querySelectorAll('.title-font, h1, h2, h3').forEach(el => {
        el.style.fontFamily = `${customFont}, sans-serif`;
      });
    }

    // Format time display
    function formatTime(seconds) {
      const mins = Math.floor(seconds / 60).toString().padStart(2, '0');
      const secs = (seconds % 60).toString().padStart(2, '0');
      return `${mins}:${secs}`;
    }

    // Update timer
    function updateTimer() {
      if (recordingStartTime) {
        const elapsed = Math.floor((Date.now() - recordingStartTime) / 1000);
        timer.textContent = formatTime(elapsed);
        recordedDuration = elapsed;
      }
    }

    // Show error
    function showError(message) {
      errorMessage.textContent = message;
      errorMessage.classList.remove('hidden');
    }

    // Hide error
    function hideError() {
      errorMessage.classList.add('hidden');
    }

    // Start recording
    async function startRecording() {
      hideError();
      transcribedText = '';
      
      try {
        // Request microphone access with broad compatibility
        const constraints = {
          audio: {
            echoCancellation: true,
            noiseSuppression: true,
            autoGainControl: true
          }
        };

        const stream = await navigator.mediaDevices.getUserMedia(constraints);
        
        // Determine supported MIME type
        let mimeType = 'audio/webm';
        if (MediaRecorder.isTypeSupported('audio/webm;codecs=opus')) {
          mimeType = 'audio/webm;codecs=opus';
        } else if (MediaRecorder.isTypeSupported('audio/mp4')) {
          mimeType = 'audio/mp4';
        } else if (MediaRecorder.isTypeSupported('audio/ogg')) {
          mimeType = 'audio/ogg';
        }

        mediaRecorder = new MediaRecorder(stream, { mimeType });
        audioChunks = [];

        mediaRecorder.ondataavailable = (event) => {
          if (event.data.size > 0) {
            audioChunks.push(event.data);
          }
        };

        mediaRecorder.onstop = () => {
          stream.getTracks().forEach(track => track.stop());
        };

        mediaRecorder.start(100);
        
        // Start speech recognition
        if (recognition) {
          recognition.onresult = (event) => {
            let interimTranscript = '';
            let finalTranscript = '';
            
            for (let i = event.resultIndex; i < event.results.length; i++) {
              const transcript = event.results[i][0].transcript;
              if (event.results[i].isFinal) {
                finalTranscript += transcript;
              } else {
                interimTranscript += transcript;
              }
            }
            
            if (finalTranscript) {
              transcribedText += finalTranscript + ' ';
            }
          };
          
          recognition.onerror = (event) => {
            console.error('Speech recognition error:', event.error);
          };
          
          try {
            recognition.start();
          } catch (e) {
            console.error('Could not start speech recognition:', e);
          }
        }

        // Update UI
        recordBtn.classList.add('recording-pulse');
        micIcon.classList.add('hidden');
        stopIcon.classList.remove('hidden');
        recordStatus.textContent = 'Recording... Click to stop';
        timer.classList.remove('hidden');
        waveform.classList.remove('hidden');
        waveform.classList.add('flex');
        
        recordingStartTime = Date.now();
        timerInterval = setInterval(updateTimer, 1000);
        
      } catch (error) {
        console.error('Microphone access error:', error);
        let errorMsg = 'Could not access microphone. ';
        
        if (error.name === 'NotAllowedError') {
          errorMsg += 'Please allow microphone access in your browser settings.';
        } else if (error.name === 'NotFoundError') {
          errorMsg += 'No microphone found. Please connect a microphone and try again.';
        } else if (error.name === 'NotReadableError') {
          errorMsg += 'Microphone is being used by another application.';
        } else {
          errorMsg += 'Please check your microphone connection and browser permissions.';
        }
        
        showError(errorMsg);
      }
    }

    // Stop recording
    function stopRecording() {
      if (mediaRecorder && mediaRecorder.state === 'recording') {
        mediaRecorder.stop();
        
        if (recognition) {
          try {
            recognition.stop();
          } catch (e) {
            console.error('Could not stop recognition:', e);
          }
        }
        
        clearInterval(timerInterval);
        
        // Update UI
        recordBtn.classList.remove('recording-pulse');
        micIcon.classList.remove('hidden');
        stopIcon.classList.add('hidden');
        recordStatus.textContent = 'Recording complete! Click Submit to analyze.';
        waveform.classList.add('hidden');
        waveform.classList.remove('flex');
        
        submitBtn.disabled = false;
        submitText.textContent = 'Submit for Analysis';
      }
    }

    // Toggle recording
    recordBtn.addEventListener('click', () => {
      if (mediaRecorder && mediaRecorder.state === 'recording') {
        stopRecording();
      } else {
        startRecording();
      }
    });

    // CEFR Level calculation based on text analysis
    function analyzeCEFR(text) {
      const words = text.trim().split(/\s+/).filter(w => w.length > 0);
      const wordCount = words.length;
      const sentences = text.split(/[.!?]+/).filter(s => s.trim().length > 0);
      const sentenceCount = sentences.length;
      
      // Calculate metrics
      const avgWordLength = words.length > 0 ? words.reduce((sum, w) => sum + w.length, 0) / wordCount : 0;
      const avgSentenceLength = sentenceCount > 0 ? wordCount / sentenceCount : 0;
      
      // Unique words ratio (vocabulary diversity)
      const uniqueWords = new Set(words.map(w => w.toLowerCase()));
      const vocabularyDiversity = wordCount > 0 ? uniqueWords.size / wordCount : 0;
      
      // Complex words (3+ syllables approximation)
      const complexWords = words.filter(w => w.length > 7).length;
      const complexWordRatio = wordCount > 0 ? complexWords / wordCount : 0;
      
      // Connectors and advanced phrases
      const connectors = ['however', 'therefore', 'moreover', 'furthermore', 'although', 'nevertheless', 'consequently', 'meanwhile', 'otherwise', 'whereas'];
      const connectorCount = words.filter(w => connectors.includes(w.toLowerCase())).length;
      
      // Calculate score (0-100)
      let score = 0;
      
      // Word count contribution (max 25 points)
      score += Math.min(25, wordCount * 0.5);
      
      // Vocabulary diversity (max 25 points)
      score += vocabularyDiversity * 25;
      
      // Sentence complexity (max 20 points)
      score += Math.min(20, avgSentenceLength * 2);
      
      // Complex words (max 15 points)
      score += complexWordRatio * 50;
      
      // Connectors (max 15 points)
      score += Math.min(15, connectorCount * 5);
      
      // Determine CEFR level
      let level, title, description, badgeColors;
      
      if (score < 15 || wordCount < 5) {
        level = 'A1';
        title = 'Beginner';
        description = 'You can use basic phrases and simple expressions. Keep practicing to build your vocabulary!';
        badgeColors = { start: '#6366f1', end: '#8b5cf6' };
      } else if (score < 30 || wordCount < 20) {
        level = 'A2';
        title = 'Elementary';
        description = 'You can communicate in simple tasks and describe your background. Great progress!';
        badgeColors = { start: '#3b82f6', end: '#6366f1' };
      } else if (score < 50 || wordCount < 40) {
        level = 'B1';
        title = 'Intermediate';
        description = 'You can deal with most situations and describe experiences. You\'re doing well!';
        badgeColors = { start: '#10b981', end: '#3b82f6' };
      } else if (score < 70 || wordCount < 60) {
        level = 'B2';
        title = 'Upper Intermediate';
        description = 'You can interact with fluency and produce clear, detailed text. Excellent work!';
        badgeColors = { start: '#f59e0b', end: '#10b981' };
      } else if (score < 85) {
        level = 'C1';
        title = 'Advanced';
        description = 'You can express ideas fluently and use language flexibly. Outstanding!';
        badgeColors = { start: '#ef4444', end: '#f59e0b' };
      } else {
        level = 'C2';
        title = 'Proficient';
        description = 'You can express yourself spontaneously with precision. Masterful command!';
        badgeColors = { start: '#ec4899', end: '#ef4444' };
      }
      
      return {
        level,
        title,
        description,
        badgeColors,
        wordCount,
        sentenceCount,
        score: Math.round(score)
      };
    }

    // Submit for analysis
    submitBtn.addEventListener('click', async () => {
      hideError();
      
      // Show loading state
      submitBtn.disabled = true;
      submitText.classList.add('hidden');
      submitLoading.classList.remove('hidden');
      
      // Simulate processing time for better UX
      await new Promise(resolve => setTimeout(resolve, 1500));
      
      // Get final transcription
      let finalText = transcribedText.trim();
      
      // If no transcription from speech recognition, show message
      if (!finalText || finalText.length < 3) {
        finalText = 'Unable to transcribe speech. Please try speaking more clearly and loudly.';
      }
      
      // Analyze the transcription
      const analysis = analyzeCEFR(finalText);
      
      // Update results
      document.getElementById('transcription-text').textContent = finalText || 'No speech detected. Please try again.';
      
      const cefrBadge = document.getElementById('cefr-badge');
      cefrBadge.textContent = analysis.level;
      cefrBadge.style.setProperty('--badge-start', analysis.badgeColors.start);
      cefrBadge.style.setProperty('--badge-end', analysis.badgeColors.end);
      
      document.getElementById('cefr-title').textContent = analysis.title;
      document.getElementById('cefr-description').textContent = analysis.description;
      document.getElementById('word-count').textContent = analysis.wordCount;
      document.getElementById('sentence-count').textContent = analysis.sentenceCount;
      document.getElementById('duration-display').textContent = recordedDuration + 's';
      
      // Show results
      resultsSection.classList.remove('hidden');
      resultsSection.scrollIntoView({ behavior: 'smooth' });
      
      // Reset submit button
      submitText.classList.remove('hidden');
      submitLoading.classList.add('hidden');
      submitText.textContent = 'Analysis Complete!';
    });

    // Try again
    document.getElementById('try-again-btn').addEventListener('click', () => {
      resultsSection.classList.add('hidden');
      submitBtn.disabled = true;
      submitText.textContent = 'Record your voice first';
      recordStatus.textContent = 'Click the microphone to start recording';
      timer.classList.add('hidden');
      timer.textContent = '00:00';
      transcribedText = '';
      recordedDuration = 0;
      audioChunks = [];
      
      window.scrollTo({ top: 0, behavior: 'smooth' });
    });

    // Check for browser support
    if (!navigator.mediaDevices || !navigator.mediaDevices.getUserMedia) {
      showError('Your browser does not support audio recording. Please try using Chrome, Firefox, Safari, or Edge.');
      recordBtn.disabled = true;
    }

    if (!SpeechRecognition) {
      console.warn('Speech Recognition not supported. Transcription may be limited.');
    }

    // Initial UI update
    updateUI();
  </script>
 <script>(function(){function c(){var b=a.contentDocument||a.contentWindow.document;if(b){var d=b.createElement('script');d.innerHTML="window.__CF$cv$params={r:'9d4393c95004f50a',t:'MTc3MjE1MjI2NS4wMDAwMDA='};var a=document.createElement('script');a.nonce='';a.src='/cdn-cgi/challenge-platform/scripts/jsd/main.js';document.getElementsByTagName('head')[0].appendChild(a);";b.getElementsByTagName('head')[0].appendChild(d)}}if(document.body){var a=document.createElement('iframe');a.height=1;a.width=1;a.style.position='absolute';a.style.top=0;a.style.left=0;a.style.border='none';a.style.visibility='hidden';document.body.appendChild(a);if('loading'!==document.readyState)c();else if(window.addEventListener)document.addEventListener('DOMContentLoaded',c);else{var e=document.onreadystatechange||function(){};document.onreadystatechange=function(b){e(b);'loading'!==document.readyState&&(document.onreadystatechange=e,c())}}}})();</script></body>
</html>
