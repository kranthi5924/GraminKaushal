<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Gramina Kaushal</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        .screen {
            display: none;
        }
        .screen.active {
            display: block;
        }
        .slide-in {
            animation: slideIn 0.3s ease-out;
        }
        @keyframes slideIn {
            from {
                opacity: 0;
                transform: translateY(20px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }
        .modal {
            display: none;
            position: fixed;
            z-index: 1000;
            left: 0;
            top: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0,0,0,0.5);
        }
        .modal.active {
            display: flex;
            align-items: center;
            justify-content: center;
        }
        .notification-badge {
            animation: pulse 2s infinite;
        }
        @keyframes pulse {
            0% {
                box-shadow: 0 0 0 0 rgba(239, 68, 68, 0.7);
            }
            70% {
                box-shadow: 0 0 0 10px rgba(239, 68, 68, 0);
            }
            100% {
                box-shadow: 0 0 0 0 rgba(239, 68, 68, 0);
            }
        }
        .chat-bubble {
            position: relative;
            background: #f3f4f6;
            border-radius: 18px;
            padding: 12px 16px;
            margin-bottom: 8px;
            max-width: 80%;
            word-wrap: break-word;
        }
        .chat-bubble.user {
            background: #3b82f6;
            color: white;
            margin-left: auto;
        }
        .chat-bubble.admin {
            background: #10b981;
            color: white;
        }
        .chat-bubble.pending {
            background: #f59e0b;
            color: white;
            margin-left: auto;
        }
        .unread-badge {
            background: #ef4444;
            color: white;
            border-radius: 50%;
            width: 20px;
            height: 20px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 12px;
            font-weight: bold;
        }
        .chat-contact-btn {
            position: fixed;
            bottom: 20px;
            right: 20px;
            z-index: 100;
            background: linear-gradient(45deg, #3b82f6, #1d4ed8);
            color: white;
            border-radius: 50%;
            width: 60px;
            height: 60px;
            display: flex;
            align-items: center;
            justify-content: center;
            box-shadow: 0 4px 20px rgba(59, 130, 246, 0.4);
            transition: all 0.3s ease;
        }
        .chat-contact-btn:hover {
            transform: scale(1.1);
            box-shadow: 0 6px 25px rgba(59, 130, 246, 0.6);
        }
        .chat-logo {
            position: absolute;
            bottom: -10px;
            left: 50%;
            transform: translateX(-50%);
            background: white;
            border-radius: 50%;
            width: 40px;
            height: 40px;
            display: flex;
            align-items: center;
            justify-content: center;
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
            border: 2px solid #e5e7eb;
        }
        .message-timestamp {
            font-size: 11px;
            opacity: 0.7;
            margin-top: 4px;
        }
        .message-status {
            font-size: 10px;
            opacity: 0.6;
            margin-top: 2px;
        }
    </style>
</head>
<body class="bg-gradient-to-br from-blue-50 to-indigo-100 min-h-screen">
    
    <!-- Language Toggle and Voice (Fixed) -->
    <div class="fixed top-4 right-4 z-50 flex items-center space-x-2">
        <button id="voiceToggle" onclick="toggleVoice()" class="bg-white border-2 border-purple-500 text-purple-700 rounded-full p-2 shadow-lg hover:shadow-xl transition" title="Toggle Voice">
            <svg id="voiceIcon" class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15.536 8.464a5 5 0 010 7.072m2.828-9.9a9 9 0 010 12.728M5.586 15H4a1 1 0 01-1-1v-4a1 1 0 011-1h1.586l4.707-4.707C10.923 3.663 12 4.109 12 5v14c0 .891-1.077 1.337-1.707.707L5.586 15z"></path>
            </svg>
        </button>
        <select id="languageToggle" class="bg-white border-2 border-indigo-500 text-indigo-700 rounded-full px-4 py-2 font-semibold shadow-lg cursor-pointer">
            <option value="en">English</option>
            <option value="te">తెలుగు</option>
            <option value="hi">हिंदी</option>
        </select>
    </div>

    <!-- Contact Admin Button -->
    <div id="contactAdminBtn" class="chat-contact-btn hidden" onclick="openChat()">
        <svg class="w-8 h-8" fill="none" stroke="currentColor" viewBox="0 0 24 24">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 12h.01M12 12h.01M16 12h.01M21 12c0 4.418-4.03 8-9 8a9.863 9.863 0 01-4.255-.949L3 20l1.395-3.72C3.512 15.042 3 13.574 3 12c0-4.418 4.03-8 9-8s9 3.582 9 8z"></path>
        </svg>
    </div>

    <!-- Home Screen -->
    <div id="homeScreen" class="screen active">
        <div class="container mx-auto px-4 py-8 max-w-md min-h-screen flex flex-col justify-center">
            <div class="text-center mb-12 slide-in">
                <h1 class="text-4xl font-bold text-indigo-900 mb-2" data-lang="appTitle">Gramina Kaushal</h1>
                <p class="text-gray-600" data-lang="appSubtitle">Find professionals for your needs</p>
            </div>
            
            <div class="space-y-4">
                <button onclick="showScreen('customerLoginScreen')" class="w-full bg-gradient-to-r from-blue-500 to-blue-600 text-white py-4 rounded-xl shadow-lg hover:shadow-xl transform hover:scale-105 transition duration-200 font-semibold text-lg">
                    <span data-lang="customerLogin">👤 Customer Login/Register</span>
                </button>
                
                <button onclick="showScreen('workerLoginScreen')" class="w-full bg-gradient-to-r from-green-500 to-green-600 text-white py-4 rounded-xl shadow-lg hover:shadow-xl transform hover:scale-105 transition duration-200 font-semibold text-lg">
                    <span data-lang="workerLogin">🔧 Worker Login/Register</span>
                </button>

                <button onclick="showScreen('adminLoginScreen')" class="w-full bg-gradient-to-r from-purple-500 to-purple-600 text-white py-4 rounded-xl shadow-lg hover:shadow-xl transform hover:scale-105 transition duration-200 font-semibold text-lg">
                    <span data-lang="adminLogin">⚙️ Admin Login</span>
                </button>
            </div>

            <div class="mt-12 text-center">
                <p class="text-gray-500 text-sm" data-lang="serviceTypes">Services: Plumber | Electrician | Carpenter</p>
            </div>
        </div>
    </div>

    <!-- Admin Login Screen -->
    <div id="adminLoginScreen" class="screen">
        <div class="container mx-auto px-4 py-8 max-w-md">
            <button onclick="showScreen('homeScreen')" class="mb-4 text-indigo-600 hover:text-indigo-800">
                ← <span data-lang="back">Back</span>
            </button>
            
            <div class="bg-white rounded-2xl shadow-xl p-6">
                <h2 class="text-2xl font-bold text-gray-800 mb-6" data-lang="adminLoginTitle">Admin Login</h2>
                
                <form id="adminLoginForm" class="space-y-4">
                    <div>
                        <label class="block text-gray-700 mb-2 font-semibold" data-lang="password">Password</label>
                        <input type="password" id="adminPassword" class="w-full px-4 py-3 border-2 border-gray-300 rounded-lg focus:border-purple-500 focus:outline-none" placeholder="Enter admin password">
                        <span class="text-red-500 text-sm hidden" id="adminPasswordError"></span>
                    </div>
                    
                    <button type="submit" class="w-full bg-purple-600 text-white py-3 rounded-lg font-semibold hover:bg-purple-700 transition">
                        <span data-lang="login">Login</span>
                    </button>
                </form>
            </div>
        </div>
    </div>

    <!-- Customer Login Screen -->
    <div id="customerLoginScreen" class="screen">
        <div class="container mx-auto px-4 py-8 max-w-md">
            <button onclick="showScreen('homeScreen')" class="mb-4 text-indigo-600 hover:text-indigo-800">
                ← <span data-lang="back">Back</span>
            </button>
            
            <div class="bg-white rounded-2xl shadow-xl p-6">
                <div class="text-center mb-6">
                    <div class="mx-auto w-20 h-20 bg-blue-100 rounded-full flex items-center justify-center">
                        <svg class="w-12 h-12 text-blue-500" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M16 7a4 4 0 11-8 0 4 4 0 018 0zM12 14a7 7 0 00-7 7h14a7 7 0 00-7-7z"></path></svg>
                    </div>
                </div>
                <h2 class="text-2xl font-bold text-gray-800 mb-6 text-center" data-lang="customerLoginTitle">Customer Login</h2>
                
                <form id="customerLoginForm" class="space-y-4">
                    <div>
                        <label class="block text-gray-700 mb-2 font-semibold" data-lang="mobile">Mobile Number</label>
                        <div class="flex items-center">
                            <span class="inline-flex items-center px-3 border-2 border-r-0 border-gray-300 bg-gray-50 text-gray-500 rounded-l-lg py-3">+91</span>
                            <input type="tel" id="custLoginMobile" class="flex-1 px-4 py-3 border-2 border-gray-300 rounded-r-lg focus:border-blue-500 focus:outline-none" placeholder="10 digit number" maxlength="10">
                        </div>
                        <span class="text-red-500 text-sm hidden" id="custLoginMobileError"></span>
                    </div>
                    
                    <div id="custLoginPasswordContainer">
                        <label class="block text-gray-700 mb-2 font-semibold" data-lang="password">Password</label>
                        <input type="password" id="custLoginPassword" class="w-full px-4 py-3 border-2 border-gray-300 rounded-lg focus:border-blue-500 focus:outline-none" placeholder="Minimum 6 characters">
                        <span class="text-red-500 text-sm hidden" id="custLoginPasswordError"></span>
                    </div>
                    
                    <div id="custLoginOTPContainer" class="hidden">
                         <label class="block text-gray-700 mb-2 font-semibold" data-lang="enterOTP">Enter OTP</label>
                         <input type="text" id="custLoginOTP" class="w-full px-4 py-3 border-2 border-gray-300 rounded-lg focus:border-blue-500 focus:outline-none" placeholder="6 digit OTP" maxlength="6">
                         <span class="text-red-500 text-sm hidden" id="custLoginOTPError"></span>
                    </div>
                    
                    <button type="submit" class="w-full bg-blue-600 text-white py-3 rounded-lg font-semibold hover:bg-blue-700 transition">
                        <span id="custLoginBtnText" data-lang="login">Login</span>
                    </button>
                    
                    <p class="text-center text-gray-500 text-sm">OR</p>
                    
                    <button type="button" onclick="handleCustomerOTPRequest()" class="w-full bg-gray-600 text-white py-3 rounded-lg font-semibold hover:bg-gray-700 transition">
                        <span id="custOtpBtnText">Login with OTP</span>
                    </button>
                </form>
                
                <div class="mt-4 text-center">
                    <button onclick="showScreen('customerForgotPasswordScreen')" class="text-blue-600 hover:underline text-sm" data-lang="forgotPassword">Forgot Password?</button>
                </div>
                
                <div class="mt-6 text-center">
                    <p class="text-gray-600 mb-2"><span data-lang="noAccount">Don't have an account?</span></p>
                    <button onclick="showScreen('customerRegisterScreen')" class="text-blue-600 font-semibold hover:underline" data-lang="registerNow">Register Now</button>
                </div>
            </div>
        </div>
    </div>

    <!-- Customer Register Screen -->
    <div id="customerRegisterScreen" class="screen">
        <div class="container mx-auto px-4 py-8 max-w-md">
            <button onclick="showScreen('customerLoginScreen')" class="mb-4 text-indigo-600 hover:text-indigo-800">
                ← <span data-lang="back">Back</span>
            </button>
            
            <div class="bg-white rounded-2xl shadow-xl p-6">
                <div class="text-center mb-6">
                    <div class="mx-auto w-20 h-20 bg-blue-100 rounded-full flex items-center justify-center">
                        <svg class="w-12 h-12 text-blue-500" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M16 7a4 4 0 11-8 0 4 4 0 018 0zM12 14a7 7 0 00-7 7h14a7 7 0 00-7-7z"></path></svg>
                    </div>
                </div>
                <h2 class="text-2xl font-bold text-gray-800 mb-6 text-center" data-lang="customerRegisterTitle">Customer Registration</h2>
                
                <form id="customerRegisterForm" class="space-y-4">
                    <div>
                        <label class="block text-gray-700 mb-2 font-semibold" data-lang="name">Name</label>
                        <input type="text" id="custRegName" class="w-full px-4 py-3 border-2 border-gray-300 rounded-lg focus:border-blue-500 focus:outline-none" placeholder="Enter your name">
                        <span class="text-red-500 text-sm hidden" id="custRegNameError"></span>
                    </div>
                    
                    <div>
                        <label class="block text-gray-700 mb-2 font-semibold" data-lang="mobile">Mobile Number</label>
                        <div class="flex items-center">
                            <span class="inline-flex items-center px-3 border-2 border-r-0 border-gray-300 bg-gray-50 text-gray-500 rounded-l-lg py-3">+91</span>
                            <input type="tel" id="custRegMobile" class="flex-1 px-4 py-3 border-2 border-gray-300 rounded-r-lg focus:border-blue-500 focus:outline-none" placeholder="10 digit number" maxlength="10">
                        </div>
                        <span class="text-red-500 text-sm hidden" id="custRegMobileError"></span>
                    </div>
                    
                    <div>
                        <label class="block text-gray-700 mb-2 font-semibold" data-lang="password">Password</label>
                        <input type="password" id="custRegPassword" class="w-full px-4 py-3 border-2 border-gray-300 rounded-lg focus:border-blue-500 focus:outline-none" placeholder="Minimum 6 characters">
                        <span class="text-red-500 text-sm hidden" id="custRegPasswordError"></span>
                    </div>
                    
                    <div>
                        <label class="block text-gray-700 mb-2 font-semibold" data-lang="location">Location</label>
                        <input type="text" id="custRegLocation" class="w-full px-4 py-3 border-2 border-gray-300 rounded-lg focus:border-blue-500 focus:outline-none" placeholder="Enter your location">
                        <span class="text-red-500 text-sm hidden" id="custRegLocationError"></span>
                    </div>
                    
                    <button type="submit" class="w-full bg-blue-600 text-white py-3 rounded-lg font-semibold hover:bg-blue-700 transition">
                        <span data-lang="register">Register</span>
                    </button>
                </form>
            </div>
        </div>
    </div>

    <!-- Customer Forgot Password Screen -->
    <div id="customerForgotPasswordScreen" class="screen">
        <div class="container mx-auto px-4 py-8 max-w-md">
            <button onclick="showScreen('customerLoginScreen')" class="mb-4 text-indigo-600 hover:text-indigo-800">
                ← <span data-lang="back">Back</span>
            </button>
            
            <div class="bg-white rounded-2xl shadow-xl p-6">
                <h2 class="text-2xl font-bold text-gray-800 mb-6" data-lang="resetPassword">Reset Password</h2>
                
                <div id="custForgotStep1">
                    <form id="customerForgotForm" class="space-y-4">
                        <div>
                            <label class="block text-gray-700 mb-2 font-semibold" data-lang="mobile">Mobile Number</label>
                             <div class="flex items-center">
                                <span class="inline-flex items-center px-3 border-2 border-r-0 border-gray-300 bg-gray-50 text-gray-500 rounded-l-lg py-3">+91</span>
                                <input type="tel" id="custForgotMobile" class="flex-1 px-4 py-3 border-2 border-gray-300 rounded-r-lg focus:border-blue-500 focus:outline-none" placeholder="10 digit number" maxlength="10">
                            </div>
                            <span class="text-red-500 text-sm hidden" id="custForgotMobileError"></span>
                        </div>
                        
                        <button type="submit" class="w-full bg-blue-600 text-white py-3 rounded-lg font-semibold hover:bg-blue-700 transition">
                            <span data-lang="sendOTP">Send OTP</span>
                        </button>
                    </form>
                </div>
                
                <div id="custForgotStep2" class="hidden">
                    <form id="customerVerifyOTPForm" class="space-y-4">
                        <div>
                            <label class="block text-gray-700 mb-2 font-semibold" data-lang="enterOTP">Enter OTP</label>
                            <input type="text" id="custForgotOTP" class="w-full px-4 py-3 border-2 border-gray-300 rounded-lg focus:border-blue-500 focus:outline-none" placeholder="6 digit OTP" maxlength="6">
                            <p class="text-sm text-gray-500 mt-1"><span data-lang="otpSent">OTP sent to your mobile:</span> +91<span id="custOTPMobile" class="font-semibold"></span></p>
                            <span class="text-red-500 text-sm hidden" id="custForgotOTPError"></span>
                        </div>
                        
                        <div>
                            <label class="block text-gray-700 mb-2 font-semibold" data-lang="newPassword">New Password</label>
                            <input type="password" id="custNewPassword" class="w-full px-4 py-3 border-2 border-gray-300 rounded-lg focus:border-blue-500 focus:outline-none" placeholder="Minimum 6 characters">
                            <span class="text-red-500 text-sm hidden" id="custNewPasswordError"></span>
                        </div>
                        
                        <button type="submit" class="w-full bg-blue-600 text-white py-3 rounded-lg font-semibold hover:bg-blue-700 transition">
                            <span data-lang="resetPasswordBtn">Reset Password</span>
                        </button>
                    </form>
                </div>
            </div>
        </div>
    </div>

    <!-- Worker Login Screen -->
    <div id="workerLoginScreen" class="screen">
        <div class="container mx-auto px-4 py-8 max-w-md">
            <button onclick="showScreen('homeScreen')" class="mb-4 text-indigo-600 hover:text-indigo-800">
                ← <span data-lang="back">Back</span>
            </button>
            
            <div class="bg-white rounded-2xl shadow-xl p-6">
                 <div class="text-center mb-6">
                    <div class="mx-auto w-20 h-20 bg-green-100 rounded-full flex items-center justify-center">
                        <svg class="w-12 h-12 text-green-500" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 14v3m4-3v3m4-3v3M3 21h18M3 10h18M3 7l9-4 9 4M4 10h16v11H4V10z"></path></svg>
                    </div>
                </div>
                <h2 class="text-2xl font-bold text-gray-800 mb-6 text-center" data-lang="workerLoginTitle">Worker Login</h2>
                
                <form id="workerLoginForm" class="space-y-4">
                    <div>
                        <label class="block text-gray-700 mb-2 font-semibold" data-lang="mobile">Mobile Number</label>
                        <div class="flex items-center">
                            <span class="inline-flex items-center px-3 border-2 border-r-0 border-gray-300 bg-gray-50 text-gray-500 rounded-l-lg py-3">+91</span>
                            <input type="tel" id="workerLoginMobile" class="flex-1 px-4 py-3 border-2 border-gray-300 rounded-r-lg focus:border-green-500 focus:outline-none" placeholder="10 digit number" maxlength="10">
                        </div>
                        <span class="text-red-500 text-sm hidden" id="workerLoginMobileError"></span>
                    </div>
                    
                    <div id="workerLoginPasswordContainer">
                        <label class="block text-gray-700 mb-2 font-semibold" data-lang="password">Password</label>
                        <input type="password" id="workerLoginPassword" class="w-full px-4 py-3 border-2 border-gray-300 rounded-lg focus:border-green-500 focus:outline-none" placeholder="Minimum 6 characters">
                        <span class="text-red-500 text-sm hidden" id="workerLoginPasswordError"></span>
                    </div>

                    <div id="workerLoginOTPContainer" class="hidden">
                         <label class="block text-gray-700 mb-2 font-semibold" data-lang="enterOTP">Enter OTP</label>
                         <input type="text" id="workerLoginOTP" class="w-full px-4 py-3 border-2 border-gray-300 rounded-lg focus:border-green-500 focus:outline-none" placeholder="6 digit OTP" maxlength="6">
                         <span class="text-red-500 text-sm hidden" id="workerLoginOTPError"></span>
                    </div>
                    
                    <button type="submit" class="w-full bg-green-600 text-white py-3 rounded-lg font-semibold hover:bg-green-700 transition">
                        <span id="workerLoginBtnText" data-lang="login">Login</span>
                    </button>

                    <p class="text-center text-gray-500 text-sm">OR</p>
                    
                    <button type="button" onclick="handleWorkerOTPRequest()" class="w-full bg-gray-600 text-white py-3 rounded-lg font-semibold hover:bg-gray-700 transition">
                        <span id="workerOtpBtnText">Login with OTP</span>
                    </button>
                </form>
                
                <div class="mt-4 text-center">
                    <button onclick="showScreen('workerForgotPasswordScreen')" class="text-green-600 hover:underline text-sm" data-lang="forgotPassword">Forgot Password?</button>
                </div>
                
                <div class="mt-6 text-center">
                    <p class="text-gray-600 mb-2"><span data-lang="noAccount">Don't have an account?</span></p>
                    <button onclick="showScreen('workerRegisterScreen')" class="text-green-600 font-semibold hover:underline" data-lang="registerNow">Register Now</button>
                </div>
            </div>
        </div>
    </div>

    <!-- Worker Register Screen -->
    <div id="workerRegisterScreen" class="screen">
        <div class="container mx-auto px-4 py-8 max-w-md">
            <button onclick="showScreen('workerLoginScreen')" class="mb-4 text-indigo-600 hover:text-indigo-800">
                ← <span data-lang="back">Back</span>
            </button>
            
            <div class="bg-white rounded-2xl shadow-xl p-6">
                <h2 class="text-2xl font-bold text-gray-800 mb-6" data-lang="workerRegisterTitle">Worker Registration</h2>
                
                <form id="workerRegisterForm" class="space-y-4">
                    <div>
                        <label class="block text-gray-700 mb-2 font-semibold" data-lang="name">Name</label>
                        <input type="text" id="workerRegName" class="w-full px-4 py-3 border-2 border-gray-300 rounded-lg focus:border-green-500 focus:outline-none" placeholder="Enter your name">
                        <span class="text-red-500 text-sm hidden" id="workerRegNameError"></span>
                    </div>
                    
                    <div>
                        <label class="block text-gray-700 mb-2 font-semibold" data-lang="mobile">Mobile Number</label>
                        <input type="tel" id="workerRegMobile" class="w-full px-4 py-3 border-2 border-gray-300 rounded-lg focus:border-green-500 focus:outline-none" placeholder="10 digit number" maxlength="10">
                        <span class="text-red-500 text-sm hidden" id="workerRegMobileError"></span>
                    </div>
                    
                    <div>
                        <label class="block text-gray-700 mb-2 font-semibold" data-lang="password">Password</label>
                        <input type="password" id="workerRegPassword" class="w-full px-4 py-3 border-2 border-gray-300 rounded-lg focus:border-green-500 focus:outline-none" placeholder="Minimum 6 characters">
                        <span class="text-red-500 text-sm hidden" id="workerRegPasswordError"></span>
                    </div>
                    
                    <div>
                        <label class="block text-gray-700 mb-2 font-semibold" data-lang="profession">Profession</label>
                        <select id="workerRegProfession" class="w-full px-4 py-3 border-2 border-gray-300 rounded-lg focus:border-green-500 focus:outline-none">
                            <option value="">Select Profession</option>
                            <option value="Plumber" data-lang-option="plumber">Plumber</option>
                            <option value="Electrician" data-lang-option="electrician">Electrician</option>
                            <option value="Carpenter" data-lang-option="carpenter">Carpenter</option>
                        </select>
                        <span class="text-red-500 text-sm hidden" id="workerRegProfessionError"></span>
                    </div>
                    
                    <div>
                        <label class="block text-gray-700 mb-2 font-semibold" data-lang="location">Location</label>
                        <input type="text" id="workerRegLocation" class="w-full px-4 py-3 border-2 border-gray-300 rounded-lg focus:border-green-500 focus:outline-none" placeholder="Enter your location">
                        <span class="text-red-500 text-sm hidden" id="workerRegLocationError"></span>
                    </div>
                    
                    <div>
                        <label class="block text-gray-700 mb-2 font-semibold" data-lang="photo">Photo</label>
                        <input type="file" id="workerRegPhoto" accept="image/*" class="w-full px-4 py-3 border-2 border-gray-300 rounded-lg focus:border-green-500 focus:outline-none" onchange="previewPhoto(this, 'workerRegPhotoPreview')">
                        <img id="workerRegPhotoPreview" class="mt-2 w-32 h-32 object-cover rounded-lg hidden" alt="Preview">
                    </div>
                    
                    <button type="submit" class="w-full bg-green-600 text-white py-3 rounded-lg font-semibold hover:bg-green-700 transition">
                        <span data-lang="register">Register</span>
                    </button>
                </form>
            </div>
        </div>
    </div>

    <!-- Worker Forgot Password Screen -->
    <div id="workerForgotPasswordScreen" class="screen">
        <div class="container mx-auto px-4 py-8 max-w-md">
            <button onclick="showScreen('workerLoginScreen')" class="mb-4 text-indigo-600 hover:text-indigo-800">
                ← <span data-lang="back">Back</span>
            </button>
            
            <div class="bg-white rounded-2xl shadow-xl p-6">
                <h2 class="text-2xl font-bold text-gray-800 mb-6" data-lang="resetPassword">Reset Password</h2>
                
                <div id="workerForgotStep1">
                    <form id="workerForgotForm" class="space-y-4">
                        <div>
                            <label class="block text-gray-700 mb-2 font-semibold" data-lang="mobile">Mobile Number</label>
                            <input type="tel" id="workerForgotMobile" class="w-full px-4 py-3 border-2 border-gray-300 rounded-lg focus:border-green-500 focus:outline-none" placeholder="10 digit number" maxlength="10">
                            <span class="text-red-500 text-sm hidden" id="workerForgotMobileError"></span>
                        </div>
                        
                        <button type="submit" class="w-full bg-green-600 text-white py-3 rounded-lg font-semibold hover:bg-green-700 transition">
                            <span data-lang="sendOTP">Send OTP</span>
                        </button>
                    </form>
                </div>
                
                <div id="workerForgotStep2" class="hidden">
                    <form id="workerVerifyOTPForm" class="space-y-4">
                        <div>
                            <label class="block text-gray-700 mb-2 font-semibold" data-lang="enterOTP">Enter OTP</label>
                            <input type="text" id="workerForgotOTP" class="w-full px-4 py-3 border-2 border-gray-300 rounded-lg focus:border-green-500 focus:outline-none" placeholder="6 digit OTP" maxlength="6">
                            <p class="text-sm text-gray-500 mt-1"><span data-lang="otpSent">OTP sent to your mobile:</span> <span id="workerOTPMobile" class="font-semibold"></span></p>
                            <span class="text-red-500 text-sm hidden" id="workerForgotOTPError"></span>
                        </div>
                        
                        <div>
                            <label class="block text-gray-700 mb-2 font-semibold" data-lang="newPassword">New Password</label>
                            <input type="password" id="workerNewPassword" class="w-full px-4 py-3 border-2 border-gray-300 rounded-lg focus:border-green-500 focus:outline-none" placeholder="Minimum 6 characters">
                            <span class="text-red-500 text-sm hidden" id="workerNewPasswordError"></span>
                        </div>
                        
                        <button type="submit" class="w-full bg-green-600 text-white py-3 rounded-lg font-semibold hover:bg-green-700 transition">
                            <span data-lang="resetPasswordBtn">Reset Password</span>
                        </button>
                    </form>
                </div>
            </div>
        </div>
    </div>

    <!-- Admin Dashboard -->
    <div id="adminDashboard" class="screen">
        <div class="container mx-auto px-4 py-8 max-w-6xl">
            <div class="flex justify-between items-center mb-6">
                <div>
                    <h2 class="text-3xl font-bold text-gray-800" data-lang="adminDashboard">Admin Dashboard</h2>
                    <p class="text-gray-600" data-lang="manageWorkers">Manage workers and bookings</p>
                </div>
                <div class="flex items-center space-x-4">
                    <button onclick="showMessagesModal()" class="bg-orange-600 text-white px-4 py-2 rounded-lg hover:bg-orange-700 transition relative">
                        <span data-lang="helpSupport">Help & Support</span>
                        <div id="adminMessageBadge" class="unread-badge hidden" style="position: absolute; top: -8px; right: -8px;">0</div>
                    </button>
                    <button onclick="logout()" class="bg-red-500 text-white px-4 py-2 rounded-lg hover:bg-red-600 transition">
                        <span data-lang="logout">Logout</span>
                    </button>
                </div>
            </div>

            <div class="grid grid-cols-1 md:grid-cols-4 gap-6 mb-8">
                <button onclick="showAddWorkerModal()" class="bg-purple-600 text-white p-6 rounded-xl shadow-lg hover:bg-purple-700 transition">
                    <h3 class="text-xl font-bold mb-2">➕ Add Worker</h3>
                    <p data-lang="addWorkerDesc">Register a new worker</p>
                </button>
                
                <button onclick="showEditWorkerModal()" class="bg-blue-600 text-white p-6 rounded-xl shadow-lg hover:bg-blue-700 transition">
                    <h3 class="text-xl font-bold mb-2">✏️ Edit Worker</h3>
                    <p data-lang="editWorkerDesc">Update worker details</p>
                </button>
                
                <button onclick="showBookingModal()" class="bg-green-600 text-white p-6 rounded-xl shadow-lg hover:bg-green-700 transition">
                    <h3 class="text-xl font-bold mb-2">📅 Book Worker</h3>
                    <p data-lang="bookWorkerDesc">Create booking for customer</p>
                </button>

                <button onclick="showMessagesModal()" class="bg-orange-600 text-white p-6 rounded-xl shadow-lg hover:bg-orange-700 transition relative">
                    <div class="flex items-center justify-between mb-2">
                        <h3 class="text-xl font-bold">💬 Messages</h3>
                        <div id="adminMessageBadgeMain" class="unread-badge hidden">0</div>
                    </div>
                    <p data-lang="messagesDesc">View user messages</p>
                </button>
            </div>

            <!-- Workers List -->
            <div class="bg-white rounded-xl shadow-md p-6">
                <h3 class="text-xl font-bold text-gray-800 mb-4" data-lang="allWorkers">All Workers</h3>
                <div id="adminWorkersList" class="overflow-x-auto">
                    <!-- Workers will be loaded here -->
                </div>
            </div>
        </div>
    </div>

    <!-- Customer Dashboard -->
    <div id="customerDashboard" class="screen">
        <div class="container mx-auto px-4 py-8 max-w-6xl">
            <div class="flex justify-between items-center mb-6">
                <div>
                    <h2 class="text-2xl font-bold text-gray-800"><span data-lang="welcomeCustomer">Welcome,</span> <span id="custDashName"></span>!</h2>
                    <p class="text-gray-600" data-lang="findWorkers">Find skilled workers for your needs</p>
                </div>
                <div class="flex items-center space-x-2">
                    <button onclick="logout()" class="bg-red-500 text-white px-4 py-2 rounded-lg hover:bg-red-600 transition">
                        <span data-lang="logout">Logout</span>
                    </button>
                </div>
            </div>

            <!-- Booking Notifications -->
            <div id="customerNotifications" class="mb-6">
                <!-- Notifications will be loaded here -->
            </div>

            <!-- Filter -->
            <div class="bg-white rounded-xl shadow-md p-4 mb-6">
                <label class="block text-gray-700 mb-2 font-semibold" data-lang="filterByProfession">Filter by Profession:</label>
                <select id="professionFilter" class="w-full px-4 py-3 border-2 border-gray-300 rounded-lg focus:border-blue-500 focus:outline-none">
                    <option value="">All Professions</option>
                    <option value="Plumber" data-lang-option="plumber">Plumber</option>
                    <option value="Electrician" data-lang-option="electrician">Electrician</option>
                    <option value="Carpenter" data-lang-option="carpenter">Carpenter</option>
                </select>
            </div>

            <!-- My Bookings -->
            <div class="bg-white rounded-xl shadow-md p-6 mb-6">
                <h3 class="text-xl font-bold text-gray-800 mb-4" data-lang="myBookings">My Bookings</h3>
                <div id="customerBookingsList" class="grid grid-cols-1 md:grid-cols-2 gap-4">
                    <!-- Customer bookings will be loaded here -->
                </div>
            </div>

            <!-- Workers List -->
            <div class="bg-white rounded-xl shadow-md p-6">
                <h3 class="text-xl font-bold text-gray-800 mb-4" data-lang="availableWorkers">Available Workers</h3>
                <div id="workersList" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
                    <!-- Workers will be loaded here -->
                </div>
            </div>
        </div>
    </div>

    <!-- Worker Dashboard -->
    <div id="workerDashboard" class="screen">
        <div class="container mx-auto px-4 py-8 max-w-md">
            <div class="flex justify-between items-center mb-6">
                <div>
                    <h2 class="text-2xl font-bold text-gray-800"><span data-lang="welcomeWorker">Welcome,</span> <span id="workerDashName"></span>!</h2>
                    <p class="text-gray-600"><span data-lang="yourProfession">Your Profession:</span> <span id="workerDashProfession" class="font-semibold"></span></p>
                </div>
                <div class="flex items-center space-x-2">
                    <div class="relative">
                        <button onclick="logout()" class="bg-red-500 text-white px-4 py-2 rounded-lg hover:bg-red-600 transition">
                            <span data-lang="logout">Logout</span>
                        </button>
                        <div id="notificationBadge" class="absolute -top-2 -right-2 bg-red-500 text-white rounded-full w-6 h-6 flex items-center justify-center text-xs notification-badge hidden">0</div>
                    </div>
                </div>
            </div>

            <div class="bg-white rounded-xl shadow-md p-6 mb-6">
                <div class="flex justify-between items-center mb-4">
                    <h3 class="text-xl font-bold text-gray-800" data-lang="yourProfile">Your Profile</h3>
                    <button onclick="showWorkerEditProfileModal()" class="text-blue-600 hover:underline">
                        <span data-lang="editProfile">Edit Profile</span>
                    </button>
                </div>

                <div class="text-center mb-4">
                    <img id="workerProfilePhoto" src="" alt="Profile Photo" class="w-24 h-24 rounded-full mx-auto object-cover">
                </div>
                
                <div class="space-y-3">
                    <div class="flex justify-between items-center p-3 bg-gray-50 rounded-lg">
                        <span class="text-gray-600" data-lang="name">Name:</span>
                        <span class="font-semibold" id="workerProfileName"></span>
                    </div>
                    
                    <div class="flex justify-between items-center p-3 bg-gray-50 rounded-lg">
                        <span class="text-gray-600" data-lang="mobile">Mobile:</span>
                        <span class="font-semibold" id="workerProfileMobile"></span>
                    </div>
                    
                    <div class="flex justify-between items-center p-3 bg-gray-50 rounded-lg">
                        <span class="text-gray-600" data-lang="profession">Profession:</span>
                        <span class="font-semibold" id="workerProfileProfession"></span>
                    </div>
                    
                    <div class="flex justify-between items-center p-3 bg-gray-50 rounded-lg">
                        <span class="text-gray-600" data-lang="location">Location:</span>
                        <span class="font-semibold" id="workerProfileLocation"></span>
                    </div>
                </div>

                <div class="mt-6 p-4 bg-green-50 border-2 border-green-200 rounded-lg">
                    <p class="text-green-800 text-center font-semibold" data-lang="workerStatus">✓ Your profile is active and visible to customers</p>
                </div>
            </div>

            <!-- Work Requests -->
            <div class="bg-white rounded-xl shadow-md p-6">
                <h3 class="text-xl font-bold text-gray-800 mb-4" data-lang="workRequests">Work Requests</h3>
                <div id="workRequestsList" class="space-y-4">
                    <p class="text-gray-500 text-center py-4">No new requests</p>
                </div>
            </div>
        </div>
    </div>

    <!-- Add Worker Modal -->
    <div id="addWorkerModal" class="modal">
        <div class="bg-white rounded-2xl shadow-xl p-6 max-w-md w-full mx-4">
            <h3 class="text-2xl font-bold text-gray-800 mb-6" data-lang="addNewWorker">Add New Worker</h3>
            <form id="addWorkerForm" class="space-y-4">
                <div>
                    <label class="block text-gray-700 mb-2 font-semibold" data-lang="name">Name</label>
                    <input type="text" id="addWorkerName" class="w-full px-4 py-3 border-2 border-gray-300 rounded-lg focus:border-purple-500 focus:outline-none" required>
                </div>
                <div>
                    <label class="block text-gray-700 mb-2 font-semibold" data-lang="mobile">Mobile Number</label>
                    <input type="tel" id="addWorkerMobile" class="w-full px-4 py-3 border-2 border-gray-300 rounded-lg focus:border-purple-500 focus:outline-none" maxlength="10" required>
                </div>
                <div>
                    <label class="block text-gray-700 mb-2 font-semibold" data-lang="password">Password</label>
                    <input type="password" id="addWorkerPassword" class="w-full px-4 py-3 border-2 border-gray-300 rounded-lg focus:border-purple-500 focus:outline-none" required>
                </div>
                <div>
                    <label class="block text-gray-700 mb-2 font-semibold" data-lang="profession">Profession</label>
                    <select id="addWorkerProfession" class="w-full px-4 py-3 border-2 border-gray-300 rounded-lg focus:border-purple-500 focus:outline-none" required>
                        <option value="Plumber">Plumber</option>
                        <option value="Electrician">Electrician</option>
                        <option value="Carpenter">Carpenter</option>
                    </select>
                </div>
                <div>
                    <label class="block text-gray-700 mb-2 font-semibold" data-lang="location">Location</label>
                    <input type="text" id="addWorkerLocation" class="w-full px-4 py-3 border-2 border-gray-300 rounded-lg focus:border-purple-500 focus:outline-none" required>
                </div>
                <div class="flex space-x-4">
                    <button type="submit" class="flex-1 bg-purple-600 text-white py-3 rounded-lg font-semibold hover:bg-purple-700 transition">
                        <span data-lang="add">Add</span>
                    </button>
                    <button type="button" onclick="closeModal('addWorkerModal')" class="flex-1 bg-gray-300 text-gray-700 py-3 rounded-lg font-semibold hover:bg-gray-400 transition">
                        <span data-lang="cancel">Cancel</span>
                    </button>
                </div>
            </form>
        </div>
    </div>

    <!-- Edit Worker Modal -->
    <div id="editWorkerModal" class="modal">
        <div class="bg-white rounded-2xl shadow-xl p-6 max-w-md w-full mx-4">
            <h3 class="text-2xl font-bold text-gray-800 mb-6" data-lang="editWorkerDetails">Edit Worker Details</h3>
            <form id="editWorkerForm" class="space-y-4">
                <div>
                    <label class="block text-gray-700 mb-2 font-semibold" data-lang="selectWorker">Select Worker</label>
                    <select id="editWorkerSelect" class="w-full px-4 py-3 border-2 border-gray-300 rounded-lg focus:border-purple-500 focus:outline-none" required>
                        <option value="">Select Worker</option>
                    </select>
                </div>
                <div>
                    <label class="block text-gray-700 mb-2 font-semibold" data-lang="name">Name</label>
                    <input type="text" id="editWorkerName" class="w-full px-4 py-3 border-2 border-gray-300 rounded-lg focus:border-purple-500 focus:outline-none" required>
                </div>
                <div>
                    <label class="block text-gray-700 mb-2 font-semibold" data-lang="profession">Profession</label>
                    <select id="editWorkerProfession" class="w-full px-4 py-3 border-2 border-gray-300 rounded-lg focus:border-purple-500 focus:outline-none" required>
                        <option value="Plumber">Plumber</option>
                        <option value="Electrician">Electrician</option>
                        <option value="Carpenter">Carpenter</option>
                    </select>
                </div>
                <div>
                    <label class="block text-gray-700 mb-2 font-semibold" data-lang="location">Location</label>
                    <input type="text" id="editWorkerLocation" class="w-full px-4 py-3 border-2 border-gray-300 rounded-lg focus:border-purple-500 focus:outline-none" required>
                </div>
                <div class="flex space-x-4">
                    <button type="submit" class="flex-1 bg-blue-600 text-white py-3 rounded-lg font-semibold hover:bg-blue-700 transition">
                        <span data-lang="update">Update</span>
                    </button>
                    <button type="button" onclick="closeModal('editWorkerModal')" class="flex-1 bg-gray-300 text-gray-700 py-3 rounded-lg font-semibold hover:bg-gray-400 transition">
                        <span data-lang="cancel">Cancel</span>
                    </button>
                </div>
            </form>
        </div>
    </div>

    <!-- Booking Modal -->
    <div id="bookingModal" class="modal">
        <div class="bg-white rounded-2xl shadow-xl p-6 max-w-md w-full mx-4">
            <h3 class="text-2xl font-bold text-gray-800 mb-6" data-lang="bookWorker">Book Worker</h3>
            <form id="bookingForm" class="space-y-4">
                <div>
                    <label class="block text-gray-700 mb-2 font-semibold" data-lang="customerName">Customer Name</label>
                    <input type="text" id="bookingCustomerName" class="w-full px-4 py-3 border-2 border-gray-300 rounded-lg focus:border-green-500 focus:outline-none" required>
                </div>
                <div>
                    <label class="block text-gray-700 mb-2 font-semibold" data-lang="customerMobile">Customer Mobile</label>
                    <input type="tel" id="bookingCustomerMobile" class="w-full px-4 py-3 border-2 border-gray-300 rounded-lg focus:border-green-500 focus:outline-none" maxlength="10" required>
                </div>
                <div>
                    <label class="block text-gray-700 mb-2 font-semibold" data-lang="selectWorker">Select Worker</label>
                    <select id="bookingWorkerSelect" class="w-full px-4 py-3 border-2 border-gray-300 rounded-lg focus:border-green-500 focus:outline-none" required>
                        <option value="">Select Worker</option>
                    </select>
                </div>
                <div>
                    <label class="block text-gray-700 mb-2 font-semibold" data-lang="workDescription">Work Description</label>
                    <textarea id="bookingDescription" class="w-full px-4 py-3 border-2 border-gray-300 rounded-lg focus:border-green-500 focus:outline-none" rows="3" required></textarea>
                </div>
                <div>
                    <label class="block text-gray-700 mb-2 font-semibold" data-lang="location">Location</label>
                    <input type="text" id="bookingLocation" class="w-full px-4 py-3 border-2 border-gray-300 rounded-lg focus:border-green-500 focus:outline-none" required>
                </div>
                <div class="flex space-x-4">
                    <button type="submit" class="flex-1 bg-green-600 text-white py-3 rounded-lg font-semibold hover:bg-green-700 transition">
                        <span data-lang="book">Book</span>
                    </button>
                    <button type="button" onclick="closeModal('bookingModal')" class="flex-1 bg-gray-300 text-gray-700 py-3 rounded-lg font-semibold hover:bg-gray-400 transition">
                        <span data-lang="cancel">Cancel</span>
                    </button>
                </div>
            </form>
        </div>
    </div>

    <!-- Work Request Modal -->
    <div id="workRequestModal" class="modal">
        <div class="bg-white rounded-2xl shadow-xl p-6 max-w-md w-full mx-4">
            <h3 class="text-2xl font-bold text-gray-800 mb-6" data-lang="newWorkRequest">New Work Request!</h3>
            <div id="workRequestDetails" class="space-y-4">
                <!-- Request details will be loaded here -->
            </div>
            <div class="flex space-x-4 mt-6">
                <button onclick="acceptRequest()" class="flex-1 bg-green-600 text-white py-3 rounded-lg font-semibold hover:bg-green-700 transition">
                    <span data-lang="accept">Accept</span>
                </button>
                <button onclick="rejectRequest()" class="flex-1 bg-red-600 text-white py-3 rounded-lg font-semibold hover:bg-red-700 transition">
                    <span data-lang="reject">Reject</span>
                </button>
            </div>
        </div>
    </div>

    <!-- Worker Edit Profile Modal -->
    <div id="workerEditProfileModal" class="modal">
        <div class="bg-white rounded-2xl shadow-xl p-6 max-w-md w-full mx-4">
            <h3 class="text-2xl font-bold text-gray-800 mb-6" data-lang="editProfile">Edit Profile</h3>
            <form id="workerEditProfileForm" class="space-y-4">
                <div>
                    <label class="block text-gray-700 mb-2 font-semibold" data-lang="name">Name</label>
                    <input type="text" id="workerEditName" class="w-full px-4 py-3 border-2 border-gray-300 rounded-lg focus:border-green-500 focus:outline-none" required>
                </div>
                <div>
                    <label class="block text-gray-700 mb-2 font-semibold" data-lang="location">Location</label>
                    <input type="text" id="workerEditLocation" class="w-full px-4 py-3 border-2 border-gray-300 rounded-lg focus:border-green-500 focus:outline-none" required>
                </div>
                <div>
                    <label class="block text-gray-700 mb-2 font-semibold" data-lang="photo">Photo</label>
                    <input type="file" id="workerEditPhoto" accept="image/*" class="w-full px-4 py-3 border-2 border-gray-300 rounded-lg focus:border-green-500 focus:outline-none" onchange="previewPhoto(this, 'workerEditPhotoPreview')">
                    <img id="workerEditPhotoPreview" class="mt-2 w-32 h-32 object-cover rounded-lg hidden" alt="Preview">
                </div>
                <div class="flex space-x-4">
                    <button type="submit" class="flex-1 bg-green-600 text-white py-3 rounded-lg font-semibold hover:bg-green-700 transition">
                        <span data-lang="update">Update</span>
                    </button>
                    <button type="button" onclick="closeModal('workerEditProfileModal')" class="flex-1 bg-gray-300 text-gray-700 py-3 rounded-lg font-semibold hover:bg-gray-400 transition">
                        <span data-lang="cancel">Cancel</span>
                    </button>
                </div>
            </form>
        </div>
    </div>

    <!-- Chat Modal -->
    <div id="chatModal" class="modal">
        <div class="bg-white rounded-2xl shadow-xl p-6 max-w-lg w-full mx-4 max-h-[80vh] flex flex-col relative">
            <div class="flex justify-between items-center mb-4">
                <h3 class="text-2xl font-bold text-gray-800" data-lang="helpSupport">Contact Admin</h3>
                <button type="button" onclick="closeModal('chatModal')" class="text-gray-500 hover:text-gray-700">
                    <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"></path>
                    </svg>
                </button>
            </div>
            
            <div class="flex-1 overflow-y-auto mb-4 space-y-3 pr-2" id="chatMessages" style="min-height: 300px; max-height: 400px;">
                <!-- Messages will appear here -->
            </div>
            
            <form id="chatForm" class="flex space-x-2">
                <input type="text" id="chatInput" class="flex-1 px-4 py-2 border-2 border-gray-300 rounded-lg focus:border-blue-500 focus:outline-none" placeholder="Type your message..." required>
                <button type="submit" class="bg-blue-600 text-white px-4 py-2 rounded-lg hover:bg-blue-700 transition">
                    <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 19l9 2-9-18-9 18 9-2zm0 0v-8"></path>
                    </svg>
                </button>
            </form>
            
            <!-- Chat Logo -->
            <div class="chat-logo">
                <div class="text-center">
                    <h1 class="text-xs font-bold text-indigo-900">GK</h1>
                </div>
            </div>
        </div>
    </div>

    <!-- Admin Messages Modal -->
    <div id="messagesModal" class="modal">
        <div class="bg-white rounded-2xl shadow-xl p-6 max-w-6xl w-full mx-4 max-h-[90vh] flex flex-col">
            <div class="flex justify-between items-center mb-4">
                <h3 class="text-2xl font-bold text-gray-800" data-lang="messagesTitle">Admin Message Center</h3>
                <button type="button" onclick="closeModal('messagesModal')" class="text-gray-500 hover:text-gray-700">
                    <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"></path>
                    </svg>
                </button>
            </div>
            
            <div class="grid grid-cols-1 lg:grid-cols-2 gap-6 flex-1">
                <!-- Message List -->
                <div class="border-r border-gray-200 pr-4">
                    <h4 class="text-lg font-semibold mb-4">Recent Conversations</h4>
                    <div id="messageList" class="space-y-2 max-h-96 overflow-y-auto">
                        <!-- Message list will appear here -->
                    </div>
                </div>
                
                <!-- Chat Area -->
                <div class="flex flex-col">
                    <div class="flex justify-between items-center mb-4">
                        <h4 class="text-lg font-semibold">Chat with User</h4>
                        <div class="flex items-center space-x-2">
                            <select id="adminChatUserSelect" class="px-3 py-1 border border-gray-300 rounded text-sm">
                                <option value="">Select User</option>
                            </select>
                        </div>
                    </div>
                    
                    <div class="flex-1 overflow-y-auto mb-4 space-y-3 pr-2" id="adminChatMessages" style="min-height: 300px; max-height: 400px;">
                        <!-- Chat messages will appear here -->
                    </div>
                    
                    <form id="adminChatForm" class="flex space-x-2">
                        <input type="text" id="adminChatInput" class="flex-1 px-4 py-2 border-2 border-gray-300 rounded-lg focus:border-purple-500 focus:outline-none" placeholder="Type your reply..." required>
                        <button type="submit" class="bg-purple-600 text-white px-4 py-2 rounded-lg hover:bg-purple-700 transition">
                            <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 19l9 2-9-18-9 18 9-2zm0 0v-8"></path>
                            </svg>
                        </button>
                    </form>
                </div>
            </div>
        </div>
    </div>

    <!-- Footer Logo -->
    <div class="fixed bottom-4 left-1/2 transform -translate-x-1/2 z-40">
        <div class="bg-white rounded-full p-3 shadow-lg">
            <div class="text-center">
                <h1 class="text-xl font-bold text-indigo-900">Gramina Kaushal</h1>
                <p class="text-xs text-gray-600">Find professionals for your needs</p>
            </div>
        </div>
    </div>

    <script>
        // Language translations
        const translations = {
            en: {
                appTitle: "Gramina Kaushal",
                appSubtitle: "Find professionals for your needs",
                customerLogin: "👤 Customer Login/Register",
                workerLogin: "🔧 Worker Login/Register",
                adminLogin: "⚙️ Admin Login",
                serviceTypes: "Services: Plumber | Electrician | Carpenter",
                back: "Back",
                customerLoginTitle: "Customer Login",
                workerLoginTitle: "Worker Login",
                adminLoginTitle: "Admin Login",
                customerRegisterTitle: "Customer Registration",
                workerRegisterTitle: "Worker Registration",
                mobile: "Mobile Number",
                password: "Password",
                name: "Name",
                profession: "Profession",
                location: "Location",
                photo: "Photo URL",
                login: "Login",
                register: "Register",
                forgotPassword: "Forgot Password?",
                noAccount: "Don't have an account?",
                registerNow: "Register Now",
                resetPassword: "Reset Password",
                sendOTP: "Send OTP",
                enterOTP: "Enter OTP",
                otpSent: "OTP sent to your mobile:",
                newPassword: "New Password",
                resetPasswordBtn: "Reset Password",
                welcomeCustomer: "Welcome,",
                welcomeWorker: "Welcome,",
                findWorkers: "Find skilled workers for your needs",
                logout: "Logout",
                filterByProfession: "Filter by Profession:",
                availableWorkers: "Available Workers",
                yourProfession: "Your Profession:",
                yourProfile: "Your Profile",
                workerStatus: "✓ Your profile is active and visible to customers",
                plumber: "Plumber",
                electrician: "Electrician",
                carpenter: "Carpenter",
                adminDashboard: "Admin Dashboard",
                manageWorkers: "Manage workers and bookings",
                addWorkerDesc: "Register a new worker",
                editWorkerDesc: "Update worker details",
                bookWorkerDesc: "Create booking for customer",
                allWorkers: "All Workers",
                workRequests: "Work Requests",
                addNewWorker: "Add New Worker",
                editWorkerDetails: "Edit Worker Details",
                selectWorker: "Select Worker",
                add: "Add",
                update: "Update",
                cancel: "Cancel",
                bookWorker: "Book Worker",
                customerName: "Customer Name",
                customerMobile: "Customer Mobile",
                workDescription: "Work Description",
                book: "Book",
                newWorkRequest: "New Work Request!",
                accept: "Accept",
                reject: "Reject",
                editProfile: "Edit Profile",
                myBookings: "My Bookings",
                helpSupport: "Contact Admin",
                messagesDesc: "View user messages",
                messagesTitle: "Admin Message Center",
                typeMessage: "Type your message..."
            },
            te: {
                appTitle: "గ్రామీణ కౌశల్",
                appSubtitle: "మీ అవసరాలకు నిపుణులను కనుగొనండి",
                customerLogin: "👤 కస్టమర్ లాగిన్/రిజిస్టర్",
                workerLogin: "🔧 వర్కర్ లాగిన్/రిజిస్టర్",
                adminLogin: "⚙️ అడ్మిన్ లాగిన్",
                serviceTypes: "సేవలు: ప్లంబర్ | ఎలక్ట్రీషియన్ | వడ్రంగి",
                back: "వెనక్కి",
                customerLoginTitle: "కస్టమర్ లాగిన్",
                workerLoginTitle: "వర్కర్ లాగిన్",
                adminLoginTitle: "అడ్మిన్ లాగిన్",
                customerRegisterTitle: "కస్టమర్ నమోదు",
                workerRegisterTitle: "వర్కర్ నమోదు",
                mobile: "మొబైల్ నంబర్",
                password: "పాస్‌వర్డ్",
                name: "పేరు",
                profession: "వృత్తి",
                location: "స్థానం",
                photo: "ఫోటో URL",
                login: "లాగిన్",
                register: "నమోదు చేయండి",
                forgotPassword: "పాస్‌వర్డ్ మరచిపోయారా?",
                noAccount: "ఖాతా లేదా?",
                registerNow: "ఇప్పుడే నమోదు చేయండి",
                resetPassword: "పాస్‌వర్డ్ రీసెట్ చేయండి",
                sendOTP: "OTP పంపండి",
                enterOTP: "OTP నమోదు చేయండి",
                otpSent: "మీ మొబైల్‌కు OTP పంపబడింది:",
                newPassword: "కొత్త పాస్‌వర్డ్",
                resetPasswordBtn: "పాస్‌వర్డ్ రీసెట్ చేయండి",
                welcomeCustomer: "స్వాగతం,",
                welcomeWorker: "స్వాగతం,",
                findWorkers: "మీ అవసరాలకు నైపుణ్యం కలిగిన కార్మికులను కనుగొనండి",
                logout: "లాగ్అవుట్",
                filterByProfession: "వృత్తి ద్వారా ఫిల్టర్ చేయండి:",
                availableWorkers: "అందుబాటులో ఉన్న కార్మికులు",
                yourProfession: "మీ వృత్తి:",
                yourProfile: "మీ ప్రొఫైల్",
                workerStatus: "✓ మీ ప్రొఫైల్ యాక్టివ్ మరియు కస్టమర్‌లకు కనిపిస్తుంది",
                plumber: "ప్లంబర్",
                electrician: "ఎలక్ట్రీషియన్",
                carpenter: "వడ్రంగి",
                adminDashboard: "అడ్మిన్ డాష్‌బోర్డ్",
                manageWorkers: "కార్మికులు మరియు బుకింగ్‌లను నిర్వహించండి",
                addWorkerDesc: "కొత్త కార్మికుడిని నమోదు చేయండి",
                editWorkerDesc: "కార్మికుడి వివరాలను నవీకరించండి",
                bookWorkerDesc: "కస్టమర్ కోసం బుకింగ్ సృష్టించండి",
                allWorkers: "అన్ని కార్మికులు",
                workRequests: "పని అభ్యర్థనలు",
                addNewWorker: "కొత్త కార్మికుడిని జోడించండి",
                editWorkerDetails: "కార్మికుడి వివరాలను సవరించండి",
                selectWorker: "కార్మికుడిని ఎంచుకోండి",
                add: "జోడించండి",
                update: "నవీకరించండి",
                cancel: "రద్దు చేయండి",
                bookWorker: "కార్మికుడిని బుక్ చేయండి",
                customerName: "కస్టమర్ పేరు",
                customerMobile: "కస్టమర్ మొబైల్",
                workDescription: "పని వివరణ",
                book: "బుక్ చేయండి",
                newWorkRequest: "కొత్త పని అభ్యర్థన!",
                accept: "అంగీకరించండి",
                reject: "తిరస్కరించండి",
                editProfile: "ప్రొఫైల్ సవరించండి",
                myBookings: "నా బుకింగ్‌లు",
                helpSupport: "అడ్మిన్‌ను సంప్రదించండి",
                messagesDesc: "వినియోగదారు సందేశాలను చూడండి",
                messagesTitle: "అడ్మిన్ సందేశ కేంద్రం",
                typeMessage: "మీ సందేశం టైప్ చేయండి..."
            },
            hi: {
                appTitle: "ग्रामीण कौशल",
                appSubtitle: "अपनी जरूरतों के लिए पेशेवरों को खोजें",
                customerLogin: "👤 ग्राहक लॉगिन/रजिस्टर",
                workerLogin: "🔧 कार्यकर्ता लॉगिन/रजिस्टर",
                adminLogin: "⚙️ एडमिन लॉगिन",
                serviceTypes: "सेवाएं: प्लंबर | इलेक्ट्रीशियन | बढ़ई",
                back: "वापस",
                customerLoginTitle: "ग्राहक लॉगिन",
                workerLoginTitle: "कार्यकर्ता लॉगिन",
                adminLoginTitle: "एडमिन लॉगिन",
                customerRegisterTitle: "ग्राहक पंजीकरण",
                workerRegisterTitle: "कार्यकर्ता पंजीकरण",
                mobile: "मोबाइल नंबर",
                password: "पासवर्ड",
                name: "नाम",
                profession: "पेशा",
                location: "स्थान",
                photo: "फोटो URL",
                login: "लॉगिन",
                register: "रजिस्टर करें",
                forgotPassword: "पासवर्ड भूल गए?",
                noAccount: "खाता नहीं है?",
                registerNow: "अभी रजिस्टर करें",
                resetPassword: "पासवर्ड रीसेट करें",
                sendOTP: "OTP भेजें",
                enterOTP: "OTP दर्ज करें",
                otpSent: "आपके मोबाइल पर OTP भेजा गया:",
                newPassword: "नया पासवर्ड",
                resetPasswordBtn: "पासवर्ड रीसेट करें",
                welcomeCustomer: "स्वागत है,",
                welcomeWorker: "स्वागत है,",
                findWorkers: "अपनी जरूरतों के लिए कुशल कार्यकर्ताओं को खोजें",
                logout: "लॉगआउट",
                filterByProfession: "पेशे के अनुसार फ़िल्टर करें:",
                availableWorkers: "उपलब्ध कार्यकर्ता",
                yourProfession: "आपका पेशा:",
                yourProfile: "आपकी प्रोफ़ाइल",
                workerStatus: "✓ आपकी प्रोफ़ाइल सक्रिय है और ग्राहकों को दिखाई दे रही है",
                plumber: "प्लंबर",
                electrician: "इलेक्ट्रीशियन",
                carpenter: "बढ़ई",
                adminDashboard: "एडमिन डैशबोर्ड",
                manageWorkers: "कार्यकर्ताओं और बुकिंग प्रबंधित करें",
                addWorkerDesc: "नया कार्यकर्ता पंजीकृत करें",
                editWorkerDesc: "कार्यकर्ता विवरण अपडेट करें",
                bookWorkerDesc: "ग्राहक के लिए बुकिंग बनाएं",
                allWorkers: "सभी कार्यकर्ता",
                workRequests: "कार्य अनुरोध",
                addNewWorker: "नया कार्यकर्ता जोड़ें",
                editWorkerDetails: "कार्यकर्ता विवरण संपादित करें",
                selectWorker: "कार्यकर्ता चुनें",
                add: "जोड़ें",
                update: "अपडेट करें",
                cancel: "रद्द करें",
                bookWorker: "कार्यकर्ता बुक करें",
                customerName: "ग्राहक का नाम",
                customerMobile: "ग्राहक का मोबाइल",
                workDescription: "कार्य विवरण",
                book: "बुक करें",
                newWorkRequest: "नया कार्य अनुरोध!",
                accept: "स्वीकार करें",
                reject: "अस्वीकार करें",
                editProfile: "प्रोफ़ाइल संपादित करें",
                myBookings: "मेरी बुकिंग",
                helpSupport: "एडमिन से संपर्क करें",
                messagesDesc: "उपयोगकर्ता संदेश देखें",
                messagesTitle: "एडमिन संदेश केंद्र",
                typeMessage: "अपना संदेश लिखें..."
            }
        };

        let currentLanguage = 'en';
        let currentUser = null;
        let generatedOTP = null;
        let otpMobile = null;
        let otpType = null; // 'customer' or 'worker'
        let loginMode = 'password'; // 'password' or 'otp'
        let currentRequest = null;
        let voiceEnabled = false;
        let selectedChatUser = null;

        // Initialize
        document.addEventListener('DOMContentLoaded', function() {
            loadLanguage();
            checkSession();
            setupEventListeners();
            updateMessageBadges();
        });

        function setupEventListeners() {
            // Language toggle
            document.getElementById('languageToggle').addEventListener('change', function(e) {
                currentLanguage = e.target.value;
                loadLanguage();
            });

            // Admin Login Form
            document.getElementById('adminLoginForm').addEventListener('submit', function(e) {
                e.preventDefault();
                handleAdminLogin();
            });

            // Customer Login Form
            document.getElementById('customerLoginForm').addEventListener('submit', function(e) {
                e.preventDefault();
                handleCustomerLogin();
            });

            // Customer Register Form
            document.getElementById('customerRegisterForm').addEventListener('submit', function(e) {
                e.preventDefault();
                handleCustomerRegister();
            });

            // Customer Forgot Password
            document.getElementById('customerForgotForm').addEventListener('submit', function(e) {
                e.preventDefault();
                handleCustomerForgotPassword();
            });

            // Customer Verify OTP
            document.getElementById('customerVerifyOTPForm').addEventListener('submit', function(e) {
                e.preventDefault();
                handleCustomerVerifyOTP();
            });

            // Worker Login Form
            document.getElementById('workerLoginForm').addEventListener('submit', function(e) {
                e.preventDefault();
                handleWorkerLogin();
            });

            // Worker Register Form
            document.getElementById('workerRegisterForm').addEventListener('submit', function(e) {
                e.preventDefault();
                handleWorkerRegister();
            });

            // Worker Forgot Password
            document.getElementById('workerForgotForm').addEventListener('submit', function(e) {
                e.preventDefault();
                handleWorkerForgotPassword();
            });

            // Worker Verify OTP
            document.getElementById('workerVerifyOTPForm').addEventListener('submit', function(e) {
                e.preventDefault();
                handleWorkerVerifyOTP();
            });

            // Profession Filter
            document.getElementById('professionFilter').addEventListener('change', function(e) {
                loadWorkers(e.target.value);
            });

            // Add Worker Form
            document.getElementById('addWorkerForm').addEventListener('submit', function(e) {
                e.preventDefault();
                handleAddWorker();
            });

            // Edit Worker Form
            document.getElementById('editWorkerForm').addEventListener('submit', function(e) {
                e.preventDefault();
                handleEditWorker();
            });

            // Edit Worker Select
            document.getElementById('editWorkerSelect').addEventListener('change', function(e) {
                loadWorkerDetailsForEdit(e.target.value);
            });

            // Booking Form
            document.getElementById('bookingForm').addEventListener('submit', function(e) {
                e.preventDefault();
                handleBooking();
            });

            // Worker Edit Profile Form
            document.getElementById('workerEditProfileForm').addEventListener('submit', function(e) {
                e.preventDefault();
                handleWorkerUpdateProfile();
            });

            // Chat Form
            document.getElementById('chatForm').addEventListener('submit', function(e) {
                e.preventDefault();
                handleChatMessage();
            });

            // Admin Chat Form
            document.getElementById('adminChatForm').addEventListener('submit', function(e) {
                e.preventDefault();
                handleAdminChatMessage();
            });

            // Admin Chat User Select
            document.getElementById('adminChatUserSelect').addEventListener('change', function(e) {
                selectedChatUser = e.target.value;
                loadAdminChatMessages();
            });
        }

        function loadLanguage() {
            const elements = document.querySelectorAll('[data-lang]');
            elements.forEach(el => {
                const key = el.getAttribute('data-lang');
                if (translations[currentLanguage][key]) {
                    el.textContent = translations[currentLanguage][key];
                }
            });

            // Update select options
            const options = document.querySelectorAll('[data-lang-option]');
            options.forEach(opt => {
                const key = opt.getAttribute('data-lang-option');
                if (translations[currentLanguage][key]) {
                    opt.textContent = translations[currentLanguage][key];
                }
            });
        }

        function showScreen(screenId) {
            document.querySelectorAll('.screen').forEach(s => s.classList.remove('active'));
            document.getElementById(screenId).classList.add('active');
            window.scrollTo(0, 0);
            announceScreen(screenId);
            
            // Show/hide contact admin button
            if (screenId === 'customerDashboard' || screenId === 'workerDashboard') {
                document.getElementById('contactAdminBtn').classList.remove('hidden');
            } else {
                document.getElementById('contactAdminBtn').classList.add('hidden');
            }
        }

        // Voice announcement function
        function announceScreen(screenId) {
            if (!voiceEnabled) return;

            const screenNames = {
                'homeScreen': 'Home Screen. Welcome to Gramina Kaushal',
                'customerLoginScreen': 'Customer Login Screen',
                'customerRegisterScreen': 'Customer Registration Screen',
                'customerForgotPasswordScreen': 'Customer Password Reset Screen',
                'customerDashboard': 'Customer Dashboard',
                'workerLoginScreen': 'Worker Login Screen',
                'workerRegisterScreen': 'Worker Registration Screen',
                'workerForgotPasswordScreen': 'Worker Password Reset Screen',
                'workerDashboard': 'Worker Dashboard',
                'adminLoginScreen': 'Admin Login Screen',
                'adminDashboard': 'Admin Dashboard'
            };

            const message = screenNames[screenId] || 'Screen changed';
            speak(message);
        }

        // Text-to-speech function
        function speak(text) {
            if ('speechSynthesis' in window) {
                window.speechSynthesis.cancel();
                
                const utterance = new SpeechSynthesisUtterance(text);
                utterance.lang = currentLanguage === 'te' ? 'te-IN' : currentLanguage === 'hi' ? 'hi-IN' : 'en-US';
                utterance.rate = 0.9;
                utterance.pitch = 1;
                utterance.volume = 1;
                
                window.speechSynthesis.speak(utterance);
            }
        }

        // Toggle voice function
        function toggleVoice() {
            voiceEnabled = !voiceEnabled;
            const voiceIcon = document.getElementById('voiceIcon');
            const voiceToggle = document.getElementById('voiceToggle');
            
            if (voiceEnabled) {
                voiceToggle.classList.remove('border-purple-500', 'text-purple-700');
                voiceToggle.classList.add('border-green-500', 'text-green-700', 'bg-green-50');
                voiceIcon.innerHTML = `
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15.536 8.464a5 5 0 010 7.072m2.828-9.9a9 9 0 010 12.728M5.586 15H4a1 1 0 01-1-1v-4a1 1 0 011-1h1.586l4.707-4.707C10.923 3.663 12 4.109 12 5v14c0 .891-1.077 1.337-1.707.707L5.586 15z"></path>
                `;
                speak('Voice assistance enabled');
            } else {
                voiceToggle.classList.remove('border-green-500', 'text-green-700', 'bg-green-50');
                voiceToggle.classList.add('border-purple-500', 'text-purple-700');
                voiceIcon.innerHTML = `
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5.586 15H4a1 1 0 01-1-1v-4a1 1 0 011-1h1.586l4.707-4.707C10.923 3.663 12 4.109 12 5v14c0 .891-1.077 1.337-1.707.707L5.586 15z"></path>
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M17 14l2-2m0 0l2-2m-2 2l-2-2m2 2l2 2"></path>
                `;
                window.speechSynthesis.cancel();
            }
        }

        function showModal(modalId) {
            document.getElementById(modalId).classList.add('active');
        }

        function closeModal(modalId) {
            document.getElementById(modalId).classList.remove('active');
        }

        function showError(elementId, message) {
            const errorEl = document.getElementById(elementId);
            errorEl.textContent = message;
            errorEl.classList.remove('hidden');
        }

        function hideError(elementId) {
            document.getElementById(elementId).classList.add('hidden');
        }

        function validateName(name) {
            return name.trim().length > 0 && /^[a-zA-Z\s]+$/.test(name);
        }

        function validateMobile(mobile) {
            return /^\d{10}$/.test(mobile);
        }

        function validatePassword(password) {
            return password.length >= 6;
        }

        // Open Chat Function
        function openChat() {
            showModal('chatModal');
            if (currentUser.type === 'customer') {
                loadCustomerMessages();
            } else {
                loadWorkerMessages();
            }
        }

        // Admin Login
        function handleAdminLogin() {
            hideError('adminPasswordError');
            const password = document.getElementById('adminPassword').value;

            if (password === 'MASKS@Gramin') {
                currentUser = { type: 'admin' };
                localStorage.setItem('currentUser', JSON.stringify(currentUser));
                showAdminDashboard();
            } else {
                showError('adminPasswordError', 'Invalid password');
            }
        }

        // Customer Login
        function handleCustomerLogin() {
            if (loginMode === 'password') {
                handleCustomerPasswordLogin();
            } else {
                handleCustomerOTPLogin();
            }
        }

        function handleWorkerLogin() {
            if (loginMode === 'password') {
                handleWorkerPasswordLogin();
            } else {
                handleWorkerOTPLogin();
            }
        }

        function handleCustomerPasswordLogin() {
            hideError('custLoginMobileError');
            hideError('custLoginPasswordError');

            const mobile = document.getElementById('custLoginMobile').value;
            const password = document.getElementById('custLoginPassword').value;

            let valid = true;

            if (!validateMobile(mobile)) {
                showError('custLoginMobileError', 'Please enter a valid 10-digit mobile number');
                valid = false;
            }

            if (!validatePassword(password)) {
                showError('custLoginPasswordError', 'Password must be at least 6 characters');
                valid = false;
            }

            if (valid) {
                const customers = JSON.parse(localStorage.getItem('customers') || '[]');
                const customer = customers.find(c => c.mobile === mobile && c.password === password);

                if (customer) {
                    currentUser = { ...customer, type: 'customer' };
                    localStorage.setItem('currentUser', JSON.stringify(currentUser));
                    showCustomerDashboard();
                } else {
                    alert('Invalid credentials. Please try again.');
                }
            }
        }

        function handleWorkerPasswordLogin() {
            hideError('workerLoginMobileError');
            hideError('workerLoginPasswordError');

            const mobile = document.getElementById('workerLoginMobile').value;
            const password = document.getElementById('workerLoginPassword').value;

            let valid = true;

            if (!validateMobile(mobile)) {
                showError('workerLoginMobileError', 'Please enter a valid 10-digit mobile number');
                valid = false;
            }

            if (!validatePassword(password)) {
                showError('workerLoginPasswordError', 'Password must be at least 6 characters');
                valid = false;
            }

            if (valid) {
                const workers = JSON.parse(localStorage.getItem('workers') || '[]');
                const worker = workers.find(w => w.mobile === mobile && w.password === password);

                if (worker) {
                    currentUser = { ...worker, type: 'worker' };
                    localStorage.setItem('currentUser', JSON.stringify(currentUser));
                    showWorkerDashboard();
                } else {
                    alert('Invalid credentials. Please try again.');
                }
            }
        }

        function handleCustomerOTPRequest() {
            const mobile = document.getElementById('custLoginMobile').value;
            if (!validateMobile(mobile)) {
                showError('custLoginMobileError', 'Please enter a valid 10-digit mobile number to receive an OTP.');
                return;
            }
            
            const customers = JSON.parse(localStorage.getItem('customers') || '[]');
            const customer = customers.find(c => c.mobile === mobile);

            if (!customer) {
                alert('Mobile number not found. Please register first.');
                return;
            }

            loginMode = 'otp';
            generatedOTP = Math.floor(100000 + Math.random() * 900000).toString();
            otpMobile = mobile;
            otpType = 'customer';

            document.getElementById('custLoginPasswordContainer').classList.add('hidden');
            document.getElementById('custLoginOTPContainer').classList.remove('hidden');
            document.getElementById('custLoginBtnText').textContent = 'Verify OTP';
            document.getElementById('custOtpBtnText').textContent = 'Login with Password';

            alert(`OTP sent to ${mobile}: ${generatedOTP}`);
        }

        function handleWorkerOTPRequest() {
            const mobile = document.getElementById('workerLoginMobile').value;
            if (!validateMobile(mobile)) {
                showError('workerLoginMobileError', 'Please enter a valid 10-digit mobile number to receive an OTP.');
                return;
            }

            const workers = JSON.parse(localStorage.getItem('workers') || '[]');
            const worker = workers.find(w => w.mobile === mobile);

            if (!worker) {
                alert('Mobile number not found. Please register first.');
                return;
            }

            loginMode = 'otp';
            generatedOTP = Math.floor(100000 + Math.random() * 900000).toString();
            otpMobile = mobile;
            otpType = 'worker';

            document.getElementById('workerLoginPasswordContainer').classList.add('hidden');
            document.getElementById('workerLoginOTPContainer').classList.remove('hidden');
            document.getElementById('workerLoginBtnText').textContent = 'Verify OTP';
            document.getElementById('workerOtpBtnText').textContent = 'Login with Password';

            alert(`OTP sent to ${mobile}: ${generatedOTP}`);
        }

        function handleCustomerOTPLogin() {
            const otp = document.getElementById('custLoginOTP').value;
            if (otp === generatedOTP) {
                const customers = JSON.parse(localStorage.getItem('customers') || '[]');
                const customer = customers.find(c => c.mobile === otpMobile);
                currentUser = { ...customer, type: 'customer' };
                localStorage.setItem('currentUser', JSON.stringify(currentUser));
                showCustomerDashboard();
            } else {
                showError('custLoginOTPError', 'Invalid OTP.');
            }
        }

        function handleWorkerOTPLogin() {
            const otp = document.getElementById('workerLoginOTP').value;
            if (otp === generatedOTP) {
                const workers = JSON.parse(localStorage.getItem('workers') || '[]');
                const worker = workers.find(w => w.mobile === otpMobile);
                currentUser = { ...worker, type: 'worker' };
                localStorage.setItem('currentUser', JSON.stringify(currentUser));
                showWorkerDashboard();
            } else {
                showError('workerLoginOTPError', 'Invalid OTP.');
            }
        }

        // Customer Register
        function handleCustomerRegister() {
            hideError('custRegNameError');
            hideError('custRegMobileError');
            hideError('custRegPasswordError');
            hideError('custRegLocationError');

            const name = document.getElementById('custRegName').value;
            const mobile = document.getElementById('custRegMobile').value;
            const password = document.getElementById('custRegPassword').value;
            const location = document.getElementById('custRegLocation').value;

            let valid = true;

            if (!validateName(name)) {
                showError('custRegNameError', 'Please enter a valid name (letters only)');
                valid = false;
            }

            if (!validateMobile(mobile)) {
                showError('custRegMobileError', 'Please enter a valid 10-digit mobile number');
                valid = false;
            }

            if (!validatePassword(password)) {
                showError('custRegPasswordError', 'Password must be at least 6 characters');
                valid = false;
            }

            if (!location.trim()) {
                showError('custRegLocationError', 'Please enter your location');
                valid = false;
            }

            if (valid) {
                const customers = JSON.parse(localStorage.getItem('customers') || '[]');
                
                if (customers.find(c => c.mobile === mobile)) {
                    alert('Mobile number already registered. Please login.');
                    return;
                }

                customers.push({ name, mobile, password, location });
                localStorage.setItem('customers', JSON.stringify(customers));

                alert('Registration successful! Please login.');
                showScreen('customerLoginScreen');
                document.getElementById('customerRegisterForm').reset();
            }
        }

        // Customer Forgot Password
        function handleCustomerForgotPassword() {
            hideError('custForgotMobileError');

            const mobile = document.getElementById('custForgotMobile').value;

            if (!validateMobile(mobile)) {
                showError('custForgotMobileError', 'Please enter a valid 10-digit mobile number');
                return;
            }

            const customers = JSON.parse(localStorage.getItem('customers') || '[]');
            const customer = customers.find(c => c.mobile === mobile);

            if (!customer) {
                alert('Mobile number not found. Please register first.');
                return;
            }

            // Generate OTP
            generatedOTP = Math.floor(100000 + Math.random() * 900000).toString();
            otpMobile = mobile;
            otpType = 'customer';

            document.getElementById('custOTPMobile').textContent = mobile;
            document.getElementById('custForgotStep1').classList.add('hidden');
            document.getElementById('custForgotStep2').classList.remove('hidden');

            alert(`OTP sent to ${mobile}: ${generatedOTP}`);
        }

        // Customer Verify OTP
        function handleCustomerVerifyOTP() {
            hideError('custForgotOTPError');
            hideError('custNewPasswordError');

            const otp = document.getElementById('custForgotOTP').value;
            const newPassword = document.getElementById('custNewPassword').value;

            let valid = true;

            if (otp !== generatedOTP) {
                showError('custForgotOTPError', 'Invalid OTP. Please try again.');
                valid = false;
            }

            if (!validatePassword(newPassword)) {
                showError('custNewPasswordError', 'Password must be at least 6 characters');
                valid = false;
            }

            if (valid) {
                const customers = JSON.parse(localStorage.getItem('customers') || '[]');
                const customerIndex = customers.findIndex(c => c.mobile === otpMobile);

                if (customerIndex !== -1) {
                    customers[customerIndex].password = newPassword;
                    localStorage.setItem('customers', JSON.stringify(customers));

                    alert('Password reset successful! Please login.');
                    showScreen('customerLoginScreen');
                    document.getElementById('custForgotStep1').classList.remove('hidden');
                    document.getElementById('custForgotStep2').classList.add('hidden');
                    document.getElementById('customerForgotForm').reset();
                    document.getElementById('customerVerifyOTPForm').reset();
                }
            }
        }

        // Photo preview function
        function previewPhoto(input, previewId) {
            const preview = document.getElementById(previewId);
            const file = input.files[0];
            
            if (file) {
                const reader = new FileReader();
                reader.onload = function(e) {
                    preview.src = e.target.result;
                    preview.classList.remove('hidden');
                }
                reader.readAsDataURL(file);
            }
        }

        // Worker Register
        async function handleWorkerRegister() {
            hideError('workerRegNameError');
            hideError('workerRegMobileError');
            hideError('workerRegPasswordError');
            hideError('workerRegProfessionError');
            hideError('workerRegLocationError');

            const name = document.getElementById('workerRegName').value;
            const mobile = document.getElementById('workerRegMobile').value;
            const password = document.getElementById('workerRegPassword').value;
            const profession = document.getElementById('workerRegProfession').value;
            const location = document.getElementById('workerRegLocation').value;
            const photoInput = document.getElementById('workerRegPhoto');
            
            let photoData = null;
            if (photoInput.files && photoInput.files[0]) {
                const reader = new FileReader();
                photoData = await new Promise((resolve) => {
                    reader.onload = function(e) {
                        resolve(e.target.result);
                    }
                    reader.readAsDataURL(photoInput.files[0]);
                });
            }

            let valid = true;

            if (!validateName(name)) {
                showError('workerRegNameError', 'Please enter a valid name (letters only)');
                valid = false;
            }

            if (!validateMobile(mobile)) {
                showError('workerRegMobileError', 'Please enter a valid 10-digit mobile number');
                valid = false;
            }

            if (!validatePassword(password)) {
                showError('workerRegPasswordError', 'Password must be at least 6 characters');
                valid = false;
            }

            if (!profession) {
                showError('workerRegProfessionError', 'Please select a profession');
                valid = false;
            }

            if (!location.trim()) {
                showError('workerRegLocationError', 'Please enter your location');
                valid = false;
            }

            if (valid) {
                const workers = JSON.parse(localStorage.getItem('workers') || '[]');
                
                if (workers.find(w => w.mobile === mobile)) {
                    alert('Mobile number already registered. Please login.');
                    return;
                }

                workers.push({ 
                    name, 
                    mobile, 
                    password, 
                    profession, 
                    location,
                    photo: photoData || `https://picsum.photos/seed/${mobile}/200/200.jpg`
                });
                localStorage.setItem('workers', JSON.stringify(workers));

                alert('Registration successful! Please login.');
                showScreen('workerLoginScreen');
                document.getElementById('workerRegisterForm').reset();
                document.getElementById('workerRegPhotoPreview').classList.add('hidden');
            }
        }

        // Worker Forgot Password
        function handleWorkerForgotPassword() {
            hideError('workerForgotMobileError');

            const mobile = document.getElementById('workerForgotMobile').value;

            if (!validateMobile(mobile)) {
                showError('workerForgotMobileError', 'Please enter a valid 10-digit mobile number');
                return;
            }

            const workers = JSON.parse(localStorage.getItem('workers') || '[]');
            const worker = workers.find(w => w.mobile === mobile);

            if (!worker) {
                alert('Mobile number not found. Please register first.');
                return;
            }

            // Generate OTP
            generatedOTP = Math.floor(100000 + Math.random() * 900000).toString();
            otpMobile = mobile;
            otpType = 'worker';

            document.getElementById('workerOTPMobile').textContent = mobile;
            document.getElementById('workerForgotStep1').classList.add('hidden');
            document.getElementById('workerForgotStep2').classList.remove('hidden');

            alert(`OTP sent to ${mobile}: ${generatedOTP}`);
        }

        // Worker Verify OTP
        function handleWorkerVerifyOTP() {
            hideError('workerForgotOTPError');
            hideError('workerNewPasswordError');

            const otp = document.getElementById('workerForgotOTP').value;
            const newPassword = document.getElementById('workerNewPassword').value;

            let valid = true;

            if (otp !== generatedOTP) {
                showError('workerForgotOTPError', 'Invalid OTP. Please try again.');
                valid = false;
            }

            if (!validatePassword(newPassword)) {
                showError('workerNewPasswordError', 'Password must be at least 6 characters');
                valid = false;
            }

            if (valid) {
                const workers = JSON.parse(localStorage.getItem('workers') || '[]');
                const workerIndex = workers.findIndex(w => w.mobile === otpMobile);

                if (workerIndex !== -1) {
                    workers[workerIndex].password = newPassword;
                    localStorage.setItem('workers', JSON.stringify(workers));

                    alert('Password reset successful! Please login.');
                    showScreen('workerLoginScreen');
                    document.getElementById('workerForgotStep1').classList.remove('hidden');
                    document.getElementById('workerForgotStep2').classList.add('hidden');
                    document.getElementById('workerForgotForm').reset();
                    document.getElementById('workerVerifyOTPForm').reset();
                }
            }
        }

        // Show Admin Dashboard
        function showAdminDashboard() {
            loadAdminWorkersList();
            loadAdminMessageUsers();
            showScreen('adminDashboard');
        }

        // Load Admin Workers List
        function loadAdminWorkersList() {
            const workers = JSON.parse(localStorage.getItem('workers') || '[]');
            const adminWorkersList = document.getElementById('adminWorkersList');

            if (workers.length === 0) {
                adminWorkersList.innerHTML = '<p class="text-gray-500 text-center py-8">No workers registered</p>';
                return;
            }

            adminWorkersList.innerHTML = `
                <table class="w-full">
                    <thead>
                        <tr class="border-b">
                            <th class="text-left py-2">Name</th>
                            <th class="text-left py-2">Mobile</th>
                            <th class="text-left py-2">Profession</th>
                            <th class="text-left py-2">Location</th>
                        </tr>
                    </thead>
                    <tbody>
                        ${workers.map(worker => `
                            <tr class="border-b">
                                <td class="py-2">${worker.name}</td>
                                <td class="py-2">${worker.mobile}</td>
                                <td class="py-2">${worker.profession}</td>
                                <td class="py-2">${worker.location}</td>
                            </tr>
                        `).join('')}
                    </tbody>
                </table>
            `;
        }

        // Show Add Worker Modal
        function showAddWorkerModal() {
            showModal('addWorkerModal');
        }

        // Handle Add Worker
        function handleAddWorker() {
            const name = document.getElementById('addWorkerName').value;
            const mobile = document.getElementById('addWorkerMobile').value;
            const password = document.getElementById('addWorkerPassword').value;
            const profession = document.getElementById('addWorkerProfession').value;
            const location = document.getElementById('addWorkerLocation').value;

            if (!validateMobile(mobile) || !validatePassword(password)) {
                alert('Please enter valid details');
                return;
            }

            const workers = JSON.parse(localStorage.getItem('workers') || '[]');
            
            if (workers.find(w => w.mobile === mobile)) {
                alert('Mobile number already registered');
                return;
            }

            workers.push({ 
                name, 
                mobile, 
                password, 
                profession, 
                location,
                photo: `https://picsum.photos/seed/${mobile}/200/200.jpg`
            });
            localStorage.setItem('workers', JSON.stringify(workers));

            alert('Worker added successfully!');
            closeModal('addWorkerModal');
            document.getElementById('addWorkerForm').reset();
            loadAdminWorkersList();
        }

        // Show Edit Worker Modal
        function showEditWorkerModal() {
            const workers = JSON.parse(localStorage.getItem('workers') || '[]');
            const select = document.getElementById('editWorkerSelect');
            
            select.innerHTML = '<option value="">Select Worker</option>' +
                workers.map(w => `<option value="${w.mobile}">${w.name} - ${w.profession}</option>`).join('');
            
            showModal('editWorkerModal');
        }

        // Load Worker Details for Edit
        function loadWorkerDetailsForEdit(mobile) {
            if (!mobile) return;
            
            const workers = JSON.parse(localStorage.getItem('workers') || '[]');
            const worker = workers.find(w => w.mobile === mobile);
            
            if (worker) {
                document.getElementById('editWorkerName').value = worker.name;
                document.getElementById('editWorkerProfession').value = worker.profession;
                document.getElementById('editWorkerLocation').value = worker.location;
            }
        }

        // Handle Edit Worker
        function handleEditWorker() {
            const mobile = document.getElementById('editWorkerSelect').value;
            const name = document.getElementById('editWorkerName').value;
            const profession = document.getElementById('editWorkerProfession').value;
            const location = document.getElementById('editWorkerLocation').value;

            if (!mobile) {
                alert('Please select a worker');
                return;
            }

            const workers = JSON.parse(localStorage.getItem('workers') || '[]');
            const workerIndex = workers.findIndex(w => w.mobile === mobile);

            if (workerIndex !== -1) {
                workers[workerIndex].name = name;
                workers[workerIndex].profession = profession;
                workers[workerIndex].location = location;
                localStorage.setItem('workers', JSON.stringify(workers));

                alert('Worker updated successfully!');
                closeModal('editWorkerModal');
                document.getElementById('editWorkerForm').reset();
                loadAdminWorkersList();
            }
        }

        // Show Booking Modal
        function showBookingModal() {
            const workers = JSON.parse(localStorage.getItem('workers') || '[]');
            const select = document.getElementById('bookingWorkerSelect');
            
            select.innerHTML = '<option value="">Select Worker</option>' +
                workers.map(w => `<option value="${w.mobile}">${w.name} - ${w.profession}</option>`).join('');
            
            showModal('bookingModal');
        }

        // Handle Booking
        function handleBooking() {
            const customerName = document.getElementById('bookingCustomerName').value;
            const customerMobile = document.getElementById('bookingCustomerMobile').value;
            const workerMobile = document.getElementById('bookingWorkerSelect').value;
            const description = document.getElementById('bookingDescription').value;
            const location = document.getElementById('bookingLocation').value;

            if (!workerMobile) {
                alert('Please select a worker');
                return;
            }

            const bookings = JSON.parse(localStorage.getItem('bookings') || '[]');
            const booking = {
                id: Date.now(),
                customerName,
                customerMobile,
                workerMobile,
                description,
                location,
                status: 'pending',
                createdAt: new Date().toISOString()
            };

            bookings.push(booking);
            localStorage.setItem('bookings', JSON.stringify(bookings));

            // Add to worker requests
            const workerRequests = JSON.parse(localStorage.getItem('workerRequests') || '[]');
            workerRequests.push({
                ...booking,
                workerNotified: false
            });
            localStorage.setItem('workerRequests', JSON.stringify(workerRequests));

            alert('Booking created successfully!');
            closeModal('bookingModal');
            document.getElementById('bookingForm').reset();
        }

        // Show Customer Dashboard
        function showCustomerDashboard() {
            document.getElementById('custDashName').textContent = currentUser.name;
            loadWorkers();
            loadCustomerBookings();
            updateMessageBadges();
            showScreen('customerDashboard');
        }
        
        // Load Customer Bookings
        function loadCustomerBookings() {
            const bookings = JSON.parse(localStorage.getItem('bookings') || '[]');
            const myBookings = bookings.filter(b => b.customerMobile === currentUser.mobile);
            const bookingsList = document.getElementById('customerBookingsList');
            const notifications = document.getElementById('customerNotifications');
            
            // Check for status updates
            const acceptedBookings = myBookings.filter(b => b.status === 'accepted');
            const rejectedBookings = myBookings.filter(b => b.status === 'rejected');
            
            // Show notifications
            if (acceptedBookings.length > 0 || rejectedBookings.length > 0) {
                let notificationHTML = '';
                
                acceptedBookings.forEach(booking => {
                    const workers = JSON.parse(localStorage.getItem('workers') || '[]');
                    const worker = workers.find(w => w.mobile === booking.workerMobile);
                    if (worker) {
                        notificationHTML += `
                            <div class="bg-green-50 border-2 border-green-300 rounded-lg p-4 mb-3">
                                <p class="text-green-800 font-semibold">✅ Booking Accepted!</p>
                                <p class="text-green-700">${worker.name} has accepted your booking request.</p>
                                <p class="text-green-600 text-sm">Contact: ${worker.mobile}</p>
                            </div>
                        `;
                    }
                });
                
                rejectedBookings.forEach(booking => {
                    const workers = JSON.parse(localStorage.getItem('workers') || '[]');
                    const worker = workers.find(w => w.mobile === booking.workerMobile);
                    if (worker) {
                        notificationHTML += `
                            <div class="bg-red-50 border-2 border-red-300 rounded-lg p-4 mb-3">
                                <p class="text-red-800 font-semibold">❌ Booking Rejected</p>
                                <p class="text-red-700">${worker.name} is unavailable for your request.</p>
                            </div>
                        `;
                    }
                });
                
                notifications.innerHTML = notificationHTML;
            }
            
            // Show all bookings
            if (myBookings.length === 0) {
                bookingsList.innerHTML = '<p class="text-gray-500 text-center py-4 col-span-full">No bookings yet</p>';
                return;
            }
            
            bookingsList.innerHTML = myBookings.map(booking => {
                const workers = JSON.parse(localStorage.getItem('workers') || '[]');
                const worker = workers.find(w => w.mobile === booking.workerMobile);
                
                let statusBadge = '';
                if (booking.status === 'pending') {
                    statusBadge = '<span class="bg-yellow-100 text-yellow-800 px-2 py-1 rounded-full text-xs font-semibold">Pending</span>';
                } else if (booking.status === 'accepted') {
                    statusBadge = '<span class="bg-green-100 text-green-800 px-2 py-1 rounded-full text-xs font-semibold">Accepted</span>';
                } else if (booking.status === 'rejected') {
                    statusBadge = '<span class="bg-red-100 text-red-800 px-2 py-1 rounded-full text-xs font-semibold">Rejected</span>';
                }
                
                return `
                    <div class="border-2 border-gray-200 rounded-lg p-4">
                        <div class="flex justify-between items-start mb-2">
                            <h4 class="font-bold text-gray-800">${worker ? worker.name : 'Unknown Worker'}</h4>
                            ${statusBadge}
                        </div>
                        <p class="text-gray-600 text-sm">${worker ? worker.profession : ''}</p>
                        <p class="text-gray-600 text-sm">📞 ${worker ? worker.mobile : ''}</p>
                        <p class="text-gray-500 text-xs mt-2">${new Date(booking.createdAt).toLocaleString()}</p>
                    </div>
                `;
            }).join('');
        }

        // Load Workers
        function loadWorkers(filterProfession = '') {
            const workers = JSON.parse(localStorage.getItem('workers') || '[]');
            const workersList = document.getElementById('workersList');

            let filteredWorkers = workers;
            if (filterProfession) {
                filteredWorkers = workers.filter(w => w.profession === filterProfession);
            }

            if (filteredWorkers.length === 0) {
                workersList.innerHTML = '<p class="text-gray-500 text-center py-8 col-span-full">No workers available</p>';
                return;
            }

            workersList.innerHTML = filteredWorkers.map(worker => `
                <div class="border-2 border-gray-200 rounded-lg p-4 hover:border-blue-400 transition">
                    <div class="text-center mb-3">
                        <img src="${worker.photo}" alt="${worker.name}" class="w-24 h-24 rounded-full mx-auto object-cover mb-2">
                        <h4 class="text-lg font-bold text-gray-800">${worker.name}</h4>
                    </div>
                    <div class="space-y-2">
                        <p class="text-gray-600">
                            <span class="inline-block bg-green-100 text-green-800 px-3 py-1 rounded-full text-sm font-semibold">
                                ${getProfessionIcon(worker.profession)} ${worker.profession}
                            </span>
                        </p>
                        <p class="text-gray-600">📞 ${worker.mobile}</p>
                        <p class="text-gray-600">📍 ${worker.location}</p>
                    </div>
                    <div class="mt-4 space-y-2">
                        <button onclick="contactWorker('${worker.mobile}')" class="w-full bg-blue-600 text-white py-2 rounded-lg hover:bg-blue-700 transition text-sm">
                            Contact
                        </button>
                        <button onclick="bookWorker('${worker.mobile}', '${worker.name}')" class="w-full bg-green-600 text-white py-2 rounded-lg hover:bg-green-700 transition text-sm">
                            Book Now
                        </button>
                    </div>
                </div>
            `).join('');
        }

        function getProfessionIcon(profession) {
            const icons = {
                'Plumber': '🔧',
                'Electrician': '⚡',
                'Carpenter': '🔨'
            };
            return icons[profession] || '👷';
        }

        function contactWorker(mobile) {
            alert(`Contact Worker at: ${mobile}`);
        }

        function bookWorker(workerMobile, workerName) {
            const booking = {
                id: Date.now(),
                customerName: currentUser.name,
                customerMobile: currentUser.mobile,
                workerMobile,
                workerName,
                description: 'Customer wants to book your services',
                location: currentUser.location || 'Location not specified',
                status: 'pending',
                createdAt: new Date().toISOString()
            };

            const bookings = JSON.parse(localStorage.getItem('bookings') || '[]');
            bookings.push(booking);
            localStorage.setItem('bookings', JSON.stringify(bookings));

            // Add to worker requests
            const workerRequests = JSON.parse(localStorage.getItem('workerRequests') || '[]');
            workerRequests.push({
                ...booking,
                workerNotified: false
            });
            localStorage.setItem('workerRequests', JSON.stringify(workerRequests));

            alert(`Booking request sent to ${workerName}!`);
        }

        // Show Worker Dashboard
        function showWorkerDashboard() {
            document.getElementById('workerDashName').textContent = currentUser.name;
            document.getElementById('workerDashProfession').textContent = currentUser.profession;
            document.getElementById('workerProfileName').textContent = currentUser.name;
            document.getElementById('workerProfileMobile').textContent = `+91 ${currentUser.mobile}`;
            document.getElementById('workerProfileProfession').textContent = currentUser.profession;
            document.getElementById('workerProfileLocation').textContent = currentUser.location;
            document.getElementById('workerProfilePhoto').src = currentUser.photo || 'https://picsum.photos/seed/worker/200/200.jpg';
            
            loadWorkRequests();
            updateMessageBadges();
            showScreen('workerDashboard');
        }

        function showWorkerEditProfileModal() {
            document.getElementById('workerEditName').value = currentUser.name;
            document.getElementById('workerEditLocation').value = currentUser.location;
            document.getElementById('workerEditPhotoPreview').src = currentUser.photo;
            document.getElementById('workerEditPhotoPreview').classList.remove('hidden');
            showModal('workerEditProfileModal');
        }

        async function handleWorkerUpdateProfile() {
            const name = document.getElementById('workerEditName').value;
            const location = document.getElementById('workerEditLocation').value;
            const photoInput = document.getElementById('workerEditPhoto');
            
            let photoData = currentUser.photo;
            if (photoInput.files && photoInput.files[0]) {
                const reader = new FileReader();
                photoData = await new Promise((resolve) => {
                    reader.onload = function(e) {
                        resolve(e.target.result);
                    }
                    reader.readAsDataURL(photoInput.files[0]);
                });
            }

            const workers = JSON.parse(localStorage.getItem('workers') || '[]');
            const workerIndex = workers.findIndex(w => w.mobile === currentUser.mobile);

            if (workerIndex !== -1) {
                workers[workerIndex].name = name;
                workers[workerIndex].location = location;
                workers[workerIndex].photo = photoData;
                localStorage.setItem('workers', JSON.stringify(workers));

                currentUser.name = name;
                currentUser.location = location;
                currentUser.photo = photoData;
                localStorage.setItem('currentUser', JSON.stringify(currentUser));

                alert('Profile updated successfully!');
                closeModal('workerEditProfileModal');
                showWorkerDashboard();
            }
        }

        // Load Work Requests
        function loadWorkRequests() {
            const workerRequests = JSON.parse(localStorage.getItem('workerRequests') || '[]');
            const myRequests = workerRequests.filter(r => r.workerMobile === currentUser.mobile && r.status === 'pending');
            const requestsList = document.getElementById('workRequestsList');
            const badge = document.getElementById('notificationBadge');

            if (myRequests.length > 0) {
                badge.textContent = myRequests.length;
                badge.classList.remove('hidden');
                
                requestsList.innerHTML = myRequests.map(request => `
                    <div class="border-2 border-gray-200 rounded-lg p-4">
                        <h4 class="font-bold text-gray-800">${request.customerName}</h4>
                        <p class="text-gray-600">📞 ${request.customerMobile}</p>
                        <p class="text-gray-600">📍 ${request.location}</p>
                        <p class="text-gray-600 mt-2">${request.description}</p>
                        <div class="flex space-x-2 mt-3">
                            <button onclick="viewRequest('${request.id}')" class="flex-1 bg-blue-600 text-white py-2 rounded-lg hover:bg-blue-700 transition text-sm">
                                View Details
                            </button>
                        </div>
                    </div>
                `).join('');

                // Show popup for new requests
                const newRequests = myRequests.filter(r => !r.workerNotified);
                if (newRequests.length > 0) {
                    currentRequest = newRequests[0];
                    showWorkRequestModal(currentRequest);
                    
                    // Mark as notified
                    workerRequests.forEach(r => {
                        if (r.id === currentRequest.id) {
                            r.workerNotified = true;
                        }
                    });
                    localStorage.setItem('workerRequests', JSON.stringify(workerRequests));
                }
            } else {
                badge.classList.add('hidden');
                requestsList.innerHTML = '<p class="text-gray-500 text-center py-4">No new requests</p>';
            }
        }

        // Show Work Request Modal
        function showWorkRequestModal(request) {
            const details = document.getElementById('workRequestDetails');
            details.innerHTML = `
                <div class="space-y-3">
                    <div>
                        <label class="text-gray-600 text-sm">Customer Name</label>
                        <p class="font-semibold">${request.customerName}</p>
                    </div>
                    <div>
                        <label class="text-gray-600 text-sm">Mobile</label>
                        <p class="font-semibold">${request.customerMobile}</p>
                    </div>
                    <div>
                        <label class="text-gray-600 text-sm">Location</label>
                        <p class="font-semibold">${request.location}</p>
                    </div>
                    <div>
                        <label class="text-gray-600 text-sm">Work Description</label>
                        <p class="font-semibold">${request.description}</p>
                    </div>
                </div>
            `;
            showModal('workRequestModal');
        }

        function viewRequest(requestId) {
            const workerRequests = JSON.parse(localStorage.getItem('workerRequests') || '[]');
            const request = workerRequests.find(r => r.id == requestId);
            if (request) {
                currentRequest = request;
                showWorkRequestModal(request);
            }
        }

        function acceptRequest() {
            if (!currentRequest) return;

            const workerRequests = JSON.parse(localStorage.getItem('workerRequests') || '[]');
            const requestIndex = workerRequests.findIndex(r => r.id === currentRequest.id);
            
            if (requestIndex !== -1) {
                workerRequests[requestIndex].status = 'accepted';
                localStorage.setItem('workerRequests', JSON.stringify(workerRequests));
            }

            const bookings = JSON.parse(localStorage.getItem('bookings') || '[]');
            const bookingIndex = bookings.findIndex(b => b.id === currentRequest.id);
            
            if (bookingIndex !== -1) {
                bookings[bookingIndex].status = 'accepted';
                localStorage.setItem('bookings', JSON.stringify(bookings));
            }

            alert('Request accepted! Customer will be notified.');
            closeModal('workRequestModal');
            loadWorkRequests();
        }

        function rejectRequest() {
            if (!currentRequest) return;

            const workerRequests = JSON.parse(localStorage.getItem('workerRequests') || '[]');
            const requestIndex = workerRequests.findIndex(r => r.id === currentRequest.id);
            
            if (requestIndex !== -1) {
                workerRequests[requestIndex].status = 'rejected';
                localStorage.setItem('workerRequests', JSON.stringify(workerRequests));
            }

            const bookings = JSON.parse(localStorage.getItem('bookings') || '[]');
            const bookingIndex = bookings.findIndex(b => b.id === currentRequest.id);
            
            if (bookingIndex !== -1) {
                bookings[bookingIndex].status = 'rejected';
                localStorage.setItem('bookings', JSON.stringify(bookings));
            }

            alert('Request rejected.');
            closeModal('workRequestModal');
            loadWorkRequests();
        }

        // === MESSAGING FUNCTIONALITY ===

        // Customer Chat Functions
        function loadCustomerMessages() {
            const messagesContainer = document.getElementById('chatMessages');
            const messages = JSON.parse(localStorage.getItem('admin_messages') || '[]')
                .filter(m => m.sender_type === 'customer' && m.sender_id === currentUser.mobile)
                .sort((a, b) => new Date(a.timestamp) - new Date(b.timestamp));

            if (messages.length === 0) {
                messagesContainer.innerHTML = `
                    <div class="text-center text-gray-500 py-8">
                        <p>No messages yet. Start a conversation with admin support!</p>
                    </div>
                `;
                return;
            }

            messagesContainer.innerHTML = messages.map(msg => {
                const isAdminReply = msg.sender_type === 'admin';
                const bubbleClass = isAdminReply ? 'chat-bubble admin' : 
                                   msg.status === 'pending' ? 'chat-bubble pending' : 'chat-bubble user';
                
                return `
                    <div class="flex ${isAdminReply ? 'justify-start' : 'justify-end'}">
                        <div class="${bubbleClass}">
                            <p class="text-sm font-semibold mb-1">
                                ${isAdminReply ? 'Admin Support' : 'You'}
                            </p>
                            <p>${msg.message_text}</p>
                            <div class="message-timestamp">
                                ${new Date(msg.timestamp).toLocaleString()}
                            </div>
                            ${msg.status && !isAdminReply ? `
                                <div class="message-status">
                                    ${msg.status === 'pending' ? 'Sending...' : msg.status === 'read' ? 'Delivered' : 'Sent'}
                                </div>
                            ` : ''}
                        </div>
                    </div>
                `;
            }).join('');

            messagesContainer.scrollTop = messagesContainer.scrollHeight;

            // Mark messages as read
            messages.forEach(msg => {
                if (msg.sender_type === 'admin' && !msg.read) {
                    msg.read = true;
                }
            });
            localStorage.setItem('admin_messages', JSON.stringify(messages));
        }

        // Worker Chat Functions
        function loadWorkerMessages() {
            const messagesContainer = document.getElementById('chatMessages');
            const messages = JSON.parse(localStorage.getItem('admin_messages') || '[]')
                .filter(m => m.sender_type === 'worker' && m.sender_id === currentUser.mobile)
                .sort((a, b) => new Date(a.timestamp) - new Date(b.timestamp));

            if (messages.length === 0) {
                messagesContainer.innerHTML = `
                    <div class="text-center text-gray-500 py-8">
                        <p>No messages yet. Start a conversation with admin support!</p>
                    </div>
                `;
                return;
            }

            messagesContainer.innerHTML = messages.map(msg => {
                const isAdminReply = msg.sender_type === 'admin';
                const bubbleClass = isAdminReply ? 'chat-bubble admin' : 
                                   msg.status === 'pending' ? 'chat-bubble pending' : 'chat-bubble user';
                
                return `
                    <div class="flex ${isAdminReply ? 'justify-start' : 'justify-end'}">
                        <div class="${bubbleClass}">
                            <p class="text-sm font-semibold mb-1">
                                ${isAdminReply ? 'Admin Support' : 'You'}
                            </p>
                            <p>${msg.message_text}</p>
                            <div class="message-timestamp">
                                ${new Date(msg.timestamp).toLocaleString()}
                            </div>
                            ${msg.status && !isAdminReply ? `
                                <div class="message-status">
                                    ${msg.status === 'pending' ? 'Sending...' : msg.status === 'read' ? 'Delivered' : 'Sent'}
                                </div>
                            ` : ''}
                        </div>
                    </div>
                `;
            }).join('');

            messagesContainer.scrollTop = messagesContainer.scrollHeight;

            // Mark messages as read
            messages.forEach(msg => {
                if (msg.sender_type === 'admin' && !msg.read) {
                    msg.read = true;
                }
            });
            localStorage.setItem('admin_messages', JSON.stringify(messages));
        }

        function handleChatMessage() {
            const messageInput = document.getElementById('chatInput');
            const message = messageInput.value.trim();
            
            if (!message) return;

            // Create message object
            const messageObj = {
                id: Date.now().toString(),
                sender_type: currentUser.type,
                sender_id: currentUser.mobile,
                message_text: message,
                timestamp: new Date().toISOString(),
                reply: null,
                status: 'sent',
                read: false
            };

            // Save to localStorage
            const messages = JSON.parse(localStorage.getItem('admin_messages') || '[]');
            messages.push(messageObj);
            localStorage.setItem('admin_messages', JSON.stringify(messages));

            messageInput.value = '';
            
            // Reload messages
            if (currentUser.type === 'customer') {
                loadCustomerMessages();
            } else {
                loadWorkerMessages();
            }
            
            updateMessageBadges();
            
            if (voiceEnabled) {
                speak('Message sent to admin');
            }
        }

        // Admin Messages Functions
        function showMessagesModal() {
            showModal('messagesModal');
            loadAllMessages();
            loadAdminMessageUsers();
        }

        function loadAdminMessageUsers() {
            const messages = JSON.parse(localStorage.getItem('admin_messages') || '[]');
            const uniqueUsers = [...new Set(messages.map(m => JSON.stringify({
                mobile: m.sender_id,
                name: getUserName(m.sender_id, m.sender_type),
                type: m.sender_type
            })))].map(u => JSON.parse(u));

            const userSelect = document.getElementById('adminChatUserSelect');
            
            userSelect.innerHTML = '<option value="">Select User</option>' +
                uniqueUsers.map(user => 
                    `<option value="${user.mobile}">${user.name} (${user.type}) - ${user.mobile}</option>`
                ).join('');
        }

        function getUserName(mobile, type) {
            if (type === 'customer') {
                const customers = JSON.parse(localStorage.getItem('customers') || '[]');
                const customer = customers.find(c => c.mobile === mobile);
                return customer ? customer.name : 'Unknown Customer';
            } else if (type === 'worker') {
                const workers = JSON.parse(localStorage.getItem('workers') || '[]');
                const worker = workers.find(w => w.mobile === mobile);
                return worker ? worker.name : 'Unknown Worker';
            }
            return 'Unknown User';
        }

        function loadAllMessages() {
            const messageList = document.getElementById('messageList');
            const messages = JSON.parse(localStorage.getItem('admin_messages') || '[]')
                .sort((a, b) => new Date(b.timestamp) - new Date(a.timestamp));

            if (messages.length === 0) {
                messageList.innerHTML = `
                    <div class="text-center text-gray-500 py-8">
                        <p>No messages yet.</p>
                    </div>
                `;
                return;
            }

            // Group messages by user
            const groupedMessages = messages.reduce((acc, msg) => {
                const key = `${msg.sender_type}_${msg.sender_id}`;
                if (!acc[key]) {
                    acc[key] = [];
                }
                acc[key].push(msg);
                return acc;
            }, {});

            messageList.innerHTML = Object.entries(groupedMessages).map(([key, msgs]) => {
                const latestMsg = msgs[0];
                const userName = getUserName(latestMsg.sender_id, latestMsg.sender_type);
                const unreadCount = msgs.filter(m => m.sender_type === 'admin' && !m.read).length;
                
                return `
                    <div class="p-3 border border-gray-200 rounded-lg cursor-pointer hover:bg-gray-50 ${selectedChatUser === latestMsg.sender_id ? 'bg-blue-50 border-blue-300' : ''}" 
                         onclick="selectChatUser('${latestMsg.sender_id}')">
                        <div class="flex justify-between items-start">
                            <div class="flex-1">
                                <p class="font-semibold text-gray-800">${userName}</p>
                                <p class="text-sm text-gray-600">${latestMsg.sender_type}</p>
                                <p class="text-sm text-gray-500 mt-1">${latestMsg.message_text.substring(0, 50)}...</p>
                                <p class="text-xs text-gray-400 mt-1">${new Date(latestMsg.timestamp).toLocaleString()}</p>
                            </div>
                            ${unreadCount > 0 ? `
                                <div class="unread-badge ml-2">${unreadCount}</div>
                            ` : ''}
                        </div>
                    </div>
                `;
            }).join('');
        }

        function selectChatUser(mobile) {
            selectedChatUser = mobile;
            document.getElementById('adminChatUserSelect').value = mobile;
            loadAdminChatMessages();
            loadAllMessages(); // Refresh to update selection highlight
        }

        function loadAdminChatMessages() {
            const messagesContainer = document.getElementById('adminChatMessages');
            
            if (!selectedChatUser) {
                messagesContainer.innerHTML = `
                    <div class="text-center text-gray-500 py-8">
                        <p>Select a user to view messages</p>
                    </div>
                `;
                return;
            }

            const allMessages = JSON.parse(localStorage.getItem('admin_messages') || '[]');
            const userMessages = allMessages
                .filter(m => m.sender_id === selectedChatUser)
                .sort((a, b) => new Date(a.timestamp) - new Date(b.timestamp));

            if (userMessages.length === 0) {
                messagesContainer.innerHTML = `
                    <div class="text-center text-gray-500 py-8">
                        <p>No messages with this user</p>
                    </div>
                `;
                return;
            }

            messagesContainer.innerHTML = userMessages.map(msg => {
                const isAdminReply = msg.sender_type === 'admin';
                const isUser = !isAdminReply;
                
                return `
                    <div class="flex ${isAdminReply ? 'justify-end' : 'justify-start'}">
                        <div class="chat-bubble ${isAdminReply ? 'admin' : 'user'}">
                            <p class="text-sm font-semibold mb-1">
                                ${isAdminReply ? 'You' : getUserName(msg.sender_id, msg.sender_type)}
                            </p>
                            <p>${msg.message_text}</p>
                            <div class="message-timestamp">
                                ${new Date(msg.timestamp).toLocaleString()}
                            </div>
                        </div>
                    </div>
                `;
            }).join('');

            messagesContainer.scrollTop = messagesContainer.scrollHeight;

            // Mark messages as read
            allMessages.forEach(msg => {
                if (msg.sender_id === selectedChatUser && msg.sender_type === 'admin' && !msg.read) {
                    msg.read = true;
                }
            });
            localStorage.setItem('admin_messages', JSON.stringify(allMessages));
            
            updateMessageBadges();
        }

        function handleAdminChatMessage() {
            const messageInput = document.getElementById('adminChatInput');
            const message = messageInput.value.trim();
            const userMobile = document.getElementById('adminChatUserSelect').value;

            if (!userMobile || !message) {
                alert('Please select a user and enter a message');
                return;
            }

            // Determine sender type based on selected user
            let senderType = 'customer';
            const customer = JSON.parse(localStorage.getItem('customers') || '[]').find(c => c.mobile === userMobile);
            if (!customer) {
                senderType = 'worker';
            }

            // Create admin reply message
            const messageObj = {
                id: Date.now().toString(),
                sender_type: 'admin',
                sender_id: 'admin',
                message_text: message,
                timestamp: new Date().toISOString(),
                reply_to: userMobile,
                reply_to_type: senderType,
                read: false
            };

            // Save to localStorage
            const messages = JSON.parse(localStorage.getItem('admin_messages') || '[]');
            messages.push(messageObj);
            localStorage.setItem('admin_messages', JSON.stringify(messages));

            messageInput.value = '';
            loadAdminChatMessages();
            loadAllMessages();
            updateMessageBadges();

            if (voiceEnabled) {
                speak('Reply sent to user');
            }
        }

        // Badge Updates
        function updateMessageBadges() {
            const messages = JSON.parse(localStorage.getItem('admin_messages') || '[]');
            
            // Customer/Worker badges
            if (currentUser && (currentUser.type === 'customer' || currentUser.type === 'worker')) {
                const userMessages = messages.filter(m => 
                    m.sender_id === currentUser.mobile && 
                    m.sender_type === 'admin' && 
                    !m.read
                ).length;
                
                const contactBtn = document.getElementById('contactAdminBtn');
                if (userMessages > 0) {
                    if (!contactBtn.querySelector('.unread-badge')) {
                        const badge = document.createElement('div');
                        badge.className = 'unread-badge';
                        badge.textContent = userMessages;
                        badge.style.position = 'absolute';
                        badge.style.top = '-8px';
                        badge.style.right = '-8px';
                        contactBtn.appendChild(badge);
                    } else {
                        contactBtn.querySelector('.unread-badge').textContent = userMessages;
                    }
                } else {
                    const badge = contactBtn.querySelector('.unread-badge');
                    if (badge) {
                        badge.remove();
                    }
                }
            }
            
            // Admin badges
            const unreadAdminMessages = messages.filter(m => 
                m.sender_type !== 'admin' && !m.read
            ).length;
            
            const adminBadge = document.getElementById('adminMessageBadge');
            const adminBadgeMain = document.getElementById('adminMessageBadgeMain');
            
            if (unreadAdminMessages > 0) {
                if (adminBadge) {
                    adminBadge.textContent = unreadAdminMessages;
                    adminBadge.classList.remove('hidden');
                }
                if (adminBadgeMain) {
                    adminBadgeMain.textContent = unreadAdminMessages;
                    adminBadgeMain.classList.remove('hidden');
                }
            } else {
                if (adminBadge) adminBadge.classList.add('hidden');
                if (adminBadgeMain) adminBadgeMain.classList.add('hidden');
            }
        }

        // Check Session
        function checkSession() {
            const user = localStorage.getItem('currentUser');
            if (user) {
                currentUser = JSON.parse(user);
                if (currentUser.type === 'customer') {
                    showCustomerDashboard();
                } else if (currentUser.type === 'worker') {
                    showWorkerDashboard();
                } else if (currentUser.type === 'admin') {
                    showAdminDashboard();
                }
            }
        }

        // Logout
        function logout() {
            localStorage.removeItem('currentUser');
            currentUser = null;
            showScreen('homeScreen');
        }
/* === 🗣️ Multilingual Voice Assistant === */
const voiceMessages = {
  name: {
    en: "Please enter your name.",
    te: "దయచేసి మీ పేరు నమోదు చేయండి.",
    hi: "कृपया अपना नाम दर्ज करें।"
  },
  mobile: {
    en: "Please enter your mobile number.",
    te: "దయచేసి మీ మొబైల్ నంబర్ నమోదు చేయండి.",
    hi: "कृपया अपना मोबाइल नंबर दर्ज करें।"
  },
  password: {
    en: "Please enter your password.",
    te: "దయచేసి మీ పాస్‌వర్డ్ నమోదు చేయండి.",
    hi: "कृपया अपना पासवर्ड दर्ज करें।"
  }
};

// Function to speak messages in selected language
function speakField(fieldType) {
  if (!voiceEnabled) return;
  const msg = new SpeechSynthesisUtterance(voiceMessages[fieldType][currentLanguage]);
  msg.lang =
    currentLanguage === "te" ? "te-IN" :
    currentLanguage === "hi" ? "hi-IN" :
    "en-IN";
  window.speechSynthesis.speak(msg);
}

// Add voice triggers to all input fields
function setupVoiceAssist() {
  const nameInputs = document.querySelectorAll('input[id*="Name"]');
  const mobileInputs = document.querySelectorAll('input[id*="Mobile"]');
  const passwordInputs = document.querySelectorAll('input[type="password"]');

  nameInputs.forEach(inp => inp.addEventListener('focus', () => speakField('name')));
  mobileInputs.forEach(inp => inp.addEventListener('focus', () => speakField('mobile')));
  passwordInputs.forEach(inp => inp.addEventListener('focus', () => speakField('password')));
}

// Toggle voice feature
function toggleVoice() {
  voiceEnabled = !voiceEnabled;
  const icon = document.getElementById('voiceIcon');
  icon.classList.toggle('text-green-600', voiceEnabled);
  if (voiceEnabled) {
    const msg = new SpeechSynthesisUtterance(
      currentLanguage === "en"
        ? "Voice assistant enabled."
        : currentLanguage === "te"
        ? "వాయిస్ అసిస్టెంట్ ప్రారంభించబడింది."
        : "वॉइस असिस्टेंट सक्रिय किया गया है।"
    );
    msg.lang = currentLanguage === "te" ? "te-IN" : currentLanguage === "hi" ? "hi-IN" : "en-IN";
    window.speechSynthesis.speak(msg);
  } else {
    const msg = new SpeechSynthesisUtterance(
      currentLanguage === "en"
        ? "Voice assistant disabled."
        : currentLanguage === "te"
        ? "వాయిస్ అసిస్టెంట్ ఆపివేయబడింది."
        : "वॉइस असिस्टेंट बंद किया गया है।"
    );
    msg.lang = currentLanguage === "te" ? "te-IN" : currentLanguage === "hi" ? "hi-IN" : "en-IN";
    window.speechSynthesis.speak(msg);
  }
}

// Run after page loads
document.addEventListener('DOMContentLoaded', setupVoiceAssist);

/* === 🔘 Reposition Logout Buttons === */
function repositionLogoutButtons() {
  const customerLogout = document.querySelector('#customerDashboard button span[data-lang="logout"]')?.parentElement;
  const workerLogout = document.querySelector('#workerDashboard button span[data-lang="logout"]')?.parentElement;

  if (customerLogout) {
    customerLogout.style.position = 'fixed';
    customerLogout.style.top = '20px';
    customerLogout.style.right = '20px';
    customerLogout.style.zIndex = '999';
  }

  if (workerLogout) {
    workerLogout.style.position = 'fixed';
    workerLogout.style.top = '20px';
    workerLogout.style.right = '20px';
    workerLogout.style.zIndex = '999';
  }
}

document.addEventListener('DOMContentLoaded', repositionLogoutButtons);

    </script>
</body>
</html>
