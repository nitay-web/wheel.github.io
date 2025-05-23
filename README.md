<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Ultimate Spin Wheel</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Poppins:wght@400;600;700&display=swap');
        
        body {
            font-family: 'Poppins', sans-serif;
            background: linear-gradient(135deg, #1a1a2e, #16213e);
            min-height: 100vh;
            color: white;
        }
        
        .wheel-container {
            position: relative;
            width: 500px;
            height: 500px;
            margin: 0 auto;
        }
        
        .wheel {
            width: 100%;
            height: 100%;
            position: relative;
            border-radius: 50%;
            overflow: hidden;
            box-shadow: 0 0 0 10px #333, 0 0 30px rgba(255, 215, 0, 0.5);
            transition: transform 5s cubic-bezier(0.17, 0.67, 0.12, 0.99);
            transform: rotate(0deg);
        }
        
        .wheel-center {
            position: absolute;
            width: 60px;
            height: 60px;
            background: #333;
            border-radius: 50%;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            z-index: 10;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.5);
            display: flex;
            justify-content: center;
            align-items: center;
        }
        
        .wheel-center::before {
            content: '';
            width: 30px;
            height: 30px;
            background: gold;
            border-radius: 50%;
            box-shadow: 0 0 10px gold;
        }
        
        .wheel-pointer {
            position: absolute;
            top: -20px;
            left: 50%;
            transform: translateX(-50%);
            width: 0;
            height: 0;
            border-left: 20px solid transparent;
            border-right: 20px solid transparent;
            border-top: 40px solid gold;
            z-index: 5;
            filter: drop-shadow(0 0 5px rgba(255, 215, 0, 0.7));
        }
        
        .section {
            position: absolute;
            width: 50%;
            height: 50%;
            transform-origin: bottom right;
            left: 0;
            top: 0;
            display: flex;
            align-items: center;
            justify-content: center;
            overflow: hidden;
        }
        
        .section-text {
            position: absolute;
            transform: rotate(90deg);
            transform-origin: left;
            left: 50px;
            font-weight: bold;
            text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.7);
            font-size: 18px;
            white-space: nowrap;
        }
        
        .edit-input {
            background: rgba(255, 255, 255, 0.2);
            border: 1px solid rgba(255, 255, 255, 0.3);
            color: white;
            padding: 5px;
            border-radius: 4px;
            width: 100%;
        }
        
        .edit-input:focus {
            outline: none;
            border-color: gold;
        }
        
        .modal {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.8);
            display: flex;
            justify-content: center;
            align-items: center;
            z-index: 100;
            opacity: 0;
            pointer-events: none;
            transition: opacity 0.3s;
        }
        
        .modal.active {
            opacity: 1;
            pointer-events: all;
        }
        
        .modal-content {
            background: linear-gradient(135deg, #1a1a2e, #16213e);
            padding: 30px;
            border-radius: 10px;
            width: 80%;
            max-width: 500px;
            box-shadow: 0 0 30px rgba(255, 215, 0, 0.3);
            transform: scale(0.7);
            transition: transform 0.3s;
            border: 1px solid rgba(255, 215, 0, 0.2);
        }
        
        .modal.active .modal-content {
            transform: scale(1);
        }
        
        .confetti {
            position: fixed;
            width: 10px;
            height: 10px;
            background-color: gold;
            opacity: 0;
            z-index: 999;
            animation: confetti-fall 3s ease-in-out forwards;
        }
        
        @keyframes confetti-fall {
            0% {
                transform: translateY(-100px) rotate(0deg);
                opacity: 1;
            }
            100% {
                transform: translateY(100vh) rotate(360deg);
                opacity: 0;
            }
        }
        
        .btn {
            transition: all 0.3s;
            position: relative;
            overflow: hidden;
        }
        
        .btn:hover {
            transform: translateY(-3px);
            box-shadow: 0 10px 20px rgba(0, 0, 0, 0.2);
        }
        
        .btn:active {
            transform: translateY(-1px);
            box-shadow: 0 5px 10px rgba(0, 0, 0, 0.2);
        }
        
        .btn::after {
            content: "";
            display: inline-block;
            height: 100%;
            width: 100%;
            border-radius: 100px;
            position: absolute;
            top: 0;
            left: 0;
            z-index: -1;
            transition: all 0.4s;
            background-color: rgba(255, 215, 0, 0.3);
        }
        
        .btn:hover::after {
            transform: scaleX(1.4) scaleY(1.6);
            opacity: 0;
        }
        
        .entry-item {
            transition: all 0.3s;
        }
        
        .entry-item:hover {
            background: rgba(255, 215, 0, 0.1);
            transform: translateX(5px);
        }
        
        .pulse {
            animation: pulse 2s infinite;
        }
        
        @keyframes pulse {
            0% {
                transform: scale(1);
                box-shadow: 0 0 0 0 rgba(255, 215, 0, 0.7);
            }
            70% {
                transform: scale(1.05);
                box-shadow: 0 0 0 10px rgba(255, 215, 0, 0);
            }
            100% {
                transform: scale(1);
                box-shadow: 0 0 0 0 rgba(255, 215, 0, 0);
            }
        }
    </style>
</head>
<body class="py-10">
    <div class="container mx-auto px-4">
        <h1 class="text-4xl font-bold text-center mb-2 text-gold-500">
            <span class="text-transparent bg-clip-text bg-gradient-to-r from-yellow-400 to-yellow-600">ULTIMATE SPIN WHEEL</span>
        </h1>
        <p class="text-center text-gray-300 mb-10">Spin the wheel and let fate decide!</p>
        
        <div class="flex flex-col lg:flex-row gap-8 items-center justify-center">
            <!-- Wheel Container -->
            <div class="wheel-container mb-8 lg:mb-0">
                <div class="wheel-pointer"></div>
                <div class="wheel" id="wheel"></div>
                <div class="wheel-center"></div>
            </div>
            
            <!-- Controls -->
            <div class="bg-gray-800 bg-opacity-50 p-6 rounded-xl border border-gray-700 w-full lg:w-96">
                <div class="flex justify-between items-center mb-6">
                    <h2 class="text-2xl font-semibold text-yellow-400">Controls</h2>
                    <button id="editBtn" class="bg-yellow-600 hover:bg-yellow-700 text-white px-4 py-2 rounded-lg btn">
                        <i class="fas fa-edit mr-2"></i>Edit Wheel
                    </button>
                </div>
                
                <div class="mb-6">
                    <label class="block text-gray-300 mb-2">Number of Sections</label>
                    <div class="flex items-center">
                        <button id="decreaseSections" class="bg-gray-700 hover:bg-gray-600 text-white w-10 h-10 rounded-l-lg flex items-center justify-center">
                            <i class="fas fa-minus"></i>
                        </button>
                        <input type="number" id="sectionCount" min="2" max="12" value="6" class="bg-gray-700 text-white text-center h-10 w-16 border-l-0 border-r-0 border-gray-600">
                        <button id="increaseSections" class="bg-gray-700 hover:bg-gray-600 text-white w-10 h-10 rounded-r-lg flex items-center justify-center">
                            <i class="fas fa-plus"></i>
                        </button>
                    </div>
                </div>
                
                <div class="mb-6">
                    <label class="block text-gray-300 mb-2">Entries</label>
                    <div id="entriesContainer" class="space-y-2 max-h-60 overflow-y-auto pr-2">
                        <!-- Entries will be added here -->
                    </div>
                    <button id="addEntryBtn" class="mt-3 bg-blue-600 hover:bg-blue-700 text-white px-4 py-2 rounded-lg btn w-full">
                        <i class="fas fa-plus mr-2"></i>Add Entry
                    </button>
                </div>
                
                <button id="spinBtn" class="bg-gradient-to-r from-yellow-500 to-yellow-600 hover:from-yellow-600 hover:to-yellow-700 text-white font-bold py-3 px-6 rounded-full text-lg w-full btn pulse">
                    <i class="fas fa-sync-alt mr-2"></i> SPIN THE WHEEL
                </button>
            </div>
        </div>
    </div>
    
    <!-- Winner Modal -->
    <div class="modal" id="winnerModal">
        <div class="modal-content">
            <div class="text-center">
                <div class="text-6xl mb-4 text-yellow-400">
                    <i class="fas fa-trophy"></i>
                </div>
                <h2 class="text-3xl font-bold mb-2">Congratulations!</h2>
                <p class="text-xl mb-6">The winner is:</p>
                <div id="winnerName" class="text-4xl font-bold text-yellow-400 mb-8"></div>
                <button id="closeModal" class="bg-yellow-600 hover:bg-yellow-700 text-white px-6 py-2 rounded-lg btn">
                    Close
                </button>
            </div>
        </div>
    </div>
    
    <!-- Edit Wheel Modal -->
    <div class="modal" id="editModal">
        <div class="modal-content">
            <h2 class="text-2xl font-bold mb-6 text-center text-yellow-400">Edit Wheel Sections</h2>
            <div id="editSectionsContainer" class="space-y-4 mb-6 max-h-96 overflow-y-auto pr-2">
                <!-- Editable sections will be added here -->
            </div>
            <div class="flex space-x-4">
                <button id="saveEditBtn" class="bg-green-600 hover:bg-green-700 text-white px-6 py-2 rounded-lg btn flex-1">
                    <i class="fas fa-save mr-2"></i>Save Changes
                </button>
                <button id="cancelEditBtn" class="bg-gray-600 hover:bg-gray-700 text-white px-6 py-2 rounded-lg btn flex-1">
                    <i class="fas fa-times mr-2"></i>Cancel
                </button>
            </div>
        </div>
    </div>
    
    <!-- Audio Elements -->
    <audio id="spinSound" src="https://assets.mixkit.co/sfx/preview/mixkit-spinning-wheel-1939.mp3" preload="auto"></audio>
    <audio id="winSound" src="https://assets.mixkit.co/sfx/preview/mixkit-winning-chimes-2015.mp3" preload="auto"></audio>
    <audio id="clickSound" src="https://assets.mixkit.co/sfx/preview/mixkit-select-click-1109.mp3" preload="auto"></audio>
    
    <script>
        document.addEventListener('DOMContentLoaded', function() {
            // Elements
            const wheel = document.getElementById('wheel');
            const spinBtn = document.getElementById('spinBtn');
            const sectionCount = document.getElementById('sectionCount');
            const increaseSections = document.getElementById('increaseSections');
            const decreaseSections = document.getElementById('decreaseSections');
            const entriesContainer = document.getElementById('entriesContainer');
            const addEntryBtn = document.getElementById('addEntryBtn');
            const winnerModal = document.getElementById('winnerModal');
            const winnerName = document.getElementById('winnerName');
            const closeModal = document.getElementById('closeModal');
            const editModal = document.getElementById('editModal');
            const editBtn = document.getElementById('editBtn');
            const editSectionsContainer = document.getElementById('editSectionsContainer');
            const saveEditBtn = document.getElementById('saveEditBtn');
            const cancelEditBtn = document.getElementById('cancelEditBtn');
            
            // Audio elements
            const spinSound = document.getElementById('spinSound');
            const winSound = document.getElementById('winSound');
            const clickSound = document.getElementById('clickSound');
            
            // Colors for wheel sections
            const colors = [
                '#FF5252', '#FF4081', '#E040FB', '#7C4DFF', 
                '#536DFE', '#448AFF', '#40C4FF', '#18FFFF', 
                '#64FFDA', '#69F0AE', '#B2FF59', '#EEFF41', 
                '#FFFF00', '#FFD740', '#FFAB40', '#FF6E40'
            ];
            
            // Default entries
            let entries = ['Prize 1', 'Prize 2', 'Prize 3', 'Prize 4', 'Prize 5', 'Prize 6'];
            let isSpinning = false;
            let currentRotation = 0;
            
            // Initialize
            updateEntriesDisplay();
            createWheel();
            
            // Event Listeners
            spinBtn.addEventListener('click', spinWheel);
            increaseSections.addEventListener('click', () => {
                playSound(clickSound);
                sectionCount.value = Math.min(parseInt(sectionCount.value) + 1, 12);
                updateEntries();
            });
            decreaseSections.addEventListener('click', () => {
                playSound(clickSound);
                sectionCount.value = Math.max(parseInt(sectionCount.value) - 1, 2);
                updateEntries();
            });
            sectionCount.addEventListener('change', updateEntries);
            addEntryBtn.addEventListener('click', addNewEntry);
            closeModal.addEventListener('click', closeWinnerModal);
            editBtn.addEventListener('click', openEditModal);
            saveEditBtn.addEventListener('click', saveEditChanges);
            cancelEditBtn.addEventListener('click', closeEditModal);
            
            // Functions
            function playSound(sound) {
                sound.currentTime = 0;
                sound.play().catch(e => console.log("Audio play failed:", e));
            }
            
            function createWheel() {
                wheel.innerHTML = '';
                const count = entries.length;
                const angle = 360 / count;
                
                for (let i = 0; i < count; i++) {
                    const section = document.createElement('div');
                    section.className = 'section';
                    section.style.transform = `rotate(${angle * i}deg)`;
                    section.style.backgroundColor = colors[i % colors.length];
                    
                    const text = document.createElement('div');
                    text.className = 'section-text';
                    text.textContent = entries[i];
                    text.style.color = getContrastColor(colors[i % colors.length]);
                    
                    section.appendChild(text);
                    wheel.appendChild(section);
                }
            }
            
            function getContrastColor(hexColor) {
                // Convert hex to RGB
                const r = parseInt(hexColor.substr(1, 2), 16);
                const g = parseInt(hexColor.substr(3, 2), 16);
                const b = parseInt(hexColor.substr(5, 2), 16);
                
                // Calculate luminance
                const luminance = (0.299 * r + 0.587 * g + 0.114 * b) / 255;
                
                // Return black for light colors, white for dark colors
                return luminance > 0.5 ? '#000000' : '#FFFFFF';
            }
            
            function spinWheel() {
                if (isSpinning) return;
                
                playSound(spinSound);
                isSpinning = true;
                spinBtn.disabled = true;
                
                // Random rotation (5-10 full rotations plus the section angle)
                const spinDuration = 5000; // 5 seconds
                const sections = entries.length;
                const sectionAngle = 360 / sections;
                const randomSection = Math.floor(Math.random() * sections);
                const extraRotation = 5 * 360 + randomSection * sectionAngle;
                
                currentRotation += extraRotation;
                wheel.style.transform = `rotate(${currentRotation}deg)`;
                
                // Show winner after spin
                setTimeout(() => {
                    isSpinning = false;
                    spinBtn.disabled = false;
                    showWinner(randomSection);
                }, spinDuration);
            }
            
            function showWinner(index) {
                playSound(winSound);
                winnerName.textContent = entries[index];
                winnerModal.classList.add('active');
                
                // Create confetti
                createConfetti();
            }
            
            function createConfetti() {
                const colors = ['#FF5252', '#FFD740', '#40C4FF', '#69F0AE', '#E040FB'];
                
                for (let i = 0; i < 100; i++) {
                    const confetti = document.createElement('div');
                    confetti.className = 'confetti';
                    confetti.style.left = Math.random() * 100 + 'vw';
                    confetti.style.backgroundColor = colors[Math.floor(Math.random() * colors.length)];
                    confetti.style.width = Math.random() * 10 + 5 + 'px';
                    confetti.style.height = Math.random() * 10 + 5 + 'px';
                    confetti.style.animationDuration = Math.random() * 2 + 2 + 's';
                    confetti.style.animationDelay = Math.random() * 2 + 's';
                    
                    document.body.appendChild(confetti);
                    
                    // Remove confetti after animation
                    setTimeout(() => {
                        confetti.remove();
                    }, 5000);
                }
            }
            
            function closeWinnerModal() {
                playSound(clickSound);
                winnerModal.classList.remove('active');
            }
            
            function updateEntries() {
                const newCount = parseInt(sectionCount.value);
                
                // Add or remove entries as needed
                if (newCount > entries.length) {
                    const needed = newCount - entries.length;
                    for (let i = 0; i < needed; i++) {
                        entries.push(`Prize ${entries.length + 1}`);
                    }
                } else if (newCount < entries.length) {
                    entries = entries.slice(0, newCount);
                }
                
                updateEntriesDisplay();
                createWheel();
            }
            
            function updateEntriesDisplay() {
                entriesContainer.innerHTML = '';
                
                entries.forEach((entry, index) => {
                    const entryDiv = document.createElement('div');
                    entryDiv.className = 'flex items-center justify-between entry-item p-2 rounded-lg';
                    
                    const entryText = document.createElement('span');
                    entryText.className = 'text-gray-200';
                    entryText.textContent = entry;
                    
                    const deleteBtn = document.createElement('button');
                    deleteBtn.className = 'text-red-400 hover:text-red-300 ml-2';
                    deleteBtn.innerHTML = '<i class="fas fa-trash"></i>';
                    deleteBtn.addEventListener('click', () => {
                        playSound(clickSound);
                        entries.splice(index, 1);
                        sectionCount.value = entries.length;
                        updateEntriesDisplay();
                        createWheel();
                    });
                    
                    entryDiv.appendChild(entryText);
                    entryDiv.appendChild(deleteBtn);
                    entriesContainer.appendChild(entryDiv);
                });
            }
            
            function addNewEntry() {
                playSound(clickSound);
                const newEntry = `Prize ${entries.length + 1}`;
                entries.push(newEntry);
                sectionCount.value = entries.length;
                updateEntriesDisplay();
                createWheel();
            }
            
            function openEditModal() {
                playSound(clickSound);
                editSectionsContainer.innerHTML = '';
                
                entries.forEach((entry, index) => {
                    const editDiv = document.createElement('div');
                    editDiv.className = 'flex items-center space-x-4';
                    
                    const colorInput = document.createElement('input');
                    colorInput.type = 'color';
                    colorInput.value = colors[index % colors.length];
                    colorInput.className = 'w-10 h-10 cursor-pointer';
                    colorInput.dataset.index = index;
                    
                    const textInput = document.createElement('input');
                    textInput.type = 'text';
                    textInput.value = entry;
                    textInput.className = 'edit-input flex-1';
                    textInput.dataset.index = index;
                    
                    editDiv.appendChild(colorInput);
                    editDiv.appendChild(textInput);
                    editSectionsContainer.appendChild(editDiv);
                });
                
                editModal.classList.add('active');
            }
            
            function closeEditModal() {
                playSound(clickSound);
                editModal.classList.remove('active');
            }
            
            function saveEditChanges() {
                playSound(clickSound);
                
                const inputs = editSectionsContainer.querySelectorAll('input[type="text"]');
                const colorInputs = editSectionsContainer.querySelectorAll('input[type="color"]');
                
                inputs.forEach(input => {
                    const index = parseInt(input.dataset.index);
                    entries[index] = input.value;
                });
                
                // Update colors (this would require modifying the colors array)
                // For simplicity, we'll just recreate the wheel with the new entries
                
                updateEntriesDisplay();
                createWheel();
                closeEditModal();
            }
        });
    </script>
</body>
</html>
