<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Deep Chatbot - On-Demand Services</title>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css" rel="stylesheet">
    <link href="https://cdnjs.cloudflare.com/ajax/libs/flatpickr/4.6.13/flatpickr.min.css" rel="stylesheet">
    <style>
        :root {
            --primary-color: #FFD700;
            --secondary-color: #FDB931;
            --background-dark: #1a1a1f;
            --text-light: #ffffff;
            --text-dark: #000000;
            --accent-gradient: linear-gradient(135deg, var(--primary-color), var(--secondary-color));
            --error-color: #ff4444;
            --success-color: #00C851;
        }
        
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }
        
        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, sans-serif;
            background-color: var(--background-dark);
            color: var(--text-light);
            line-height: 1.6;
            min-height: 100vh;
            overflow-x: hidden;
        }
        
        .header {
            padding: 8px 16px;
            background: rgba(26, 26, 31, 0.95);
            position: fixed;
            width: 100%;
            top: 0;
            left: 0;
            z-index: 30;
            border-bottom: 2px solid var(--primary-color);
            backdrop-filter: blur(10px);
            height: 60px;
        }
        
        .header-content {
            max-width: 1400px;
            margin: 0 auto;
            display: flex;
            justify-content: space-between;
            align-items: center;
            height: 100%;
        }
        
        .logo {
            font-size: 1.8em;
            font-weight: 900;
            background: var(--accent-gradient);
            -webkit-background-clip: text;
            color: transparent;
            letter-spacing: 1.2px;
        }
        
        .wallet {
            padding: 10px 20px;
            background: var(--accent-gradient);
            border: none;
            border-radius: 50px;
            color: var(--text-dark);
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
            font-size: 0.95em;
        }
        
        .chatbot-container {
            position: fixed;
            top: 60px;
            left: 0;
            right: 0;
            bottom: 60px;
            background: rgba(26, 26, 31, 0.95);
            display: flex;
            flex-direction: column;
        }
        
        .chat-header {
            padding: 15px;
            background: linear-gradient(135deg, rgba(255, 215, 0, 0.1), rgba(253, 185, 49, 0.1));
            border-bottom: 1px solid rgba(255, 215, 0, 0.2);
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        
        .chat-status {
            display: flex;
            align-items: center;
            color: var(--primary-color);
            font-weight: 600;
        }
        
        .status-dot {
            width: 8px;
            height: 8px;
            background: var(--primary-color);
            border-radius: 50%;
            margin-right: 8px;
            animation: pulse 2s infinite;
        }
        
        .chat-messages {
            flex: 1;
            overflow-y: auto;
            padding: 20px;
            scroll-behavior: smooth;
        }
        
        .message {
            display: flex;
            gap: 10px;
            max-width: 85%;
            margin-bottom: 16px;
            animation: messageSlide 0.3s ease-out;
        }
        
        .bot-message {
            align-self: flex-start;
        }
        
        .user-message {
            align-self: flex-end;
            flex-direction: row-reverse;
        }
        
        .message-avatar {
            width: 36px;
            height: 36px;
            background: rgba(255, 215, 0, 0.1);
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            color: var(--primary-color);
        }
        
        .message-content {
            background: rgba(255, 255, 255, 0.05);
            padding: 12px 16px;
            border-radius: 16px;
            color: var(--text-light);
        }
        
        .user-message .message-content {
            background: rgba(255, 215, 0, 0.1);
        }

        .service-card {
            background: rgba(255, 255, 255, 0.05);
            border-radius: 12px;
            padding: 15px;
            margin-top: 10px;
            border: 1px solid rgba(255, 215, 0, 0.2);
            transition: transform 0.2s ease;
        }

        .service-card:hover {
            transform: translateY(-2px);
        }

        .service-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 10px;
        }

        .service-rating {
            display: flex;
            align-items: center;
            color: var(--primary-color);
            gap: 4px;
        }

        .service-details {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 10px;
            margin-top: 10px;
        }

        .service-detail-item {
            display: flex;
            align-items: center;
            gap: 8px;
            font-size: 0.9em;
        }

        .service-actions {
            display: flex;
            gap: 10px;
            margin-top: 15px;
        }

        .service-button {
            padding: 8px 15px;
            border-radius: 8px;
            border: none;
            cursor: pointer;
            font-weight: 500;
            transition: all 0.3s ease;
            display: flex;
            align-items: center;
            gap: 6px;
        }

        .book-button {
            background: var(--accent-gradient);
            color: var(--text-dark);
        }

        .contact-button {
            background: rgba(255, 215, 0, 0.1);
            color: var(--primary-color);
            border: 1px solid var(--primary-color);
        }
        
        .chat-input-container {
            padding: 20px;
            background: rgba(26, 26, 31, 0.98);
            border-top: 1px solid rgba(255, 215, 0, 0.1);
            display: flex;
            gap: 12px;
            align-items: center;
        }
        
        #chatInput {
            flex: 1;
            background: rgba(255, 255, 255, 0.05);
            border: 1px solid rgba(255, 215, 0, 0.2);
            border-radius: 12px;
            padding: 20px 20px;
            color: var(--text-light);
            font-size: 0.95em;
        }
        
        .input-actions {
            display: flex;
            gap: 8px;
        }
        
        .input-actions button {
            background: none;
            border: none;
            color: rgba(255, 215, 0, 0.7);
            cursor: pointer;
            padding: 10px;
            border-radius: 50%;
            transition: all 0.3s ease;
        }

        /* Filter Modal Styles */
        .filter-modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: rgba(0, 0, 0, 0.8);
            z-index: 1000;
            padding: 20px;
        }

        .filter-modal.active {
            display: flex;
            justify-content: center;
            align-items: center;
        }

        .filter-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
        }

        .filter-close {
            background: none;
            border: none;
            color: var(--text-light);
            cursor: pointer;
            font-size: 1.5em;
        }

/* Updated Filter Section Styles */
.filter-section {
    margin-bottom: 10px;
    overflow: hidden; /* Contains the scrolling content */
}

.filter-section h3 {
    margin-bottom: 7px;
    color: var(--primary-color);
    font-size: 0.9em;
    padding-left: 4px;
}

.filter-options {
    display: flex;
    overflow-x: auto;
    overflow-y: hidden;
    scroll-snap-type: x mandatory;
    scrollbar-width: none; /* Firefox */
    -ms-overflow-style: none; /* IE and Edge */
    padding: 4px;
    gap: 12px;
    -webkit-overflow-scrolling: touch; /* Smooth scrolling on iOS */
}

/* Hide scrollbar for Chrome, Safari and Opera */
.filter-options::-webkit-scrollbar {
    display: none;
}

.filter-option {
    flex: 0 0 auto;
    padding: 8px 16px;
    background: rgba(255, 215, 0, 0.1);
    border: 1px solid rgba(255, 215, 0, 0.2);
    border-radius: 20px;
    cursor: pointer;
    white-space: nowrap;
    scroll-snap-align: start;
    transition: all 0.3s ease;
    font-size: 0.9em;
}

.filter-option.active {
    background: var(--primary-color);
    color: var(--text-dark);
}

/* Add visual indicator for scrollable content */
.filter-section::after {
    content: '';
    position: absolute;
    right: 0;
    top: 0;
    bottom: 0;
    width: 32px;
    background: linear-gradient(to right, transparent, var(--background-dark));
    pointer-events: none;
}

/* Optional: Add padding to container to accommodate the gradient */
.filter-content {
    background: var(--background-dark);
    padding: 15px;
    border-radius: 12px;
    width: 100%;
    max-width: 450px;
    border: 2px solid var(--primary-color);
    position: relative;
}

/* Ensure proper spacing between sections */
.filter-section:not(:last-child) {
    margin-bottom: 7px;
}

        /* Booking Modal Styles */
        .booking-modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: rgba(0, 0, 0, 0.8);
            z-index: 1000;
            padding: 20px;
        }

        .booking-modal.active {
            display: flex;
            justify-content: center;
            align-items: center;
        }

        .booking-content {
            background: var(--background-dark);
            padding: 20px;
            border-radius: 12px;
            width: 100%;
            max-width: 500px;
            border: 1px solid var(--primary-color);
        }

        .booking-form {
            display: flex;
            flex-direction: column;
            gap: 10px;
            max-width: 400px;
            margin: 0 auto;
        }

        .form-group {
            display: flex;
            flex-direction: column;
            gap: 5px;
        }

        /* Updated styles for form inputs */
        .form-group input:not([type="date"], [type="time"]) {
            padding: 8px 12px;
            background: rgba(255, 255, 255, 0.05);
            border: 1px solid rgba(255, 215, 0, 0.2);
            border-radius: 6px;
            color: var(--text-light);
            width: 100%;
        }

        .form-group textarea {
            padding: 8px 12px;
            background: rgba(255, 255, 255, 0.05);
            border: 1px solid rgba(255, 215, 0, 0.2);
            border-radius: 6px;
            color: var(--text-light);
            width: 100%;
            min-height: 80px;
        }

        /* Styles for horizontal date/time layout */
        .form-row {
            display: flex;
            gap: 15px;
        }

        .form-row .form-group {
            flex: 1;
        }

        /* Specific styles for date and time inputs */
        input[type="date"],
        input[type="time"] {
            padding: 8px 12px;
            background: rgba(255, 255, 255, 0.05);
            border: 1px solid rgba(255, 215, 0, 0.2);
            border-radius: 6px;
            color: var(--text-light);
            width: 100%;
        }

        .form-group label {
            color: var(--primary-color);
            font-size: 0.9em;
        }

        .form-group label {
            color: var(--primary-color);
            font-size: 0.9em;
        }

        .form-group input,
        .form-group textarea {
            padding: 8px 12px;
            background: rgba(255, 255, 255, 0.05);
            border: 1px solid rgba(255, 215, 0, 0.2);
            border-radius: 6px;
            color: var(--text-light);
        }

        .form-group textarea {
            min-height: 50px;
            resize: vertical;
        }

        .submit-button {
            background: var(--accent-gradient);
            color: var(--text-dark);
            padding: 12px;
            border: none;
            border-radius: 6px;
            cursor: pointer;
            font-weight: 600;
            transition: all 0.3s ease;
        }

        /* Provider Info Modal Styles */
        .provider-info-modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: rgba(0, 0, 0, 0.8);
            z-index: 1000;
            padding: 20px;
        }

        .provider-info-modal.active {
            display: flex;
            justify-content: center;
            align-items: center;
        }

        .provider-info-content {
            background: var(--background-dark);
            padding: 20px;
            border-radius: 12px;
            width: 100%;
            max-width: 500px;
            border: 1px solid var(--primary-color);
        }

        .provider-info-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
        }

        .provider-details {
            display: flex;
            flex-direction: column;
            gap: 15px;
        }

        .provider-detail-item {
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .provider-detail-item i {
            color: var(--primary-color);
        }

        .category-grid-container {
        display: flex;
        align-items: center;
        justify-content: center;
        gap: 1rem;
        position: relative;
        width: 100%;
        max-width: 1200px;
        margin: 0 auto;
    }
    
    .category-grid-wrapper {
        display: grid;
        grid-template-columns: repeat(4, 1fr);
        gap: 1rem;
        width: 100%;
        transition: transform 0.3s ease;
        overflow: hidden;
    }
    
    .nav-button {
        background: rgba(255, 255, 255, 0.1);
        border: none;
        border-radius: 50%;
        width: 40px;
        height: 40px;
        display: flex;
        align-items: center;
        justify-content: center;
        cursor: pointer;
        transition: background-color 0.3s ease;
        z-index: 2;
    }
    
    .nav-button:hover {
        background: rgba(255, 255, 255, 0.2);
    }
    
    .nav-button:disabled {
        opacity: 0.5;
        cursor: not-allowed;
    }
    
    .category-item {
        background: rgba(255, 255, 255, 0.1);
        border-radius: 12px;
        padding: 1.5rem;
        text-align: center;
        cursor: pointer;
        transition: transform 0.3s ease, background-color 0.3s ease;
    }
    
    .category-item:hover {
        background: rgba(255, 255, 255, 0.2);
        transform: translateY(-5px);
    }
    
    .pagination-dots {
        display: flex;
        justify-content: center;
        gap: 0.5rem;
        margin-top: 1rem;
    }
    
    .pagination-dot {
        width: 8px;
        height: 8px;
        border-radius: 50%;
        background: rgba(255, 255, 255, 0.3);
        cursor: pointer;
        transition: background-color 0.3s ease;
    }
    
    .pagination-dot.active {
        background: rgba(255, 255, 255, 0.9);
    }

        /* Alert Styles */
        .alert {
            position: fixed;
            top: 20px;
            right: 20px;
            padding: 15px 20px;
            border-radius: 8px;
            color: var(--text-light);
            z-index: 1000;
            animation: slideIn 0.3s ease-out;
        }

        .alert-success {
            background: var(--success-color);
        }

.alert-error {
            background: var(--error-color);
        }

        /* Animations */
        @keyframes pulse {
            0% { opacity: 1; }
            50% { opacity: 0.5; }
            100% { opacity: 1; }
        }
        
        @keyframes messageSlide {
            from {
                opacity: 0;
                transform: translateY(10px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        @keyframes slideIn {
            from {
                opacity: 0;
                transform: translateX(100px);
            }
            to {
                opacity: 1;
                transform: translateX(0);
            }
        }

        /* Responsive Design */
        @media (max-width: 768px) {
            .category-grid {
                grid-template-columns: repeat(2, 1fr);
            }

            .service-details {
                grid-template-columns: 1fr;
            }

            .filter-options {
                grid-template-columns: 1fr;
            }

            .message {
                max-width: 90%;
            }
        }

        .bottom-nav {
            position: fixed;
            bottom: 0;
            left: 0;
            right: 0;
            height: 60px;
            background: rgba(26, 26, 31, 0.98);
            padding: 8px 0;
            border-top: 1px solid rgba(255, 215, 0, 0.2);
        }
        
        .nav-container {
            display: flex;
            justify-content: space-around;
            align-items: center;
            height: 100%;
            max-width: 600px;
            margin: 0 auto;
        }
        
        .nav-item {
            display: flex;
            flex-direction: column;
            align-items: center;
            text-decoration: none;
            color: #fff;
            transition: all 0.3s ease;
            padding: 5px;
            cursor: pointer;
        }
        
        .nav-item i {
            color: #fff;
            font-size: 20px;
            margin-bottom: 4px;
        }
        
        .nav-item span {
            font-size: 12px;
        }
        
        .nav-item.active i {
            color: var(--primary-color);
        }
        
        @keyframes pulse {
            0% { opacity: 1; }
            50% { opacity: 0.5; }
            100% { opacity: 1; }
        }

        body > h1:first-of-type:not(.heading) {
          display: none !important;
        }
        
        /* Alternative method if the above doesn't work */
        .markdown-body h1:first-child {
          display: none !important;
        }
        
        /* If the number appears in a different container */
        .position-relative h1:first-child {
          display: none !important;
        }

    </style>
</head>
<body>
    <header class="header">
        <div class="header-content">
            <div class="logo">Deep</div>
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
            <button id="filterButton" class="service-button">
                <i class="fas fa-filter"></i>
                Filter
            </button>
        </div>
        
        <div class="chat-messages" id="chatMessages">
            <div class="category-grid" id="categoryGrid">
                <!-- Categories will be dynamically populated -->
            </div>
            
            <div class="message bot-message">
                <div class="message-avatar">
                    <i class="fas fa-robot"></i>
                </div>
                <div class="message-content">
                    <p>Hello! I'm your On-Demand services assistant. How can I help you today?</p>
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
    </div>

    <!-- Bottom Navigation -->
<nav class="bottom-nav">
    <div class="nav-container">
        <div class="nav-item active" data-page="home">
            <a href="https://nysaabhi.github.io/chat">
                <i class="fas fa-envelope-open-text"></i>
            </a>
            <span>Subscription</span>
        </div>
        <div class="nav-item" data-page="tournament">
            <a href="https://nysaabhi.github.io/mymom">
                <i class="fas fa-tag"></i>
            </a>
            <span>Deals</span>
        </div>
        <div class="nav-item" data-page="gallery">
            <a href="gallery.html">
                <i class="fas fa-calendar-alt"></i>
            </a>
            <span>Bookings</span>
        </div>
        <div class="nav-item" data-page="location">
            <a href="location.html">
                <i class="fas fa-map-marker-alt"></i>
            </a>
            <span>Location</span>
        </div>
        <div class="nav-item" data-page="listings">
            <a href="listings.html">
                <i class="fas fa-tasks"></i>
            </a>
            <span>Listings</span>
        </div>
    </div>
</nav>

<!-- Filter Modal -->
<div class="filter-modal" id="filterModal">
    <div class="filter-content">
        <div class="filter-header">
            <h2>Filter Services</h2>
            <button class="filter-close" id="closeFilter">×</button>
        </div>
        <div class="filter-section">
            <h3>Category</h3>
            <div class="filter-options" id="categoryFilter">
                <div class="filter-option" data-category="electrician">Electrician</div>
                <div class="filter-option" data-category="plumber">Plumber</div>
                <div class="filter-option" data-category="mechanic">Mechanic</div>
                <div class="filter-option" data-category="carpenter">Carpenter</div>
                <div class="filter-option" data-category="painter">Painter</div>
                <div class="filter-option" data-category="gardener">Gardener</div>
                <div class="filter-option" data-category="cleaner">Cleaner</div>
                <div class="filter-option" data-category="mason">Mason</div>
                <div class="filter-option" data-category="roofer">Roofer</div>
                <div class="filter-option" data-category="pest-control">Pest Control</div>
                <div class="filter-option" data-category="HVAC">HVAC Technician</div>
                <div class="filter-option" data-category="handyman">Handyman</div>
            </div>
        </div>
        <div class="filter-section">
            <h3>City</h3>
            <div class="filter-options" id="cityFilter">
                <div class="filter-option" data-city="mumbai">Mumbai</div>
                <div class="filter-option" data-city="delhi">Delhi</div>
                <div class="filter-option" data-city="bangalore">Bangalore</div>
                <div class="filter-option" data-city="hyderabad">Hyderabad</div>
                <div class="filter-option" data-city="chennai">Chennai</div>
                <div class="filter-option" data-city="kolkata">Kolkata</div>
                <div class="filter-option" data-city="pune">Pune</div>
                <div class="filter-option" data-city="jaipur">Jaipur</div>
                <div class="filter-option" data-city="ahmedabad">Ahmedabad</div>
                <div class="filter-option" data-city="chandigarh">Chandigarh</div>
                <div class="filter-option" data-city="lucknow">Lucknow</div>
                <div class="filter-option" data-city="bhopal">Bhopal</div>
                <div class="filter-option" data-city="patna">Patna</div>
                <div class="filter-option" data-city="kochi">Kochi</div>
                <div class="filter-option" data-city="vizag">Visakhapatnam</div>
                <div class="filter-option" data-city="nagpur">Nagpur</div>
                <div class="filter-option" data-city="indore">Indore</div>
                <div class="filter-option" data-city="surat">Surat</div>
                <div class="filter-option" data-city="kanpur">Kanpur</div>
                <div class="filter-option" data-city="guwahati">Guwahati</div>
            </div>
        </div>
        <div class="filter-section">
            <h3>Price Range</h3>
            <div class="filter-options" id="priceFilter">
                <div class="filter-option" data-price="very-low">₹0-100</div>
                <div class="filter-option" data-price="low">₹100-500</div>
                <div class="filter-option" data-price="medium">₹501-1000</div>
                <div class="filter-option" data-price="high">₹1001-2000</div>
                <div class="filter-option" data-price="premium">₹2001-5000</div>
                <div class="filter-option" data-price="luxury">₹5000+</div>
            </div>
        </div>
                <div class="filter-section">
            <h3>Rating</h3>
            <div class="filter-options" id="ratingFilter">
                <div class="filter-option" data-rating="4">4+ Stars</div>
                <div class="filter-option" data-rating="3">3+ Stars</div>
            </div>
        </div>
        <div class="filter-section">
            <h3>Experience</h3>
            <div class="filter-options" id="experienceFilter">
                <div class="filter-option" data-exp="1">1+ Years</div>
                <div class="filter-option" data-exp="3">3+ Years</div>
                <div class="filter-option" data-exp="5">5+ Years</div>
            </div>
        </div>
        <button class="submit-button" id="applyFilters">Apply Filters</button>
    </div>
</div>

<div class="booking-modal" id="bookingModal">
        <div class="booking-content">
            <div class="filter-header">
                <h2>Book Service</h2>
                <button class="filter-close" id="closeBooking">×</button>
            </div>
            <form class="booking-form" id="bookingForm">
                <div class="form-group">
                    <label for="userName">Name</label>
                    <input type="text" id="userName" placeholder="Your name" required>
                </div>
                <div class="form-group">
                    <label for="userContact">Contact Number</label>
                    <input type="tel" id="userContact" placeholder="Your contact number" required>
                </div>
                <div class="form-group">
                    <label for="userAddress">Address</label>
                    <textarea id="userAddress" placeholder="Your address" required></textarea>
                </div>
                <!-- New horizontal layout for date and time -->
                <div class="form-row">
                    <div class="form-group">
                        <label for="bookingDate">Date</label>
                        <input type="date" id="bookingDate" required>
                    </div>
                    <div class="form-group">
                        <label for="bookingTime">Time</label>
                        <input type="time" id="bookingTime" required>
                    </div>
                </div>
                <div class="form-group">
                    <label for="workDescription">Description of Work</label>
                    <textarea id="workDescription" placeholder="Please describe the work you need..." required></textarea>
                </div>
                <button type="submit" class="submit-button">Book Now</button>
            </form>
        </div>
    </div>
    
    <!-- Provider Info Modal -->
    <div class="provider-info-modal" id="providerInfoModal">
        <div class="provider-info-content">
            <div class="provider-info-header">
                <h2>Service Provider Details</h2>
                <button class="filter-close" id="closeProviderInfo">×</button>
            </div>
            <div class="provider-details" id="providerDetails">
                <!-- Provider details will be dynamically populated -->
            </div>
        </div>
    </div>

    <script src="https://cdnjs.cloudflare.com/ajax/libs/flatpickr/4.6.13/flatpickr.min.js"></script>
    <script>
const categories = [
    { id: 'plumber', name: 'Plumber', icon: 'fa-wrench' },
    { id: 'electrician', name: 'Electrician', icon: 'fa-bolt' },
    { id: 'mechanic', name: 'Mechanic', icon: 'fa-car' },
    { id: 'carpenter', name: 'Carpenter', icon: 'fa-hammer' },
    { id: 'painter', name: 'Painter', icon: 'fa-paint-brush' },
    { id: 'gardener', name: 'Gardener', icon: 'fa-leaf' },
    { id: 'chef', name: 'Chef', icon: 'fa-utensils' },
    { id: 'cleaner', name: 'Cleaner', icon: 'fa-broom' },
    { id: 'teacher', name: 'Teacher', icon: 'fa-chalkboard-teacher' },
    { id: 'babysitter', name: 'Babysitter', icon: 'fa-baby' },
    { id: 'driver', name: 'Driver', icon: 'fa-car-alt' },
    { id: 'mover', name: 'Mover', icon: 'fa-dolly' },
    { id: 'locksmith', name: 'Locksmith', icon: 'fa-lock' },
    { id: 'pest_control', name: 'Pest Control', icon: 'fa-spider' },
    { id: 'roofer', name: 'Roofer', icon: 'fa-house-damage' },
    { id: 'welder', name: 'Welder', icon: 'fa-fire' },
    { id: 'fitness_trainer', name: 'Fitness', icon: 'fa-dumbbell' },
    { id: 'massage_therapist', name: 'Therapist', icon: 'fa-hand-holding-water' },
    { id: 'plasterer', name: 'Plasterer', icon: 'fa-brush' },
    { id: 'interior_designer', name: 'Interior', icon: 'fa-couch' },
    { id: 'photographer', name: 'Photographer', icon: 'fa-camera' },
    { id: 'event_planner', name: 'Event', icon: 'fa-calendar-alt' }
];

// Split categories into pages
const categoryPages = [];
const itemsPerPage = 4;
let currentPage = 0;

for (let i = 0; i < categories.length; i += itemsPerPage) {
    categoryPages.push(categories.slice(i, i + itemsPerPage));
}

// Sample service providers data
        const serviceProvidersData = [
            {
                id: 1,
                name: "Rajesh Kumar",
                category: "plumber",
                city: "Delhi",
                pincode: "110001",
                rating: 4.8,
                experience: "8 years",
                price: "₹500/hour",
                phone: "+91-9876543210",
                email: "rajesh@example.com",
                availability: "Mon-Sat",
                reviews: 156,
                skills: ["Pipe Fitting", "Leak Repair", "Bathroom Installation"],
                description: "Professional plumber with expertise in all types of plumbing work",
                languages: ["Hindi", "English"],
                certifications: ["ISO Certified", "Government Approved"]
            },
            // Add more service providers here
        ];

// Initialize elements
const chatMessages = document.getElementById('chatMessages');
const chatInput = document.getElementById('chatInput');
const sendButton = document.getElementById('sendButton');
const filterModal = document.getElementById('filterModal');
const bookingModal = document.getElementById('bookingModal');
const providerInfoModal = document.getElementById('providerInfoModal');
const categoryGrid = document.getElementById('categoryGrid');

// Initialize category grid with pagination
function initializeCategoryGrid() {
    const gridContainer = document.createElement('div');
    gridContainer.className = 'category-grid-container';
    
    // Create navigation buttons
    const prevButton = document.createElement('button');
    prevButton.className = 'nav-button prev-button';
    prevButton.innerHTML = '<i class="fas fa-chevron-left"></i>';
    
    const nextButton = document.createElement('button');
    nextButton.className = 'nav-button next-button';
    nextButton.innerHTML = '<i class="fas fa-chevron-right"></i>';
    
    // Create dots container
    const dotsContainer = document.createElement('div');
    dotsContainer.className = 'pagination-dots';
    
    // Create grid wrapper
    const gridWrapper = document.createElement('div');
    gridWrapper.className = 'category-grid-wrapper';
    
    function renderCurrentPage() {
        gridWrapper.innerHTML = '';
        
        categoryPages[currentPage].forEach(category => {
            const categoryElement = document.createElement('div');
            categoryElement.className = 'category-item';
            categoryElement.innerHTML = `
                <i class="fas ${category.icon}"></i>
                <span>${category.name}</span>
            `;
            categoryElement.addEventListener('click', () => handleCategorySelection(category.id));
            gridWrapper.appendChild(categoryElement);
        });
        
        // Update pagination dots
        dotsContainer.innerHTML = '';
        for (let i = 0; i < categoryPages.length; i++) {
            const dot = document.createElement('div');
            dot.className = `pagination-dot ${i === currentPage ? 'active' : ''}`;
            dot.addEventListener('click', () => {
                currentPage = i;
                renderCurrentPage();
            });
            dotsContainer.appendChild(dot);
        }
        
        // Update button states
        prevButton.disabled = currentPage === 0;
        nextButton.disabled = currentPage === categoryPages.length - 1;
    }
    
    // Add navigation event listeners
    prevButton.addEventListener('click', () => {
        if (currentPage > 0) {
            currentPage--;
            renderCurrentPage();
        }
    });
    
    nextButton.addEventListener('click', () => {
        if (currentPage < categoryPages.length - 1) {
            currentPage++;
            renderCurrentPage();
        }
    });
    
    // Assemble the grid container
    gridContainer.appendChild(prevButton);
    gridContainer.appendChild(gridWrapper);
    gridContainer.appendChild(nextButton);
    categoryGrid.innerHTML = '';
    categoryGrid.appendChild(gridContainer);
    categoryGrid.appendChild(dotsContainer);
    
    // Initial render
    renderCurrentPage();
}

// Handle category selection
function handleCategorySelection(categoryId) {
    const filteredProviders = serviceProvidersData.filter(
        provider => provider.category === categoryId
    );
    displayServiceProviders(filteredProviders);
}

// Display service providers
function displayServiceProviders(providers) {
    const servicesContainer = document.createElement('div');
    servicesContainer.className = 'services-container';

    providers.forEach(provider => {
        const serviceCard = createServiceCard(provider);
        servicesContainer.appendChild(serviceCard);
    });

    // Clear previous results and add new ones
    chatMessages.innerHTML = '';
    chatMessages.appendChild(categoryGrid);
    chatMessages.appendChild(servicesContainer);
}


// Create service card
        function createServiceCard(provider) {
            const card = document.createElement('div');
            card.className = 'service-card';
            card.innerHTML = `
                <div class="service-header">
                    <h3>${provider.name}</h3>
                    <div class="service-rating">
                        <i class="fas fa-star"></i>
                        <span>${provider.rating}</span>
                        <span>(${provider.reviews} reviews)</span>
                    </div>
                </div>
                <div class="service-details">
                    <div class="service-detail-item">
                        <i class="fas fa-map-marker-alt"></i>
                        <span>${provider.city}, ${provider.pincode}</span>
                    </div>
                    <div class="service-detail-item">
                        <i class="fas fa-clock"></i>
                        <span>${provider.availability}</span>
                    </div>
                    <div class="service-detail-item">
                        <i class="fas fa-money-bill-alt"></i>
                        <span>${provider.price}</span>
                    </div>
                    <div class="service-detail-item">
                        <i class="fas fa-briefcase"></i>
                        <span>${provider.experience}</span>
                    </div>
                </div>
                <div class="service-actions">
                    <button class="service-button book-button" onclick="openBookingModal('${provider.id}')">
                        <i class="fas fa-calendar-check"></i>
                        Book Now
                    </button>
                    <button class="service-button contact-button" onclick="initiateCall('${provider.phone}')">
                        <i class="fas fa-phone"></i>
                        Contact
                    </button>
                    <button class="service-button contact-button" onclick="showProviderInfo('${provider.id}')">
                        <i class="fas fa-info-circle"></i>
                        Info
                    </button>
                </div>
            `;
            return card;
        }

        // Search functionality
        function searchServices(query) {
            const keywords = query.toLowerCase().split(' ');
            const matchingProviders = serviceProvidersData.filter(provider => {
                return keywords.some(keyword => 
                    provider.name.toLowerCase().includes(keyword) ||
                    provider.category.toLowerCase().includes(keyword) ||
                    provider.city.toLowerCase().includes(keyword) ||
                    provider.skills.some(skill => skill.toLowerCase().includes(keyword))
                );
            });
            displayServiceProviders(matchingProviders);
        }

// Booking functionality
function openBookingModal(providerId) {
    const provider = serviceProvidersData.find(p => p.id === parseInt(providerId));
    if (!provider) return;

    bookingModal.classList.add('active');

    // Initialize flatpickr for date and time inputs
    flatpickr("#bookingDate", {
        minDate: "today",
        dateFormat: "Y-m-d"
    });

    flatpickr("#bookingTime", {
        enableTime: true,
        noCalendar: true,
        dateFormat: "H:i",
        minTime: "09:00",
        maxTime: "18:00"
    });
}

// Handle booking submission
document.getElementById('bookingForm').addEventListener('submit', (e) => {
    e.preventDefault();

    const bookingData = {
        name: document.getElementById('userName').value,
        contact: document.getElementById('userContact').value,
        address: document.getElementById('userAddress').value,
        date: document.getElementById('bookingDate').value,
        time: document.getElementById('bookingTime').value,
        description: document.getElementById('workDescription').value
    };

    // Send booking to WhatsApp
    const whatsappMessage = encodeURIComponent(
        `New Booking Request:\n` +
        `Name: ${bookingData.name}\n` +
        `Contact: ${bookingData.contact}\n` +
        `Address: ${bookingData.address}\n` +
        `Date: ${bookingData.date}\n` +
        `Time: ${bookingData.time}\n` +
        `Description: ${bookingData.description}`
    );

    window.open(`https://wa.me/919111478356?text=${whatsappMessage}`, '_blank');

    // Close modal and show success message
    bookingModal.classList.remove('active');
    showAlert('Booking request sent successfully!', 'success');
});

        // Show provider info
        function showProviderInfo(providerId) {
            const provider = serviceProvidersData.find(p => p.id === parseInt(providerId));
            if (!provider) return;

            const providerDetails = document.getElementById('providerDetails');
            providerDetails.innerHTML = `
                <div class="provider-detail-item">
                    <i class="fas fa-user"></i>
                    <span>${provider.name}</span>
                </div>
                <div class="provider-detail-item">
                    <i class="fas fa-star"></i>
                    <span>${provider.rating} (${provider.reviews} reviews)</span>
                </div>
                <div class="provider-detail-item">
                    <i class="fas fa-briefcase"></i>
                    <span>${provider.experience}</span>
                </div>
                <div class="provider-detail-item">
                    <i class="fas fa-tools"></i>
                    <span>${provider.skills.join(', ')}</span>
                </div>
                <div class="provider-detail-item">
                    <i class="fas fa-language"></i>
                    <span>${provider.languages.join(', ')}</span>
                </div>
                <div class="provider-detail-item">
                    <i class="fas fa-certificate"></i>
                    <span>${provider.certifications.join(', ')}</span>
                </div>
                <div class="provider-detail-item">
                    <i class="fas fa-info-circle"></i>
                    <span>${provider.description}</span>
                </div>
            `;
            
            providerInfoModal.classList.add('active');
        }

        // Show alert message
        function showAlert(message, type) {
            const alert = document.createElement('div');
            alert.className = `alert alert-${type}`;
            alert.textContent = message;
            document.body.appendChild(alert);

            setTimeout(() => {
                alert.remove();
            }, 3000);
        }

// Initialize event listeners
document.addEventListener('DOMContentLoaded', () => {
    initializeCategoryGrid();
    initializeEventListeners();
    
    // Add chat input handlers
    chatInput.addEventListener('keypress', (e) => {
        if (e.key === 'Enter') {
            handleUserInput();
        }
    });
    
    sendButton.addEventListener('click', handleUserInput);
});

function initializeEventListeners() {
    // Filter modal events
    document.getElementById('filterButton').addEventListener('click', () => {
        filterModal.classList.add('active');
    });

    document.getElementById('closeFilter').addEventListener('click', () => {
        filterModal.classList.remove('active');
    });

    document.getElementById('closeBooking').addEventListener('click', () => {
        bookingModal.classList.remove('active');
    });

    document.getElementById('closeProviderInfo').addEventListener('click', () => {
        providerInfoModal.classList.remove('active');
    });

    // Chat input events
    chatInput.addEventListener('keypress', (e) => {
        if (e.key === 'Enter') {
            handleUserInput();
        }
    });

    sendButton.addEventListener('click', handleUserInput);

    // Filter options selection
    const filterOptions = document.querySelectorAll('.filter-option');
    filterOptions.forEach(option => {
        option.addEventListener('click', () => {
            option.classList.toggle('active');
        });
    });

    // Apply filters
    document.getElementById('applyFilters').addEventListener('click', applyFilters);
}

// Initialize when DOM is loaded
document.addEventListener('DOMContentLoaded', () => {
    initializeCategoryGrid();
});

// Handle user input
function handleUserInput() {
    const message = chatInput.value.trim();
    if (!message) return;

    // Add user message to chat
    addMessageToChat(message, 'user');
    
    // Process the message
    processUserMessage(message);
    
    // Clear input
    chatInput.value = '';
}

// Add message to chat
function addMessageToChat(message, type) {
    const messageDiv = document.createElement('div');
    messageDiv.className = `message ${type}-message`;
    
    const avatar = document.createElement('div');
    avatar.className = 'message-avatar';
    avatar.innerHTML = `<i class="fas fa-${type === 'user' ? 'user' : 'robot'}"></i>`;
    
    const content = document.createElement('div');
    content.className = 'message-content';
    content.textContent = message;
    
    messageDiv.appendChild(type === 'user' ? content : avatar);
    messageDiv.appendChild(type === 'user' ? avatar : content);
    
    chatMessages.appendChild(messageDiv);
    chatMessages.scrollTop = chatMessages.scrollHeight;
}

// Process user message
function processUserMessage(message) {
    const lowerMessage = message.toLowerCase();
    
    // Check for category keywords
    const category = categories.find(cat => 
        lowerMessage.includes(cat.name.toLowerCase())
    );
    
    if (category) {
        handleCategorySelection(category.id);
        addMessageToChat(`Here are the available ${category.name} services:`, 'bot');
    } else {
        // Perform general search
        searchServices(message);
        addMessageToChat('Here are the services matching your search:', 'bot');
    }
}

// Apply filters function
function applyFilters() {
    const selectedCategories = Array.from(document.querySelectorAll('#categoryFilter .filter-option.active'))
        .map(option => option.dataset.category);

    const selectedCities = Array.from(document.querySelectorAll('#cityFilter .filter-option.active'))
        .map(option => option.dataset.city);

    const selectedPrices = Array.from(document.querySelectorAll('#priceFilter .filter-option.active'))
        .map(option => option.dataset.price);

    const selectedRatings = Array.from(document.querySelectorAll('#ratingFilter .filter-option.active'))
        .map(option => parseFloat(option.dataset.rating));

    const selectedExperience = Array.from(document.querySelectorAll('#experienceFilter .filter-option.active'))
        .map(option => parseInt(option.dataset.exp));

    let filteredProviders = serviceProvidersData.filter(provider => {
        // Category filter
        const categoryMatch = selectedCategories.length === 0 || 
            selectedCategories.includes(provider.category.toLowerCase());

        // City filter
        const cityMatch = selectedCities.length === 0 || 
            selectedCities.includes(provider.city.toLowerCase());

        // Price filter
        const priceMatch = selectedPrices.length === 0 || selectedPrices.some(price => {
            const providerPrice = parseInt(provider.price.replace(/[^\d]/g, ''));
            switch(price) {
                case 'low': return providerPrice <= 500;
                case 'medium': return providerPrice > 500 && providerPrice <= 1000;
                case 'high': return providerPrice > 1000;
                default: return true;
            }
        });

        // Rating filter
        const ratingMatch = selectedRatings.length === 0 || 
            selectedRatings.some(rating => provider.rating >= rating);

        // Experience filter
        const experienceMatch = selectedExperience.length === 0 ||
            selectedExperience.some(exp => parseInt(provider.experience) >= exp);

        return categoryMatch && cityMatch && priceMatch && ratingMatch && experienceMatch;
    });

    displayServiceProviders(filteredProviders);
    filterModal.classList.remove('active');

    // Show filter summary with active filters
    let summaryParts = [];
    if (selectedCategories.length) summaryParts.push(`Category: ${selectedCategories.join(', ')}`);
    if (selectedCities.length) summaryParts.push(`City: ${selectedCities.join(', ')}`);
    if (selectedPrices.length) summaryParts.push(`Price Range: ${selectedPrices.join(', ')}`);
    if (selectedRatings.length) summaryParts.push(`Rating: ${selectedRatings.join('+ stars, ')}+ stars`);
    if (selectedExperience.length) summaryParts.push(`Experience: ${selectedExperience.join('+ years, ')}+ years`);

    // Show filter summary
    const filterSummary = `Showing ${filteredProviders.length} services matching your filters`;
    addMessageToChat(filterSummary, 'bot');
}

// Initiate call
function initiateCall(phoneNumber) {
    window.location.href = `tel:${phoneNumber}`;
}

// Close modals when clicking outside
window.addEventListener('click', (e) => {
    if (e.target === filterModal) {
        filterModal.classList.remove('active');
    }
    if (e.target === bookingModal) {
        bookingModal.classList.remove('active');
    }
    if (e.target === providerInfoModal) {
        providerInfoModal.classList.remove('active');
    }
});

// Handle errors
window.onerror = function(msg, url, lineNo, columnNo, error) {
    console.error('Error: ', msg, 'URL: ', url, 'Line: ', lineNo, 'Column: ', columnNo, 'Error object: ', error);
    showAlert('An error occurred. Please try again.', 'error');
    return false;
};
</script>
</body>
