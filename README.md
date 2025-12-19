# <!doctype html>
<html lang="en" class="h-full">
 <head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Dedidox Softcraft Card Designer</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
  <script src="/_sdk/element_sdk.js"></script>
  <script src="/_sdk/data_sdk.js"></script>
  <style>
        body {
            box-sizing: border-box;
        }

        @import url('https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600;700;900&family=Playfair+Display:wght@400;700&display=swap');

        :root {
            --primary: #6366f1;
            --secondary: #8b5cf6;
            --accent: #ec4899;
            --dark: #1e1b4b;
            --light: #f8fafc;
        }

        body {
            font-family: 'Poppins', sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 50%, #f093fb 100%);
            margin: 0;
            padding: 0;
            height: 100%;
            overflow: hidden;
        }

        .main-wrapper {
            width: 100%;
            height: 100%;
            display: flex;
            align-items: center;
            justify-content: center;
            position: relative;
        }

        .page {
            position: absolute;
            width: 100%;
            height: 100%;
            display: flex;
            align-items: center;
            justify-content: center;
            opacity: 0;
            transform: scale(0.9);
            transition: all 0.6s cubic-bezier(0.4, 0, 0.2, 1);
            pointer-events: none;
        }

        .page.active {
            opacity: 1;
            transform: scale(1);
            pointer-events: all;
        }

        /* Page 1 - Welcome */
        .welcome-container {
            text-align: center;
            padding: 3rem;
            max-width: 700px;
        }

        .logo {
            font-size: 4rem;
            margin-bottom: 1rem;
            animation: float 3s ease-in-out infinite;
        }

        @keyframes float {
            0%, 100% { transform: translateY(0px); }
            50% { transform: translateY(-20px); }
        }

        .welcome-title {
            font-size: 3.5rem;
            font-weight: 900;
            background: linear-gradient(135deg, #fff 0%, #e0e7ff 100%);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
            margin-bottom: 1rem;
            line-height: 1.2;
        }

        .welcome-subtitle {
            font-size: 1.5rem;
            color: rgba(255, 255, 255, 0.9);
            margin-bottom: 3rem;
            font-weight: 300;
        }

        .btn {
            padding: 1rem 3rem;
            border: none;
            border-radius: 50px;
            cursor: pointer;
            font-weight: 600;
            font-size: 1.1rem;
            transition: all 0.3s;
            display: inline-flex;
            align-items: center;
            gap: 0.75rem;
            font-family: 'Poppins', sans-serif;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.3);
        }

        .btn-primary {
            background: linear-gradient(135deg, #fff 0%, #e0e7ff 100%);
            color: var(--dark);
        }

        .btn-primary:hover {
            transform: translateY(-3px);
            box-shadow: 0 15px 40px rgba(0, 0, 0, 0.4);
        }

        .btn-secondary {
            background: linear-gradient(135deg, var(--primary) 0%, var(--secondary) 100%);
            color: white;
        }

        .btn-secondary:hover {
            transform: translateY(-3px);
            box-shadow: 0 15px 40px rgba(99, 102, 241, 0.5);
        }

        .btn:disabled {
            opacity: 0.6;
            cursor: not-allowed;
            transform: none !important;
        }

        /* Page 2 - Form */
        .form-container {
            background: rgba(255, 255, 255, 0.95);
            backdrop-filter: blur(20px);
            padding: 3rem;
            border-radius: 30px;
            box-shadow: 0 20px 60px rgba(0, 0, 0, 0.3);
            max-width: 600px;
            width: 90%;
            max-height: 90%;
            overflow-y: auto;
        }

        .form-title {
            font-size: 2.5rem;
            font-weight: 700;
            color: var(--dark);
            margin-bottom: 0.5rem;
            text-align: center;
        }

        .form-subtitle {
            font-size: 1rem;
            color: #64748b;
            margin-bottom: 2rem;
            text-align: center;
        }

        .form-group {
            margin-bottom: 1.5rem;
        }

        .form-label {
            display: block;
            font-size: 0.95rem;
            font-weight: 600;
            color: var(--dark);
            margin-bottom: 0.5rem;
        }

        .form-input, .form-select, .form-textarea {
            width: 100%;
            padding: 1rem;
            border: 2px solid #e2e8f0;
            border-radius: 12px;
            outline: none;
            font-size: 1rem;
            transition: all 0.3s;
            font-family: 'Poppins', sans-serif;
            box-sizing: border-box;
        }

        .form-input:focus, .form-select:focus, .form-textarea:focus {
            border-color: var(--primary);
            box-shadow: 0 0 0 4px rgba(99, 102, 241, 0.1);
        }

        .form-textarea {
            resize: vertical;
            min-height: 120px;
        }

        /* Page 3 - Card Display */
        .card-display-container {
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 2rem;
            padding: 2rem;
            max-height: 100%;
            overflow-y: auto;
        }

        .final-card {
            width: 450px;
            height: 600px;
            background: linear-gradient(135deg, #1e1b4b 0%, #312e81 100%);
            border-radius: 30px;
            padding: 3rem;
            box-shadow: 0 30px 90px rgba(0, 0, 0, 0.5);
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            text-align: center;
            position: relative;
            overflow: hidden;
        }

        .final-card::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            height: 200px;
            background: radial-gradient(circle at 50% 0%, rgba(236, 72, 153, 0.3) 0%, transparent 70%);
            pointer-events: none;
        }

        .card-type-badge {
            position: absolute;
            top: 2rem;
            left: 50%;
            transform: translateX(-50%);
            background: linear-gradient(135deg, var(--accent) 0%, var(--secondary) 100%);
            color: white;
            padding: 0.5rem 1.5rem;
            border-radius: 50px;
            font-size: 0.85rem;
            font-weight: 600;
            text-transform: uppercase;
            letter-spacing: 1px;
        }

        .card-icon {
            font-size: 4rem;
            margin-bottom: 1.5rem;
            animation: pulse 2s ease-in-out infinite;
        }

        @keyframes pulse {
            0%, 100% { transform: scale(1); }
            50% { transform: scale(1.1); }
        }

        .card-recipient {
            font-size: 2rem;
            font-weight: 700;
            color: #fbbf24;
            margin-bottom: 2rem;
            font-family: 'Playfair Display', serif;
        }

        .card-message-display {
            font-size: 1.2rem;
            color: #e0e7ff;
            line-height: 1.8;
            margin-bottom: 2rem;
            font-style: italic;
            font-weight: 300;
        }

        .card-sender {
            font-size: 1.3rem;
            font-weight: 600;
            color: #a78bfa;
            margin-top: auto;
        }

        .action-buttons {
            display: flex;
            gap: 1rem;
            flex-wrap: wrap;
            justify-content: center;
        }

        .sparkle-bg {
            position: fixed;
            width: 4px;
            height: 4px;
            background: white;
            border-radius: 50%;
            pointer-events: none;
            animation: twinkle 2s ease-in-out infinite;
        }

        @keyframes twinkle {
            0%, 100% { opacity: 0.3; transform: scale(1); }
            50% { opacity: 1; transform: scale(1.5); }
        }

        .toast {
            position: fixed;
            bottom: 2rem;
            right: 2rem;
            background: rgba(30, 27, 75, 0.95);
            border: 2px solid var(--accent);
            color: white;
            padding: 1rem 1.5rem;
            border-radius: 12px;
            box-shadow: 0 4px 20px rgba(0, 0, 0, 0.5);
            z-index: 2000;
            animation: slideIn 0.3s ease-out;
            font-family: 'Poppins', sans-serif;
        }

        @keyframes slideIn {
            from {
                transform: translateX(400px);
                opacity: 0;
            }
            to {
                transform: translateX(0);
                opacity: 1;
            }
        }

        @media (max-width: 768px) {
            .welcome-title {
                font-size: 2.5rem;
            }

            .welcome-subtitle {
                font-size: 1.2rem;
            }

            .form-container {
                padding: 2rem;
            }

            .final-card {
                width: 320px;
                height: 480px;
                padding: 2rem;
            }

            .card-recipient {
                font-size: 1.5rem;
            }

            .card-message-display {
                font-size: 1rem;
            }
        }
    </style>
  <style>@view-transition { navigation: auto; }</style>
 </head>
 <body class="h-full">
  <div class="main-wrapper"><!-- Page 1: Welcome -->
   <div class="page active" id="page1">
    <div class="welcome-container">
     <div class="logo">
      🎨
     </div>
     <h1 class="welcome-title">Welcome to<br>
      Dedidox Softcraft<br>
      Card Designer</h1>
     <p class="welcome-subtitle">Create beautiful, personalized cards in seconds</p><button class="btn btn-primary" id="beginBtn"> Click to Begin ✨ </button>
    </div>
   </div><!-- Page 2: Form -->
   <div class="page" id="page2">
    <div class="form-container">
     <h2 class="form-title">Design Your Card</h2>
     <p class="form-subtitle">Fill in the details to create your personalized card</p>
     <form id="cardForm">
      <div class="form-group"><label class="form-label" for="cardType">Type of Card</label> <select class="form-select" id="cardType" required> <option value="">Select card type...</option> <option value="Birthday">🎂 Birthday</option> <option value="Holiday">🎄 Holiday</option> <option value="Thank You">🙏 Thank You</option> <option value="Congratulations">🎉 Congratulations</option> <option value="Get Well Soon">💐 Get Well Soon</option> <option value="Love">❤️ Love</option> <option value="Friendship">🤝 Friendship</option> <option value="Anniversary">💍 Anniversary</option> </select>
      </div>
      <div class="form-group"><label class="form-label" for="recipient">Name of Recipient</label> <input type="text" class="form-input" id="recipient" placeholder="Enter recipient's name" required>
      </div>
      <div class="form-group"><label class="form-label" for="message">Your Message</label> <textarea class="form-textarea" id="message" placeholder="Write your heartfelt message here..." required></textarea>
      </div>
      <div class="form-group"><label class="form-label" for="sender">Sender Name</label> <input type="text" class="form-input" id="sender" placeholder="Your name" required>
      </div><button type="submit" class="btn btn-secondary" style="width: 100%; justify-content: center; margin-top: 1rem;"> Design and Show Me My Card 🎨 </button>
     </form>
    </div>
   </div><!-- Page 3: Card Display -->
   <div class="page" id="page3">
    <div class="card-display-container">
     <div id="cardCaptureArea">
      <div class="final-card">
       <div class="card-type-badge" id="displayCardType">
        Birthday
       </div>
       <div class="card-icon" id="displayIcon">
        🎂
       </div>
       <div class="card-recipient" id="displayRecipient">
        Dear Friend
       </div>
       <div class="card-message-display" id="displayMessage">
        Your message will appear here
       </div>
       <div class="card-sender" id="displaySender">
        From: You
       </div>
      </div>
     </div>
     <div class="action-buttons"><button class="btn btn-primary" id="shareBtn"> 🔗 Share to Social Media </button> <button class="btn btn-secondary" id="createNewBtn"> ✨ Create New Card </button> <button class="btn btn-secondary" id="viewSavedBtn"> 📚 View My Saved Cards </button>
     </div><!-- Social Media Share Modal -->
     <div id="shareModal" style="display: none; position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0, 0, 0, 0.7); z-index: 1000; align-items: center; justify-content: center;">
      <div style="background: white; padding: 2rem; border-radius: 20px; max-width: 400px; width: 90%; box-shadow: 0 20px 60px rgba(0, 0, 0, 0.5);">
       <h3 style="font-size: 1.8rem; font-weight: 700; color: var(--dark); margin-bottom: 1.5rem; text-align: center;">Share Your Card</h3>
       <div style="display: flex; flex-direction: column; gap: 1rem;"><button class="btn btn-secondary" id="whatsappShare" style="width: 100%; justify-content: center; background: linear-gradient(135deg, #25D366 0%, #128C7E 100%);"> 📱 Share on WhatsApp </button> <button class="btn btn-secondary" id="tiktokShare" style="width: 100%; justify-content: center; background: linear-gradient(135deg, #000000 0%, #EE1D52 100%);"> 🎵 Share on TikTok </button> <button class="btn btn-secondary" id="emailShare" style="width: 100%; justify-content: center; background: linear-gradient(135deg, #EA4335 0%, #FBBC05 100%);"> 📧 Share via Email </button> <button class="btn btn-primary" id="closeModal" style="width: 100%; justify-content: center; margin-top: 0.5rem;"> ✕ Close </button>
       </div>
      </div>
     </div>
    </div>
   </div><!-- Page 4: Saved Cards -->
   <div class="page" id="page4">
    <div style="width: 90%; max-width: 1200px; height: 90%; background: rgba(255, 255, 255, 0.95); backdrop-filter: blur(20px); border-radius: 30px; padding: 3rem; overflow-y: auto; box-shadow: 0 20px 60px rgba(0, 0, 0, 0.3);">
     <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 2rem;">
      <h2 style="font-size: 2.5rem; font-weight: 700; color: var(--dark);">📚 Saved Cards</h2><button class="btn btn-secondary" id="backToCardBtn"> ← Back to Card </button>
     </div>
     <div id="savedCardsContainer" style="display: grid; grid-template-columns: repeat(auto-fill, minmax(280px, 1fr)); gap: 2rem;"><!-- Saved cards will be displayed here -->
     </div>
     <div id="emptyState" style="display: none; text-align: center; padding: 4rem 2rem; color: #64748b;">
      <div style="font-size: 4rem; margin-bottom: 1rem;">
       📭
      </div>
      <h3 style="font-size: 1.5rem; font-weight: 600; margin-bottom: 0.5rem;">No Saved Cards Yet</h3>
      <p style="font-size: 1rem;">Create and save your first card to see it here!</p>
     </div>
    </div>
   </div>
  </div>
  <script>
        let currentPage = 1;
        const cardData = {
            type: '',
            recipient: '',
            message: '',
            sender: ''
        };

        let savedCards = [];
        let currentRecordCount = 0;

        // Initialize Data SDK
        const dataHandler = {
            onDataChanged(data) {
                savedCards = data;
                currentRecordCount = data.length;
                renderSavedCards();
            }
        };

        async function initDataSDK() {
            if (window.dataSdk) {
                const result = await window.dataSdk.init(dataHandler);
                if (!result.isOk) {
                    console.error('Failed to initialize Data SDK');
                }
            }
        }

        initDataSDK();

        // Card type icons mapping
        const cardIcons = {
            'Birthday': '🎂',
            'Holiday': '🎄',
            'Thank You': '🙏',
            'Congratulations': '🎉',
            'Get Well Soon': '💐',
            'Love': '❤️',
            'Friendship': '🤝',
            'Anniversary': '💍'
        };

        // Create background sparkles
        function createSparkles() {
            for (let i = 0; i < 50; i++) {
                const sparkle = document.createElement('div');
                sparkle.className = 'sparkle-bg';
                sparkle.style.left = Math.random() * 100 + '%';
                sparkle.style.top = Math.random() * 100 + '%';
                sparkle.style.animationDelay = Math.random() * 2 + 's';
                document.body.appendChild(sparkle);
            }
        }

        function showPage(pageNumber) {
            document.querySelectorAll('.page').forEach(page => {
                page.classList.remove('active');
            });
            document.getElementById(`page${pageNumber}`).classList.add('active');
            currentPage = pageNumber;
        }

        // Begin button
        document.getElementById('beginBtn').addEventListener('click', () => {
            showPage(2);
        });

        // Form submission
        document.getElementById('cardForm').addEventListener('submit', async (e) => {
            e.preventDefault();

            cardData.type = document.getElementById('cardType').value;
            cardData.recipient = document.getElementById('recipient').value;
            cardData.message = document.getElementById('message').value;
            cardData.sender = document.getElementById('sender').value;

            // Update card display
            document.getElementById('displayCardType').textContent = cardData.type;
            document.getElementById('displayIcon').textContent = cardIcons[cardData.type] || '🎨';
            document.getElementById('displayRecipient').textContent = `Dear ${cardData.recipient}`;
            document.getElementById('displayMessage').textContent = cardData.message;
            document.getElementById('displaySender').textContent = `From: ${cardData.sender}`;

            // Automatically save to Canva Sheet
            if (currentRecordCount >= 999) {
                showToast('⚠️ Maximum limit of 999 cards reached. Please delete some cards first.');
                showPage(3);
                return;
            }

            if (window.dataSdk) {
                const result = await window.dataSdk.create({
                    cardType: cardData.type,
                    recipient: cardData.recipient,
                    message: cardData.message,
                    sender: cardData.sender,
                    createdAt: new Date().toISOString()
                });

                if (result.isOk) {
                    showToast('✅ Card saved to your Canva Sheet!');
                } else {
                    showToast('⚠️ Card created but not saved. Please try again.');
                }
            }

            showPage(3);
        });

        // Share button - Opens modal
        document.getElementById('shareBtn').addEventListener('click', () => {
            document.getElementById('shareModal').style.display = 'flex';
        });

        // Close modal
        document.getElementById('closeModal').addEventListener('click', () => {
            document.getElementById('shareModal').style.display = 'none';
        });

        // Close modal when clicking outside
        document.getElementById('shareModal').addEventListener('click', (e) => {
            if (e.target.id === 'shareModal') {
                document.getElementById('shareModal').style.display = 'none';
            }
        });

        // Helper function to capture card as image
        async function captureCardImage() {
            const cardElement = document.querySelector('.final-card');
            
            const canvas = await html2canvas(cardElement, {
                backgroundColor: '#1e1b4b',
                scale: 3,
                useCORS: true,
                logging: false,
                width: cardElement.offsetWidth,
                height: cardElement.offsetHeight,
                windowWidth: cardElement.offsetWidth,
                windowHeight: cardElement.offsetHeight
            });

            return new Promise((resolve) => {
                canvas.toBlob((blob) => {
                    resolve(blob);
                }, 'image/png', 1.0);
            });
        }

        // WhatsApp share - Downloads image and opens WhatsApp
        document.getElementById('whatsappShare').addEventListener('click', async () => {
            const btn = document.getElementById('whatsappShare');
            const originalHTML = btn.innerHTML;
            btn.disabled = true;
            btn.innerHTML = '📸 Preparing card...';

            try {
                const blob = await captureCardImage();
                
                if (!blob) {
                    showToast('❌ Failed to prepare card. Please try again.');
                    btn.disabled = false;
                    btn.innerHTML = originalHTML;
                    return;
                }

                // Try Web Share API first (works on mobile)
                if (navigator.share && navigator.canShare) {
                    const file = new File([blob], `dedidox-${cardData.type.toLowerCase().replace(/\s+/g, '-')}-card.png`, { type: 'image/png' });
                    const shareData = {
                        files: [file],
                        title: `${cardData.type} Card for ${cardData.recipient}`,
                        text: `🎨 ${cardData.type} card for ${cardData.recipient}\nFrom: ${cardData.sender}\n\nCreated with Dedidox Softcraft Card Designer`
                    };

                    if (navigator.canShare(shareData)) {
                        try {
                            await navigator.share(shareData);
                            showToast('✅ Card shared successfully!');
                            btn.disabled = false;
                            btn.innerHTML = originalHTML;
                            document.getElementById('shareModal').style.display = 'none';
                            return;
                        } catch (err) {
                            if (err.name === 'AbortError') {
                                btn.disabled = false;
                                btn.innerHTML = originalHTML;
                                return;
                            }
                        }
                    }
                }

                // Fallback: Download image and open WhatsApp with text
                const url = URL.createObjectURL(blob);
                const link = document.createElement('a');
                link.href = url;
                link.download = `dedidox-${cardData.type.toLowerCase().replace(/\s+/g, '-')}-card.png`;
                document.body.appendChild(link);
                link.click();
                document.body.removeChild(link);
                setTimeout(() => URL.revokeObjectURL(url), 100);

                // Open WhatsApp with message
                const text = encodeURIComponent(
                    `🎨 Check out this ${cardData.type} card I created!\n\n` +
                    `For: ${cardData.recipient}\n` +
                    `Message: "${cardData.message}"\n` +
                    `From: ${cardData.sender}\n\n` +
                    `Created with Dedidox Softcraft Card Designer\n\n` +
                    `📸 Card image downloaded - please attach it to your WhatsApp message!`
                );
                
                const whatsappUrl = `https://wa.me/?text=${text}`;
                window.open(whatsappUrl, '_blank');
                
                showToast('📱 Card downloaded! Opening WhatsApp - attach the image to your message.');
                
            } catch (error) {
                console.error('Share error:', error);
                showToast('❌ Failed to prepare card. Please try again.');
            }

            btn.disabled = false;
            btn.innerHTML = originalHTML;
            document.getElementById('shareModal').style.display = 'none';
        });

        // TikTok share (downloads image with instructions)
        document.getElementById('tiktokShare').addEventListener('click', async () => {
            const btn = document.getElementById('tiktokShare');
            const originalHTML = btn.innerHTML;
            btn.disabled = true;
            btn.innerHTML = '📸 Preparing card...';

            try {
                const blob = await captureCardImage();
                
                if (!blob) {
                    showToast('❌ Failed to prepare card. Please try again.');
                    btn.disabled = false;
                    btn.innerHTML = originalHTML;
                    return;
                }

                // Download the image
                const url = URL.createObjectURL(blob);
                const link = document.createElement('a');
                link.href = url;
                link.download = `dedidox-${cardData.type.toLowerCase().replace(/\s+/g, '-')}-card.png`;
                document.body.appendChild(link);
                link.click();
                document.body.removeChild(link);
                setTimeout(() => URL.revokeObjectURL(url), 100);

                showToast('🎵 Card downloaded! Open TikTok and upload the image from your gallery.');
                
            } catch (error) {
                console.error('TikTok share error:', error);
                showToast('❌ Failed to prepare card. Please try again.');
            }

            btn.disabled = false;
            btn.innerHTML = originalHTML;
            document.getElementById('shareModal').style.display = 'none';
        });

        // Email share
        document.getElementById('emailShare').addEventListener('click', () => {
            const subject = encodeURIComponent(`${cardData.type} Card for ${cardData.recipient}`);
            const body = encodeURIComponent(
                `Hi!\n\nI've created a special ${cardData.type} card for ${cardData.recipient}.\n\n` +
                `Message: "${cardData.message}"\n\n` +
                `From: ${cardData.sender}\n\n` +
                `Created with Dedidox Softcraft Card Designer\n\n` +
                `💡 Tip: Use the "💾 Save" button on the saved card to download the card image and attach it to your email!`
            );
            
            const mailtoLink = `mailto:?subject=${subject}&body=${body}`;
            window.open(mailtoLink, '_blank');
            
            showToast('📧 Opening email client...');
            document.getElementById('shareModal').style.display = 'none';
        });

        // Create new card button
        document.getElementById('createNewBtn').addEventListener('click', () => {
            document.getElementById('cardForm').reset();
            showPage(2);
        });

        // View Saved Cards button
        document.getElementById('viewSavedBtn').addEventListener('click', () => {
            renderSavedCards(); // Refresh the display
            showPage(4);
        });

        // Back to Card button
        document.getElementById('backToCardBtn').addEventListener('click', () => {
            showPage(3);
        });

        function showToast(message) {
            const toast = document.createElement('div');
            toast.className = 'toast';
            toast.textContent = message;
            document.body.appendChild(toast);

            setTimeout(() => {
                toast.style.animation = 'slideIn 0.3s ease-out reverse';
                setTimeout(() => toast.remove(), 300);
            }, 3000);
        }

        // Render saved cards
        function renderSavedCards() {
            const container = document.getElementById('savedCardsContainer');
            const emptyState = document.getElementById('emptyState');

            if (savedCards.length === 0) {
                container.style.display = 'none';
                emptyState.style.display = 'block';
                return;
            }

            container.style.display = 'grid';
            emptyState.style.display = 'none';

            // Create a map of existing cards
            const existingCards = new Map();
            container.querySelectorAll('[data-card-id]').forEach(el => {
                existingCards.set(el.dataset.cardId, el);
            });

            // Update or create cards
            savedCards.forEach(card => {
                const cardId = card.__backendId;
                
                if (existingCards.has(cardId)) {
                    // Card already exists, update if needed
                    existingCards.delete(cardId);
                } else {
                    // Create new card element
                    const cardEl = createSavedCardElement(card);
                    container.appendChild(cardEl);
                }
            });

            // Remove deleted cards
            existingCards.forEach(el => el.remove());
        }

        function createSavedCardElement(card) {
            const cardEl = document.createElement('div');
            cardEl.dataset.cardId = card.__backendId;
            cardEl.style.cssText = 'background: linear-gradient(135deg, #1e1b4b 0%, #312e81 100%); border-radius: 20px; padding: 2rem; box-shadow: 0 10px 30px rgba(0, 0, 0, 0.3); position: relative; display: flex; flex-direction: column; gap: 1rem; transition: transform 0.3s;';
            
            cardEl.innerHTML = `
                <div style="position: absolute; top: 1rem; right: 1rem;">
                    <button class="delete-card-btn" data-card-id="${card.__backendId}" style="background: rgba(239, 68, 68, 0.9); color: white; border: none; border-radius: 50%; width: 32px; height: 32px; cursor: pointer; font-size: 1.2rem; display: flex; align-items: center; justify-content: center; transition: all 0.3s;">
                        ×
                    </button>
                </div>
                <div style="text-align: center;">
                    <div style="background: linear-gradient(135deg, var(--accent) 0%, var(--secondary) 100%); color: white; padding: 0.4rem 1rem; border-radius: 50px; font-size: 0.75rem; font-weight: 600; text-transform: uppercase; display: inline-block; margin-bottom: 1rem;">
                        ${card.cardType}
                    </div>
                    <div style="font-size: 2rem; margin-bottom: 0.5rem;">
                        ${cardIcons[card.cardType] || '🎨'}
                    </div>
                    <div style="font-size: 1.3rem; font-weight: 700; color: #fbbf24; margin-bottom: 1rem; font-family: 'Playfair Display', serif;">
                        ${card.recipient}
                    </div>
                    <div style="font-size: 0.9rem; color: #e0e7ff; line-height: 1.6; margin-bottom: 1rem; font-style: italic; max-height: 100px; overflow: hidden; text-overflow: ellipsis;">
                        "${card.message}"
                    </div>
                    <div style="font-size: 1rem; font-weight: 600; color: #a78bfa;">
                        From: ${card.sender}
                    </div>
                    <div style="font-size: 0.75rem; color: #94a3b8; margin-top: 0.5rem;">
                        ${new Date(card.createdAt).toLocaleDateString()}
                    </div>
                </div>
                <div style="display: flex; gap: 0.5rem; margin-top: auto;">
                    <button class="view-card-btn" data-card-id="${card.__backendId}" style="background: linear-gradient(135deg, var(--primary) 0%, var(--secondary) 100%); color: white; border: none; border-radius: 12px; padding: 0.75rem; cursor: pointer; font-weight: 600; font-family: 'Poppins', sans-serif; transition: all 0.3s; flex: 1;">
                        👁️ View
                    </button>
                    <button class="save-card-image-btn" data-card-id="${card.__backendId}" style="background: linear-gradient(135deg, #10b981 0%, #059669 100%); color: white; border: none; border-radius: 12px; padding: 0.75rem; cursor: pointer; font-weight: 600; font-family: 'Poppins', sans-serif; transition: all 0.3s; flex: 1;">
                        💾 Save
                    </button>
                </div>
            `;

            // Add hover effect
            cardEl.addEventListener('mouseenter', () => {
                cardEl.style.transform = 'translateY(-5px)';
            });
            cardEl.addEventListener('mouseleave', () => {
                cardEl.style.transform = 'translateY(0)';
            });

            return cardEl;
        }

        // Event delegation for view, save, and delete buttons
        document.getElementById('savedCardsContainer').addEventListener('click', async (e) => {
            // Save card image button
            if (e.target.classList.contains('save-card-image-btn') || e.target.closest('.save-card-image-btn')) {
                const btn = e.target.classList.contains('save-card-image-btn') ? e.target : e.target.closest('.save-card-image-btn');
                const cardId = btn.dataset.cardId;
                const card = savedCards.find(c => c.__backendId === cardId);
                
                if (card) {
                    const originalHTML = btn.innerHTML;
                    btn.disabled = true;
                    btn.innerHTML = '💾 Saving...';

                    try {
                        // Temporarily load card data to display
                        const tempCardData = {
                            type: card.cardType,
                            recipient: card.recipient,
                            message: card.message,
                            sender: card.sender
                        };

                        // Update the card display temporarily (hidden)
                        const displayCard = document.querySelector('.final-card');
                        const originalDisplay = displayCard.style.display;
                        
                        document.getElementById('displayCardType').textContent = card.cardType;
                        document.getElementById('displayIcon').textContent = cardIcons[card.cardType] || '🎨';
                        document.getElementById('displayRecipient').textContent = `Dear ${card.recipient}`;
                        document.getElementById('displayMessage').textContent = card.message;
                        document.getElementById('displaySender').textContent = `From: ${card.sender}`;

                        // Capture the card
                        const blob = await captureCardImage();
                        
                        if (!blob) {
                            showToast('❌ Failed to save card. Please try again.');
                            btn.disabled = false;
                            btn.innerHTML = originalHTML;
                            return;
                        }

                        // Download the image
                        const url = URL.createObjectURL(blob);
                        const link = document.createElement('a');
                        link.href = url;
                        link.download = `dedidox-${card.cardType.toLowerCase().replace(/\s+/g, '-')}-card.png`;
                        document.body.appendChild(link);
                        link.click();
                        document.body.removeChild(link);
                        setTimeout(() => URL.revokeObjectURL(url), 100);

                        showToast(`✅ Card saved as ${link.download}!`);
                        btn.disabled = false;
                        btn.innerHTML = originalHTML;
                    } catch (error) {
                        console.error('Save error:', error);
                        showToast('❌ Failed to save card. Please try again.');
                        btn.disabled = false;
                        btn.innerHTML = originalHTML;
                    }
                }
                return;
            }

            // View card button
            if (e.target.classList.contains('view-card-btn') || e.target.closest('.view-card-btn')) {
                const btn = e.target.classList.contains('view-card-btn') ? e.target : e.target.closest('.view-card-btn');
                const cardId = btn.dataset.cardId;
                const card = savedCards.find(c => c.__backendId === cardId);
                
                if (card) {
                    // Load card data
                    cardData.type = card.cardType;
                    cardData.recipient = card.recipient;
                    cardData.message = card.message;
                    cardData.sender = card.sender;

                    // Update display
                    document.getElementById('displayCardType').textContent = card.cardType;
                    document.getElementById('displayIcon').textContent = cardIcons[card.cardType] || '🎨';
                    document.getElementById('displayRecipient').textContent = `Dear ${card.recipient}`;
                    document.getElementById('displayMessage').textContent = card.message;
                    document.getElementById('displaySender').textContent = `From: ${card.sender}`;

                    showPage(3);
                }
            }

            // Delete card button
            if (e.target.classList.contains('delete-card-btn') || e.target.closest('.delete-card-btn')) {
                const btn = e.target.classList.contains('delete-card-btn') ? e.target : e.target.closest('.delete-card-btn');
                const cardId = btn.dataset.cardId;
                const card = savedCards.find(c => c.__backendId === cardId);
                
                if (card) {
                    // Show inline confirmation
                    const originalHTML = btn.innerHTML;
                    btn.innerHTML = '✓';
                    btn.style.background = 'rgba(34, 197, 94, 0.9)';
                    btn.disabled = false;

                    const confirmDelete = async () => {
                        btn.disabled = true;
                        const result = await window.dataSdk.delete(card);
                        
                        if (result.isOk) {
                            showToast('✅ Card deleted successfully!');
                        } else {
                            showToast('❌ Failed to delete card. Please try again.');
                            btn.innerHTML = originalHTML;
                            btn.style.background = 'rgba(239, 68, 68, 0.9)';
                            btn.disabled = false;
                        }
                    };

                    const cancelDelete = () => {
                        btn.innerHTML = originalHTML;
                        btn.style.background = 'rgba(239, 68, 68, 0.9)';
                        btn.removeEventListener('click', confirmDelete);
                        document.removeEventListener('click', outsideClick);
                    };

                    const outsideClick = (event) => {
                        if (!btn.contains(event.target)) {
                            cancelDelete();
                        }
                    };

                    setTimeout(() => {
                        btn.addEventListener('click', confirmDelete, { once: true });
                        document.addEventListener('click', outsideClick, { once: true });
                    }, 100);
                }
            }
        });

        // Initialize
        createSparkles();
    </script>
 <script>(function(){function c(){var b=a.contentDocument||a.contentWindow.document;if(b){var d=b.createElement('script');d.innerHTML="window.__CF$cv$params={r:'9b05bae81523ef2f',t:'MTc2NjEzNTA0MS4wMDAwMDA='};var a=document.createElement('script');a.nonce='';a.src='/cdn-cgi/challenge-platform/scripts/jsd/main.js';document.getElementsByTagName('head')[0].appendChild(a);";b.getElementsByTagName('head')[0].appendChild(d)}}if(document.body){var a=document.createElement('iframe');a.height=1;a.width=1;a.style.position='absolute';a.style.top=0;a.style.left=0;a.style.border='none';a.style.visibility='hidden';document.body.appendChild(a);if('loading'!==document.readyState)c();else if(window.addEventListener)document.addEventListener('DOMContentLoaded',c);else{var e=document.onreadystatechange||function(){};document.onreadystatechange=function(b){e(b);'loading'!==document.readyState&&(document.onreadystatechange=e,c())}}}})();</script></body>
</html>

