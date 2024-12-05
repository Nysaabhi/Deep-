<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Deep Chatbot - On-Demand Services</title>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css" rel="stylesheet">
    <link href="https://cdnjs.cloudflare.com/ajax/libs/flatpickr/4.6.13/flatpickr.min.css" rel="stylesheet">
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;500;600&display=swap" rel="stylesheet">
    <style>
        :root { --primary-color: #FFD700; --secondary-color: #FDB931; --background-dark: #1a1a1f; --text-light: #ffffff; --text-dark: #000000; --accent-gradient: linear-gradient(135deg, var(--primary-color), var(--secondary-color)); --error-color: #ff4444; --success-color: #00C851; }
        
        * { box-sizing: border-box; margin: 0; padding: 0; }
        
        body { font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, sans-serif; background-color: var(--background-dark); color: var(--text-light); line-height: 1.6; min-height: 100vh; overflow-x: hidden; }
        
        .header { padding: 8px 16px; background: rgba(26, 26, 31, 0.95); position: fixed; width: 100%; top: 0; left: 0; z-index: 30; border-bottom: 2px solid var(--primary-color); backdrop-filter: blur(10px); height: 60px; }
        
        .header-content { max-width: 1400px; margin: 0 auto; display: flex; justify-content: space-between; align-items: center; height: 100%; }
        
        .logo { font-size: 1.8em; font-weight: 900; background: var(--accent-gradient); -webkit-background-clip: text; color: transparent; letter-spacing: 1.2px; }
        
        .wallet { padding: 10px 20px; background: var(--accent-gradient); border: none; border-radius: 50px; color: var(--text-dark); font-weight: 600; cursor: pointer; transition: all 0.3s ease; font-size: 0.95em; }
        
        .chatbot-container { position: fixed; top: 60px; left: 0; right: 0; bottom: 60px; background: rgba(26, 26, 31, 0.95); display: flex; flex-direction: column; }
        
        .chat-header { padding: 15px; background: linear-gradient(135deg, rgba(255, 215, 0, 0.1), rgba(253, 185, 49, 0.1)); border-bottom: 1px solid rgba(255, 215, 0, 0.2); display: flex; justify-content: space-between; align-items: center; }
        
        .chat-status { display: flex; align-items: center; color: var(--primary-color); font-weight: 600; }
        
        .status-dot { width: 8px; height: 8px; background: var(--primary-color); border-radius: 50%; margin-right: 8px; animation: pulse 2s infinite; }
        
        .chat-messages { flex: 1; overflow-y: auto; padding: 20px; scroll-behavior: smooth; }
        
        .message { display: flex; gap: 10px; max-width: 85%; margin-bottom: 16px; animation: messageSlide 0.3s ease-out; }
        
        .bot-message { align-self: flex-start; }
        
        .user-message { align-self: flex-end; flex-direction: row-reverse; }
        
        .message-avatar { width: 36px; height: 36px; min-width: 36px; min-height: 36px; background: rgba(255, 215, 0, 0.1); border-radius: 50%; display: flex; align-items: center; justify-content: center; color: var(--primary-color); flex-shrink: 0; }
        
        .message-content { background: rgba(255, 255, 255, 0.05); padding: 12px 16px; border-radius: 16px; color: var(--text-light); }
        
        .user-message .message-content { background: rgba(255, 215, 0, 0.1); }
        
        .chat-input-container { padding: 20px; background: rgba(26, 26, 31, 0.98); border-top: 1px solid rgba(255, 215, 0, 0.1); display: flex; gap: 12px; align-items: center; }
        
        #chatInput { flex: 1; background: rgba(255, 255, 255, 0.05); border: 1px solid rgba(255, 215, 0, 0.2); border-radius: 12px; padding: 20px 20px; color: var(--text-light); font-size: 0.95em; }
        
        .input-actions { display: flex; gap: 8px; }
        
        .input-actions button { padding: 20px 20px; background: var(--accent-gradient); border: none; border-radius: 8px; color: #ffffff; cursor: pointer; }
        
        .send-message i { font-size: 1.5em; }
        
        .alert { position: fixed; top: 20px; right: 20px; padding: 15px 20px; border-radius: 8px; color: var(--text-light); z-index: 1000; animation: slideIn 0.3s ease-out; }
        
        .alert-success { background: var(--success-color); }
        
        .alert-error { background: var(--error-color); }
        
        .bottom-nav { position: fixed; bottom: 0; left: 0; right: 0; height: 60px; background: rgba(26, 26, 31, 0.98); padding: 8px 0; border-top: 1px solid rgba(255, 215, 0, 0.2); }
        
        .nav-container { display: flex; justify-content: space-around; align-items: center; height: 100%; max-width: 600px; margin: 0 auto; }
        
        .nav-item { display: flex; flex-direction: column; align-items: center; text-decoration: none; color: #fff; transition: all 0.3s ease; padding: 5px; cursor: pointer; font-family: 'Poppins', sans-serif; }
        
        .nav-item i { color: #fff; font-size: 20px; margin-bottom: 4px; }
        
        .nav-item span { font-size: 12px; font-family: 'Poppins', sans-serif; font-weight: 400; }
        
        @keyframes pulse { 0% { opacity: 1; } 50% { opacity: 0.5; } 100% { opacity: 1; } }
        
        @keyframes messageSlide { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
        
        @keyframes slideIn { from { opacity: 0; transform: translateX(100px); } to { opacity: 1; transform: translateX(0); } }
        
        @media (max-width: 768px) { .message { max-width: 90%; } }
        
        body > h1:first-of-type:not(.heading) { display: none !important; }
        
        .markdown-body h1:first-child { display: none !important; }
        
        .position-relative h1:first-child { display: none !important; }
        
        .category-carousel { display: flex; width: 100%; overflow-x: auto; scroll-snap-type: x mandatory; scroll-behavior: smooth; -webkit-overflow-scrolling: touch; padding: 20px; gap: 15px; scrollbar-width: none; -ms-overflow-style: none; }
        
        .category-carousel::-webkit-scrollbar { display: none; }
        
        .category-card { flex: 0 0 calc(25% - 15px); max-width: 1400px; scroll-snap-align: start; border: 1px solid rgba(255, 215, 0, 0.2); background: rgba(255, 215, 0, 0.1); border-radius: 12px; padding: 20px; cursor: pointer; transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1); text-align: center; }
        
        .category-card:hover { transform: translateY(-2px); background: rgba(255, 215, 0, 0.2); box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1); }
        
        .category-icon { font-size: 2em; color: var(--primary-color); margin-bottom: 10px; }
        
        .page-grid { display: grid; grid-template-columns: repeat(4, 1fr); gap: 15px; padding: 20px; }
        
        .package-grid { display: none; flex-direction: column; gap: 15px; padding: 20px; }
        
        .package-card { background: rgba(255, 215, 0, 0.1); border-radius: 12px; padding: 20px; }
        
        .package-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 15px; }
        
        .package-title { font-size: 1.2em; font-weight: 600; color: var(--primary-color); }
        
        .package-price { font-size: 1.1em; color: var(--text-light); }
        
        .package-features { margin: 15px 0; }
        
        .package-actions { display: flex; gap: 10px; margin-top: 15px; }
        
        .action-button { flex: 1; padding: 10px; border: none; border-radius: 8px; cursor: pointer; font-weight: 600; transition: all 0.3s ease; }
        
        .book-button { background: var(--accent-gradient); color: var(--text-dark); }
        
        .call-button { background: rgba(255, 255, 255, 0.1); color: var(--text-light); }
        
        .info-button { background: none; border: none; color: var(--primary-color); cursor: pointer; padding: 0 5px; font-size: 1.1em; transition: color 0.3s ease; }
        
        .info-button:hover { color: #ffffff; }
        
        #packageInfoModal { display: none; position: fixed; top: 0; left: 0; width: 100%; height: 100%; z-index: 1000; }
        
        .modal-overlay { position: absolute; top: 0; left: 0; width: 100%; height: 100%; background-color: rgba(0, 0, 0, 0.5); display: flex; justify-content: center; align-items: center; padding: 20px; }
        
        .modal-content { background-color: #1a1a1f; border-radius: 12px; width: 100%; max-width: 600px; max-height: 90vh; overflow-y: auto; box-shadow: 0 4px 20px rgba(0, 0, 0, 0.15); }
        
        .modal-header { padding: 20px; border-bottom: 1px solid #eee; display: flex; justify-content: space-between; align-items: center; }
        
        .modal-header h2 { margin: 0; font-size: 1.5rem; color: #fffdfd; }
        
        .close-button { background: none; border: none; font-size: 1.5rem; cursor: pointer; color: #b1b1b1; padding: 5px; }
        
        .modal-body { padding: 20px; }
        
        .package-price-section { text-align: center; margin-bottom: 30px; }
        
        .price-tag { font-size: 2.5rem; font-weight: bold; color: #ffffff; }
        
        .price-duration { color: #ffffff; font-size: 0.9rem; }
        
        .package-details-section { margin-bottom: 30px; }
        
        .feature-list { list-style: none; padding: 0; margin: 15px 0; }
        
        .feature-list li { padding: 10px 0; display: flex; align-items: center; color: #ffffff; }
        
        .feature-list li i { color: #4CAF50; margin-right: 10px; }
        
        .benefits-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(150px, 1fr)); gap: 15px; margin-top: 15px; }
        
        .benefit-item { display: flex; align-items: center; gap: 10px; padding: 10px; background-color: #1a1a1f; border-radius: 8px; }
        
        .benefit-item i { color: #FFD700; font-size: 1.2rem; }
        
        .modal-footer { padding: 20px; border-top: 1px solid #eee; display: flex; gap: 10px; justify-content: flex-end; }
        
        .modal-footer .action-button { padding: 12px 24px; border-radius: 6px; font-weight: 600; cursor: pointer; transition: all 0.3s ease; }
        
        .modal-footer .book-button { background-color: #4CAF50; color: white; border: none; }
        
        .modal-footer .call-button { background-color: #f8f9fa; color: #333; border: 1px solid #ddd; }
        
        .modal-footer .action-button:hover { transform: translateY(-1px); box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1); }
        
        .booking-form { display: none; padding: 20px; background: rgba(26, 26, 31, 0.98); position: fixed; bottom: 60px; left: 0; right: 0; z-index: 20; border-top: 1px solid rgba(255, 215, 0, 0.2); animation: slideUp 0.3s ease-out; }
        
        .form-group { margin-bottom: 15px; }
        
        .form-group label { display: block; margin-bottom: 5px; color: var(--primary-color); }
        
        .form-group input { width: 100%; padding: 10px; background: rgba(255, 255, 255, 0.05); border: 1px solid rgba(255, 215, 0, 0.2); border-radius: 8px; color: var(--text-light); }
        
        .form-row { display: flex; justify-content: space-between; gap: 10px; }
        
        .form-row .form-group { flex: 1; }
        
        .submit-booking { width: 100%; padding: 12px; background: var(--accent-gradient); border: none; border-radius: 8px; color: var(--text-dark); font-weight: 600; cursor: pointer; }
        
        .menu-button { background: none; border: none; color: var(--primary-color); cursor: pointer; padding: 10px; }
        
        .menu-overlay { display: none; position: fixed; top: 60px; left: 0; right: 0; bottom: 60px; background: rgba(26, 26, 31, 0.98); z-index: 25; padding: 0; animation: fadeIn 0.3s ease-out; overflow-y: auto; }
        
        .menu-sections { padding: 20px; }
        
        .menu-section { margin-bottom: 24px; }
        
        .menu-section-title { color: var(--primary-color); font-size: 1.2em; font-weight: 600; margin-bottom: 12px; padding-bottom: 8px; border-bottom: 1px solid rgba(255, 215, 0, 0.2); }
        
        .menu-items { display: flex; flex-direction: column; gap: 8px; }
        
        .menu-item { padding: 12px 16px; background: rgba(255, 255, 255, 0.05); border-radius: 8px; color: var(--text-light); cursor: pointer; transition: all 0.3s ease; display: flex; justify-content: space-between; align-items: center; }
        
        .menu-item:hover { background: rgba(255, 215, 0, 0.1); }
        
        .menu-item-price { color: var(--primary-color); font-weight: 600; }
        
        .social-links { display: flex; gap: 12px; }
        
        .social-link { width: 40px; height: 40px; border-radius: 50%; background: rgba(255, 255, 255, 0.05); display: flex; align-items: center; justify-content: center; color: var(--text-light); text-decoration: none; transition: all 0.3s ease; }
        
        .social-link:hover { background: var(--primary-color); color: var(--text-dark); }
        
        .about-content { line-height: 1.6; color: #cccccc; }
        
        .menu-item { animation: slideIn 0.3s ease-out; animation-fill-mode: both; }
        
        .menu-item:nth-child(1) { animation-delay: 0.1s; }
        .menu-item:nth-child(2) { animation-delay: 0.2s; }
        .menu-item:nth-child(3) { animation-delay: 0.3s; }
        .menu-item:nth-child(4) { animation-delay: 0.4s; }
        .menu-item:nth-child(5) { animation-delay: 0.5s; }
        
        @keyframes slideIn { from { opacity: 0; transform: translateX(-20px); } to { opacity: 1; transform: translateX(0); } }
        
        @media (max-width: 768px) { 
            .category-card { flex: 0 0 50%; }
            .page-grid { grid-template-columns: repeat(2, 1fr); } 
        }
        
        .whatsapp-item { background-color: #25D366; color: white; border-radius: 8px; padding: 10px; text-align: center; }
        
        .whatsapp-item a i { color: white; }
        
        .whatsapp-item:hover { background-color: #1ebe57; color: white; }
        
        .confirmation-icon { font-size: 80px; color: #4CAF50; margin-bottom: 20px; }
        
        .confirmation-icon i { display: block; }
        
        .alert { position: fixed; top: 20px; left: 50%; transform: translateX(-50%); padding: 10px 20px; border-radius: 5px; z-index: 1100; }
        
        .alert-success { background-color: #4CAF50; color: white; }
        
        .alert-error { background-color: #f44336; color: white; }
        
        .location-links, .partner-links { display: flex; flex-direction: column; } .location-link, .partner-link { text-decoration: none; color: #ffffff; padding: 8px 10px; transition: all 0.3s ease; border-radius: 4px; } .location-link:hover, .partner-link:hover { background-color: #f0f0f0; color: #333; } .location-link i, .partner-link i { margin-right: 10px; color: #fff; } .location-link:hover i, .partner-link:hover i { color: #2c7cd1; }

                 </style>        
        </head>
        <body>
            <header class="header">
                <div class="header-content">
                    <a href="https://nysaabhi.github.io/Deepintellgence" class="logo">Deep</a>
                    <button class="wallet">
                        <i class="fas fa-wallet"></i>
                        Wallet
                    </button>
                </div>
            </header>
            
            <div class="chatbot-container">
                <div class="chat-header">
                    <div class="chat-status">
                        <span class="status-dot"></span>
                        Deep AI Assistant
                    </div>
                    <button class="menu-button" id="menuButton">
                        <i class="fas fa-bars"></i>
                    </button>
                </div>
        
                <div class="chat-messages" id="chatMessages">
                    <div class="category-grid" id="categoryGrid">
                        <!-- Categories will be dynamically populated -->
                    </div>
        
                    <div class="package-grid" id="packageGrid">
                        <!-- Packages will be dynamically populated -->
                    </div>
        
                    <div class="category-carousel" id="categoryCarousel">
                        <!-- Dynamically populated category cards -->
                    </div>
                
                    <div class="page-grid" id="pageGrid" style="display: none;">
                        <!-- Dynamically populated category pages -->
                    </div>
                
                <div class="booking-form" id="bookingForm">
                    <div class="form-group">
                        <label>Name</label>
                        <input type="text" id="nameInput" placeholder="Enter your name" />
                    </div>
                    <div class="form-group">
                        <label>Contact</label>
                        <input type="tel" id="contactInput" placeholder="Enter your contact number" />
                    </div>
                    <div class="form-group">
                        <label>Address</label>
                        <input type="text" id="addressInput" placeholder="Enter your address (optional)" />
                    </div>
                    <div class="form-group">
                        <label>Pincode</label>
                        <input type="text" id="pincodeInput" placeholder="Enter pincode (optional)" />
                    </div>
                    <div class="form-row">
                        <div class="form-group">
                            <label>Date</label>
                            <input type="text" id="dateInput" placeholder="Select date" />
                        </div>
                        <div class="form-group">
                            <label>Time Slot</label>
                            <input type="text" id="timeInput" placeholder="Select time slot" />
                        </div>
                    </div>
                                <button class="submit-booking" id="submitBooking">Book Now</button>
                </div>
            </div>
        
            
            <!-- Bottom Navigation -->
            <nav class="bottom-nav">
                <div class="nav-container">
                    <div class="nav-item" data-page="services">
                        <a href="https://example.com/services">
        <i class="fas fa-user-tie"></i>
                        </a>
                        <span>Services</span>
                    </div>
                    <div class="nav-item" data-page="reserve">
                        <a href="https://example.com/reserve">
                            <i class="fas fa-calendar-check"></i>
                        </a>
                        <span>Reserve</span>
                    </div>
                    <div class="nav-item whatsapp-item" data-page="chat">
                        <a href="https://wa.me/9111478356" target="_blank">
                            <i class="fas fa-message"></i>
                        </a>
                        <span>WhatsApp</span>
                    </div>
                    <div class="nav-item" data-page="slot">
                        <a href="https://example.com/slot">
                            <i class="fas fa-calendar-alt"></i>
                        </a>
                     <span>Slot</span>
                   </div>
                   <div class="nav-item" data-page="franchise">
                        <a href="https://example.com/franchise">
                            <i class="fas fa-store"></i>
                        </a>
                        <span>Franchise</span>
                    </div>
                </div>
            </nav>
            
            <div class="menu-overlay" id="menuOverlay">
                <div class="menu-sections">
                    <div class="menu-section">
                        <div class="menu-section-title">About Us</div>
                        <div class="about-content">
                            Deep Chatbot is your premier destination for on-demand services. We connect you with skilled professionals to meet all your service needs with just a few taps.
                        </div>
                    </div>
                    
                    <div class="menu-section">
                        <div class="menu-section-title">Our Services</div>
                        <div class="menu-items">
                            <div class="menu-item" data-category="cleaning">
                                <span>Home Cleaning</span>
                                <span class="menu-item-price">₹49/hr</span>
                            </div>
                            <div class="menu-item" data-category="plumbing">
                                <span>Plumbing Services</span>
                                <span class="menu-item-price">₹199/hr</span>
                            </div>
                            <div class="menu-item" data-category="electrical">
                                <span>Electrical Work</span>
                                <span class="menu-item-price">₹249/hr</span>
                            </div>
                            <div class="menu-item" data-category="painting">
                                <span>House Painting</span>
                                <span class="menu-item-price">₹99/hr</span>
                            </div>
                        </div>
                    </div>
                    
                    <div class="menu-section">
                        <div class="menu-section-title">Connect With Us</div>
                        <div class="social-links">
                            <a href="https://nysaabhi.github.io/chat" class="social-link">
                                <i class="fab fa-facebook-f"></i>
                            </a>
                            <a href="#" class="social-link">
                                <i class="fab fa-twitter"></i>
                            </a>
                            <a href="#" class="social-link">
                                <i class="fab fa-instagram"></i>
                            </a>
                            <a href="#" class="social-link">
                                <i class="fab fa-linkedin-in"></i>
                            </a>
                        </div>
                    </div>
                    
                    <div class="menu-section">
                        <div class="menu-section-title">Our Locations</div>
                        <div class="location-links">
                            <a href="location.html" class="location-link">
                                <i class="fas fa-map-marker-alt"></i> India
                            </a>
                        </div>
                    </div>
                    
                    <div class="menu-section">
                        <div class="menu-section-title">Our Partners</div>
                        <div class="partner-links">
                            <a href="https://www.example1.com" class="partner-link">
                                <i class="fas fa-handshake"></i> TechSolutions Inc.
                            </a>
                            <a href="https://www.example2.com" class="partner-link">
                                <i class="fas fa-handshake"></i> HomeServices Pro
                            </a>
                            <a href="https://www.example3.com" class="partner-link">
                                <i class="fas fa-handshake"></i> Quick Repairs Network
                            </a>
                            <a href="https://www.example4.com" class="partner-link">
                                <i class="fas fa-handshake"></i> Smart Home Innovations
                            </a>
                        </div>
                    </div>
                </div>
            </div>
                
            <div class="chat-input-container">
                <input type="text" id="chatInput" placeholder="Search for services or ask a question..." />
                <div class="input-actions">
                    <button class="send-message" id="sendButton">
                        <i class="fas fa-paper-plane"></i>
                    </button>
                </div>
            </div>
        
        
        
            <script src="https://cdnjs.cloudflare.com/ajax/libs/flatpickr/4.6.13/flatpickr.min.js"></script>
            <script>
                const categories = [{ id: 'home', name: 'Home', icon: 'fa-home', services: ['Plumbing', 'Electrical', 'Carpentry', 'Painting', 'Appliance Repair', 'AC/Heating Repair', 'Cleaning Services'] }, { id: 'education', name: 'Education', icon: 'fa-graduation-cap', services: ['Haircut', 'Manicure', 'Pedicure', 'Massage', 'Facial'] }, { id: 'healthcare', name: 'Healthcare', icon: 'fa-heartbeat', services: ['TV Repair', 'Phone Repair', 'Computer Repair', 'Furniture Repair'] }, { id: 'hospitality', name: 'Hospitality', icon: 'fa-bed', services: ['Doctor Visit', 'Nursing', 'Physiotherapy', 'Lab Tests'] }, { id: 'retail', name: 'Retail', icon: 'fa-shopping-cart', services: ['Tutoring', 'Language Classes', 'Test Prep', 'Skills Training'] }, { id: 'manufacturing', name: 'Manufacturing', icon: 'fa-industry', services: ['Babysitting', 'Pet Grooming', 'Fitness Trainer'] }, { id: 'co-working-space', name: 'Co-Working', icon: 'fa-users', services: ['Plumbing', 'Electrical', 'Carpentry', 'Painting', 'Appliance Repair', 'AC/Heating Repair', 'Cleaning Services'] }, { id: 'corporate-office', name: 'Corporate', icon: 'fa-briefcase', services: ['Haircut', 'Manicure', 'Pedicure', 'Massage', 'Facial'] }, { id: 'warehouse', name: 'Warehouse', icon: 'fa-boxes', services: ['TV Repair', 'Phone Repair', 'Computer Repair', 'Furniture Repair'] }, { id: 'logistics', name: 'Logistics', icon: 'fa-truck', services: ['Doctor Visit', 'Nursing', 'Physiotherapy', 'Lab Tests'] }, { id: 'residential', name: 'Residential', icon: 'fa-home', services: ['Tutoring', 'Language Classes', 'Test Prep', 'Skills Training'] }, { id: 'fitness', name: 'Fitness', icon: 'fa-dumbbell', services: ['Babysitting', 'Pet Grooming', 'Fitness Trainer'] }, { id: 'banks', name: 'Banks', icon: 'fa-university', services: ['Plumbing', 'Electrical', 'Carpentry', 'Painting', 'Appliance Repair', 'AC/Heating Repair', 'Cleaning Services'] }, { id: 'restaurants', name: 'Restaurants', icon: 'fa-utensils', services: ['Haircut', 'Manicure', 'Pedicure', 'Massage', 'Facial'] }, { id: 'energy-utilities', name: 'Energy', icon: 'fa-bolt', services: ['TV Repair', 'Phone Repair', 'Computer Repair', 'Furniture Repair'] }, { id: 'entertainment', name: 'Entertainment', icon: 'fa-film', services: ['Doctor Visit', 'Nursing', 'Physiotherapy', 'Lab Tests'] }, { id: 'agriculture', name: 'Agriculture', icon: 'fa-seedling', services: ['Tutoring', 'Language Classes', 'Test Prep', 'Skills Training'] }, { id: 'government', name: 'Government', icon: 'fa-landmark', services: ['Babysitting', 'Pet Grooming', 'Fitness Trainer'] }, { id: 'construction', name: 'Construction', icon: 'fa-hard-hat', services: ['Plumbing', 'Electrical', 'Carpentry', 'Painting', 'Appliance Repair', 'AC/Heating Repair', 'Cleaning Services'] }, { id: 'shop', name: 'Shop', icon: 'fa-store', services: ['Haircut', 'Manicure', 'Pedicure', 'Massage', 'Facial'] }, { id: 'banquet-hall', name: 'Banquet Hall', icon: 'fa-building', services: ['TV Repair', 'Phone Repair', 'Computer Repair', 'Furniture Repair'] }, { id: 'occasion', name: 'Occasion', icon: 'fa-calendar-alt', services: ['Tutoring', 'Language Classes', 'Test Prep', 'Skills Training'] }];
                        
                const servicePricing = { 'Electrical': { 'switch installation': { price: 49, unit: 'per switch', description: 'Installation of standard electrical switches', keywords: ['switch', 'switches', 'light switch', 'power switch', 'switch installation'] }, 'fan installation': { price: 299, unit: 'per fan', description: 'Ceiling or wall fan installation', keywords: ['fan', 'ceiling fan', 'wall fan', 'fan fitting', 'fan installation'] }, 'wiring': { price: 799, unit: 'per room', description: 'Complete electrical wiring service', keywords: ['wire', 'wiring', 'electrical wiring', 'rewiring', 'wire installation'] } }, 'Plumbing': { 'tap installation': { price: 199, unit: 'per tap', description: 'Installation of new taps or faucets', keywords: ['tap', 'faucet', 'tap fitting', 'tap installation', 'faucet installation'] }, 'pipe repair': { price: 349, unit: 'basic service', description: 'Repair of leaking or damaged pipes', keywords: ['pipe', 'leaking pipe', 'pipe repair', 'pipe fixing', 'pipe leak'] }, 'drainage': { price: 499, unit: 'per service', description: 'Drainage cleaning and repair', keywords: ['drain', 'drainage', 'blocked drain', 'drain cleaning', 'drainage system'] } }, 'Carpentry': { 'furniture repair': { price: 299, unit: 'basic service', description: 'Basic furniture repair and fixing', keywords: ['furniture', 'chair', 'table', 'furniture repair', 'wood repair'] }, 'door installation': { price: 899, unit: 'per door', description: 'New door installation service', keywords: ['door', 'door fitting', 'door installation', 'new door'] } } };
                
                        const servicePackages = { 'Plumbing': { silver: { title: 'Basic Plumbing', price: '₹49', features: ['Basic pipe repair', 'Leak detection', '1 Hour service', 'Standard tools'] }, gold: { title: 'Advanced Plumbing', price: '₹99', features: ['Complex repairs', 'Drain cleaning', '2 Hour service', 'Professional tools', '24/7 support'] }, platinum: { title: 'Premium Plumbing', price: '₹149', features: ['Complete plumbing solutions', 'Emergency service', 'Unlimited duration', 'Premium tools', 'Priority support'] } }, 'default': { silver: { title: 'Silver Package', price: '₹49', features: ['Basic service', '1 Hour duration', 'Standard tools', 'Phone support'] }, gold: { title: 'Gold Package', price: '₹99', features: ['Premium service', '2 Hour duration', 'Professional tools', '24/7 support'] }, platinum: { title: 'Platinum Package', price: '₹149', features: ['VIP service', 'Unlimited duration', 'Premium tools', 'Priority support'] } } };
                
                        const searchConfig = { services: { 'plumbing': { keywords: ['pipe', 'leak', 'water', 'tap', 'faucet', 'drain', 'bathroom', 'sink', 'toilet', 'shower'], intents: ['water not coming', 'need plumber'] }, 'appliance repair': { keywords: ['refrigerator', 'fridge', 'washing', 'machine', 'dishwasher', 'microwave', 'oven', 'appliance'], intents: ['fridge not working', 'fix my appliance'] } }, priceRelatedKeywords: ['price', 'cost', 'charge', 'fee', 'rate', 'pricing', 'how much', 'what is the price', 'what is the cost'], qaDatabase: { 'how do i book a service': { answer: 'To book a service, click on the category, select a service, choose a package, and fill out the booking form. You can also call us directly at +91 9669181789 for immediate assistance.', category: 'Booking' }, 'what services do you offer': { answer: 'We offer a wide range of home and occasion services including Plumbing, Electrical, Carpentry, Painting, Appliance Repair, AC/Heating Repair, Cleaning Services, and Skills Training.', category: 'Services' } } };
                        
                function displayWelcomeMessage() { const chatMessages = document.getElementById('chatMessages'); const welcomeMessageDiv = document.createElement('div'); welcomeMessageDiv.id = 'welcomeMessage'; welcomeMessageDiv.className = 'message bot-message welcome-message'; welcomeMessageDiv.innerHTML = `<div class="message-avatar"><i class="fas fa-robot"></i></div><div class="message-content"><p>Hello! I'm your On-Demand services assistant. How can I help you today?</p></div>`; chatMessages.appendChild(welcomeMessageDiv); chatMessages.scrollTop = chatMessages.scrollHeight; } document.addEventListener('DOMContentLoaded', function() { initializeCategoryCarousel(); initializeDateTimePickers(); setupEventListeners(); initializeSearchFunctionality(); displayWelcomeMessage(); }); function initializeSearchFunctionality() { const chatInput = document.getElementById('chatInput'); const sendButton = document.getElementById('sendButton'); sendButton.addEventListener('click', handleSearch); chatInput.addEventListener('keypress', function(e) { if (e.key === 'Enter') { handleSearch(); } }); } function isPriceQuery(query) { return searchConfig.priceRelatedKeywords.some(keyword => query.toLowerCase().includes(keyword.toLowerCase())); } function findServicePricing(query) { query = query.toLowerCase(); for (const [category, services] of Object.entries(servicePricing)) { for (const [service, details] of Object.entries(services)) { if (details.keywords.some(keyword => query.includes(keyword.toLowerCase()))) { return { category, service, ...details }; } } } return null; } function processSearch(query) { query = query.toLowerCase().trim(); const results = { services: new Set(), qaAnswers: [], pricing: null }; if (isPriceQuery(query)) { const pricingInfo = findServicePricing(query); if (pricingInfo) { results.pricing = pricingInfo; results.services.add(pricingInfo.category); return results; } } const qaMatch = Object.entries(searchConfig.qaDatabase).find(([question, data]) => calculateSimilarity(query, question.toLowerCase()) > 0.7); if (qaMatch) { results.qaAnswers.push({ question: qaMatch[0], answer: qaMatch[1].answer, category: qaMatch[1].category }); return results; } Object.keys(searchConfig.services).forEach(service => { if (service.toLowerCase().includes(query)) { results.services.add(service); } }); Object.entries(searchConfig.services).forEach(([service, config]) => { if (config.keywords.some(keyword => query.includes(keyword.toLowerCase()))) { results.services.add(service); } }); Object.entries(searchConfig.services).forEach(([service, config]) => { if (config.intents && config.intents.some(intent => calculateSimilarity(query, intent.toLowerCase()) > 0.6)) { results.services.add(service); } }); return results; } function calculateSimilarity(str1, str2) { const set1 = new Set(str1.split(/\s+/)); const set2 = new Set(str2.split(/\s+/)); const intersection = new Set([...set1].filter(x => set2.has(x))); const union = new Set([...set1, ...set2]); return intersection.size / union.size; } function displayBotMessage(message) { const chatMessages = document.getElementById('chatMessages'); const messageDiv = document.createElement('div'); messageDiv.className = 'message bot-message'; messageDiv.innerHTML = `<div class="message-avatar"><i class="fas fa-robot"></i></div><div class="message-content"><p>${message}</p></div>`; chatMessages.appendChild(messageDiv); chatMessages.scrollTop = chatMessages.scrollHeight; } function displayUserMessage(message) { const chatMessages = document.getElementById('chatMessages'); const messageDiv = document.createElement('div'); messageDiv.className = 'message user-message'; messageDiv.innerHTML = `<div class="message-content"><p>${message}</p></div>`; chatMessages.appendChild(messageDiv); chatMessages.scrollTop = chatMessages.scrollHeight; } function handleSearch() { const chatInput = document.getElementById('chatInput'); const query = chatInput.value.trim(); const welcomeMessage = document.getElementById('welcomeMessage'); if (welcomeMessage) { welcomeMessage.remove(); } if (query) { displayUserMessage(query); const searchResults = processSearch(query); if (searchResults.pricing) { const pricing = searchResults.pricing; const message = `<div class="pricing-response"><strong>${pricing.service}</strong> (${pricing.category})<br>Price: ₹${pricing.price} ${pricing.unit}<br>${pricing.description}<br><button class="action-button book-button" onclick="showBookingForm('${pricing.category.toLowerCase()}', '${pricing.service}', 'silver')">Book Now</button></div>`; displayBotMessage(message); } else if (searchResults.qaAnswers.length > 0) { searchResults.qaAnswers.forEach(qa => { displayBotMessage(`<strong>${qa.category} Information:</strong> ${qa.answer}`); }); } else if (searchResults.services.size > 0) { const serviceArray = Array.from(searchResults.services); displayBotMessage(`I found these services matching your request: ${serviceArray.join(', ')}`); serviceArray.forEach(service => { const category = categories.find(cat => cat.services.some(s => s.toLowerCase() === service.toLowerCase())); if (category) { showServices(category.id, service); } }); } else { displayBotMessage("I couldn't find specific pricing for that service. Here are our popular services:"); document.getElementById('categoryCarousel').style.display = 'flex'; } chatInput.value = ''; } }
                        
                function initializeSearchFunctionality() { const chatInput = document.getElementById('chatInput'); const sendButton = document.getElementById('sendButton'); sendButton.addEventListener('click', handleSearch); chatInput.addEventListener('keypress', function(e) { if (e.key === 'Enter') { handleSearch(); } }); } function addQAEntry(question, answer, category = 'General') { searchConfig.qaDatabase[question.toLowerCase()] = { answer, category }; } function initializeCategoryCarousel() { const categoryCarousel = document.getElementById('categoryCarousel'); categoryCarousel.innerHTML = ''; categories.forEach(category => { const categoryCard = document.createElement('div'); categoryCard.className = 'category-card'; categoryCard.innerHTML = `<div class="category-icon"><i class="fas ${category.icon}"></i></div><div class="category-name">${category.name}</div>`; categoryCard.addEventListener('click', () => showCategoryPage(category)); categoryCarousel.appendChild(categoryCard); }); } const serviceIcons = { 'Plumbing': 'fa-faucet', 'Electrical': 'fa-bolt', 'Carpentry': 'fa-hammer', 'Painting': 'fa-paint-roller', 'Appliance Repair': 'fa-tv', 'AC/Heating Repair': 'fa-wind', 'Cleaning Services': 'fa-broom', 'Skills Training': 'fa-chalkboard-teacher' }; function showCategoryPage(category) { const pageGrid = document.getElementById('pageGrid'); pageGrid.style.display = 'grid'; pageGrid.innerHTML = ''; category.services.forEach(service => { const serviceCard = document.createElement('div'); serviceCard.className = 'category-card'; const iconClass = serviceIcons[service] || 'fa-tools'; serviceCard.innerHTML = `<div class="category-icon"><i class="fas ${iconClass}"></i></div><div class="category-name">${service}</div>`; serviceCard.addEventListener('click', () => showServices(category.id, service)); pageGrid.appendChild(serviceCard); }); document.getElementById('categoryCarousel').style.display = 'none'; document.getElementById('packageGrid').style.display = 'none'; } function showServices(categoryId, serviceName) { const packages = servicePackages[serviceName] || servicePackages['default']; const packageGrid = document.getElementById('packageGrid'); packageGrid.innerHTML = ''; const serviceTitle = document.createElement('div'); serviceTitle.className = 'package-header'; serviceTitle.innerHTML = `<h2 class="package-title">${serviceName} Packages</h2>`; packageGrid.appendChild(serviceTitle); Object.entries(packages).forEach(([type, package]) => { const packageCard = document.createElement('div'); packageCard.className = 'package-card'; packageCard.innerHTML = `<div class="package-header"><div class="package-title">${package.title}</div><div class="package-price">${package.price}<button class="info-button" onclick="showPackageInfo('${serviceName}', '${type}')"><i class="fas fa-info-circle"></i></button></div></div><div class="package-features">${package.features.map(feature => `<div>• ${feature}</div>`).join('')}</div><div class="package-actions"><button class="action-button book-button" onclick="showBookingForm('${categoryId}', '${serviceName}', '${type}')">Book Now</button><button class="action-button call-button" onclick="initiateCall()"><i class="fas fa-phone"></i> Call</button></div>`; packageGrid.appendChild(packageCard); }); packageGrid.style.display = 'flex'; document.getElementById('categoryCarousel').style.display = 'none'; document.getElementById('pageGrid').style.display = 'none'; } function showPackageInfo(serviceName, packageType) { const packages = servicePackages[serviceName] || servicePackages['default']; const package = packages[packageType]; let modalContainer = document.getElementById('packageInfoModal'); if (!modalContainer) { modalContainer = document.createElement('div'); modalContainer.id = 'packageInfoModal'; document.body.appendChild(modalContainer); } modalContainer.innerHTML = `<div class="modal-overlay" onclick="closePackageInfo()"><div class="modal-content" onclick="event.stopPropagation()"><div class="modal-header"><h2>${package.title}</h2><button class="close-button" onclick="closePackageInfo()"><i class="fas fa-times"></i></button></div><div class="modal-body"><div class="package-price-section"><div class="price-tag">${package.price}</div><div class="price-duration">per service</div></div><div class="package-details-section"><h3>Package Features</h3><ul class="feature-list">${package.features.map(feature => `<li><i class="fas fa-check"></i> ${feature}</li>`).join('')}</ul></div><div class="package-benefits-section"><h3>Additional Benefits</h3><div class="benefits-grid">${getBenefitsByPackageType(packageType)}</div></div></div><div class="modal-footer"><button class="action-button book-button" onclick="showBookingForm('${serviceName}', '${packageType}'); closePackageInfo();">Book Now</button><button class="action-button call-button" onclick="initiateCall()"><i class="fas fa-phone"></i> Call for Inquiry</button></div></div></div>`; modalContainer.style.display = 'block'; document.body.style.overflow = 'hidden'; }
                        
                function closePackageInfo() { const modalContainer = document.getElementById('packageInfoModal'); if (modalContainer) { modalContainer.style.display = 'none'; document.body.style.overflow = 'auto'; } } function getBenefitsByPackageType(packageType) { const benefits = { silver: [ { icon: 'fa-clock', text: 'Standard Response Time' }, { icon: 'fa-tools', text: 'Basic Tools Included' }, { icon: 'fa-phone', text: 'Phone Support' } ], gold: [ { icon: 'fa-clock', text: 'Priority Response' }, { icon: 'fa-tools', text: 'Professional Tools' }, { icon: 'fa-headset', text: '24/7 Support' }, { icon: 'fa-percent', text: '10% Off Next Booking' } ], platinum: [ { icon: 'fa-clock', text: 'VIP Response' }, { icon: 'fa-tools', text: 'Premium Tools' }, { icon: 'fa-headset', text: 'Dedicated Support' }, { icon: 'fa-percent', text: '20% Off Next Booking' }, { icon: 'fa-shield-alt', text: 'Extended Warranty' } ] }; return benefits[packageType].map(benefit => `<div class="benefit-item"><i class="fas ${benefit.icon}"></i><span>${benefit.text}</span></div>`).join(''); } function showBookingForm(categoryId, serviceName, packageType) { const bookingForm = document.getElementById('bookingForm'); bookingForm.style.display = 'block'; bookingForm.dataset.categoryId = categoryId; bookingForm.dataset.serviceName = serviceName; bookingForm.dataset.packageType = packageType; } function initializeDateTimePickers() { flatpickr("#dateInput", { minDate: "today", maxDate: new Date().fp_incr(30), dateFormat: "Y-m-d" }); flatpickr("#timeInput", { enableTime: true, noCalendar: true, dateFormat: "H:i", minTime: "09:00", maxTime: "18:00", minuteIncrement: 30 }); } const menuButton = document.getElementById('menuButton'); const menuOverlay = document.getElementById('menuOverlay'); const menuItems = document.querySelectorAll('.menu-item'); const chatMessages = document.getElementById('chatMessages'); const closeButton = document.createElement('button'); closeButton.innerHTML = '<i class="fas fa-long-arrow-alt-left"></i>'; closeButton.className = 'menu-close-button'; menuOverlay.appendChild(closeButton); const style = document.createElement('style'); style.textContent = `.menu-close-button { position: absolute; top: 15px; right: 15px; background:  #1a1a1f; border: none; color: #FFD700; font-size: 24px; cursor: pointer; padding: 5px; z-index: 1001; } .menu-close-button:hover { color: #FFD700; }`; document.head.appendChild(style); function toggleMenu(show) { menuOverlay.style.display = show ? 'block' : 'none'; if (show) { menuOverlay.classList.add('fade-in'); document.body.style.overflow = 'hidden'; } else { menuOverlay.classList.remove('fade-in'); document.body.style.overflow = ''; } } function handleServiceSelection(category) { const message = document.createElement('div'); message.className = 'message bot-message'; message.innerHTML = `<div class="message-avatar"><i class="fas fa-robot"></i></div><div class="message-content">You've selected ${category} service. Would you like to book an appointment?</div>`; chatMessages.appendChild(message); chatMessages.scrollTop = chatMessages.scrollHeight; } menuButton.addEventListener('click', () => toggleMenu(true)); closeButton.addEventListener('click', () => toggleMenu(false)); menuOverlay.addEventListener('click', (e) => { if (e.target === menuOverlay) { toggleMenu(false); } }); menuItems.forEach(item => { item.addEventListener('click', () => { const category = item.getAttribute('data-category'); handleServiceSelection(category); toggleMenu(false); }); }); document.addEventListener('keydown', (e) => { if (e.key === 'Escape') { toggleMenu(false); } }); function initiateCall() { window.location.href = 'tel:9669181789'; }
                                
const townsData = [{ name: "Mumbai", image: "https://i.pinimg.com/originals/2c/72/40/2c72408b797d4841b6fe6c395183ba35.png", quote: "The town of dreams where ambitions take flight and opportunities never sleep" }, { name: "Delhi", image: "https://www.mistay.in/travel-blog/content/images/2020/07/travel-4813658_1920.jpg", quote: "Where ancient heritage meets modern aspirations in perfect harmony" }, { name: "Jaipur", image: "https://www.tripsavvy.com/thmb/Afl1v6bgmGid9kPfseymDiAYWa0=/3595x2397/filters:no_upscale():max_bytes(150000):strip_icc()/GettyImages-518387310-04a30994bfb1461bb8000f1864ca1fc5.jpg", quote: "Pink Town, where royal heritage colors modern dreams" }]; function showTownsOverlay() { let overlay = document.getElementById('townsOverlay'); if (!overlay) { overlay = document.createElement('div'); overlay.id = 'townsOverlay'; overlay.className = 'towns-overlay'; document.body.appendChild(overlay); } overlay.innerHTML = `<div class="towns-content"><div class="towns-header"><h2>Service Locations</h2><button class="close-towns" onclick="hideTownsOverlay()"><i class="fas fa-long-arrow-alt-left"></i></button></div><div class="towns-grid">${townsData.map(town => `<div class="town-card" onclick="selectTown('${town.name}')"><div class="town-image-container"><img src="${town.image}" alt="${town.name}" class="town-image"><div class="town-overlay"></div></div><div class="town-info"><h3 class="town-name">${town.name}</h3><p class="town-quote">${town.quote}</p></div></div>`).join('')}</div></div>`; setTimeout(() => overlay.classList.add('active'), 10); const style = document.createElement('style'); style.textContent = `.towns-overlay { position: fixed; top: 60px; left: 0; right: 0; bottom: 60px; background: rgba(26, 26, 31, 0.98); z-index: 1000; opacity: 0; visibility: hidden; transition: all 0.3s ease; overflow-y: auto; } .towns-overlay.active { opacity: 1; visibility: visible; } .towns-content { padding: 20px; max-width: 1200px; margin: 0 auto; } .towns-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 30px; padding-bottom: 15px; border-bottom: 1px solid rgba(255, 215, 0, 0.2); } .towns-header h2 { color: var(--primary-color); margin: 0; font-size: 1.5em; } .close-towns { background: none; border: none; color: var(--primary-color); font-size: 1.5em; cursor: pointer; padding: 5px; } .towns-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(300px, 1fr)); gap: 20px; animation: fadeInUp 0.5s ease-out; } .town-card { border-radius: 12px; overflow: hidden; background: rgba(255, 215, 0, 0.1); cursor: pointer; transition: all 0.3s ease; position: relative; } .town-card:hover { transform: translateY(-5px); box-shadow: 0 5px 15px rgba(0, 0, 0, 0.2); } .town-image-container { position: relative; width: 100%; height: 200px; overflow: hidden; } .town-image { width: 100%; height: 100%; object-fit: cover; transition: transform 0.3s ease; } .town-card:hover .town-image { transform: scale(1.1); } .town-overlay { position: absolute; top: 0; left: 0; right: 0; bottom: 0; } .town-info { padding: 20px; position: relative; z-index: 1; } .town-name { color: var(--text-light); margin: 0 0 10px 0; font-size: 1.4em; } .town-quote { color: #cccccc; font-size: 0.9em; line-height: 1.4; margin: 0; } @keyframes fadeInUp { from { opacity: 0; transform: translateY(20px); } to { opacity: 1; transform: translateY(0); } } @media (max-width: 768px) { .towns-grid { grid-template-columns: repeat(auto-fill, minmax(250px, 1fr)); } .town-image-container { height: 160px; } }`; document.head.appendChild(style); } function hideTownsOverlay() { const overlay = document.getElementById('townsOverlay'); if (overlay) { overlay.classList.remove('active'); setTimeout(() => overlay.remove(), 300); } } function selectTown(townName) { console.log(`Selected town: ${townName}`); showAlert(`Selected location: ${townName}`, 'success'); hideTownsOverlay(); }                
              
document.addEventListener('DOMContentLoaded', function() {
    // Select the location link in the menu section
    const menuLocationLink = document.querySelector('.menu-section .location-link');
    
    if (menuLocationLink) {
        menuLocationLink.addEventListener('click', function(e) {
            e.preventDefault(); // Prevent default link behavior
            showTownsOverlay(); // Call the existing function to show towns overlay
        });
    }
});

const reserveData=[{category:"Venue",items:[{name:"Royal Wedding Hall",city:"Udaipur",image:"http://www.udaipurweddings.com/wp-content/uploads/2017/11/13-3.jpg",description:"Elegant venue for grand celebrations",price:"₹50,000 - ₹1,00,000",details:{capacity:"500-1000 guests",facilities:["AC","Stage","Parking","Catering Area"],amenities:["Sound System","Decoration Services","Bridal Room"]},whatsappNumber:"+917869809022"},{name:"Riverside Resort",city:"Jaipur",image:"https://di5fgdew4nptq.cloudfront.net/api2/media/images/140a5288-3ccf-eb11-80dd-f8bc124783a3",description:"Scenic location for intimate gatherings",price:"₹30,000 - ₹75,000",details:{capacity:"100-300 guests",facilities:["Open Area","River View","Garden"],amenities:["Accommodation","Bonfire Area","Photography Spots"]},whatsappNumber:"+917869809022"}]},{category:"Entertainment",items:[{name:"Traditional Dance Troupe",city:"Udaipur",image:"https://example.com/dance-troupe.jpg",description:"Authentic cultural performance",price:"₹25,000 - ₹50,000",details:{duration:"1-2 hours",performers:"5-10 artists",styles:["Bharatanatyam","Kathak","Folk Dances"]},whatsappNumber:"+917869809022"},{name:"Live Musical Ensemble",city:"Jaipur",image:"https://example.com/music-ensemble.jpg",description:"Professional live music band",price:"₹35,000 - ₹75,000",details:{duration:"2-3 hours",musicians:"4-6 members",genres:["Classical","Bollywood","Fusion"]},whatsappNumber:"+917869809022"}]},{category:"Ritual Services",items:[{name:"Traditional Wedding Rituals",city:"Udaipur",image:"https://example.com/wedding-rituals.jpg",description:"Complete wedding ceremony arrangement",price:"₹1,00,000 - ₹3,00,000",details:{services:["Priest","Mandap Decoration","Ritual Accessories"],duration:"Full Day",includes:["Vedic Ceremonies","Customized Rituals"]},whatsappNumber:"+917869809022"}]}];

function showLocationOverlay(onCitySelect) {
    // Create location overlay
    const locationOverlay = document.createElement('div');
    locationOverlay.id = 'locationOverlay';
    locationOverlay.className = 'location-overlay';

    // Get unique cities
    const cities = [...new Set(reserveData.flatMap(category => 
        category.items.map(item => item.city)
    ))];

    locationOverlay.innerHTML = `
        <div class="location-content">
            <div class="location-header">
                <h2>Select Your City</h2>
                <button class="close-location" onclick="hideLocationOverlay()">
                    <i class="fas fa-long-arrow-alt-left"></i>
                </button>
            </div>
            
            <div class="cities-grid">
                ${cities.map(city => `
                    <div class="city-card" onclick="selectCity('${city}')">
                        <h3>${city}</h3>
                    </div>
                `).join('')}
            </div>
        </div>
    `;

    document.body.appendChild(locationOverlay);

    // Add styles for location overlay
    const style = document.createElement('style');
    style.textContent = `
.location-overlay { position: fixed; top: 0; left: 0; right: 0; bottom: 0; background: rgba(26, 26, 31, 0.98); z-index: 1100; opacity: 0; visibility: hidden; transition: all 0.3s ease; overflow-y: auto; } .location-overlay.active { opacity: 1; visibility: visible; } .location-content { padding: 20px; max-width: 1200px; margin: 0 auto; } .location-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 20px; padding-bottom: 15px; border-bottom: 1px solid rgba(255, 215, 0, 0.2); } .close-location { background: none; border: none; color: #FFD700; cursor: pointer; font-size: 24px; } .cities-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(200px, 1fr)); gap: 20px; } .city-card { background: rgba(255, 215, 0, 0.1); border-radius: 12px; padding: 20px; text-align: center; cursor: pointer; transition: all 0.3s ease; } .city-card:hover { background: var(--primary-color); color: black; }
    `;
    document.head.appendChild(style);

    // Window method to select city
    window.selectCity = (city) => {
        hideLocationOverlay();
        onCitySelect(city);
    };

    // Activate overlay
    setTimeout(() => {
        locationOverlay.classList.add('active');
    }, 10);
}

function hideLocationOverlay() {
    const overlay = document.getElementById('locationOverlay');
    if (overlay) {
        overlay.classList.remove('active');
        setTimeout(() => overlay.remove(), 300);
    }
}

function showReserveOverlay(selectedCity = null) {
    // Create overlay container
    let overlay = document.createElement('div');
    overlay.id = 'reserveOverlay';
    overlay.className = 'reserve-overlay';
    document.body.appendChild(overlay);
    
    // Generate filter buttons
    const categories = ['All', ...new Set(reserveData.map(category => category.category))];
    const filterButtons = categories.map(cat => 
        `<button class="filter-btn" data-category="${cat}">${cat}</button>`
    ).join('');

    // Generate reserve items HTML
    const generateReserveItems = (category = 'All', city = null) => {
        let items = reserveData.flatMap(cat => cat.items);
        
        // Filter by city if specified
        if (city) {
            items = items.filter(item => item.city === city);
        }

        // Filter by category if not 'All'
        if (category !== 'All') {
            items = items.filter(item => 
                reserveData.find(cat => cat.category === category)?.items.includes(item)
            );
        }

        return items.map(item => `
            <div class="reserve-card" data-category="${item.category}" data-city="${item.city}">
                <div class="reserve-image-container">
                    <img src="${item.image}" alt="${item.name}" class="reserve-image">
                    <div class="reserve-image-overlay">
                        <span class="city-tag">${item.city}</span>
                    </div>
                </div>
                <div class="reserve-info">
                    <h3 class="reserve-name">${item.name}</h3>
                    <p class="reserve-description">${item.description}</p>
                    <div class="reserve-price">${item.price}</div>
                    <div class="reserve-actions">
                        <button class="book-now-btn" onclick="showBookingForm('${item.name}')">Book Now</button>
                        <button class="chat-btn" onclick="openWhatsApp('${item.whatsappNumber}')">
                            <i class="fab fa-whatsapp"></i> Chat
                        </button>
                        <button class="info-btn" onclick="showItemDetails('${encodeURIComponent(JSON.stringify(item))}')">
                            <i class="fas fa-info-circle"></i>
                        </button>
                    </div>
                </div>
            </div>
        `).join('');
    };

    // Overlay content
    overlay.innerHTML = `
        <div class="reserve-content">
            <div class="reserve-header">
                <h2>Deep Reserve</h2>
                <div class="header-actions">
                    <button class="location-btn" onclick="showLocationOverlay(filterByCity)">
                        <i class="fas fa-map-marker-alt"></i> 
                        ${selectedCity || 'Select City'}
                    </button>
                    <button class="close-reserve" onclick="hideReserveOverlay()">
                        <i class="fas fa-long-arrow-alt-left"></i>
                    </button>
                </div>
            </div>
            
            <div class="filter-container">
                <div class="filter-buttons">
                    ${filterButtons}
                </div>
            </div>

            <div class="reserve-grid-container">
                <div class="reserve-grid" id="reserveItemsContainer">
                    ${generateReserveItems('All', selectedCity)}
                </div>
            </div>

            <!-- New Chat Input Container -->
            <div class="chat-input-container" style="margin-top: 0; padding-bottom: 60px;">
                <input type="text" id="chatInput" placeholder="Search for services or ask a question..." />
                <div class="input-actions">
                    <button class="send-message" id="sendButton">
                        <i class="fas fa-paper-plane"></i>
                    </button>
                </div>
            </div>

            <!-- Bottom Navigation -->
            <nav class="bottom-nav">
                <div class="nav-container">
                    <div class="nav-item" data-page="services">
                        <a href="https://example.com/services">
                            <i class="fas fa-user-tie"></i>
                        </a>
                        <span>Services</span>
                    </div>
                    <div class="nav-item" data-page="reserve">
                        <a href="https://example.com/reserve">
                            <i class="fas fa-calendar-check"></i>
                        </a>
                        <span>Reserve</span>
                    </div>
                    <div class="nav-item whatsapp-item" data-page="chat">
                        <a href="https://wa.me/9111478356" target="_blank">
                            <i class="fas fa-message"></i>
                        </a>
                        <span>WhatsApp</span>
                    </div>
                    <div class="nav-item" data-page="slot">
                         <a href="https://example.com/slot">
                    <i class="fas fa-calendar-alt"></i>
                        </a>
                      <span>Slot</span>
                    </div>
                    <div class="nav-item" data-page="franchise">
                        <a href="https://example.com/franchise">
                            <i class="fas fa-store"></i>
                        </a>
                        <span>Franchise</span>
                    </div>
                </div>
            </nav>
        </div>
    `;

    // Define a method to filter by city
    window.filterByCity = (city) => {
        const titleElement = overlay.querySelector('.location-btn');
        titleElement.innerHTML = `<i class="fas fa-map-marker-alt"></i> ${city}`;
        
        // Regenerate items with city filter
        document.getElementById('reserveItemsContainer').innerHTML = generateReserveItems('All', city);

        // Update filter buttons state
        const filterBtns = overlay.querySelectorAll('.filter-btn');
        filterBtns.forEach(btn => btn.classList.remove('active'));
        filterBtns[0].classList.add('active'); // 'All' category
    };

    // Add event listeners for filter buttons
    setTimeout(() => {
        overlay.classList.add('active');
        const filterBtns = overlay.querySelectorAll('.filter-btn');
        filterBtns.forEach(btn => {
            btn.addEventListener('click', function() {
                // Remove active class from all buttons
                filterBtns.forEach(b => b.classList.remove('active'));
                this.classList.add('active');

                // Filter items
                const category = this.dataset.category;
                const currentCity = overlay.querySelector('.location-btn').textContent.trim().replace('Select City', '');
                document.getElementById('reserveItemsContainer').innerHTML = generateReserveItems(
                    category, 
                    currentCity === '' ? null : currentCity
                );
            });
        });
    }, 10);

    // Add styles
    const style = document.createElement('style');
    style.textContent = `
.reserve-overlay { 
    position: fixed; 
    top: 0; 
    left: 0; 
    right: 0; 
    bottom: 0; 
    background: #1a1a1f; 
    z-index: 1000; 
    opacity: 0; 
    visibility: hidden; 
    transition: all 0.3s ease; 
    display: flex; 
    flex-direction: column;
    width: 100%; /* Ensure full viewport width */
}

.reserve-overlay.active { 
    opacity: 1; 
    visibility: visible; 
}

.reserve-content {
    flex: 1;
    display: flex;
    flex-direction: column;
    overflow: hidden;
    width: 100vw; /* Full viewport width */
    max-width: 100vw; /* Remove max-width constraint */
    padding: 20px; 
    margin: 0; 
    box-sizing: border-box; 
    align-self: center; /* Center content if needed */
}

.reserve-header { 
    padding: 10px 20px; /* Slightly increased padding for better spacing */
    background: rgba(26, 26, 31, 0.9); /* Slightly more transparent for a modern look */
    position: fixed; 
    width: 100%; 
    top: 0; 
    left: 0; 
    z-index: 30; 
    border-bottom: 2px solid var(--primary-color); 
    backdrop-filter: blur(12px); /* Slightly stronger blur effect for depth */
    height: 70px; /* Increased height for improved proportions */
    display: flex; /* Flex layout for better alignment of child elements */
    align-items: center; /* Center content vertically */
    justify-content: space-between; /* Space out content (like logo and menu items) */
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1); /* Add a soft shadow for elevation */
    transition: background 0.3s, height 0.3s; /* Smooth transition for hover or scroll effects */
}

.header:hover {
    background: rgba(26, 26, 31, 0.85); /* Slightly lighter on hover for interaction feedback */
    height: 72px; /* Subtle size increase on hover */
}


.close-reserve {
  background: none;
  border: none;
  color: #FFD700;
  cursor: pointer;
  font-size: 24px;
}

.filter-container {
    width: 100vw; /* Full viewport width */
    max-width: 100vw; /* Override any max-width constraints */
    margin-left: calc(-50vw + 50%); /* Center container horizontally */
    padding: 0 15px; /* Optional side padding */
    margin-top: 60px;
    box-sizing: border-box;
}

.filter-container::-webkit-scrollbar {
    display: none; /* Hide scrollbar in Webkit browsers */
}

/* Responsive adjustments */
@media (max-width: 768px) {
    .reserve-content,
    .filter-container {
        width: 100vw;
        margin-left: calc(-50vw + 50%);
        padding: 10px;
    }
}

.filter-buttons {
    display: flex; /* Changed from inline-flex to flex */
    justify-content: flex-start; /* Align buttons to start */
    gap: 4px;
    padding-bottom: 10px;
    width: 100vw; /* Full viewport width */
    margin-left: calc(-50vw + 50%); /* Center buttons horizontally */
    overflow-x: auto; /* Allow horizontal scrolling if needed */
    scrollbar-width: none; /* Hide scrollbar in Firefox */
}

.filter-buttons::-webkit-scrollbar {
    display: none; /* Hide scrollbar in Webkit browsers */
}

.filter-btn {
    margin-top: 2px;
    margin-left: 2px;
    margin-right: 5px;
    flex-shrink: 0; /* Prevent buttons from shrinking */
    background: rgba(255, 215, 0, 0.1);
    color: var(--text-light);
    border: 1px solid rgba(255, 215, 0, 0.2);
    padding: 10px 15px;
    border-radius: 25px;
    transition: all 0.3s ease;
    cursor: pointer;
    white-space: nowrap;
    font-weight: 500;
    box-shadow: 0 2px 5px rgba(0,0,0,0.1);
}

.filter-btn:hover {
    background: var(--primary-color);
    color: black;
    transform: scale(1.05); /* Slight scale on hover */
    box-shadow: 0 4px 8px rgba(0,0,0,0.2);
}

.filter-btn.active {
    background: var(--primary-color);
    color: black;
    font-weight: 700; /* Bolder text when active */
    box-shadow: 0 4px 10px rgba(0,0,0,0.3);
}

.reserve-grid-container {
    flex-grow: 1;
    overflow-y: scroll; /* Enable vertical scrolling */
    padding-bottom: 20px;
    scrollbar-width: none; /* Hide scrollbar in Firefox */
}

.reserve-grid-container::-webkit-scrollbar {
    display: none; /* Hide scrollbar in Webkit browsers */
}

.reserve-grid {
    margin-top: 10px;
    display: grid; 
    grid-template-columns: repeat(auto-fill, minmax(300px, 1fr)); 
    gap: 20px; 
    animation: fadeInUp 0.5s ease-out;
    padding-bottom: 20px; /* Extra padding at bottom for scroll */
}

.reserve-card {
  background: rgba(255, 215, 0, 0.1);
  border-radius: 12px;
  overflow: hidden;
  transition: all 0.3s ease;
}

.reserve-image-container {
  height: 200px;
  overflow: hidden;
  position: relative;
}

.reserve-image {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.3s ease;
}

.reserve-card:hover .reserve-image {
  transform: scale(1.1);
}

.reserve-info {
  padding: 15px;
  color: var(--text-light);
}

.reserve-actions {
  display: flex;
  justify-content: space-between;
  margin-top: 15px;
}

.book-now-btn,
.chat-btn,
.info-btn {
  background: var(--primary-color);
  border: none;
  padding: 10px 15px;
  border-radius: 5px;
  cursor: pointer;
  transition: all 0.3s ease;
}

.header-actions {
  display: flex;
  align-items: center;
  gap: 15px;
}

.location-btn {
  background: rgba(255, 215, 0, 0.1);
  color: var(--text-light);
  border: none;
  padding: 10px 15px;
  border-radius: 20px;
  cursor: pointer;
  display: flex;
  align-items: center;
  gap: 10px;
  transition: all 0.3s ease;
}

.location-btn:hover {
  background: var(--primary-color);
  color: black;
}

@media (max-width: 768px) {
  .reserve-grid {
    grid-template-columns: 1fr;
  }
}
`;
    document.head.appendChild(style);
}

function hideReserveOverlay() {
    const overlay = document.getElementById('reserveOverlay');
    if (overlay) {
        overlay.classList.remove('active');
        setTimeout(() => overlay.remove(), 300);
    }
}

function showBookingForm(itemName) {
    // Create booking form overlay
    const formOverlay = document.createElement('div');
    formOverlay.id = 'bookingFormOverlay';
    formOverlay.className = 'booking-form-overlay';
    formOverlay.innerHTML = `
        <div class="booking-form-content">
            <h2>Book ${itemName}</h2>
            <form id="bookingForm">
                <input type="text" name="Name" placeholder="Full Name" required>
                <input type="tel" name="Phone" placeholder="Phone Number" required>
                <input type="email" name="Email" placeholder="Email Address" required>
                <input type="date" name="Date" placeholder="Preferred Date" required>
                <textarea name="Additional Requirements" placeholder="Additional Requirements"></textarea>
                <button type="submit">Book Now</button>
            </form>
            <button class="close-form" onclick="closeBookingForm()">Close</button>
        </div>
    `;
    document.body.appendChild(formOverlay);

    // Form submission handler
    const form = formOverlay.querySelector('form');
    form.addEventListener('submit', function(e) {
        e.preventDefault();
        const formData = new FormData(form);
        const whatsappMessage = `Booking Request for ${itemName}\n` +
            `Name: ${formData.get('Name')}\n` +
            `Phone: ${formData.get('Phone')}\n` +
            `Email: ${formData.get('Email')}\n` +
            `Date: ${formData.get('Date')}\n` +
            `Additional Details: ${formData.get('Additional Requirements')}`;
        
        // Redirect to WhatsApp with pre-filled message
        window.open(`https://wa.me/7869809022?text=${encodeURIComponent(whatsappMessage)}`, '_blank');
    });

    // Add form styles
    const style = document.createElement('style');
    style.textContent = `
.booking-form-overlay { position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0, 0, 0, 0.9); display: flex; justify-content: center; align-items: center; z-index: 1000; padding: 20px; box-sizing: border-box; } .booking-form-content { background: rgba(255, 255, 255, 0.1); border-radius: 15px; max-width: 500px; width: 100%; padding: 30px; color: var(--text-light); backdrop-filter: blur(10px); border: 1px solid rgba(255, 215, 0, 0.2); } .booking-form-content h2 { text-align: center; margin-bottom: 20px; color: var(--primary-color); } #bookingForm { display: flex; flex-direction: column; gap: 15px; } #bookingForm input, #bookingForm textarea { padding: 10px; background: rgba(255, 255, 255, 0.1); border: 1px solid rgba(255, 215, 0, 0.2); color: var(--text-light); border-radius: 5px; } #bookingForm button[type="submit"] { background: var(--primary-color); color: black; border: none; padding: 12px; border-radius: 5px; cursor: pointer; transition: background 0.3s ease; } .close-form { width: 100%; margin-top: 15px; padding: 10px; background: rgba(255, 0, 0, 0.1); color: var(--text-light); border: none; border-radius: 5px; cursor: pointer; }
    `;
    document.head.appendChild(style);
}

function closeBookingForm() {
    const formOverlay = document.getElementById('bookingFormOverlay');
    if (formOverlay) {
        formOverlay.remove();
    }
}

function showItemDetails(encodedItem) {
    const item = JSON.parse(decodeURIComponent(encodedItem));
    const detailsOverlay = document.createElement('div');
    detailsOverlay.id = 'itemDetailsOverlay';
    detailsOverlay.className = 'item-details-overlay';
    
    // Function to format details in a more readable way
    const formatDetails = (details) => {
        return Object.entries(details).map(([key, value]) => {
            // Convert arrays to comma-separated strings
            const formattedValue = Array.isArray(value) 
                ? value.join(', ') 
                : (typeof value === 'object' ? JSON.stringify(value) : value);
            
            // Convert camelCase to Title Case
            const formattedKey = key
                .replace(/([A-Z])/g, ' $1')
                .replace(/^./, str => str.toUpperCase());
            
            return `
                <div class="detail-item">
                    <strong>${formattedKey}:</strong> 
                    <span>${formattedValue}</span>
                </div>
            `;
        }).join('');
    };

    detailsOverlay.innerHTML = `
        <div class="item-details-content">
            <div class="details-header">
                <h2>${item.name}</h2>
                <button class="close-btn" onclick="this.closest('.item-details-overlay').remove()">
                    &times;
                </button>
            </div>
            
            <div class="details-image-container">
                <img src="${item.image}" alt="${item.name}" class="details-image">
            </div>
            
            <div class="details-info">
                <div class="details-section description">
                    <h3>Description</h3>
                    <p>${item.description}</p>
                </div>
                
                <div class="details-section price">
                    <h3>Price Range</h3>
                    <p>${item.price}</p>
                </div>
                
                <div class="details-section additional-details">
                    <h3>Additional Details</h3>
                    <div class="details-grid">
                        ${formatDetails(item.details)}
                    </div>
                </div>
                
                <div class="details-section contact">
                    <h3>Contact</h3>
                    <button class="whatsapp-btn" onclick="openWhatsApp('${item.whatsappNumber}')">
                        <i class="fab fa-whatsapp"></i> Chat on WhatsApp
                    </button>
                </div>
            </div>
        </div>
    `;

    // Add inline styles for improved presentation
    const style = document.createElement('style');
    style.textContent = `
.item-details-overlay { position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0, 0, 0, 0.9); display: flex; justify-content: center; align-items: center; z-index: 1000; padding: 20px; box-sizing: border-box; overflow: auto; } .item-details-content { background: rgba(255, 255, 255, 0.1); border-radius: 15px; max-width: 800px; width: 100%; max-height: 90vh; overflow-y: auto; color: var(--text-light); position: relative; backdrop-filter: blur(10px); border: 1px solid rgba(255, 215, 0, 0.2); } .details-header { display: flex; justify-content: space-between; align-items: center; padding: 20px; border-bottom: 1px solid rgba(255, 215, 0, 0.2); } .details-header h2 { margin: 0; font-size: 24px; } .close-btn { background: none; border: none; color: var(--text-light); font-size: 36px; cursor: pointer; line-height: 1; } .details-image-container { width: 100%; height: 400px; overflow: hidden; } .details-image { width: 100%; height: 100%; object-fit: cover; } .details-info { padding: 20px; } .details-section { margin-bottom: 20px; } .details-section h3 { border-bottom: 2px solid var(--primary-color); padding-bottom: 10px; margin-bottom: 15px; } .details-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(250px, 1fr)); gap: 15px; } .detail-item { background: rgba(255, 215, 0, 0.1); padding: 15px; border-radius: 8px; } .detail-item strong { display: block; margin-bottom: 5px; color: var(--primary-color); } .whatsapp-btn { background: #25D366; color: white; border: none; padding: 10px 15px; border-radius: 5px; display: flex; align-items: center; gap: 10px; cursor: pointer; } .whatsapp-btn i { font-size: 20px; } @media (max-width: 768px) { .details-image-container { height: 250px; } .details-header h2 { font-size: 20px; } .details-grid { grid-template-columns: 1fr; } }
    `;
    document.head.appendChild(style);
    document.body.appendChild(detailsOverlay);
}

function openWhatsApp(number) {
    window.open(`https://wa.me/${number}`, '_blank');
}

// Ensure search functionality is setup when reserve page is accessed
document.addEventListener('DOMContentLoaded', function() {
    const reserveNavItem = document.querySelector('.nav-item[data-page="reserve"]');
    if (reserveNavItem) {
        reserveNavItem.addEventListener('click', function(e) {
            e.preventDefault();
            showReserveOverlay();
        });
    }
});

function submitBookingViaWhatsApp() { const name = document.getElementById('nameInput').value.trim(); const contact = document.getElementById('contactInput').value.trim(); const address = document.getElementById('addressInput').value.trim(); const pincode = document.getElementById('pincodeInput').value.trim(); const date = document.getElementById('dateInput').value.trim(); const time = document.getElementById('timeInput').value.trim(); const bookingForm = document.getElementById('bookingForm'); const categoryId = bookingForm.dataset.categoryId || 'Not Specified'; const serviceName = bookingForm.dataset.serviceName || 'Not Specified'; const packageType = bookingForm.dataset.packageType || 'Not Specified'; if (!name || !contact) { showAlert('Please fill in Name and Contact Number', 'error'); return; } const message = `New Booking Request:\n📋 Category: ${categoryId}\n🛠 Service: ${serviceName}\n💎 Package: ${packageType}\n👤 Name: ${name}\n📞 Contact: ${contact}\n🏠 Address: ${address || 'Not Provided'}\n📍 Pincode: ${pincode || 'Not Provided'}\n📅 Date: ${date}\n⏰ Time: ${time}\n\nPlease confirm and process this booking.`; const encodedMessage = encodeURIComponent(message); const whatsappNumber = '78698 09022'; const whatsappUrl = `https://wa.me/${whatsappNumber.replace(/\s/g, '')}?text=${encodedMessage}`; window.open(whatsappUrl, '_blank'); showBookingConfirmation(); } function showBookingConfirmation() { const confirmationModal = document.createElement('div'); confirmationModal.id = 'bookingConfirmation'; confirmationModal.className = 'modal-overlay'; confirmationModal.innerHTML = `<div class="modal-content"><div class="confirmation-icon"><i class="fas fa-check-circle"></i></div><h2>Booking Submitted!</h2><p>Your booking request has been sent to WhatsApp. Our team will contact you shortly.</p><button onclick="closeConfirmation()" class="action-button">Close</button></div>`; document.body.appendChild(confirmationModal); document.getElementById('bookingForm').reset(); } function closeConfirmation() { const confirmationModal = document.getElementById('bookingConfirmation'); if (confirmationModal) { confirmationModal.remove(); } } document.addEventListener('DOMContentLoaded', function() { const submitBookingButton = document.getElementById('submitBooking'); if (submitBookingButton) { submitBookingButton.addEventListener('click', submitBookingViaWhatsApp); } }); document.addEventListener('DOMContentLoaded', function() { const locationNav = document.querySelector('.nav-item[data-page="location"]'); if (locationNav) { locationNav.addEventListener('click', function(e) { e.preventDefault(); showCitiesOverlay(); }); } }); function showAlert(message, type = 'success') { const alert = document.createElement('div'); alert.className = `alert alert-${type}`; alert.textContent = message; document.body.appendChild(alert); setTimeout(() => { alert.remove(); }, 3000); } function setupEventListeners() { window.addEventListener('click', function(event) { if (event.target.classList.contains('modal-overlay')) { closePackageInfo(); closeBookingForm(); closeConfirmation(); } }); }
                            </script>
