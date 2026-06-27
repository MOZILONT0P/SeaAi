<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Sea – AI Assistant</title>

    <!-- Google Fonts -->
    <link rel="preconnect" href="https://fonts.googleapis.com" />
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
    <link href="https://fonts.googleapis.com/css2?family=Inter:opsz,wght@14..32,300;14..32,400;14..32,500;14..32,600;14..32,700&display=swap" rel="stylesheet" />

    <style>
        /* ---------- Reset & Base ---------- */
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        :root {
            --bg-primary: #f6f8fc;
            --bg-secondary: #ffffff;
            --bg-sidebar: #f0f2f6;
            --bg-input: #ffffff;
            --text-primary: #1a1a2e;
            --text-secondary: #4a4a6a;
            --text-muted: #8a8aa8;
            --border-light: #e4e7ed;
            --border-focus: #4f6ef7;
            --accent: #4f6ef7;
            --accent-hover: #3a56d4;
            --accent-light: #eef1fe;
            --shadow-sm: 0 1px 3px rgba(0, 0, 0, 0.06);
            --shadow-md: 0 4px 20px rgba(0, 0, 0, 0.08);
            --shadow-lg: 0 12px 48px rgba(0, 0, 0, 0.12);
            --radius: 12px;
            --radius-sm: 8px;
            --transition: 0.2s ease;
            --font: 'Inter', -apple-system, BlinkMacSystemFont, sans-serif;
            --max-width: 900px;
            --sidebar-width: 280px;
        }

        html,
        body {
            height: 100%;
            font-family: var(--font);
            background: var(--bg-primary);
            color: var(--text-primary);
            font-size: 15px;
            line-height: 1.6;
            -webkit-font-smoothing: antialiased;
        }

        /* ---------- Scrollbar ---------- */
        ::-webkit-scrollbar {
            width: 5px;
            height: 5px;
        }
        ::-webkit-scrollbar-track {
            background: transparent;
        }
        ::-webkit-scrollbar-thumb {
            background: #c5c9d6;
            border-radius: 10px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: #a8adb8;
        }

        /* ---------- App Layout ---------- */
        #app {
            display: flex;
            height: 100vh;
            width: 100vw;
            overflow: hidden;
        }

        /* ---------- Sidebar ---------- */
        .sidebar {
            width: var(--sidebar-width);
            background: var(--bg-sidebar);
            border-right: 1px solid var(--border-light);
            display: flex;
            flex-direction: column;
            flex-shrink: 0;
            transition: transform 0.3s ease;
            z-index: 10;
        }

        .sidebar-header {
            padding: 20px 20px 16px;
            border-bottom: 1px solid var(--border-light);
            display: flex;
            align-items: center;
            justify-content: space-between;
        }

        .sidebar-brand {
            font-size: 22px;
            font-weight: 700;
            letter-spacing: -0.5px;
            color: var(--text-primary);
            display: flex;
            align-items: center;
            gap: 6px;
        }

        .sidebar-brand span {
            color: var(--accent);
        }

        .sidebar-brand small {
            font-size: 12px;
            font-weight: 400;
            color: var(--text-muted);
            letter-spacing: 0.3px;
            margin-left: 4px;
        }

        .sidebar-close-btn {
            display: none;
            background: none;
            border: none;
            font-size: 20px;
            color: var(--text-muted);
            cursor: pointer;
            padding: 4px 8px;
            border-radius: 6px;
        }
        .sidebar-close-btn:hover {
            background: var(--border-light);
        }

        .sidebar-user {
            padding: 14px 20px;
            border-bottom: 1px solid var(--border-light);
            display: flex;
            align-items: center;
            gap: 12px;
            min-height: 70px;
        }

        .sidebar-user-avatar {
            width: 36px;
            height: 36px;
            border-radius: 50%;
            background: var(--accent);
            color: #fff;
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: 600;
            font-size: 16px;
            flex-shrink: 0;
        }

        .sidebar-user-info {
            flex: 1;
            min-width: 0;
        }
        .sidebar-user-info .name {
            font-weight: 600;
            font-size: 14px;
            white-space: nowrap;
            overflow: hidden;
            text-overflow: ellipsis;
        }
        .sidebar-user-info .email {
            font-size: 12px;
            color: var(--text-muted);
            white-space: nowrap;
            overflow: hidden;
            text-overflow: ellipsis;
        }

        .sidebar-user-logout {
            background: none;
            border: none;
            color: var(--text-muted);
            cursor: pointer;
            font-size: 14px;
            padding: 4px 8px;
            border-radius: 6px;
            transition: var(--transition);
        }
        .sidebar-user-logout:hover {
            background: var(--border-light);
            color: var(--text-primary);
        }

        .sidebar-history {
            flex: 1;
            overflow-y: auto;
            padding: 12px 12px 20px;
        }

        .sidebar-history-title {
            font-size: 12px;
            font-weight: 600;
            text-transform: uppercase;
            letter-spacing: 0.5px;
            color: var(--text-muted);
            padding: 8px 10px 6px;
        }

        .history-item {
            display: flex;
            align-items: center;
            gap: 10px;
            padding: 8px 12px;
            border-radius: var(--radius-sm);
            cursor: pointer;
            transition: var(--transition);
            margin-bottom: 2px;
            font-size: 14px;
            color: var(--text-secondary);
            text-decoration: none;
            border: none;
            background: none;
            width: 100%;
            text-align: left;
        }

        .history-item:hover {
            background: var(--bg-primary);
        }

        .history-item.active {
            background: var(--accent-light);
            color: var(--accent);
            font-weight: 500;
        }

        .history-item .icon {
            font-size: 16px;
            flex-shrink: 0;
            opacity: 0.6;
        }

        .history-item .text {
            flex: 1;
            white-space: nowrap;
            overflow: hidden;
            text-overflow: ellipsis;
        }

        .history-item .delete-btn {
            background: none;
            border: none;
            color: var(--text-muted);
            cursor: pointer;
            font-size: 14px;
            padding: 2px 6px;
            border-radius: 4px;
            opacity: 0;
            transition: var(--transition);
        }
        .history-item:hover .delete-btn {
            opacity: 1;
        }
        .history-item .delete-btn:hover {
            background: #fee2e2;
            color: #dc2626;
        }

        .history-empty {
            padding: 20px 12px;
            color: var(--text-muted);
            font-size: 14px;
            text-align: center;
        }

        .sidebar-footer {
            padding: 12px 20px;
            border-top: 1px solid var(--border-light);
            font-size: 12px;
            color: var(--text-muted);
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .sidebar-footer .version {
            font-weight: 500;
        }

        /* ---------- Main Area ---------- */
        .main {
            flex: 1;
            display: flex;
            flex-direction: column;
            min-width: 0;
            background: var(--bg-primary);
        }

        /* Top bar */
        .main-topbar {
            padding: 14px 28px;
            background: var(--bg-secondary);
            border-bottom: 1px solid var(--border-light);
            display: flex;
            align-items: center;
            justify-content: space-between;
            flex-shrink: 0;
            min-height: 64px;
        }

        .main-topbar-left {
            display: flex;
            align-items: center;
            gap: 14px;
        }

        .sidebar-toggle-btn {
            display: none;
            background: none;
            border: none;
            font-size: 20px;
            color: var(--text-secondary);
            cursor: pointer;
            padding: 4px 8px;
            border-radius: 6px;
        }
        .sidebar-toggle-btn:hover {
            background: var(--border-light);
        }

        .main-topbar-title {
            font-size: 16px;
            font-weight: 600;
            color: var(--text-primary);
        }

        .main-topbar-title .sub {
            font-weight: 400;
            color: var(--text-muted);
            font-size: 14px;
        }

        .main-topbar-right {
            display: flex;
            align-items: center;
            gap: 12px;
        }

        .new-chat-btn {
            display: flex;
            align-items: center;
            gap: 6px;
            padding: 8px 16px;
            border-radius: var(--radius-sm);
            border: 1px solid var(--border-light);
            background: var(--bg-secondary);
            color: var(--text-secondary);
            font-size: 13px;
            font-weight: 500;
            cursor: pointer;
            transition: var(--transition);
            font-family: var(--font);
        }
        .new-chat-btn:hover {
            background: var(--bg-primary);
            border-color: var(--accent);
            color: var(--accent);
        }

        .login-btn {
            padding: 8px 20px;
            border-radius: var(--radius-sm);
            border: none;
            background: var(--accent);
            color: #fff;
            font-size: 13px;
            font-weight: 600;
            cursor: pointer;
            transition: var(--transition);
            font-family: var(--font);
        }
        .login-btn:hover {
            background: var(--accent-hover);
            transform: translateY(-1px);
            box-shadow: var(--shadow-sm);
        }

        .user-badge {
            display: flex;
            align-items: center;
            gap: 10px;
            padding: 4px 12px 4px 4px;
            border-radius: 50px;
            background: var(--bg-primary);
            border: 1px solid var(--border-light);
        }
        .user-badge .avatar {
            width: 30px;
            height: 30px;
            border-radius: 50%;
            background: var(--accent);
            color: #fff;
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: 600;
            font-size: 13px;
            flex-shrink: 0;
        }
        .user-badge .name {
            font-size: 13px;
            font-weight: 500;
        }

        /* Chat messages */
        .chat-messages {
            flex: 1;
            overflow-y: auto;
            padding: 20px 28px 8px;
            display: flex;
            flex-direction: column;
            gap: 4px;
        }

        .message {
            display: flex;
            gap: 14px;
            padding: 14px 18px;
            border-radius: var(--radius);
            max-width: 88%;
            animation: fadeIn 0.25s ease;
            margin-bottom: 4px;
        }

        .message.user {
            align-self: flex-end;
            background: var(--accent);
            color: #fff;
            border-bottom-right-radius: 4px;
        }

        .message.assistant {
            align-self: flex-start;
            background: var(--bg-secondary);
            border: 1px solid var(--border-light);
            border-bottom-left-radius: 4px;
            box-shadow: var(--shadow-sm);
        }

        .message .avatar {
            width: 30px;
            height: 30px;
            border-radius: 50%;
            flex-shrink: 0;
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: 600;
            font-size: 13px;
            background: var(--bg-primary);
            color: var(--text-secondary);
        }

        .message.user .avatar {
            background: rgba(255, 255, 255, 0.2);
            color: #fff;
        }

        .message .content {
            flex: 1;
            min-width: 0;
            word-wrap: break-word;
            overflow-wrap: break-word;
            font-size: 14px;
            line-height: 1.7;
        }

        .message .content pre {
            background: #1a1a2e;
            color: #e4e7ed;
            padding: 14px 18px;
            border-radius: var(--radius-sm);
            overflow-x: auto;
            font-size: 13px;
            line-height: 1.6;
            margin: 8px 0 4px;
            font-family: 'JetBrains Mono', 'Fira Code', monospace;
            max-width: 100%;
        }

        .message .content code {
            font-family: 'JetBrains Mono', 'Fira Code', monospace;
            font-size: 13px;
            background: rgba(0, 0, 0, 0.06);
            padding: 2px 8px;
            border-radius: 4px;
        }

        .message.user .content code {
            background: rgba(255, 255, 255, 0.15);
        }

        .message .content ul,
        .message .content ol {
            padding-left: 22px;
            margin: 6px 0;
        }

        .message .content p {
            margin: 4px 0;
        }

        .message .content blockquote {
            border-left: 3px solid var(--accent);
            padding-left: 14px;
            margin: 8px 0;
            color: var(--text-secondary);
        }

        .message .timestamp {
            font-size: 11px;
            opacity: 0.5;
            margin-top: 6px;
            display: block;
        }

        .message.user .timestamp {
            opacity: 0.7;
        }

        .typing-indicator {
            align-self: flex-start;
            background: var(--bg-secondary);
            border: 1px solid var(--border-light);
            border-radius: var(--radius);
            border-bottom-left-radius: 4px;
            padding: 14px 20px;
            display: flex;
            gap: 4px;
            align-items: center;
            box-shadow: var(--shadow-sm);
        }

        .typing-indicator span {
            display: inline-block;
            width: 8px;
            height: 8px;
            border-radius: 50%;
            background: var(--text-muted);
            animation: typing 1.4s infinite both;
        }
        .typing-indicator span:nth-child(2) {
            animation-delay: 0.2s;
        }
        .typing-indicator span:nth-child(3) {
            animation-delay: 0.4s;
        }

        @keyframes typing {
            0%,
            80%,
            100% {
                transform: scale(0.6);
                opacity: 0.4;
            }
            40% {
                transform: scale(1);
                opacity: 1;
            }
        }

        @keyframes fadeIn {
            from {
                opacity: 0;
                transform: translateY(6px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        /* Welcome / Empty state */
        .welcome-state {
            flex: 1;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            padding: 40px 20px;
            text-align: center;
            color: var(--text-secondary);
        }

        .welcome-state h1 {
            font-size: 32px;
            font-weight: 700;
            letter-spacing: -0.5px;
            color: var(--text-primary);
            margin-bottom: 8px;
        }

        .welcome-state h1 span {
            color: var(--accent);
        }

        .welcome-state p {
            font-size: 16px;
            color: var(--text-muted);
            max-width: 480px;
            margin-bottom: 24px;
        }

        .welcome-state .features {
            display: flex;
            gap: 24px;
            flex-wrap: wrap;
            justify-content: center;
            margin-top: 8px;
        }

        .welcome-state .feature {
            background: var(--bg-secondary);
            padding: 16px 22px;
            border-radius: var(--radius);
            border: 1px solid var(--border-light);
            box-shadow: var(--shadow-sm);
            font-size: 14px;
            color: var(--text-secondary);
            min-width: 140px;
        }

        .welcome-state .feature strong {
            color: var(--text-primary);
            display: block;
            font-size: 15px;
            margin-bottom: 2px;
        }

        /* Input area */
        .chat-input-area {
            padding: 12px 28px 20px;
            background: var(--bg-primary);
            flex-shrink: 0;
            border-top: 1px solid transparent;
        }

        .chat-input-wrapper {
            display: flex;
            gap: 12px;
            background: var(--bg-secondary);
            border-radius: var(--radius);
            border: 1px solid var(--border-light);
            padding: 6px 6px 6px 18px;
            box-shadow: var(--shadow-sm);
            transition: var(--transition);
            max-width: var(--max-width);
            margin: 0 auto;
        }

        .chat-input-wrapper:focus-within {
            border-color: var(--accent);
            box-shadow: 0 0 0 4px rgba(79, 110, 247, 0.12);
        }

        .chat-input-wrapper textarea {
            flex: 1;
            border: none;
            outline: none;
            resize: none;
            font-family: var(--font);
            font-size: 14px;
            padding: 10px 0;
            background: transparent;
            color: var(--text-primary);
            min-height: 48px;
            max-height: 160px;
            line-height: 1.5;
        }

        .chat-input-wrapper textarea::placeholder {
            color: var(--text-muted);
        }

        .chat-input-wrapper .send-btn {
            padding: 8px 20px;
            border-radius: var(--radius-sm);
            border: none;
            background: var(--accent);
            color: #fff;
            font-weight: 600;
            font-size: 14px;
            cursor: pointer;
            transition: var(--transition);
            font-family: var(--font);
            white-space: nowrap;
            height: 44px;
            align-self: center;
        }

        .chat-input-wrapper .send-btn:hover {
            background: var(--accent-hover);
            transform: translateY(-1px);
        }

        .chat-input-wrapper .send-btn:disabled {
            opacity: 0.5;
            cursor: not-allowed;
            transform: none;
        }

        .chat-input-footer {
            max-width: var(--max-width);
            margin: 8px auto 0;
            font-size: 12px;
            color: var(--text-muted);
            padding-left: 4px;
            display: flex;
            justify-content: space-between;
        }

        .chat-input-footer .hint {
            display: flex;
            gap: 16px;
            flex-wrap: wrap;
        }

        .chat-input-footer .hint span {
            opacity: 0.7;
        }

        /* ---------- Login overlay ---------- */
        .login-overlay {
            position: fixed;
            inset: 0;
            background: rgba(0, 0, 0, 0.3);
            backdrop-filter: blur(4px);
            display: flex;
            align-items: center;
            justify-content: center;
            z-index: 100;
            opacity: 0;
            pointer-events: none;
            transition: opacity 0.3s ease;
        }

        .login-overlay.active {
            opacity: 1;
            pointer-events: all;
        }

        .login-modal {
            background: var(--bg-secondary);
            padding: 40px 48px;
            border-radius: var(--radius);
            max-width: 420px;
            width: 90%;
            box-shadow: var(--shadow-lg);
            text-align: center;
            transform: scale(0.96);
            transition: transform 0.3s ease;
        }

        .login-overlay.active .login-modal {
            transform: scale(1);
        }

        .login-modal h2 {
            font-size: 26px;
            font-weight: 700;
            margin-bottom: 6px;
            letter-spacing: -0.3px;
        }

        .login-modal h2 span {
            color: var(--accent);
        }

        .login-modal p {
            color: var(--text-muted);
            font-size: 15px;
            margin-bottom: 24px;
        }

        .login-modal .google-btn {
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 12px;
            width: 100%;
            padding: 12px 20px;
            border: 1px solid var(--border-light);
            border-radius: var(--radius-sm);
            background: #fff;
            font-size: 15px;
            font-weight: 500;
            cursor: pointer;
            transition: var(--transition);
            font-family: var(--font);
            color: var(--text-primary);
        }

        .login-modal .google-btn:hover {
            background: var(--bg-primary);
            border-color: var(--accent);
            box-shadow: var(--shadow-sm);
        }

        .login-modal .google-btn svg {
            width: 20px;
            height: 20px;
        }

        .login-modal .close-modal {
            margin-top: 16px;
            background: none;
            border: none;
            color: var(--text-muted);
            font-size: 14px;
            cursor: pointer;
            font-family: var(--font);
        }
        .login-modal .close-modal:hover {
            color: var(--text-primary);
        }

        /* ---------- Responsive ---------- */
        @media (max-width: 768px) {
            .sidebar {
                position: fixed;
                top: 0;
                left: 0;
                bottom: 0;
                transform: translateX(-100%);
                width: 280px;
                box-shadow: var(--shadow-lg);
                border-right: none;
                border-radius: 0 12px 12px 0;
            }

            .sidebar.open {
                transform: translateX(0);
            }

            .sidebar-close-btn {
                display: block;
            }

            .sidebar-toggle-btn {
                display: block;
            }

            .main-topbar {
                padding: 10px 16px;
            }

            .chat-messages {
                padding: 12px 14px 4px;
            }

            .chat-input-area {
                padding: 8px 14px 14px;
            }

            .message {
                max-width: 94%;
                padding: 12px 14px;
            }

            .welcome-state h1 {
                font-size: 24px;
            }

            .welcome-state .features {
                flex-direction: column;
                gap: 10px;
            }

            .welcome-state .feature {
                min-width: unset;
                padding: 12px 18px;
            }

            .login-modal {
                padding: 28px 24px;
            }

            .user-badge .name {
                display: none;
            }

            .new-chat-btn span {
                display: none;
            }

            .chat-input-wrapper {
                padding: 4px 4px 4px 14px;
            }

            .chat-input-wrapper textarea {
                font-size: 13px;
                min-height: 40px;
            }

            .chat-input-wrapper .send-btn {
                padding: 6px 14px;
                font-size: 13px;
                height: 38px;
            }
        }

        @media (max-width: 420px) {
            .main-topbar-title {
                font-size: 14px;
            }
            .main-topbar-title .sub {
                font-size: 12px;
            }
            .message .content {
                font-size: 13px;
            }
            .message .content pre {
                font-size: 12px;
                padding: 10px 12px;
            }
        }
    </style>
</head>
<body>

    <!-- ===== APP ===== -->
    <div id="app">

        <!-- ===== SIDEBAR ===== -->
        <aside class="sidebar" id="sidebar">
            <div class="sidebar-header">
                <div class="sidebar-brand">
                    Sea<span>.</span>
                    <small>v1</small>
                </div>
                <button class="sidebar-close-btn" id="sidebarCloseBtn" aria-label="Close sidebar">✕</button>
            </div>

            <div class="sidebar-user" id="sidebarUser">
                <div class="sidebar-user-avatar" id="sidebarAvatar">S</div>
                <div class="sidebar-user-info">
                    <div class="name" id="sidebarName">Guest</div>
                    <div class="email" id="sidebarEmail">not signed in</div>
                </div>
                <button class="sidebar-user-logout" id="logoutBtn" title="Sign out" style="display:none;">✕</button>
            </div>

            <div class="sidebar-history" id="historyList">
                <div class="sidebar-history-title">History</div>
                <div class="history-empty" id="historyEmpty">No conversations yet</div>
            </div>

            <div class="sidebar-footer">
                <span class="version">⚡ Sea AI</span>
                <span>restricted · safe</span>
            </div>
        </aside>

        <!-- ===== MAIN ===== -->
        <main class="main">

            <!-- Top bar -->
            <div class="main-topbar">
                <div class="main-topbar-left">
                    <button class="sidebar-toggle-btn" id="sidebarToggleBtn" aria-label="Toggle sidebar">☰</button>
                    <div class="main-topbar-title">
                        Sea <span class="sub">· assistant</span>
                    </div>
                </div>
                <div class="main-topbar-right">
                    <button class="new-chat-btn" id="newChatBtn">
                        <span>+</span> <span>New</span>
                    </button>
                    <button class="login-btn" id="loginBtn">Sign in with Google</button>
                    <div class="user-badge" id="userBadge" style="display:none;">
                        <div class="avatar" id="badgeAvatar">S</div>
                        <span class="name" id="badgeName">User</span>
                    </div>
                </div>
            </div>

            <!-- Chat messages -->
            <div class="chat-messages" id="chatMessages">
                <!-- Welcome state -->
                <div class="welcome-state" id="welcomeState">
                    <h1>Sea <span>.</span></h1>
                    <p>Your intelligent assistant for code, design, search, and more.</p>
                    <div class="features">
                        <div class="feature"><strong>💻 Code</strong> Build &amp; debug</div>
                        <div class="feature"><strong>🎨 GUI</strong> Design interfaces</div>
                        <div class="feature"><strong>🔍 Search</strong> Find answers</div>
                        <div class="feature"><strong>🧠 Understand</strong> Natural language</div>
                    </div>
                </div>
            </div>

            <!-- Input area -->
            <div class="chat-input-area">
                <div class="chat-input-wrapper">
                    <textarea id="chatInput" rows="1" placeholder="Ask me anything…" disabled></textarea>
                    <button class="send-btn" id="sendBtn" disabled>Send</button>
                </div>
                <div class="chat-input-footer">
                    <div class="hint">
                        <span>⌘ ↵ to send</span>
                        <span>Safe · restricted</span>
                    </div>
                    <span id="charCount">0</span>
                </div>
            </div>
        </main>
    </div>

    <!-- ===== LOGIN OVERLAY ===== -->
    <div class="login-overlay" id="loginOverlay">
        <div class="login-modal">
            <h2>Sea <span>.</span></h2>
            <p>Sign in with your Google account to save your history and access all features.</p>
            <button class="google-btn" id="googleLoginBtn">
                <!-- Google SVG -->
                <svg viewBox="0 0 48 48">
                    <path fill="#FFC107" d="M43.611 20.083H42V20H24v8h11.303c-1.649 4.657-6.08 8-11.303 8c-6.627 0-12-5.373-12-12s5.373-12 12-12c3.059 0 5.842 1.154 7.961 3.039l5.657-5.657C34.046 6.053 29.268 4 24 4C12.955 4 4 12.955 4 24s8.955 20 20 20s20-8.955 20-20c0-1.341-.138-2.65-.389-3.917z"/>
                    <path fill="#FF3D00" d="m6.306 14.691 6.571 4.819C14.655 15.108 18.961 12 24 12c3.059 0 5.842 1.154 7.961 3.039l5.657-5.657C34.046 6.053 29.268 4 24 4 16.318 4 9.656 8.337 6.306 14.691z"/>
                    <path fill="#4CAF50" d="M24 44c5.166 0 9.86-1.977 13.409-5.192l-6.19-5.238A11.91 11.91 0 0 1 24 36c-5.202 0-9.619-3.317-11.283-7.946l-6.522 5.025C9.505 39.556 16.227 44 24 44z"/>
                    <path fill="#1976D2" d="M43.611 20.083H42V20H24v8h11.303a12.04 12.04 0 0 1-4.087 5.571l.003-.002 6.19 5.238C36.971 39.205 44 34 44 24c0-1.341-.138-2.65-.389-3.917z"/>
                </svg>
                Sign in with Google
            </button>
            <button class="close-modal" id="closeLoginModal">Continue as guest</button>
        </div>
    </div>

    <script>
        // ===================================================================
        //  SEA – Full client‑side application
        //  (Backend would be Node.js + Express + OpenAI + Google OAuth)
        //  This demo uses a simulated AI with real‑time responses.
        //  History is stored in localStorage.
        // ===================================================================

        (function() {
            'use strict';

            // ---------- DOM refs ----------
            const sidebar = document.getElementById('sidebar');
            const sidebarToggleBtn = document.getElementById('sidebarToggleBtn');
            const sidebarCloseBtn = document.getElementById('sidebarCloseBtn');
            const chatMessages = document.getElementById('chatMessages');
            const chatInput = document.getElementById('chatInput');
            const sendBtn = document.getElementById('sendBtn');
            const newChatBtn = document.getElementById('newChatBtn');
            const loginBtn = document.getElementById('loginBtn');
            const logoutBtn = document.getElementById('logoutBtn');
            const loginOverlay = document.getElementById('loginOverlay');
            const googleLoginBtn = document.getElementById('googleLoginBtn');
            const closeLoginModal = document.getElementById('closeLoginModal');
            const welcomeState = document.getElementById('welcomeState');
            const historyList = document.getElementById('historyList');
            const historyEmpty = document.getElementById('historyEmpty');
            const charCount = document.getElementById('charCount');

            const sidebarName = document.getElementById('sidebarName');
            const sidebarEmail = document.getElementById('sidebarEmail');
            const sidebarAvatar = document.getElementById('sidebarAvatar');
            const badgeAvatar = document.getElementById('badgeAvatar');
            const badgeName = document.getElementById('badgeName');
            const userBadge = document.getElementById('userBadge');

            // ---------- State ----------
            const state = {
                user: null, // { name, email, picture? }
                conversations: [], // [{ id, title, messages }]
                currentConversationId: null,
                isProcessing: false,
                isAuthenticated: false,
                // Restriction filter (simple)
                restrictedTerms: [
                    'illegal', 'hack', 'exploit', 'malware', 'virus', 'ransomware',
                    'crack', 'keygen', 'torrent', 'pirate', 'piracy', 'drugs',
                    'weapon', 'explosive', 'terrorism', 'abuse', 'harass',
                    'doxx', 'swat', 'fraud', 'scam', 'phish'
                ]
            };

            // ---------- Utility ----------
            function generateId() {
                return Date.now().toString(36) + '-' + Math.random().toString(36).substr(2, 6);
            }

            function truncate(str, n = 40) {
                return str.length > n ? str.slice(0, n) + '…' : str;
            }

            function getInitials(name) {
                if (!name) return 'S';
                const parts = name.trim().split(/\s+/);
                if (parts.length === 1) return parts[0].slice(0, 2).toUpperCase();
                return (parts[0][0] + parts[parts.length - 1][0]).toUpperCase();
            }

            function isRestricted(text) {
                const lower = text.toLowerCase();
                return state.restrictedTerms.some(term => lower.includes(term));
            }

            // ---------- Storage ----------
            function loadConversations() {
                try {
                    const raw = localStorage.getItem('sea_conversations');
                    if (raw) {
                        state.conversations = JSON.parse(raw);
                        return;
                    }
                } catch (_) { /* ignore */ }
                state.conversations = [];
            }

            function saveConversations() {
                try {
                    localStorage.setItem('sea_conversations', JSON.stringify(state.conversations));
                } catch (_) { /* ignore */ }
            }

            function loadUser() {
                try {
                    const raw = localStorage.getItem('sea_user');
                    if (raw) {
                        const user = JSON.parse(raw);
                        state.user = user;
                        state.isAuthenticated = true;
                        return user;
                    }
                } catch (_) { /* ignore */ }
                return null;
            }

            function saveUser(user) {
                state.user = user;
                state.isAuthenticated = !!user;
                if (user) {
                    localStorage.setItem('sea_user', JSON.stringify(user));
                } else {
                    localStorage.removeItem('sea_user');
                }
                updateUI();
            }

            // ---------- UI update ----------
            function updateUI() {
                const user = state.user;
                const isAuth = state.isAuthenticated;

                // Sidebar user
                if (isAuth && user) {
                    const name = user.name || 'User';
                    const email = user.email || '';
                    sidebarName.textContent = name;
                    sidebarEmail.textContent = email;
                    const initials = getInitials(name);
                    sidebarAvatar.textContent = initials;
                    badgeAvatar.textContent = initials;
                    badgeName.textContent = name;
                    userBadge.style.display = 'flex';
                    loginBtn.style.display = 'none';
                    logoutBtn.style.display = 'block';
                } else {
                    sidebarName.textContent = 'Guest';
                    sidebarEmail.textContent = 'not signed in';
                    sidebarAvatar.textContent = 'S';
                    userBadge.style.display = 'none';
                    loginBtn.style.display = 'block';
                    logoutBtn.style.display = 'none';
                }

                // Input enabled only if authenticated
                chatInput.disabled = !isAuth;
                sendBtn.disabled = !isAuth || state.isProcessing;

                // Render history
                renderHistory();

                // Show welcome if no messages
                const hasMessages = state.conversations.some(c =>
                    c.messages && c.messages.length > 0
                );
                if (hasMessages) {
                    welcomeState.style.display = 'none';
                } else {
                    welcomeState.style.display = 'flex';
                }
            }

            function renderHistory() {
                // Remove old items (keep title + empty)
                const title = historyList.querySelector('.sidebar-history-title');
                const empty = historyList.querySelector('.history-empty');
                historyList.innerHTML = '';
                if (title) historyList.appendChild(title);
                if (empty) historyList.appendChild(empty);

                const convs = state.conversations;
                if (convs.length === 0) {
                    const emptyEl = document.createElement('div');
                    emptyEl.className = 'history-empty';
                    emptyEl.textContent = 'No conversations yet';
                    historyList.appendChild(emptyEl);
                    return;
                }

                // Sort by last message time (desc)
                const sorted = [...convs].sort((a, b) => {
                    const ta = a.updatedAt || a.createdAt || 0;
                    const tb = b.updatedAt || b.createdAt || 0;
                    return tb - ta;
                });

                for (const conv of sorted) {
                    const item = document.createElement('button');
                    item.className = 'history-item';
                    if (conv.id === state.currentConversationId) {
                        item.classList.add('active');
                    }

                    const iconSpan = document.createElement('span');
                    iconSpan.className = 'icon';
                    iconSpan.textContent = '💬';
                    item.appendChild(iconSpan);

                    const textSpan = document.createElement('span');
                    textSpan.className = 'text';
                    textSpan.textContent = conv.title || 'Untitled';
                    item.appendChild(textSpan);

                    const delBtn = document.createElement('button');
                    delBtn.className = 'delete-btn';
                    delBtn.textContent = '✕';
                    delBtn.setAttribute('aria-label', 'Delete conversation');
                    delBtn.addEventListener('click', (e) => {
                        e.stopPropagation();
                        deleteConversation(conv.id);
                    });
                    item.appendChild(delBtn);

                    item.addEventListener('click', () => {
                        loadConversation(conv.id);
                    });

                    historyList.appendChild(item);
                }
            }

            // ---------- Conversation methods ----------
            function createConversation(title) {
                const conv = {
                    id: generateId(),
                    title: title || 'New conversation',
                    messages: [],
                    createdAt: Date.now(),
                    updatedAt: Date.now()
                };
                state.conversations.push(conv);
                state.currentConversationId = conv.id;
                saveConversations();
                renderHistory();
                renderMessages(conv);
                updateUI();
                return conv;
            }

            function loadConversation(id) {
                const conv = state.conversations.find(c => c.id === id);
                if (!conv) return;
                state.currentConversationId = id;
                renderMessages(conv);
                renderHistory();
                updateUI();
                // Scroll to bottom
                setTimeout(() => {
                    chatMessages.scrollTop = chatMessages.scrollHeight;
                }, 50);
            }

            function deleteConversation(id) {
                if (!confirm('Delete this conversation?')) return;
                state.conversations = state.conversations.filter(c => c.id !== id);
                if (state.currentConversationId === id) {
                    state.currentConversationId = null;
                    // Show welcome
                    const welcome = document.getElementById('welcomeState');
                    if (welcome) welcome.style.display = 'flex';
                    // Clear messages
                    const msgs = chatMessages.querySelectorAll('.message');
                    msgs.forEach(el => el.remove());
                }
                saveConversations();
                renderHistory();
                updateUI();
            }

            function getCurrentConversation() {
                if (!state.currentConversationId) {
                    // Create a new one
                    return createConversation('New chat');
                }
                const conv = state.conversations.find(c => c.id === state.currentConversationId);
                if (!conv) {
                    return createConversation('New chat');
                }
                return conv;
            }

            function renderMessages(conv) {
                // Remove all messages but keep welcome
                const msgs = chatMessages.querySelectorAll('.message');
                msgs.forEach(el => el.remove());
                const welcome = document.getElementById('welcomeState');
                if (welcome) welcome.style.display = 'none';

                if (!conv || !conv.messages || conv.messages.length === 0) {
                    if (welcome) welcome.style.display = 'flex';
                    return;
                }

                for (const msg of conv.messages) {
                    appendMessageDOM(msg.role, msg.content, false);
                }

                // Scroll to bottom
                setTimeout(() => {
                    chatMessages.scrollTop = chatMessages.scrollHeight;
                }, 50);
            }

            // ---------- DOM message helpers ----------
            function appendMessageDOM(role, content, animate = true) {
                const wrapper = document.createElement('div');
                wrapper.className = `message ${role}`;
                if (animate) {
                    wrapper.style.animation = 'fadeIn 0.25s ease';
                }

                const avatar = document.createElement('div');
                avatar.className = 'avatar';
                if (role === 'user') {
                    avatar.textContent = state.user ? getInitials(state.user.name) : 'U';
                } else {
                    avatar.textContent = 'S';
                    avatar.style.background = 'var(--accent-light)';
                    avatar.style.color = 'var(--accent)';
                }
                wrapper.appendChild(avatar);

                const contentDiv = document.createElement('div');
                contentDiv.className = 'content';
                contentDiv.innerHTML = formatContent(content);
                wrapper.appendChild(contentDiv);

                chatMessages.appendChild(wrapper);

                // Hide welcome
                const welcome = document.getElementById('welcomeState');
                if (welcome) welcome.style.display = 'none';
            }

            function formatContent(text) {
                // Basic markdown-ish formatting
                let html = text;

                // Code blocks
                html = html.replace(/```(\w*)\n([\s\S]*?)```/g, (_, lang, code) => {
                    return `<pre><code>${escapeHtml(code.trim())}</code></pre>`;
                });

                // Inline code
                html = html.replace(/`([^`]+)`/g, (_, code) => {
                    return `<code>${escapeHtml(code)}</code>`;
                });

                // Bold
                html = html.replace(/\*\*([^*]+)\*\*/g, '<strong>$1</strong>');

                // Italic
                html = html.replace(/\*([^*]+)\*/g, '<em>$1</em>');

                // Line breaks
                html = html.replace(/\n/g, '<br>');

                // Links
                html = html.replace(/(https?:\/\/[^\s]+)/g, '<a href="$1" target="_blank" rel="noopener">$1</a>');

                return html;
            }

            function escapeHtml(text) {
                const div = document.createElement('div');
                div.textContent = text;
                return div.innerHTML;
            }

            function showTyping() {
                const el = document.createElement('div');
                el.className = 'typing-indicator';
                el.id = 'typingIndicator';
                for (let i = 0; i < 3; i++) {
                    const span = document.createElement('span');
                    el.appendChild(span);
                }
                chatMessages.appendChild(el);
                chatMessages.scrollTop = chatMessages.scrollHeight;
            }

            function hideTyping() {
                const el = document.getElementById('typingIndicator');
                if (el) el.remove();
            }

            // ---------- AI response (simulated) ----------
            function getAIResponse(userMessage) {
                // In a real app, this would call OpenAI API via backend.
                // Here we simulate with intelligent responses.
                const lower = userMessage.toLowerCase();

                // Check restrictions
                if (isRestricted(userMessage)) {
                    return "I'm sorry, but I can't respond to that request. It falls outside my safety guidelines.";
                }

                // Code generation
                if (lower.includes('code') || lower.includes('function') || lower.includes('build') && lower.includes(
                    'app')) {
                    return "Here's a simple React component for a counter:\n\n```jsx\nimport React, { useState } from 'react';\n\nfunction Counter() {\n  const [count, setCount] = useState(0);\n  return (\n    <div>\n      <p>You clicked {count} times</p>\n      <button onClick={() => setCount(count + 1)}>Click me</button>\n    </div>\n  );\n}\n\nexport default Counter;\n```\n\nYou can customize the styling and behavior as needed.";
                }

                // GUI / design
                if (lower.includes('gui') || lower.includes('design') || lower.includes('interface')) {
                    return "Here's a clean login form GUI using HTML & CSS:\n\n```html\n<div class=\"login-container\">\n  <h2>Welcome back</h2>\n  <input type=\"email\" placeholder=\"Email\" />\n  <input type=\"password\" placeholder=\"Password\" />\n  <button>Sign in</button>\n  <a href=\"#\">Forgot password?</a>\n</div>\n```\n\n```css\n.login-container {\n  max-width: 340px;\n  margin: 60px auto;\n  padding: 30px;\n  background: #fff;\n  border-radius: 16px;\n  box-shadow: 0 8px 30px rgba(0,0,0,0.12);\n}\n```\n\nYou can adapt this to your framework of choice.";
                }

                // Search
                if (lower.includes('search') || lower.includes('find') || lower.includes('look up')) {
                    return "I can help you search for information. Try being more specific about what you're looking for, and I'll provide relevant details, code snippets, or explanations.";
                }

                // General understanding
                if (lower.includes('what') || lower.includes('how') || lower.includes('why') || lower.includes(
                    'explain')) {
                    return "That's a great question! Let me break it down for you:\n\n1. **Understanding the problem**: I'll analyze your request carefully.\n2. **Finding the right approach**: Based on best practices and patterns.\n3. **Delivering a clear solution**: With examples and explanations.\n\nCould you give me a bit more context so I can tailor my response precisely?";
                }

                // Default
                return "I understand your request. Here's what I can do for you:\n\n- Write and explain code in any language\n- Design UI components and layouts\n- Search and summarize information\n- Answer questions with clarity\n\nFeel free to ask something specific, and I'll give you a detailed, practical answer.";
            }

            // ---------- Send message ----------
            async function sendMessage() {
                const text = chatInput.value.trim();
                if (!text || state.isProcessing || !state.isAuthenticated) return;

                // Restriction check
                if (isRestricted(text)) {
                    const conv = getCurrentConversation();
                    conv.messages.push({ role: 'user', content: text });
                    conv.messages.push({
                        role: 'assistant',
                        content: "I'm sorry, but I can't respond to that request. It falls outside my safety guidelines."
                    });
                    conv.updatedAt = Date.now();
                    if (!conv.title || conv.title === 'New conversation') {
                        conv.title = truncate(text, 30);
                    }
                    saveConversations();
                    renderMessages(conv);
                    renderHistory();
                    updateUI();
                    chatInput.value = '';
                    chatInput.style.height = 'auto';
                    charCount.textContent = '0';
                    return;
                }

                state.isProcessing = true;
                sendBtn.disabled = true;
                chatInput.disabled = true;

                // Get or create conversation
                const conv = getCurrentConversation();

                // Add user message
                conv.messages.push({ role: 'user', content: text });
                conv.updatedAt = Date.now();
                if (!conv.title || conv.title === 'New conversation') {
                    conv.title = truncate(text, 30);
                }
                saveConversations();

                // Render user message
                appendMessageDOM('user', text, true);
                chatInput.value = '';
                chatInput.style.height = 'auto';
                charCount.textContent = '0';
                chatMessages.scrollTop = chatMessages.scrollHeight;

                // Show typing
                showTyping();

                // Simulate AI delay
                await new Promise(r => setTimeout(r, 600 + Math.random() * 800));

                // Generate response
                const response = getAIResponse(text);

                // Hide typing
                hideTyping();

                // Add assistant message
                conv.messages.push({ role: 'assistant', content: response });
                conv.updatedAt = Date.now();
                saveConversations();

                // Render assistant message
                appendMessageDOM('assistant', response, true);
                chatMessages.scrollTop = chatMessages.scrollHeight;

                // Update history
                renderHistory();
                updateUI();

                state.isProcessing = false;
                sendBtn.disabled = false;
                chatInput.disabled = false;
                chatInput.focus();
            }

            // ---------- New chat ----------
            function startNewChat() {
                const conv = createConversation('New conversation');
                state.currentConversationId = conv.id;
                // Clear messages
                const msgs = chatMessages.querySelectorAll('.message');
                msgs.forEach(el => el.remove());
                const welcome = document.getElementById('welcomeState');
                if (welcome) welcome.style.display = 'flex';
                renderHistory();
                updateUI();
                chatInput.focus();
            }

            // ---------- Auth (simulated Google) ----------
            function simulateGoogleLogin() {
                // In production, this would redirect to Google OAuth.
                // Here we simulate a successful login.
                const user = {
                    name: 'Alex Rivera',
                    email: 'alex.rivera@gmail.com',
                    picture: null
                };
                saveUser(user);
                loginOverlay.classList.remove('active');
                // Create a welcome conversation if none
                if (state.conversations.length === 0) {
                    createConversation('Welcome to Sea');
                }
                updateUI();
                chatInput.focus();
            }

            function logout() {
                if (!confirm('Sign out of Sea?')) return;
                saveUser(null);
                // Clear history from localStorage? Keep it, but user is guest.
                // Actually we keep it but show as guest.
                updateUI();
                chatInput.disabled = true;
                sendBtn.disabled = true;
                // Show login overlay
                loginOverlay.classList.add('active');
            }

            // ---------- Event listeners ----------
            // Send
            sendBtn.addEventListener('click', sendMessage);
            chatInput.addEventListener('keydown', (e) => {
                if (e.key === 'Enter' && !e.shiftKey) {
                    e.preventDefault();
                    sendMessage();
                }
            });

            // Auto-resize textarea
            chatInput.addEventListener('input', () => {
                chatInput.style.height = 'auto';
                chatInput.style.height = Math.min(chatInput.scrollHeight, 160) + 'px';
                charCount.textContent = chatInput.value.length;
            });

            // New chat
            newChatBtn.addEventListener('click', startNewChat);

            // Sidebar toggle
            sidebarToggleBtn.addEventListener('click', () => {
                sidebar.classList.toggle('open');
            });
            sidebarCloseBtn.addEventListener('click', () => {
                sidebar.classList.remove('open');
            });

            // Login
            loginBtn.addEventListener('click', () => {
                loginOverlay.classList.add('active');
            });
            googleLoginBtn.addEventListener('click', simulateGoogleLogin);
            closeLoginModal.addEventListener('click', () => {
                loginOverlay.classList.remove('active');
                if (!state.isAuthenticated) {
                    // Continue as guest
                    chatInput.disabled = true;
                    sendBtn.disabled = true;
                }
            });

            // Logout
            logoutBtn.addEventListener('click', logout);

            // Close overlay on click outside
            loginOverlay.addEventListener('click', (e) => {
                if (e.target === loginOverlay) {
                    loginOverlay.classList.remove('active');
                }
            });

            // ---------- Init ----------
            function init() {
                loadConversations();

                const user = loadUser();
                if (user) {
                    state.isAuthenticated = true;
                    state.user = user;
                    if (state.conversations.length === 0) {
                        createConversation('Welcome to Sea');
                    }
                } else {
                    // Show login overlay after a beat
                    setTimeout(() => {
                        loginOverlay.classList.add('active');
                    }, 400);
                }

                // If there's a current conversation, load it
                if (state.currentConversationId) {
                    const conv = state.conversations.find(c => c.id === state.currentConversationId);
                    if (conv) {
                        renderMessages(conv);
                    } else if (state.conversations.length > 0) {
                        state.currentConversationId = state.conversations[0].id;
                        renderMessages(state.conversations[0]);
                    }
                } else if (state.conversations.length > 0) {
                    state.currentConversationId = state.conversations[0].id;
                    renderMessages(state.conversations[0]);
                }

                updateUI();

                // Focus input if authenticated
                if (state.isAuthenticated) {
                    chatInput.focus();
                }

                console.log('🌊 Sea AI initialized.');
                console.log(`📚 ${state.conversations.length} conversations loaded.`);
                console.log(`👤 ${state.isAuthenticated ? 'Authenticated: ' + state.user.name : 'Guest mode'}`);
            }

            // Handle click on history items (delegated)
            historyList.addEventListener('click', (e) => {
                const item = e.target.closest('.history-item');
                if (!item) return;
                // The click is handled by each item's own listener
            });

            // Expose some functions for debugging
            window.__sea = {
                state,
                startNewChat,
                sendMessage,
                logout,
                simulateGoogleLogin
            };

            // Run
            init();

        })();
    </script>

</body>
</html>
