<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AI Call Agent SaaS - Complete Platform</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/axios/dist/axios.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <script src="https://js.stripe.com/v3/"></script>
    <style>
        /* Custom Styles */
        :root {
            --primary: #3b82f6;
            --secondary: #10b981;
            --dark: #1f2937;
            --light: #f9fafb;
        }
        
        .gradient-bg {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
        }
        
        .card-hover {
            transition: transform 0.3s ease, box-shadow 0.3s ease;
        }
        
        .card-hover:hover {
            transform: translateY(-5px);
            box-shadow: 0 20px 25px -5px rgba(0, 0, 0, 0.1), 0 10px 10px -5px rgba(0, 0, 0, 0.04);
        }
        
        .sidebar {
            transition: all 0.3s ease;
        }
        
        @media (max-width: 768px) {
            .sidebar {
                transform: translateX(-100%);
                position: fixed;
                z-index: 50;
            }
            
            .sidebar.active {
                transform: translateX(0);
            }
            
            .overlay {
                display: none;
                position: fixed;
                top: 0;
                left: 0;
                right: 0;
                bottom: 0;
                background: rgba(0,0,0,0.5);
                z-index: 40;
            }
            
            .overlay.active {
                display: block;
            }
        }
        
        .call-recording {
            background: linear-gradient(90deg, #f0f9ff, #e0f2fe);
        }
        
        .status-active {
            background-color: #dcfce7;
            color: #166534;
        }
        
        .status-pending {
            background-color: #fef3c7;
            color: #92400e;
        }
        
        .status-paused {
            background-color: #f3f4f6;
            color: #6b7280;
        }
        
        .tab-active {
            border-bottom: 3px solid #3b82f6;
            color: #3b82f6;
            font-weight: 600;
        }
        
        .animate-pulse {
            animation: pulse 2s cubic-bezier(0.4, 0, 0.6, 1) infinite;
        }
        
        @keyframes pulse {
            0%, 100% { opacity: 1; }
            50% { opacity: 0.5; }
        }
    </style>
</head>
<body class="bg-gray-50">
    <!-- Loading Screen -->
    <div id="loadingScreen" class="fixed inset-0 bg-white z-50 flex items-center justify-center">
        <div class="text-center">
            <div class="w-16 h-16 border-4 border-blue-500 border-t-transparent rounded-full animate-spin mx-auto mb-4"></div>
            <h2 class="text-xl font-semibold text-gray-700">Loading AI Call Agent Platform</h2>
            <p class="text-gray-500 mt-2">Initializing your dashboard...</p>
        </div>
    </div>

    <!-- Login/Signup Page -->
    <div id="authPage" class="min-h-screen hidden">
        <div class="flex min-h-screen">
            <!-- Left Side - Form -->
            <div class="w-full md:w-1/2 flex items-center justify-center p-8">
                <div class="w-full max-w-md">
                    <!-- Logo -->
                    <div class="text-center mb-8">
                        <h1 class="text-3xl font-bold text-gray-800">
                            <i class="fas fa-robot text-blue-500 mr-2"></i>AI Call Agent
                        </h1>
                        <p class="text-gray-600 mt-2">Automate your calls with AI intelligence</p>
                    </div>

                    <!-- Tabs -->
                    <div class="flex border-b mb-8">
                        <button onclick="switchAuthTab('login')" class="flex-1 py-3 font-medium text-center auth-tab" data-tab="login">
                            Login
                        </button>
                        <button onclick="switchAuthTab('signup')" class="flex-1 py-3 font-medium text-center auth-tab" data-tab="signup">
                            Sign Up
                        </button>
                    </div>

                    <!-- Login Form -->
                    <div id="loginForm" class="auth-form">
                        <form onsubmit="handleLogin(event)" class="space-y-4">
                            <div>
                                <label class="block text-sm font-medium text-gray-700 mb-1">Email</label>
                                <input type="email" id="loginEmail" required 
                                       class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-blue-500">
                            </div>
                            <div>
                                <label class="block text-sm font-medium text-gray-700 mb-1">Password</label>
                                <input type="password" id="loginPassword" required 
                                       class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-blue-500">
                            </div>
                            <button type="submit" class="w-full bg-blue-500 text-white py-2 px-4 rounded-lg hover:bg-blue-600 transition font-medium">
                                Sign In
                            </button>
                        </form>
                        <div class="mt-4 text-center">
                            <a href="#" class="text-sm text-blue-500 hover:underline">Forgot password?</a>
                        </div>
                    </div>

                    <!-- Signup Form -->
                    <div id="signupForm" class="auth-form hidden">
                        <form onsubmit="handleSignup(event)" class="space-y-4">
                            <div>
                                <label class="block text-sm font-medium text-gray-700 mb-1">Full Name</label>
                                <input type="text" id="signupName" required 
                                       class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-blue-500">
                            </div>
                            <div>
                                <label class="block text-sm font-medium text-gray-700 mb-1">Email</label>
                                <input type="email" id="signupEmail" required 
                                       class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-blue-500">
                            </div>
                            <div>
                                <label class="block text-sm font-medium text-gray-700 mb-1">Phone Number</label>
                                <input type="tel" id="signupPhone" required 
                                       class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-blue-500">
                            </div>
                            <div>
                                <label class="block text-sm font-medium text-gray-700 mb-1">Password</label>
                                <input type="password" id="signupPassword" required 
                                       class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-blue-500">
                            </div>
                            <div>
                                <label class="block text-sm font-medium text-gray-700 mb-1">Select Plan</label>
                                <select id="signupPlan" required 
                                        class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-blue-500">
                                    <option value="">Choose a plan</option>
                                    <option value="starter">Starter - $29/month (100 calls)</option>
                                    <option value="professional">Professional - $79/month (500 calls)</option>
                                    <option value="enterprise">Enterprise - $199/month (2000 calls)</option>
                                </select>
                            </div>
                            
                            <!-- Payment Method -->
                            <div>
                                <label class="block text-sm font-medium text-gray-700 mb-2">Payment Method</label>
                                <div class="space-y-2">
                                    <label class="flex items-center">
                                        <input type="radio" name="paymentMethod" value="stripe" checked 
                                               class="mr-2" onchange="togglePaymentMethod()">
                                        <span>Credit/Debit Card (Stripe)</span>
                                    </label>
                                    <label class="flex items-center">
                                        <input type="radio" name="paymentMethod" value="bank" 
                                               class="mr-2" onchange="togglePaymentMethod()">
                                        <span>Manual Bank Transfer</span>
                                    </label>
                                    <label class="flex items-center">
                                        <input type="radio" name="paymentMethod" value="whatsapp" 
                                               class="mr-2" onchange="togglePaymentMethod()">
                                        <span>WhatsApp Payment</span>
                                    </label>
                                </div>
                            </div>

                            <!-- Bank Payment Details -->
                            <div id="bankPaymentDetails" class="hidden p-4 bg-blue-50 rounded-lg">
                                <h4 class="font-medium text-blue-800 mb-2">Bank Transfer Details</h4>
                                <div class="space-y-1 text-sm">
                                    <p><strong>Account Name:</strong> Ali Hassan</p>
                                    <p><strong>Bank:</strong> United Bank Limited (UBL)</p>
                                    <p><strong>IBAN:</strong> PK96UNIL0109000338084413</p>
                                    <p class="mt-2">After payment, upload screenshot:</p>
                                    <input type="file" id="paymentProof" accept="image/*" class="w-full mt-2">
                                </div>
                            </div>

                            <!-- WhatsApp Payment Details -->
                            <div id="whatsappPaymentDetails" class="hidden p-4 bg-green-50 rounded-lg">
                                <h4 class="font-medium text-green-800 mb-2">WhatsApp Payment</h4>
                                <p class="text-sm mb-2">Send payment screenshot to:</p>
                                <div class="flex items-center space-x-2">
                                    <i class="fab fa-whatsapp text-green-600 text-xl"></i>
                                    <span class="font-medium">0301-8973444</span>
                                </div>
                                <p class="text-sm mt-2">Upload payment screenshot for verification:</p>
                                <input type="file" id="whatsappProof" accept="image/*" class="w-full mt-2">
                            </div>

                            <!-- Stripe Payment Element -->
                            <div id="stripePaymentElement">
                                <div id="card-element" class="p-3 border border-gray-300 rounded-lg"></div>
                                <div id="card-errors" class="text-red-500 text-sm mt-2"></div>
                            </div>

                            <button type="submit" id="signupBtn" class="w-full bg-blue-500 text-white py-2 px-4 rounded-lg hover:bg-blue-600 transition font-medium">
                                Create Account
                            </button>
                        </form>
                    </div>
                </div>
            </div>

            <!-- Right Side - Info -->
            <div class="hidden md:flex md:w-1/2 gradient-bg items-center justify-center p-12">
                <div class="text-white max-w-lg">
                    <h2 class="text-4xl font-bold mb-6">Automate Your Calls with AI</h2>
                    <ul class="space-y-4">
                        <li class="flex items-center">
                            <i class="fas fa-robot text-xl mr-3"></i>
                            <span>Create AI agents in minutes</span>
                        </li>
                        <li class="flex items-center">
                            <i class="fas fa-phone text-xl mr-3"></i>
                            <span>Inbound & outbound AI calls</span>
                        </li>
                        <li class="flex items-center">
                            <i class="fas fa-language text-xl mr-3"></i>
                            <span>English & Urdu language support</span>
                        </li>
                        <li class="flex items-center">
                            <i class="fas fa-whatsapp text-xl mr-3"></i>
                            <span>WhatsApp/SMS follow-ups</span>
                        </li>
                        <li class="flex items-center">
                            <i class="fas fa-chart-line text-xl mr-3"></i>
                            <span>Complete dashboard & analytics</span>
                        </li>
                    </ul>
                </div>
            </div>
        </div>
    </div>

    <!-- Main Dashboard -->
    <div id="dashboard" class="min-h-screen hidden">
        <!-- Top Navigation -->
        <nav class="bg-white shadow-md">
            <div class="px-4 sm:px-6 lg:px-8">
                <div class="flex justify-between h-16">
                    <div class="flex items-center">
                        <button onclick="toggleSidebar()" class="md:hidden mr-3">
                            <i class="fas fa-bars text-xl"></i>
                        </button>
                        <div class="flex items-center">
                            <i class="fas fa-robot text-blue-500 text-2xl mr-2"></i>
                            <span class="font-bold text-xl">AI Call Agent</span>
                        </div>
                    </div>
                    
                    <div class="flex items-center space-x-4">
                        <!-- Demo Call Button -->
                        <button onclick="showDemoCallModal()" 
                                class="hidden md:flex items-center bg-green-500 text-white px-4 py-2 rounded-lg hover:bg-green-600 transition">
                            <i class="fas fa-phone mr-2"></i> Try Demo Call
                        </button>
                        
                        <!-- Call Balance -->
                        <div class="bg-blue-50 px-4 py-2 rounded-lg">
                            <span class="text-sm text-gray-600">Call Balance:</span>
                            <span class="font-bold ml-1" id="callBalance">100</span>
                        </div>
                        
                        <!-- User Menu -->
                        <div class="relative">
                            <button onclick="toggleUserMenu()" class="flex items-center space-x-2">
                                <div class="w-8 h-8 bg-blue-500 rounded-full flex items-center justify-center text-white">
                                    <span id="userInitial">U</span>
                                </div>
                                <span class="hidden md:inline" id="userName">User</span>
                                <i class="fas fa-chevron-down text-sm"></i>
                            </button>
                            <div id="userMenu" class="absolute right-0 mt-2 w-48 bg-white rounded-lg shadow-lg border hidden z-50">
                                <div class="p-4 border-b">
                                    <p class="font-medium" id="menuUserName">User Name</p>
                                    <p class="text-sm text-gray-500" id="menuUserEmail">user@email.com</p>
                                </div>
                                <div class="p-2">
                                    <a href="#" onclick="showSection('profile')" class="block px-4 py-2 hover:bg-gray-100 rounded">
                                        <i class="fas fa-user mr-2"></i>Profile
                                    </a>
                                    <a href="#" onclick="showSection('billing')" class="block px-4 py-2 hover:bg-gray-100 rounded">
                                        <i class="fas fa-credit-card mr-2"></i>Billing
                                    </a>
                                    <div class="border-t my-1"></div>
                                    <button onclick="logout()" class="w-full text-left px-4 py-2 text-red-600 hover:bg-red-50 rounded">
                                        <i class="fas fa-sign-out-alt mr-2"></i>Logout
                                    </button>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </nav>

        <!-- Mobile Demo Button -->
        <div class="md:hidden fixed bottom-4 right-4 z-40">
            <button onclick="showDemoCallModal()" 
                    class="bg-green-500 text-white p-3 rounded-full shadow-lg hover:bg-green-600 transition">
                <i class="fas fa-phone text-xl"></i>
            </button>
        </div>

        <!-- Overlay for mobile sidebar -->
        <div id="overlay" class="overlay" onclick="toggleSidebar()"></div>

        <!-- Sidebar -->
        <div id="sidebar" class="sidebar bg-white w-64 min-h-screen border-r fixed md:relative z-30">
            <div class="p-4">
                <!-- Navigation Links -->
                <div class="space-y-1 mt-8">
                    <a href="#" onclick="showSection('dashboardHome')" class="nav-link active">
                        <i class="fas fa-home mr-3"></i> Dashboard
                    </a>
                    <a href="#" onclick="showSection('myAgents')" class="nav-link">
                        <i class="fas fa-robot mr-3"></i> My Agents
                    </a>
                    <a href="#" onclick="showSection('callHistory')" class="nav-link">
                        <i class="fas fa-history mr-3"></i> Call History
                    </a>
                    <a href="#" onclick="showSection('recordings')" class="nav-link">
                        <i class="fas fa-microphone mr-3"></i> Recordings
                    </a>
                    <a href="#" onclick="showSection('leads')" class="nav-link">
                        <i class="fas fa-users mr-3"></i> Leads
                    </a>
                    <a href="#" onclick="showSection('billing')" class="nav-link">
                        <i class="fas fa-credit-card mr-3"></i> Billing
                    </a>
                    
                    <!-- Admin Only -->
                    <div id="adminLinks" class="hidden">
                        <div class="pt-4 mt-4 border-t">
                            <p class="text-xs uppercase text-gray-500 font-bold px-4 mb-2">Admin</p>
                            <a href="#" onclick="showSection('adminUsers')" class="nav-link">
                                <i class="fas fa-user-cog mr-3"></i> Manage Users
                            </a>
                            <a href="#" onclick="showSection('adminPayments')" class="nav-link">
                                <i class="fas fa-money-check-alt mr-3"></i> Payment Approvals
                            </a>
                            <a href="#" onclick="showSection('adminAnalytics')" class="nav-link">
                                <i class="fas fa-chart-bar mr-3"></i> Analytics
                            </a>
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <!-- Main Content -->
        <div class="md:ml-64 p-4 md:p-6">
            <!-- Dashboard Home -->
            <div id="dashboardHome" class="dashboard-section">
                <h1 class="text-2xl font-bold mb-6">Dashboard Overview</h1>
                
                <!-- Stats Cards -->
                <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-4 mb-8">
                    <div class="bg-white rounded-xl shadow p-6 card-hover">
                        <div class="flex items-center">
                            <div class="p-3 bg-blue-100 rounded-lg">
                                <i class="fas fa-phone text-blue-600 text-xl"></i>
                            </div>
                            <div class="ml-4">
                                <p class="text-sm text-gray-500">Total Calls</p>
                                <p class="text-2xl font-bold" id="totalCalls">245</p>
                            </div>
                        </div>
                    </div>
                    
                    <div class="bg-white rounded-xl shadow p-6 card-hover">
                        <div class="flex items-center">
                            <div class="p-3 bg-green-100 rounded-lg">
                                <i class="fas fa-users text-green-600 text-xl"></i>
                            </div>
                            <div class="ml-4">
                                <p class="text-sm text-gray-500">Total Leads</p>
                                <p class="text-2xl font-bold" id="totalLeads">189</p>
                            </div>
                        </div>
                    </div>
                    
                    <div class="bg-white rounded-xl shadow p-6 card-hover">
                        <div class="flex items-center">
                            <div class="p-3 bg-purple-100 rounded-lg">
                                <i class="fas fa-robot text-purple-600 text-xl"></i>
                            </div>
                            <div class="ml-4">
                                <p class="text-sm text-gray-500">Active Agents</p>
                                <p class="text-2xl font-bold" id="activeAgents">5</p>
                            </div>
                        </div>
                    </div>
                    
                    <div class="bg-white rounded-xl shadow p-6 card-hover">
                        <div class="flex items-center">
                            <div class="p-3 bg-yellow-100 rounded-lg">
                                <i class="fas fa-clock text-yellow-600 text-xl"></i>
                            </div>
                            <div class="ml-4">
                                <p class="text-sm text-gray-500">Call Minutes</p>
                                <p class="text-2xl font-bold" id="callMinutes">1,245</p>
                            </div>
                        </div>
                    </div>
                </div>
                
                <!-- Recent Activity & Chart -->
                <div class="grid grid-cols-1 lg:grid-cols-2 gap-6 mb-8">
                    <!-- Recent Calls -->
                    <div class="bg-white rounded-xl shadow p-6">
                        <h3 class="text-lg font-semibold mb-4">Recent Calls</h3>
                        <div class="space-y-3">
                            <div class="flex items-center justify-between p-3 bg-blue-50 rounded-lg">
                                <div>
                                    <p class="font-medium">+92 300 1234567</p>
                                    <p class="text-sm text-gray-500">Sales Agent • 5:32 min</p>
                                </div>
                                <span class="px-2 py-1 text-xs bg-green-100 text-green-800 rounded">Completed</span>
                            </div>
                            <div class="flex items-center justify-between p-3 bg-gray-50 rounded-lg">
                                <div>
                                    <p class="font-medium">+92 321 9876543</p>
                                    <p class="text-sm text-gray-500">Support Agent • 2:15 min</p>
                                </div>
                                <span class="px-2 py-1 text-xs bg-blue-100 text-blue-800 rounded">In Progress</span>
                            </div>
                        </div>
                    </div>
                    
                    <!-- Quick Actions -->
                    <div class="bg-white rounded-xl shadow p-6">
                        <h3 class="text-lg font-semibold mb-4">Quick Actions</h3>
                        <div class="grid grid-cols-2 gap-4">
                            <button onclick="showSection('myAgents')" class="p-4 bg-blue-50 rounded-lg hover:bg-blue-100 transition">
                                <i class="fas fa-plus text-blue-600 text-xl mb-2"></i>
                                <p class="font-medium">Create Agent</p>
                            </button>
                            <button onclick="showSection('leads')" class="p-4 bg-green-50 rounded-lg hover:bg-green-100 transition">
                                <i class="fas fa-download text-green-600 text-xl mb-2"></i>
                                <p class="font-medium">Export Leads</p>
                            </button>
                            <button onclick="showDemoCallModal()" class="p-4 bg-purple-50 rounded-lg hover:bg-purple-100 transition">
                                <i class="fas fa-phone text-purple-600 text-xl mb-2"></i>
                                <p class="font-medium">Demo Call</p>
                            </button>
                            <button onclick="showSection('billing')" class="p-4 bg-yellow-50 rounded-lg hover:bg-yellow-100 transition">
                                <i class="fas fa-credit-card text-yellow-600 text-xl mb-2"></i>
                                <p class="font-medium">Add Credits</p>
                            </button>
                        </div>
                    </div>
                </div>
                
                <!-- Call Analytics Chart -->
                <div class="bg-white rounded-xl shadow p-6 mb-8">
                    <h3 class="text-lg font-semibold mb-4">Call Analytics</h3>
                    <canvas id="callChart" height="100"></canvas>
                </div>
            </div>

            <!-- My Agents Section -->
            <div id="myAgents" class="dashboard-section hidden">
                <div class="flex justify-between items-center mb-6">
                    <h1 class="text-2xl font-bold">My AI Agents</h1>
                    <button onclick="showCreateAgentModal()" 
                            class="bg-blue-500 text-white px-4 py-2 rounded-lg hover:bg-blue-600 transition">
                        <i class="fas fa-plus mr-2"></i> Create New Agent
                    </button>
                </div>
                
                <!-- Agents Grid -->
                <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6" id="agentsGrid">
                    <!-- Agent cards will be dynamically added here -->
                </div>
            </div>

            <!-- Call History Section -->
            <div id="callHistory" class="dashboard-section hidden">
                <h1 class="text-2xl font-bold mb-6">Call History</h1>
                
                <!-- Filters -->
                <div class="bg-white rounded-lg shadow p-4 mb-6">
                    <div class="flex flex-wrap gap-4">
                        <select class="border rounded-lg px-4 py-2">
                            <option>All Agents</option>
                            <option>Sales Agent</option>
                            <option>Support Agent</option>
                        </select>
                        <select class="border rounded-lg px-4 py-2">
                            <option>All Status</option>
                            <option>Completed</option>
                            <option>Failed</option>
                            <option>No Answer</option>
                        </select>
                        <input type="date" class="border rounded-lg px-4 py-2">
                        <button class="bg-blue-500 text-white px-4 py-2 rounded-lg">Filter</button>
                        <button class="border border-gray-300 px-4 py-2 rounded-lg">Export CSV</button>
                    </div>
                </div>
                
                <!-- Calls Table -->
                <div class="bg-white rounded-xl shadow overflow-hidden">
                    <table class="w-full">
                        <thead class="bg-gray-50">
                            <tr>
                                <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase">Date/Time</th>
                                <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase">Agent</th>
                                <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase">Phone</th>
                                <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase">Duration</th>
                                <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase">Status</th>
                                <th class="px6 py-3 text-left text-xs font-medium text-gray-500 uppercase">Actions</th>
                            </tr>
                        </thead>
                        <tbody id="callsTableBody" class="divide-y divide-gray-200">
                            <!-- Calls will be dynamically added here -->
                        </tbody>
                    </table>
                </div>
            </div>

            <!-- Leads Section -->
            <div id="leads" class="dashboard-section hidden">
                <h1 class="text-2xl font-bold mb-6">Leads Management</h1>
                
                <!-- Leads Table -->
                <div class="bg-white rounded-xl shadow overflow-hidden">
                    <div class="p-4 border-b flex justify-between items-center">
                        <div>
                            <input type="text" placeholder="Search leads..." class="border rounded-lg px-4 py-2">
                        </div>
                        <button onclick="exportLeads()" class="bg-green-500 text-white px-4 py-2 rounded-lg">
                            <i class="fas fa-download mr-2"></i> Export CSV
                        </button>
                    </div>
                    <table class="w-full">
                        <thead class="bg-gray-50">
                            <tr>
                                <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase">Lead Name</th>
                                <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase">Phone</th>
                                <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase">Agent</th>
                                <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase">Responses</th>
                                <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase">Date</th>
                                <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase">Actions</th>
                            </tr>
                        </thead>
                        <tbody id="leadsTableBody" class="divide-y divide-gray-200">
                            <!-- Leads will be dynamically added here -->
                        </tbody>
                    </table>
                </div>
            </div>

            <!-- Billing Section -->
            <div id="billing" class="dashboard-section hidden">
                <h1 class="text-2xl font-bold mb-6">Billing & Payments</h1>
                
                <div class="grid grid-cols-1 lg:grid-cols-3 gap-6">
                    <!-- Current Plan -->
                    <div class="lg:col-span-2">
                        <div class="bg-white rounded-xl shadow p-6 mb-6">
                            <h3 class="text-lg font-semibold mb-4">Current Plan</h3>
                            <div class="flex items-center justify-between p-4 bg-blue-50 rounded-lg">
                                <div>
                                    <h4 class="font-bold text-xl" id="currentPlan">Professional Plan</h4>
                                    <p class="text-gray-600" id="planDetails">500 calls/month • $79/month</p>
                                </div>
                                <button onclick="showUpgradeModal()" class="bg-blue-500 text-white px-4 py-2 rounded-lg">
                                    Upgrade Plan
                                </button>
                            </div>
                        </div>
                        
                        <!-- Add Call Credits -->
                        <div class="bg-white rounded-xl shadow p-6">
                            <h3 class="text-lg font-semibold mb-4">Add Call Credits</h3>
                            <div class="grid grid-cols-1 md:grid-cols-3 gap-4">
                                <div class="border rounded-lg p-4 text-center hover:border-blue-500 cursor-pointer" onclick="selectCreditPackage(100, 29)">
                                    <p class="font-bold text-lg">100 Calls</p>
                                    <p class="text-2xl font-bold text-blue-600">$29</p>
                                    <p class="text-sm text-gray-500">$0.29 per call</p>
                                </div>
                                <div class="border rounded-lg p-4 text-center hover:border-blue-500 cursor-pointer" onclick="selectCreditPackage(500, 99)">
                                    <p class="font-bold text-lg">500 Calls</p>
                                    <p class="text-2xl font-bold text-blue-600">$99</p>
                                    <p class="text-sm text-gray-500">$0.20 per call</p>
                                </div>
                                <div class="border rounded-lg p-4 text-center hover:border-blue-500 cursor-pointer" onclick="selectCreditPackage(1000, 179)">
                                    <p class="font-bold text-lg">1000 Calls</p>
                                    <p class="text-2xl font-bold text-blue-600">$179</p>
                                    <p class="text-sm text-gray-500">$0.18 per call</p>
                                </div>
                            </div>
                            <div class="mt-6">
                                <button onclick="processPayment()" class="w-full bg-blue-500 text-white py-3 rounded-lg font-medium">
                                    Proceed to Payment
                                </button>
                            </div>
                        </div>
                    </div>
                    
                    <!-- Payment History -->
                    <div class="bg-white rounded-xl shadow p-6">
                        <h3 class="text-lg font-semibold mb-4">Payment History</h3>
                        <div class="space-y-3">
                            <div class="flex justify-between items-center p-3 border rounded-lg">
                                <div>
                                    <p class="font-medium">500 Call Credits</p>
                                    <p class="text-sm text-gray-500">Jan 15, 2024</p>
                                </div>
                                <span class="font-bold">$99.00</span>
                            </div>
                            <div class="flex justify-between items-center p-3 border rounded-lg">
                                <div>
                                    <p class="font-medium">Professional Plan</p>
                                    <p class="text-sm text-gray-500">Jan 1, 2024</p>
                                </div>
                                <span class="font-bold">$79.00</span>
                            </div>
                        </div>
                    </div>
                </div>
            </div>

            <!-- Profile Section -->
            <div id="profile" class="dashboard-section hidden">
                <h1 class="text-2xl font-bold mb-6">Profile Settings</h1>
                <div class="bg-white rounded-xl shadow p-6 max-w-2xl">
                    <form>
                        <div class="space-y-6">
                            <div>
                                <label class="block text-sm font-medium text-gray-700 mb-2">Full Name</label>
                                <input type="text" value="John Doe" class="w-full border rounded-lg px-4 py-2">
                            </div>
                            <div>
                                <label class="block text-sm font-medium text-gray-700 mb-2">Email</label>
                                <input type="email" value="john@example.com" class="w-full border rounded-lg px-4 py-2">
                            </div>
                            <div>
                                <label class="block text-sm font-medium text-gray-700 mb-2">Phone Number</label>
                                <input type="tel" value="+92 300 1234567" class="w-full border rounded-lg px-4 py-2">
                            </div>
                            <div>
                                <label class="block text-sm font-medium text-gray-700 mb-2">Password</label>
                                <input type="password" placeholder="Leave blank to keep current" class="w-full border rounded-lg px-4 py-2">
                            </div>
                            <button type="submit" class="bg-blue-500 text-white px-6 py-2 rounded-lg hover:bg-blue-600">
                                Update Profile
                            </button>
                        </div>
                    </form>
                </div>
            </div>

            <!-- Admin Sections -->
            <!-- Admin Users -->
            <div id="adminUsers" class="dashboard-section hidden">
                <h1 class="text-2xl font-bold mb-6">User Management</h1>
                <div class="bg-white rounded-xl shadow overflow-hidden">
                    <table class="w-full">
                        <thead class="bg-gray-50">
                            <tr>
                                <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase">User</th>
                                <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase">Plan</th>
                                <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase">Status</th>
                                <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase">Calls Used</th>
                                <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase">Joined</th>
                                <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase">Actions</th>
                            </tr>
                        </thead>
                        <tbody id="adminUsersTable" class="divide-y divide-gray-200">
                            <!-- Users will be dynamically added here -->
                        </tbody>
                    </table>
                </div>
            </div>

            <!-- Admin Payments -->
            <div id="adminPayments" class="dashboard-section hidden">
                <h1 class="text-2xl font-bold mb-6">Payment Approvals</h1>
                <div class="space-y-4" id="pendingPayments">
                    <!-- Pending payments will be added here -->
                </div>
            </div>
        </div>
    </div>

    <!-- MODALS -->
    
    <!-- Create Agent Modal -->
    <div id="createAgentModal" class="fixed inset-0 bg-black bg-opacity-50 hidden items-center justify-center z-50 p-4">
        <div class="bg-white rounded-2xl max-w-2xl w-full max-h-[90vh] overflow-y-auto">
            <div class="p-6">
                <div class="flex justify-between items-center mb-6">
                    <h3 class="text-xl font-bold">Create AI Call Agent</h3>
                    <button onclick="closeModal('createAgentModal')" class="text-gray-500 hover:text-gray-700">
                        <i class="fas fa-times text-xl"></i>
                    </button>
                </div>
                
                <form id="agentForm" onsubmit="saveAgent(event)">
                    <div class="space-y-6">
                        <!-- Agent Name -->
                        <div>
                            <label class="block text-sm font-medium text-gray-700 mb-2">Agent Name</label>
                            <input type="text" id="agentName" required 
                                   placeholder="e.g., Sales Bot, Support Assistant" 
                                   class="w-full border border-gray-300 rounded-lg px-4 py-2 focus:ring-2 focus:ring-blue-500">
                        </div>
                        
                        <!-- Language Selection -->
                        <div>
                            <label class="block text-sm font-medium text-gray-700 mb-2">Language</label>
                            <div class="grid grid-cols-3 gap-4">
                                <label class="border rounded-lg p-4 text-center cursor-pointer hover:border-blue-500">
                                    <input type="radio" name="language" value="english" checked class="hidden">
                                    <i class="fas fa-language text-2xl mb-2 text-blue-500"></i>
                                    <p class="font-medium">English</p>
                                </label>
                                <label class="border rounded-lg p-4 text-center cursor-pointer hover:border-blue-500">
                                    <input type="radio" name="language" value="urdu" class="hidden">
                                    <i class="fas fa-language text-2xl mb-2 text-green-500"></i>
                                    <p class="font-medium">Urdu</p>
                                </label>
                                <label class="border rounded-lg p-4 text-center cursor-pointer hover:border-blue-500">
                                    <input type="radio" name="language" value="mix" class="hidden">
                                    <i class="fas fa-language text-2xl mb-2 text-purple-500"></i>
                                    <p class="font-medium">Mix</p>
                                </label>
                            </div>
                        </div>
                        
                        <!-- Call Script -->
                        <div>
                            <label class="block text-sm font-medium text-gray-700 mb-2">
                                Call Script
                                <span class="text-gray-500 text-sm font-normal">(The AI will follow this script)</span>
                            </label>
                            <textarea id="agentScript" rows="4" required 
                                      placeholder="Hello! This is [Agent Name] from [Company]. I'm calling to follow up on your recent inquiry..."
                                      class="w-full border border-gray-300 rounded-lg px-4 py-2 focus:ring-2 focus:ring-blue-500"></textarea>
                            <div class="mt-2 text-sm text-gray-500">
                                Tip: Use natural language. The AI will adapt it conversationally.
                            </div>
                        </div>
                        
                        <!-- Questions -->
                        <div>
                            <label class="block text-sm font-medium text-gray-700 mb-2">
                                Questions to Ask
                                <span class="text-gray-500 text-sm font-normal">(Optional questions AI will ask during call)</span>
                            </label>
                            <div id="questionsContainer">
                                <div class="flex gap-2 mb-2">
                                    <input type="text" placeholder="e.g., What is your email address?" 
                                           class="flex-1 border border-gray-300 rounded-lg px-4 py-2">
                                    <button type="button" onclick="removeQuestion(this)" class="text-red-500">
                                        <i class="fas fa-times"></i>
                                    </button>
                                </div>
                            </div>
                            <button type="button" onclick="addQuestion()" class="text-blue-500 text-sm mt-2">
                                <i class="fas fa-plus mr-1"></i> Add Question
                            </button>
                        </div>
                        
                        <!-- Follow-up Settings -->
                        <div class="border rounded-lg p-4">
                            <div class="flex items-center mb-4">
                                <input type="checkbox" id="enableFollowup" checked class="mr-2">
                                <label class="font-medium">Enable Follow-up Messages</label>
                            </div>
                            
                            <div id="followupSettings" class="space-y-4">
                                <div>
                                    <label class="block text-sm font-medium text-gray-700 mb-2">Follow-up Method</label>
                                    <select id="followupMethod" class="w-full border border-gray-300 rounded-lg px-4 py-2">
                                        <option value="whatsapp">WhatsApp</option>
                                        <option value="sms">SMS</option>
                                        <option value="both">Both</option>
                                    </select>
                                </div>
                                
                                <div>
                                    <label class="block text-sm font-medium text-gray-700 mb-2">Message Template</label>
                                    <textarea id="followupMessage" rows="3" 
                                              placeholder="Hi [Name], thank you for speaking with us today! Here's the information we discussed..."
                                              class="w-full border border-gray-300 rounded-lg px-4 py-2"></textarea>
                                </div>
                                
                                <div>
                                    <label class="block text-sm font-medium text-gray-700 mb-2">Send After (minutes)</label>
                                    <input type="number" id="followupDelay" value="5" min="1" 
                                           class="w-32 border border-gray-300 rounded-lg px-4 py-2">
                                </div>
                            </div>
                        </div>
                        
                        <!-- Voice Settings -->
                        <div>
                            <label class="block text-sm font-medium text-gray-700 mb-2">Voice Settings</label>
                            <div class="grid grid-cols-2 gap-4">
                                <div>
                                    <label class="block text-sm text-gray-600 mb-1">Voice</label>
                                    <select class="w-full border border-gray-300 rounded-lg px-4 py-2">
                                        <option>Jenny (US English)</option>
                                        <option>Asad (Urdu)</option>
                                        <option>Zira (US English)</option>
                                    </select>
                                </div>
                                <div>
                                    <label class="block text-sm text-gray-600 mb-1">Speaking Rate</label>
                                    <input type="range" min="0.5" max="2" step="0.1" value="1" 
                                           class="w-full">
                                </div>
                            </div>
                        </div>
                        
                        <!-- Call Settings -->
                        <div class="border rounded-lg p-4">
                            <h4 class="font-medium mb-4">Call Settings</h4>
                            <div class="grid grid-cols-2 gap-4">
                                <div>
                                    <label class="block text-sm text-gray-600 mb-1">Max Calls Per Day</label>
                                    <input type="number" value="100" 
                                           class="w-full border border-gray-300 rounded-lg px-4 py-2">
                                </div>
                                <div>
                                    <label class="block text-sm text-gray-600 mb-1">Call Hours</label>
                                    <div class="flex gap-2">
                                        <input type="time" value="09:00" class="border rounded-lg px-4 py-2">
                                        <span class="self-center">to</span>
                                        <input type="time" value="17:00" class="border rounded-lg px-4 py-2">
                                    </div>
                                </div>
                            </div>
                        </div>
                        
                        <!-- Submit Buttons -->
                        <div class="flex justify-end gap-3 pt-4 border-t">
                            <button type="button" onclick="closeModal('createAgentModal')" 
                                    class="px-6 py-2 border border-gray-300 rounded-lg hover:bg-gray-50">
                                Cancel
                            </button>
                            <button type="submit" 
                                    class="bg-blue-500 text-white px-6 py-2 rounded-lg hover:bg-blue-600">
                                Create Agent
                            </button>
                        </div>
                    </div>
                </form>
            </div>
        </div>
    </div>

    <!-- Demo Call Modal -->
    <div id="demoCallModal" class="fixed inset-0 bg-black bg-opacity-50 hidden items-center justify-center z-50 p-4">
        <div class="bg-white rounded-2xl max-w-md w-full">
            <div class="p-6">
                <div class="flex justify-between items-center mb-6">
                    <h3 class="text-xl font-bold">Try Demo AI Call</h3>
                    <button onclick="closeModal('demoCallModal')" class="text-gray-500 hover:text-gray-700">
                        <i class="fas fa-times text-xl"></i>
                    </button>
                </div>
                
                <div class="space-y-6">
                    <div class="text-center">
                        <div class="w-20 h-20 mx-auto mb-4 bg-blue-100 rounded-full flex items-center justify-center">
                            <i class="fas fa-robot text-blue-500 text-3xl"></i>
                        </div>
                        <p class="text-gray-600">Experience how our AI agent works with a live call</p>
                    </div>
                    
                    <div>
                        <label class="block text-sm font-medium text-gray-700 mb-2">Your Phone Number</label>
                        <input type="tel" id="demoPhone" placeholder="+92 300 1234567" 
                               class="w-full border border-gray-300 rounded-lg px-4 py-2">
                        <p class="text-sm text-gray-500 mt-1">We'll call this number for the demo</p>
                    </div>
                    
                    <div class="border rounded-lg p-4 bg-gray-50">
                        <h4 class="font-medium mb-2">Demo Agent Settings:</h4>
                        <div class="text-sm text-gray-600">
                            <p><strong>Language:</strong> English/Urdu Mix</p>
                            <p><strong>Script:</strong> "Hello! This is a demo AI call. You will hear how our AI agent works."</p>
                            <p><strong>Questions:</strong> "What is your name?", "Do you want more info?"</p>
                        </div>
                    </div>
                    
                    <div class="flex gap-3">
                        <button onclick="closeModal('demoCallModal')" 
                                class="flex-1 px-6 py-3 border border-gray-300 rounded-lg hover:bg-gray-50">
                            Cancel
                        </button>
                        <button onclick="initiateDemoCall()" 
                                class="flex-1 bg-green-500 text-white px-6 py-3 rounded-lg hover:bg-green-600">
                            <i class="fas fa-phone mr-2"></i> Start Demo Call
                        </button>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <!-- Payment Modal -->
    <div id="paymentModal" class="fixed inset-0 bg-black bg-opacity-50 hidden items-center justify-center z-50 p-4">
        <div class="bg-white rounded-2xl max-w-md w-full">
            <div class="p-6">
                <div class="flex justify-between items-center mb-6">
                    <h3 class="text-xl font-bold">Complete Payment</h3>
                    <button onclick="closeModal('paymentModal')" class="text-gray-500 hover:text-gray-700">
                        <i class="fas fa-times text-xl"></i>
                    </button>
                </div>
                
                <div class="space-y-6">
                    <div class="text-center p-4 bg-blue-50 rounded-lg">
                        <h4 class="font-bold text-lg" id="paymentPlan">100 Call Credits</h4>
                        <p class="text-2xl font-bold text-blue-600" id="paymentAmount">$29.00</p>
                    </div>
                    
                    <div>
                        <label class="block text-sm font-medium text-gray-700 mb-4">Select Payment Method</label>
                        <div class="space-y-3">
                            <label class="flex items-center p-4 border rounded-lg cursor-pointer hover:border-blue-500">
                                <input type="radio" name="paymentMethodModal" value="stripe" checked class="mr-3">
                                <div>
                                    <p class="font-medium">Credit/Debit Card</p>
                                    <p class="text-sm text-gray-500">Pay securely with Stripe</p>
                                </div>
                            </label>
                            
                            <label class="flex items-center p-4 border rounded-lg cursor-pointer hover:border-blue-500">
                                <input type="radio" name="paymentMethodModal" value="bank" class="mr-3">
                                <div>
                                    <p class="font-medium">Bank Transfer</p>
                                    <p class="text-sm text-gray-500">Manual transfer - requires verification</p>
                                </div>
                            </label>
                            
                            <label class="flex items-center p-4 border rounded-lg cursor-pointer hover:border-blue-500">
                                <input type="radio" name="paymentMethodModal" value="whatsapp" class="mr-3">
                                <div>
                                    <p class="font-medium">WhatsApp Payment</p>
                                    <p class="text-sm text-gray-500">Send screenshot to 0301-8973444</p>
                                </div>
                            </label>
                        </div>
                    </div>
                    
                    <!-- Stripe Card Element -->
                    <div id="stripeCardElement" class="p-4 border rounded-lg">
                        <div id="card-element-modal"></div>
                        <div id="card-errors-modal" class="text-red-500 text-sm mt-2"></div>
                    </div>
                    
                    <!-- Bank Details -->
                    <div id="bankDetailsModal" class="hidden p-4 border rounded-lg">
                        <h5 class="font-medium mb-2">Bank Details:</h5>
                        <div class="text-sm space-y-1">
                            <p><strong>Account Name:</strong> Ali Hassan</p>
                            <p><strong>Bank:</strong> United Bank Limited (UBL)</p>
                            <p><strong>IBAN:</strong> PK96UNIL0109000338084413</p>
                        </div>
                        <div class="mt-4">
                            <label class="block text-sm mb-2">Upload Payment Proof:</label>
                            <input type="file" class="w-full border rounded-lg p-2">
                        </div>
                    </div>
                    
                    <!-- WhatsApp Details -->
                    <div id="whatsappDetailsModal" class="hidden p-4 border rounded-lg">
                        <div class="flex items-center mb-3">
                            <i class="fab fa-whatsapp text-green-600 text-xl mr-2"></i>
                            <span class="font-medium">0301-8973444</span>
                        </div>
                        <p class="text-sm mb-3">Send payment screenshot to this WhatsApp number</p>
                        <div>
                            <label class="block text-sm mb-2">Upload Payment Screenshot:</label>
                            <input type="file" class="w-full border rounded-lg p-2">
                        </div>
                    </div>
                    
                    <button onclick="processPaymentConfirmation()" 
                            class="w-full bg-blue-500 text-white py-3 rounded-lg font-medium hover:bg-blue-600">
                        Complete Payment
                    </button>
                </div>
            </div>
        </div>
    </div>

    <!-- Call Status Modal -->
    <div id="callStatusModal" class="fixed inset-0 bg-black bg-opacity-50 hidden items-center justify-center z-50 p-4">
        <div class="bg-white rounded-2xl max-w-md w-full">
            <div class="p-6">
                <div class="text-center">
                    <div class="w-20 h-20 mx-auto mb-4 bg-blue-100 rounded-full flex items-center justify-center animate-pulse">
                        <i class="fas fa-phone-alt text-blue-500 text-3xl"></i>
                    </div>
                    <h3 class="text-xl font-bold mb-2" id="callStatusTitle">Calling...</h3>
                    <p class="text-gray-600 mb-4" id="callStatusMessage">Connecting to AI agent</p>
                    
                    <div class="mb-6">
                        <p class="font-medium" id="callPhoneNumber">+92 300 1234567</p>
                        <p class="text-sm text-gray-500" id="callAgentName">Demo Agent</p>
                    </div>
                    
                    <div class="flex justify-center space-x-4">
                        <button onclick="endCall()" 
                                class="bg-red-500 text-white px-6 py-2 rounded-lg hover:bg-red-600">
                            <i class="fas fa-phone-slash mr-2"></i> End Call
                        </button>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <!-- Notification Toast -->
    <div id="toast" class="fixed top-4 right-4 bg-gray-800 text-white px-6 py-3 rounded-lg shadow-lg hidden z-50">
        <div class="flex items-center">
            <i class="fas fa-check-circle mr-3 text-green-400"></i>
            <span id="toastMessage">Operation completed successfully!</span>
        </div>
    </div>

    <!-- Script -->
    <script>
        // Global State
        let currentUser = null;
        let agents = [];
        let calls = [];
        let leads = [];
        let selectedCreditPackage = { calls: 100, amount: 29 };
        let stripe = null;
        let elements = null;

        // Initialize
        document.addEventListener('DOMContentLoaded', function() {
            setTimeout(() => {
                document.getElementById('loadingScreen').classList.add('hidden');
                showAuthPage();
                initializeStripe();
                loadSampleData();
            }, 1500);
        });

        // Initialize Stripe
        function initializeStripe() {
            stripe = Stripe('pk_test_your_stripe_key'); // Replace with your Stripe key
            elements = stripe.elements();
            const cardElement = elements.create('card');
            cardElement.mount('#card-element');
            
            const cardElementModal = elements.create('card');
            cardElementModal.mount('#card-element-modal');
        }

        // Show/Hide Pages
        function showAuthPage() {
            document.getElementById('authPage').classList.remove('hidden');
            document.getElementById('dashboard').classList.add('hidden');
        }

        function showDashboard() {
            document.getElementById('authPage').classList.add('hidden');
            document.getElementById('dashboard').classList.remove('hidden');
            document.getElementById('loadingScreen').classList.add('hidden');
            updateDashboardStats();
        }

        // Auth Functions
        function switchAuthTab(tab) {
            document.querySelectorAll('.auth-tab').forEach(t => {
                t.classList.remove('tab-active');
            });
            document.querySelectorAll('.auth-form').forEach(f => {
                f.classList.add('hidden');
            });
            
            document.querySelector(`.auth-tab[data-tab="${tab}"]`).classList.add('tab-active');
            document.getElementById(`${tab}Form`).classList.remove('hidden');
        }

        function togglePaymentMethod() {
            const method = document.querySelector('input[name="paymentMethod"]:checked').value;
            
            document.getElementById('bankPaymentDetails').classList.add('hidden');
            document.getElementById('whatsappPaymentDetails').classList.add('hidden');
            
            if (method === 'bank') {
                document.getElementById('bankPaymentDetails').classList.remove('hidden');
            } else if (method === 'whatsapp') {
                document.getElementById('whatsappPaymentDetails').classList.remove('hidden');
            }
        }

        async function handleSignup(event) {
            event.preventDefault();
            
            const userData = {
                name: document.getElementById('signupName').value,
                email: document.getElementById('signupEmail').value,
                phone: document.getElementById('signupPhone').value,
                password: document.getElementById('signupPassword').value,
                plan: document.getElementById('signupPlan').value,
                paymentMethod: document.querySelector('input[name="paymentMethod"]:checked').value
            };

            // Simulate API call
            showToast('Creating account...', 'info');
            
            setTimeout(() => {
                // For demo, auto-login after signup
                currentUser = {
                    id: 'user_123',
                    name: userData.name,
                    email: userData.email,
                    plan: userData.plan,
                    isAdmin: false,
                    callBalance: userData.plan === 'starter' ? 100 : 
                                  userData.plan === 'professional' ? 500 : 2000
                };
                
                localStorage.setItem('user', JSON.stringify(currentUser));
                showToast('Account created successfully!', 'success');
                showDashboard();
                updateUserInfo();
            }, 2000);
        }

        async function handleLogin(event) {
            event.preventDefault();
            
            const email = document.getElementById('loginEmail').value;
            const password = document.getElementById('loginPassword').value;
            
            showToast('Signing in...', 'info');
            
            setTimeout(() => {
                // Demo credentials
                if (email === 'admin@aicall.com' && password === 'admin123') {
                    currentUser = {
                        id: 'admin_123',
                        name: 'Admin User',
                        email: email,
                        plan: 'enterprise',
                        isAdmin: true,
                        callBalance: 9999
                    };
                } else {
                    currentUser = {
                        id: 'user_123',
                        name: 'Demo User',
                        email: email,
                        plan: 'professional',
                        isAdmin: false,
                        callBalance: 500
                    };
                }
                
                localStorage.setItem('user', JSON.stringify(currentUser));
                showToast('Login successful!', 'success');
                showDashboard();
                updateUserInfo();
            }, 1500);
        }

        function logout() {
            currentUser = null;
            localStorage.removeItem('user');
            showToast('Logged out successfully', 'success');
            showAuthPage();
            switchAuthTab('login');
        }

        // Dashboard Functions
        function updateUserInfo() {
            if (!currentUser) return;
            
            document.getElementById('userName').textContent = currentUser.name;
            document.getElementById('userInitial').textContent = currentUser.name.charAt(0);
            document.getElementById('menuUserName').textContent = currentUser.name;
            document.getElementById('menuUserEmail').textContent = currentUser.email;
            document.getElementById('callBalance').textContent = currentUser.callBalance;
            document.getElementById('currentPlan').textContent = 
                currentUser.plan.charAt(0).toUpperCase() + currentUser.plan.slice(1) + ' Plan';
            
            // Show admin links if admin
            if (currentUser.isAdmin) {
                document.getElementById('adminLinks').classList.remove('hidden');
            }
        }

        function toggleUserMenu() {
            document.getElementById('userMenu').classList.toggle('hidden');
        }

        function toggleSidebar() {
            document.getElementById('sidebar').classList.toggle('active');
            document.getElementById('overlay').classList.toggle('active');
        }

        function showSection(sectionId) {
            // Hide all sections
            document.querySelectorAll('.dashboard-section').forEach(section => {
                section.classList.add('hidden');
            });
            
            // Remove active from all nav links
            document.querySelectorAll('.nav-link').forEach(link => {
                link.classList.remove('active');
            });
            
            // Show selected section
            document.getElementById(sectionId).classList.remove('hidden');
            
            // Set active nav link
            const navMap = {
                'dashboardHome': 'nav-link:nth-child(1)',
                'myAgents': 'nav-link:nth-child(2)',
                'callHistory': 'nav-link:nth-child(3)',
                'recordings': 'nav-link:nth-child(4)',
                'leads': 'nav-link:nth-child(5)',
                'billing': 'nav-link:nth-child(6)',
                'profile': 'profile-btn',
                'adminUsers': 'nav-link:nth-child(8)',
                'adminPayments': 'nav-link:nth-child(9)',
                'adminAnalytics': 'nav-link:nth-child(10)'
            };
            
            if (navMap[sectionId]) {
                document.querySelector(navMap[sectionId])?.classList.add('active');
            }
            
            // Update content based on section
            if (sectionId === 'myAgents') {
                loadAgents();
            } else if (sectionId === 'callHistory') {
                loadCallHistory();
            } else if (sectionId === 'leads') {
                loadLeads();
            } else if (sectionId === 'adminUsers') {
                loadAdminUsers();
            } else if (sectionId === 'adminPayments') {
                loadPendingPayments();
            }
            
            // Close sidebar on mobile
            if (window.innerWidth < 768) {
                toggleSidebar();
            }
        }

        // Modal Functions
        function showModal(modalId) {
            document.getElementById(modalId).classList.remove('hidden');
            document.body.style.overflow = 'hidden';
        }

        function closeModal(modalId) {
            document.getElementById(modalId).classList.add('hidden');
            document.body.style.overflow = 'auto';
        }

        function showCreateAgentModal() {
            showModal('createAgentModal');
        }

        function showDemoCallModal() {
            document.getElementById('demoPhone').value = '';
            showModal('demoCallModal');
        }

        // Agent Functions
        function addQuestion() {
            const container = document.getElementById('questionsContainer');
            const div = document.createElement('div');
            div.className = 'flex gap-2 mb-2';
            div.innerHTML = `
                <input type="text" placeholder="Enter question..." 
                       class="flex-1 border border-gray-300 rounded-lg px-4 py-2">
                <button type="button" onclick="removeQuestion(this)" class="text-red-500">
                    <i class="fas fa-times"></i>
                </button>
            `;
            container.appendChild(div);
        }

        function removeQuestion(button) {
            button.parentElement.remove();
        }

        function saveAgent(event) {
            event.preventDefault();
            
            const agent = {
                id: 'agent_' + Date.now(),
                name: document.getElementById('agentName').value,
                language: document.querySelector('input[name="language"]:checked').value,
                script: document.getElementById('agentScript').value,
                questions: Array.from(document.querySelectorAll('#questionsContainer input'))
                    .map(input => input.value)
                    .filter(q => q.trim() !== ''),
                followUp: {
                    enabled: document.getElementById('enableFollowup').checked,
                    method: document.getElementById('followupMethod').value,
                    message: document.getElementById('followupMessage').value,
                    delay: document.getElementById('followupDelay').value
                },
                status: 'active',
                callCount: 0,
                leadCount: 0,
                createdAt: new Date().toISOString()
            };
            
            agents.push(agent);
            showToast('AI agent created successfully!', 'success');
            closeModal('createAgentModal');
            loadAgents();
        }

        function loadAgents() {
            const container = document.getElementById('agentsGrid');
            
            if (agents.length === 0) {
                // Load sample agents
                agents = [
                    {
                        id: 'agent_1',
                        name: 'Sales Agent',
                        language: 'english',
                        script: 'Hello! This is Sales AI calling about our special offer...',
                        status: 'active',
                        callCount: 45,
                        leadCount: 23
                    },
                    {
                        id: 'agent_2',
                        name: 'Support Agent',
                        language: 'urdu',
                        script: 'Assalam o Alaikum! Main support AI hoon...',
                        status: 'active',
                        callCount: 67,
                        leadCount: 45
                    },
                    {
                        id: 'agent_3',
                        name: 'Follow-up Agent',
                        language: 'mix',
                        script: 'Hello! I\'m following up on your recent inquiry...',
                        status: 'paused',
                        callCount: 12,
                        leadCount: 8
                    }
                ];
            }
            
            container.innerHTML = agents.map(agent => `
                <div class="bg-white rounded-xl shadow card-hover">
                    <div class="p-6">
                        <div class="flex justify-between items-start mb-4">
                            <div>
                                <h3 class="font-bold text-lg">${agent.name}</h3>
                                <div class="flex items-center mt-1">
                                    <span class="text-sm text-gray-500 mr-2">${agent.language.charAt(0).toUpperCase() + agent.language.slice(1)}</span>
                                    <span class="px-2 py-1 text-xs ${agent.status === 'active' ? 'status-active' : 'status-paused'} rounded">
                                        ${agent.status.charAt(0).toUpperCase() + agent.status.slice(1)}
                                    </span>
                                </div>
                            </div>
                            <div class="dropdown relative">
                                <button onclick="toggleAgentDropdown('${agent.id}')" class="text-gray-500 hover:text-gray-700">
                                    <i class="fas fa-ellipsis-v"></i>
                                </button>
                                <div id="dropdown-${agent.id}" class="dropdown-menu absolute right-0 mt-2 w-48 bg-white rounded-lg shadow-lg border hidden z-10">
                                    <button onclick="editAgent('${agent.id}')" class="block w-full text-left px-4 py-2 hover:bg-gray-100">
                                        <i class="fas fa-edit mr-2"></i> Edit
                                    </button>
                                    <button onclick="startAgentCall('${agent.id}')" class="block w-full text-left px-4 py-2 hover:bg-gray-100">
                                        <i class="fas fa-phone mr-2"></i> Start Call
                                    </button>
                                    <button onclick="toggleAgentStatus('${agent.id}')" class="block w-full text-left px-4 py-2 hover:bg-gray-100">
                                        <i class="fas fa-pause mr-2"></i> ${agent.status === 'active' ? 'Pause' : 'Activate'}
                                    </button>
                                    <button onclick="deleteAgent('${agent.id}')" class="block w-full text-left px-4 py-2 text-red-600 hover:bg-red-50">
                                        <i class="fas fa-trash mr-2"></i> Delete
                                    </button>
                                </div>
                            </div>
                        </div>
                        
                        <div class="mb-4">
                            <p class="text-sm text-gray-600 line-clamp-2">${agent.script?.substring(0, 100)}${agent.script?.length > 100 ? '...' : ''}</p>
                        </div>
                        
                        <div class="flex justify-between text-sm">
                            <div>
                                <p class="text-gray-500">Calls</p>
                                <p class="font-bold">${agent.callCount}</p>
                            </div>
                            <div>
                                <p class="text-gray-500">Leads</p>
                                <p class="font-bold">${agent.leadCount}</p>
                            </div>
                            <div>
                                <p class="text-gray-500">Success Rate</p>
                                <p class="font-bold">${agent.callCount > 0 ? Math.round((agent.leadCount / agent.callCount) * 100) : 0}%</p>
                            </div>
                        </div>
                    </div>
                </div>
            `).join('');
            
            updateDashboardStats();
        }

        function toggleAgentDropdown(agentId) {
            const dropdown = document.getElementById(`dropdown-${agentId}`);
            dropdown.classList.toggle('hidden');
            
            // Close other dropdowns
            document.querySelectorAll('.dropdown-menu').forEach(menu => {
                if (menu.id !== `dropdown-${agentId}`) {
                    menu.classList.add('hidden');
                }
            });
        }

        function editAgent(agentId) {
            const agent = agents.find(a => a.id === agentId);
            if (agent) {
                document.getElementById('agentName').value = agent.name;
                showCreateAgentModal();
            }
        }

        function startAgentCall(agentId) {
            showCallPromptModal(agentId);
        }

        function toggleAgentStatus(agentId) {
            const agent = agents.find(a => a.id === agentId);
            if (agent) {
                agent.status = agent.status === 'active' ? 'paused' : 'active';
                showToast(`Agent ${agent.status}`, 'success');
                loadAgents();
            }
        }

        function deleteAgent(agentId) {
            if (confirm('Are you sure you want to delete this agent?')) {
                agents = agents.filter(a => a.id !== agentId);
                showToast('Agent deleted', 'success');
                loadAgents();
            }
        }

        // Call Functions
        function showCallPromptModal(agentId) {
            const agent = agents.find(a => a.id === agentId);
            if (!agent) return;
            
            // In real app, show modal to enter phone number
            const phoneNumber = prompt('Enter phone number to call:');
            if (phoneNumber) {
                initiateCall(agentId, phoneNumber);
            }
        }

        function initiateCall(agentId, phoneNumber) {
            const agent = agents.find(a => a.id === agentId);
            
            // Update call status modal
            document.getElementById('callPhoneNumber').textContent = phoneNumber;
            document.getElementById('callAgentName').textContent = agent?.name || 'AI Agent';
            
            showModal('callStatusModal');
            
            // Simulate call progression
            setTimeout(() => {
                document.getElementById('callStatusTitle').textContent = 'Connected';
                document.getElementById('callStatusMessage').textContent = 'AI agent is speaking...';
            }, 2000);
            
            // Simulate call completion
            setTimeout(() => {
                endCall();
                
                // Add to call history
                const call = {
                    id: 'call_' + Date.now(),
                    agentName: agent?.name,
                    phoneNumber: phoneNumber,
                    duration: '2:45',
                    status: 'completed',
                    timestamp: new Date().toLocaleString(),
                    recording: true
                };
                
                calls.unshift(call);
                loadCallHistory();
                
                // Add lead
                const lead = {
                    id: 'lead_' + Date.now(),
                    name: 'Lead from call',
                    phone: phoneNumber,
                    agent: agent?.name,
                    responses: { question1: 'Yes', question2: 'Interested' },
                    date: new Date().toLocaleDateString()
                };
                
                leads.unshift(lead);
                
                // Deduct call balance
                if (currentUser && !currentUser.isAdmin) {
                    currentUser.callBalance--;
                    updateUserInfo();
                }
                
                updateDashboardStats();
            }, 5000);
        }

        function initiateDemoCall() {
            const phoneNumber = document.getElementById('demoPhone').value;
            if (!phoneNumber) {
                alert('Please enter your phone number');
                return;
            }
            
            closeModal('demoCallModal');
            
            // Update call status for demo
            document.getElementById('callPhoneNumber').textContent = phoneNumber;
            document.getElementById('callAgentName').textContent = 'Demo Agent';
            document.getElementById('callStatusTitle').textContent = 'Demo Call';
            document.getElementById('callStatusMessage').textContent = 'Connecting to demo AI agent...';
            
            showModal('callStatusModal');
            
            // Simulate demo call
            setTimeout(() => {
                document.getElementById('callStatusTitle').textContent = 'Demo in Progress';
                document.getElementById('callStatusMessage').textContent = 'You are now speaking with AI';
            }, 2000);
            
            setTimeout(() => {
                endCall();
                showToast('Demo call completed! Check how AI performed.', 'success');
            }, 8000);
        }

        function endCall() {
            closeModal('callStatusModal');
        }

        function loadCallHistory() {
            const container = document.getElementById('callsTableBody');
            
            if (calls.length === 0) {
                calls = [
                    {
                        id: 'call_1',
                        agentName: 'Sales Agent',
                        phoneNumber: '+92 300 1234567',
                        duration: '5:32',
                        status: 'completed',
                        timestamp: '2024-01-15 14:30',
                        recording: true
                    },
                    {
                        id: 'call_2',
                        agentName: 'Support Agent',
                        phoneNumber: '+92 321 9876543',
                        duration: '2:15',
                        status: 'failed',
                        timestamp: '2024-01-15 11:20',
                        recording: false
                    }
                ];
            }
            
            container.innerHTML = calls.map(call => `
                <tr>
                    <td class="px-6 py-4">${call.timestamp}</td>
                    <td class="px-6 py-4 font-medium">${call.agentName}</td>
                    <td class="px-6 py-4">${call.phoneNumber}</td>
                    <td class="px-6 py-4">${call.duration}</td>
                    <td class="px-6 py-4">
                        <span class="px-2 py-1 text-xs ${call.status === 'completed' ? 'bg-green-100 text-green-800' : 'bg-red-100 text-red-800'} rounded">
                            ${call.status}
                        </span>
                    </td>
                    <td class="px-6 py-4">
                        ${call.recording ? 
                            '<button class="text-blue-500 hover:text-blue-700 mr-2"><i class="fas fa-play"></i></button>' : 
                            ''
                        }
                        <button class="text-gray-500 hover:text-gray-700"><i class="fas fa-info-circle"></i></button>
                    </td>
                </tr>
            `).join('');
        }

        // Leads Functions
        function loadLeads() {
            const container = document.getElementById('leadsTableBody');
            
            if (leads.length === 0) {
                leads = [
                    {
                        id: 'lead_1',
                        name: 'Ahmed Khan',
                        phone: '+92 300 1234567',
                        agent: 'Sales Agent',
                        responses: { email: 'ahmed@example.com', interest: 'High' },
                        date: '2024-01-15'
                    },
                    {
                        id: 'lead_2',
                        name: 'Sara Ahmed',
                        phone: '+92 321 9876543',
                        agent: 'Support Agent',
                        responses: { issue: 'Technical', status: 'Resolved' },
                        date: '2024-01-14'
                    }
                ];
            }
            
            container.innerHTML = leads.map(lead => `
                <tr>
                    <td class="px-6 py-4 font-medium">${lead.name}</td>
                    <td class="px-6 py-4">${lead.phone}</td>
                    <td class="px-6 py-4">${lead.agent}</td>
                    <td class="px-6 py-4">
                        <button onclick="viewLeadResponses('${lead.id}')" class="text-blue-500 hover:text-blue-700">
                            View Responses
                        </button>
                    </td>
                    <td class="px-6 py-4">${lead.date}</td>
                    <td class="px-6 py-4">
                        <button onclick="sendFollowUp('${lead.phone}')" class="text-green-500 hover:text-green-700 mr-2">
                            <i class="fab fa-whatsapp"></i>
                        </button>
                        <button onclick="deleteLead('${lead.id}')" class="text-red-500 hover:text-red-700">
                            <i class="fas fa-trash"></i>
                        </button>
                    </td>
                </tr>
            `).join('');
        }

        function viewLeadResponses(leadId) {
            const lead = leads.find(l => l.id === leadId);
            if (lead) {
                alert(JSON.stringify(lead.responses, null, 2));
            }
        }

        function sendFollowUp(phoneNumber) {
            alert(`WhatsApp follow-up sent to ${phoneNumber}`);
        }

        function deleteLead(leadId) {
            if (confirm('Delete this lead?')) {
                leads = leads.filter(l => l.id !== leadId);
                loadLeads();
                showToast('Lead deleted', 'success');
            }
        }

        function exportLeads() {
            const csv = leads.map(lead => 
                `${lead.name},${lead.phone},${lead.agent},${JSON.stringify(lead.responses)},${lead.date}`
            ).join('\n');
            
            const blob = new Blob([csv], { type: 'text/csv' });
            const url = window.URL.createObjectURL(blob);
            const a = document.createElement('a');
            a.href = url;
            a.download = 'leads.csv';
            a.click();
            
            showToast('Leads exported successfully', 'success');
        }

        // Billing Functions
        function selectCreditPackage(calls, amount) {
            selectedCreditPackage = { calls, amount };
            
            // Update UI
            document.querySelectorAll('.border.rounded-lg.p-4').forEach(div => {
                div.classList.remove('border-blue-500', 'bg-blue-50');
            });
            event.target.closest('.border.rounded-lg.p-4').classList.add('border-blue-500', 'bg-blue-50');
        }

        function processPayment() {
            if (!selectedCreditPackage) {
                alert('Please select a credit package');
                return;
            }
            
            document.getElementById('paymentPlan').textContent = `${selectedCreditPackage.calls} Call Credits`;
            document.getElementById('paymentAmount').textContent = `$${selectedCreditPackage.amount}.00`;
            showModal('paymentModal');
        }

        function processPaymentConfirmation() {
            const method = document.querySelector('input[name="paymentMethodModal"]:checked').value;
            
            if (method === 'stripe') {
                // Process Stripe payment
                showToast('Processing card payment...', 'info');
                setTimeout(() => {
                    completePayment();
                }, 2000);
            } else {
                showToast('Manual payment submitted for verification', 'info');
                closeModal('paymentModal');
            }
        }

        function completePayment() {
            if (currentUser) {
                currentUser.callBalance += selectedCreditPackage.calls;
                updateUserInfo();
            }
            
            showToast('Payment successful! Credits added to your account.', 'success');
            closeModal('paymentModal');
        }

        function showUpgradeModal() {
            alert('Plan upgrade functionality would be implemented here');
        }

        // Admin Functions
        function loadAdminUsers() {
            const container = document.getElementById('adminUsersTable');
            const users = [
                { id: 'user_1', name: 'John Doe', email: 'john@example.com', plan: 'professional', status: 'active', calls: 245, joined: '2024-01-01' },
                { id: 'user_2', name: 'Jane Smith', email: 'jane@example.com', plan: 'starter', status: 'pending', calls: 0, joined: '2024-01-15' },
                { id: 'user_3', name: 'Ahmed Khan', email: 'ahmed@example.com', plan: 'enterprise', status: 'active', calls: 1200, joined: '2023-12-01' }
            ];
            
            container.innerHTML = users.map(user => `
                <tr>
                    <td class="px-6 py-4">
                        <div>
                            <p class="font-medium">${user.name}</p>
                            <p class="text-sm text-gray-500">${user.email}</p>
                        </div>
                    </td>
                    <td class="px-6 py-4">
                        <span class="px-2 py-1 text-xs bg-blue-100 text-blue-800 rounded">${user.plan}</span>
                    </td>
                    <td class="px-6 py-4">
                        <span class="px-2 py-1 text-xs ${user.status === 'active' ? 'bg-green-100 text-green-800' : 'bg-yellow-100 text-yellow-800'} rounded">
                            ${user.status}
                        </span>
                    </td>
                    <td class="px-6 py-4">${user.calls}</td>
                    <td class="px-6 py-4">${user.joined}</td>
                    <td class="px-6 py-4">
                        <button onclick="adminEditUser('${user.id}')" class="text-blue-500 hover:text-blue-700 mr-2">
                            <i class="fas fa-edit"></i>
                        </button>
                        <button onclick="adminDeleteUser('${user.id}')" class="text-red-500 hover:text-red-700">
                            <i class="fas fa-trash"></i>
                        </button>
                    </td>
                </tr>
            `).join('');
        }

        function loadPendingPayments() {
            const container = document.getElementById('pendingPayments');
            const payments = [
                { id: 'pay_1', user: 'Jane Smith', amount: 29, method: 'bank', proof: true },
                { id: 'pay_2', user: 'Ali Raza', amount: 79, method: 'whatsapp', proof: true }
            ];
            
            container.innerHTML = payments.map(payment => `
                <div class="bg-white rounded-lg shadow p-4">
                    <div class="flex justify-between items-center">
                        <div>
                            <h4 class="font-medium">${payment.user}</h4>
                            <p class="text-sm text-gray-500">${payment.method === 'bank' ? 'Bank Transfer' : 'WhatsApp Payment'}</p>
                            <p class="font-bold text-lg">$${payment.amount}.00</p>
                        </div>
                        <div class="space-x-2">
                            <button onclick="approvePayment('${payment.id}')" class="bg-green-500 text-white px-4 py-2 rounded-lg">
                                Approve
                            </button>
                            <button onclick="rejectPayment('${payment.id}')" class="bg-red-500 text-white px-4 py-2 rounded-lg">
                                Reject
                            </button>
                        </div>
                    </div>
                </div>
            `).join('');
        }

        function approvePayment(paymentId) {
            showToast('Payment approved and account activated', 'success');
            // In real app, update payment status and activate user
        }

        function rejectPayment(paymentId) {
            if (confirm('Reject this payment?')) {
                showToast('Payment rejected', 'success');
                // In real app, update payment status
            }
        }

        // Utility Functions
        function showToast(message, type = 'success') {
            const toast = document.getElementById('toast');
            const icon = toast.querySelector('i');
            const messageEl = document.getElementById('toastMessage');
            
            messageEl.textContent = message;
            
            // Set icon based on type
            if (type === 'success') {
                icon.className = 'fas fa-check-circle mr-3 text-green-400';
            } else if (type === 'error') {
                icon.className = 'fas fa-exclamation-circle mr-3 text-red-400';
            } else if (type === 'info') {
                icon.className = 'fas fa-info-circle mr-3 text-blue-400';
            }
            
            toast.classList.remove('hidden');
            
            setTimeout(() => {
                toast.classList.add('hidden');
            }, 3000);
        }

        function updateDashboardStats() {
            document.getElementById('totalCalls').textContent = calls.length;
            document.getElementById('totalLeads').textContent = leads.length;
            document.getElementById('activeAgents').textContent = agents.filter(a => a.status === 'active').length;
            
            // Calculate total call minutes
            const totalMinutes = calls.reduce((sum, call) => {
                const [min, sec] = call.duration.split(':').map(Number);
                return sum + min + (sec / 60);
            }, 0);
            
            document.getElementById('callMinutes').textContent = Math.round(totalMinutes);
        }

        function loadSampleData() {
            // This function loads sample data for demonstration
            // In a real application, this would come from an API
            loadAgents();
            loadCallHistory();
            loadLeads();
            loadAdminUsers();
            loadPendingPayments();
            
            // Initialize chart
            const ctx = document.getElementById('callChart').getContext('2d');
            new Chart(ctx, {
                type: 'line',
                data: {
                    labels: ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun'],
                    datasets: [{
                        label: 'Calls',
                        data: [65, 78, 90, 81, 96, 120],
                        borderColor: '#3b82f6',
                        backgroundColor: 'rgba(59, 130, 246, 0.1)',
                        fill: true
                    }]
                },
                options: {
                    responsive: true,
                    plugins: {
                        legend: {
                            display: false
                        }
                    }
                }
            });
        }

        // Close dropdowns when clicking outside
        document.addEventListener('click', function(event) {
            // Close user menu
            const userMenu = document.getElementById('userMenu');
            if (!event.target.closest('.relative')) {
                userMenu.classList.add('hidden');
            }
            
            // Close dropdowns
            document.querySelectorAll('.dropdown-menu').forEach(menu => {
                menu.classList.add('hidden');
            });
        });

        // Check if user is already logged in
        const savedUser = localStorage.getItem('user');
        if (savedUser) {
            currentUser = JSON.parse(savedUser);
            showDashboard();
            updateUserInfo();
        }
    </script>
</body>
</html>
