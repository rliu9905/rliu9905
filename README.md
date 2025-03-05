<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>TSG交互式艺术空间 - 应用原型</title>
    <link href="https://fonts.googleapis.com/css2?family=Jura:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <style>
        :root {
            --primary-color: #00f7ff;
            --secondary-color: #ff00e6;
            --tertiary-color: #7b00ff;
            --background-color: #0a0a12;
            --card-bg-color: #14141f;
            --text-color: #ffffff;
            --text-secondary: #b3b3cc;
            --success-color: #00ff9d;
            --warning-color: #ffcc00;
            --error-color: #ff3366;
        }
        
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        
        body {
            font-family: 'Jura', sans-serif;
            background-color: var(--background-color);
            color: var(--text-color);
            line-height: 1.6;
            padding: 20px;
        }
        
        h1, h2, h3, h4 {
            font-weight: 600;
            margin-bottom: 15px;
        }
        
        h1 {
            font-size: 28px;
            text-align: center;
            margin: 30px 0;
            background: linear-gradient(90deg, var(--primary-color), var(--secondary-color));
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
            text-shadow: 0 0 10px rgba(0, 247, 255, 0.3);
        }
        
        h2 {
            font-size: 24px;
            border-bottom: 1px solid var(--tertiary-color);
            padding-bottom: 10px;
            margin-top: 40px;
        }
        
        .app-container {
            max-width: 1200px;
            margin: 0 auto;
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
            gap: 30px;
        }
        
        .screen {
            background-color: var(--card-bg-color);
            border-radius: 20px;
            padding: 20px;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.5);
            position: relative;
            overflow: hidden;
            transition: transform 0.3s ease, box-shadow 0.3s ease;
        }
        
        .screen:hover {
            transform: translateY(-5px);
            box-shadow: 0 10px 25px rgba(0, 0, 0, 0.7);
        }
        
        .screen::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            height: 3px;
            background: linear-gradient(90deg, var(--primary-color), var(--secondary-color), var(--tertiary-color));
            z-index: 1;
        }
        
        .screen-title {
            font-size: 18px;
            margin-bottom: 15px;
            color: var(--primary-color);
        }
        
        .screen-content {
            margin-bottom: 20px;
        }
        
        .screen-image {
            width: 100%;
            height: 500px;
            background-color: #1a1a2e;
            border-radius: 12px;
            margin-bottom: 15px;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            position: relative;
            overflow: hidden;
        }
        
        .screen-image::after {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: linear-gradient(135deg, rgba(0, 247, 255, 0.1), rgba(123, 0, 255, 0.1));
            pointer-events: none;
        }
        
        .nav-bar {
            position: absolute;
            bottom: 0;
            left: 0;
            right: 0;
            height: 60px;
            background-color: rgba(10, 10, 18, 0.8);
            backdrop-filter: blur(10px);
            display: flex;
            justify-content: space-around;
            align-items: center;
            border-top: 1px solid rgba(255, 255, 255, 0.1);
        }
        
        .nav-item {
            display: flex;
            flex-direction: column;
            align-items: center;
            color: var(--text-secondary);
            font-size: 10px;
        }
        
        .nav-item.active {
            color: var(--primary-color);
        }
        
        .nav-icon {
            width: 24px;
            height: 24px;
            margin-bottom: 4px;
            background-color: currentColor;
            mask-size: contain;
            mask-repeat: no-repeat;
            mask-position: center;
            -webkit-mask-size: contain;
            -webkit-mask-repeat: no-repeat;
            -webkit-mask-position: center;
        }
        
        .home-icon {
            mask-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24'%3E%3Cpath d='M10 20v-6h4v6h5v-8h3L12 3 2 12h3v8z'/%3E%3C/svg%3E");
            -webkit-mask-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24'%3E%3Cpath d='M10 20v-6h4v6h5v-8h3L12 3 2 12h3v8z'/%3E%3C/svg%3E");
        }
        
        .explore-icon {
            mask-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24'%3E%3Cpath d='M12 10.9c-.61 0-1.1.49-1.1 1.1s.49 1.1 1.1 1.1c.61 0 1.1-.49 1.1-1.1s-.49-1.1-1.1-1.1zM12 2C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2zm2.19 12.19L6 18l3.81-8.19L18 6l-3.81 8.19z'/%3E%3C/svg%3E");
            -webkit-mask-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24'%3E%3Cpath d='M12 10.9c-.61 0-1.1.49-1.1 1.1s.49 1.1 1.1 1.1c.61 0 1.1-.49 1.1-1.1s-.49-1.1-1.1-1.1zM12 2C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2zm2.19 12.19L6 18l3.81-8.19L18 6l-3.81 8.19z'/%3E%3C/svg%3E");
        }
        
        .upload-icon {
            mask-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24'%3E%3Cpath d='M19.35 10.04C18.67 6.59 15.64 4 12 4 9.11 4 6.6 5.64 5.35 8.04 2.34 8.36 0 10.91 0 14c0 3.31 2.69 6 6 6h13c2.76 0 5-2.24 5-5 0-2.64-2.05-4.78-4.65-4.96zM14 13v4h-4v-4H7l5-5 5 5h-3z'/%3E%3C/svg%3E");
            -webkit-mask-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24'%3E%3Cpath d='M19.35 10.04C18.67 6.59 15.64 4 12 4 9.11 4 6.6 5.64 5.35 8.04 2.34 8.36 0 10.91 0 14c0 3.31 2.69 6 6 6h13c2.76 0 5-2.24 5-5 0-2.64-2.05-4.78-4.65-4.96zM14 13v4h-4v-4H7l5-5 5 5h-3z'/%3E%3C/svg%3E");
        }
        
        .archive-icon {
            mask-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24'%3E%3Cpath d='M20 6h-8l-2-2H4c-1.1 0-1.99.9-1.99 2L2 18c0 1.1.9 2 2 2h16c1.1 0 2-.9 2-2V8c0-1.1-.9-2-2-2zm0 12H4V8h16v10z'/%3E%3C/svg%3E");
            -webkit-mask-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24'%3E%3Cpath d='M20 6h-8l-2-2H4c-1.1 0-1.99.9-1.99 2L2 18c0 1.1.9 2 2 2h16c1.1 0 2-.9 2-2V8c0-1.1-.9-2-2-2zm0 12H4V8h16v10z'/%3E%3C/svg%3E");
        }
        
        .profile-icon {
            mask-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24'%3E%3Cpath d='M12 12c2.21 0 4-1.79 4-4s-1.79-4-4-4-4 1.79-4 4 1.79 4 4 4zm0 2c-2.67 0-8 1.34-8 4v2h16v-2c0-2.66-5.33-4-8-4z'/%3E%3C/svg%3E");
            -webkit-mask-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24'%3E%3Cpath d='M12 12c2.21 0 4-1.79 4-4s-1.79-4-4-4-4 1.79-4 4 1.79 4 4 4zm0 2c-2.67 0-8 1.34-8 4v2h16v-2c0-2.66-5.33-4-8-4z'/%3E%3C/svg%3E");
        }
        
        .button {
            background: linear-gradient(90deg, var(--primary-color), var(--tertiary-color));
            color: var(--text-color);
            border: none;
            padding: 12px 24px;
            border-radius: 30px;
            font-family: 'Jura', sans-serif;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
            display: inline-block;
            text-align: center;
            margin: 10px 0;
            box-shadow: 0 0 15px rgba(0, 247, 255, 0.3);
        }
        
        .button:hover {
            transform: translateY(-2px);
            box-shadow: 0 0 20px rgba(0, 247, 255, 0.5);
        }
        
        .button-secondary {
            background: transparent;
            border: 2px solid var(--primary-color);
            color: var(--primary-color);
            box-shadow: none;
        }
        
        .button-secondary:hover {
            background: rgba(0, 247, 255, 0.1);
            box-shadow: 0 0 15px rgba(0, 247, 255, 0.2);
        }
        
        .artwork-card {
            background-color: rgba(20, 20, 31, 0.8);
            border-radius: 12px;
            padding: 15px;
            margin-bottom: 15px;
            border: 1px solid rgba(123, 0, 255, 0.3);
            transition: all 0.3s ease;
        }
        
        .artwork-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 5px 15px rgba(123, 0, 255, 0.2);
        }
        
        .artwork-image {
            width: 100%;
            height: 180px;
            background-color: #1a1a2e;
            border-radius: 8px;
            margin-bottom: 10px;
            background-size: cover;
            background-position: center;
            position: relative;
        }
        
        .artwork-title {
            font-size: 16px;
            font-weight: 600;
            margin-bottom: 5px;
            color: var(--text-color);
        }
        
        .artwork-author {
            font-size: 14px;
            color: var(--text-secondary);
            margin-bottom: 10px;
        }
        
        .artwork-stats {
            display: flex;
            justify-content: space-between;
            font-size: 12px;
            color: var(--text-secondary);
        }
        
        .artwork-stat {
            display: flex;
            align-items: center;
        }
        
        .artwork-stat-icon {
            width: 16px;
            height: 16px;
            margin-right: 5px;
            background-color: currentColor;
            mask-size: contain;
            mask-repeat: no-repeat;
            mask-position: center;
            -webkit-mask-size: contain;
            -webkit-mask-repeat: no-repeat;
            -webkit-mask-position: center;
        }
        
        .like-icon {
            mask-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24'%3E%3Cpath d='M12 21.35l-1.45-1.32C5.4 15.36 2 12.28 2 8.5 2 5.42 4.42 3 7.5 3c1.74 0 3.41.81 4.5 2.09C13.09 3.81 14.76 3 16.5 3 19.58 3 22 5.42 22 8.5c0 3.78-3.4 6.86-8.55 11.54L12 21.35z'/%3E%3C/svg%3E");
            -webkit-mask-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24'%3E%3Cpath d='M12 21.35l-1.45-1.32C5.4 15.36 2 12.28 2 8.5 2 5.42 4.42 3 7.5 3c1.74 0 3.41.81 4.5 2.09C13.09 3.81 14.76 3 16.5 3 19.58 3 22 5.42 22 8.5c0 3.78-3.4 6.86-8.55 11.54L12 21.35z'/%3E%3C/svg%3E");
        }
        
        .comment-icon {
            mask-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24'%3E%3Cpath d='M21.99 4c0-1.1-.89-2-1.99-2H4c-1.1 0-2 .9-2 2v12c0 1.1.9 2 2 2h14l4 4-.01-18z'/%3E%3C/svg%3E");
            -webkit-mask-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24'%3E%3Cpath d='M21.99 4c0-1.1-.89-2-1.99-2H4c-1.1 0-2 .9-2 2v12c0 1.1.9 2 2 2h14l4 4-.01-18z'/%3E%3C/svg%3E");
        }
        
        .share-icon {
            mask-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24'%3E%3Cpath d='M18 16.08c-.76 0-1.44.3-1.96.77L8.91 12.7c.05-.23.09-.46.09-.7s-.04-.47-.09-.7l7.05-4.11c.54.5 1.25.81 2.04.81 1.66 0 3-1.34 3-3s-1.34-3-3-3-3 1.34-3 3c0 .24.04.47.09.7L8.04 9.81C7.5 9.31 6.79 9 6 9c-1.66 0-3 1.34-3 3s1.34 3 3 3c.79 0 1.5-.31 2.04-.81l7.12 4.16c-.05.21-.08.43-.08.65 0 1.61 1.31 2.92 2.92 2.92 1.61 0 2.92-1.31 2.92-2.92s-1.31-2.92-2.92-2.92z'/%3E%3C/svg%3E");
            -webkit-mask-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24'%3E%3Cpath d='M18 16.08c-.76 0-1.44.3-1.96.77L8.91 12.7c.05-.23.09-.46.09-.7s-.04-.47-.09-.7l7.05-4.11c.54.5 1.25.81 2.04.81 1.66 0 3-1.34 3-3s-1.34-3-3-3-3 1.34-3 3c0 .24.04.47.09.7L8.04 9.81C7.5 9.31 6.79 9 6 9c-1.66 0-3 1.34-3 3s1.34 3 3 3c.79 0 1.5-.31 2.04-.81l7.12 4.16c-.05.21-.08.43-.08.65 0 1.61 1.31 2.92 2.92 2.92 1.61 0 2.92-1.31 2.92-2.92s-1.31-2.92-2.92-2.92z'/%3E%3C/svg%3E");
        }
        
        .input-field {
            background-color: rgba(20, 20, 31, 0.8);
            border: 1px solid rgba(123, 0, 255, 0.3);
            border-radius: 8px;
            padding: 12px 15px;
            font-family: 'Jura', sans-serif;
            color: var(--text-color);
            width: 100%;
            margin-bottom: 15px;
            transition: all 0.3s ease;
        }
        
        .input-field:focus {
            outline: none;
            border-color: var(--primary-color);
            box-shadow: 0 0 10px rgba(0, 247, 255, 0.2);
        }
        
        .upload-option {
            display: flex;
            align-items: center;
            background-color: rgba(20, 20, 31, 0.8);
            border: 1px solid rgba(123, 0, 255, 0.3);
            border-radius: 12px;
            padding: 15px;
            margin-bottom: 15px;
            cursor: pointer;
            transition: all 0.3s ease;
        }
        
        .upload-option:hover {
            background-color: rgba(30, 30, 45, 0.8);
            border-color: var(--primary-color);
            transform: translateY(-2px);
        }
        
        .upload-option-icon {
            width: 40px;
            height: 40px;
            margin-right: 15px;
            background-color: var(--primary-color);
            border-radius: 50%;
            display: flex;
            justify-content: center;
            align-items: center;
        }
        
        .upload-option-text {
            flex: 1;
        }
        
        .upload-option-title {
            font-weight: 600;
            margin-bottom: 5px;
        }
        
        .upload-option-description {
            font-size: 14px;
            color: var(--text-secondary);
        }
        
        .progress-bar {
            width: 100%;
            height: 6px;
            background-color: rgba(20, 20, 31, 0.8);
            border-radius: 3px;
            overflow: hidden;
            margin: 20px 0;
        }
        
        .progress-bar-fill {
            height: 100%;
            background: linear-gradient(90deg, var(--primary-color), var(--tertiary-color));
            width: 75%;
            border-radius: 3px;
            animation: pulse 2s infinite;
        }
        
        @keyframes pulse {
            0% {
                opacity: 1;
            }
            50% {
                opacity: 0.7;
            }
            100% {
                opacity: 1;
            }
        }
        
        .tab-container {
            display: flex;
            margin-bottom: 20px;
            border-bottom: 1px solid rgba(123, 0, 255, 0.3);
        }
        
        .tab {
            padding: 10px 20px;
            cursor: pointer;
            color: var(--text-secondary);
            position: relative;
            transition: all 0.3s ease;
        }
        
        .tab.active {
            color: var(--primary-color);
        }
        
        .tab.active::after {
            content: '';
            position: absolute;
            bottom: -1px;
            left: 0;
            right: 0;
            height: 2px;
            background: linear-gradient(90deg, var(--primary-color), var(--tertiary-color));
        }
        
        .memory-item {
            display: flex;
            background-color: rgba(20, 20, 31, 0.8);
            border-radius: 12px;
            padding: 15px;
            margin-bottom: 15px;
            border: 1px solid rgba(123, 0, 255, 0.3);
            transition: all 0.3s ease;
        }
        
        .memory-item:hover {
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(123, 0, 255, 0.2);
        }
        
        .memory-thumbnail {
            width: 80px;
            height: 80px;
            border-radius: 8px;
            margin-right: 15px;
            background-color: #1a1a2e;
            background-size: cover;
            background-position: center;
            flex-shrink: 0;
        }
        
        .memory-content {
            flex: 1;
        }
        
        .memory-title {
            font-weight: 600;
            margin-bottom: 5px;
        }
        
        .memory-date {
            font-size: 12px;
            color: var(--text-secondary);
            margin-bottom: 10px;
        }
        
        .memory-description {
            font-size: 14px;
            color: var(--text-color);
            margin-bottom: 10px;
        }
        
        .memory-actions {
            display: flex;
            justify-content: flex-end;
        }
        
        .memory-action {
            color: var(--text-secondary);
            font-size: 12px;
            margin-left: 15px;
            cursor: pointer;
            transition: all 0.3s ease;
        }
        
        .memory-action:hover {
            color: var(--primary-color);
        }
        
        .exhibition-card {
            background-color: rgba(20, 20, 31, 0.8);
            border-radius: 12px;
            overflow: hidden;
            margin-bottom: 20px;
            border: 1px solid rgba(123, 0, 255, 0.3);
            transition: all 0.3s ease;
        }
        
        .exhibition-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 5px 15px rgba(123, 0, 255, 0.2);
        }
        
        .exhibition-image {
            width: 100%;
            height: 180px;
            background-color: #1a1a2e;
            background-size: cover;
            background-position: center;
        }
        
        .exhibition-content {
            padding: 15px;
        }
        
        .exhibition-title {
            font-weight: 600;
            margin-bottom: 5px;
        }
        
        .exhibition-date {
            font-size: 14px;
            color: var(--text-secondary);
            margin-bottom: 10px;
        }
        
        .exhibition-description {
            font-size: 14px;
            color: var(--text-color);
            margin-bottom: 15px;
        }
        
        .badge {
            display: inline-block;
            padding: 5px 10px;
            border-radius: 15px;
            font-size: 12px;
            font-weight: 600;
            margin-right: 5px;
            margin-bottom: 5px;
        }
        
        .badge-current {
            background-color: var(--primary-color);
            color: var(--background-color);
        }
        
        .badge-past {
            background-color: rgba(123, 0, 255, 0.3);
            color: var(--text-color);
        }
        
        .badge-upcoming {
            background-color: var(--warning-color);
            color: var(--background-color);
        }
        
        .comment-section {
            margin-top: 20px;
        }
        
        .comment {
            background-color: rgba(20, 20, 31, 0.8);
            border-radius: 12px;
            padding: 15px;
            margin-bottom: 15px;
            border: 1px solid rgba(123, 0, 255, 0.3);
        }
        
        .comment-header {
            display: flex;
            align-items: center;
            margin-bottom: 10px;
        }
        
        .comment-avatar {
            width: 30px;
            height: 30px;
            border-radius: 50%;
            background-color: #1a1a2e;
            margin-right: 10px;
            background-size: cover;
            background-position: center;
        }
        
        .comment-author {
            font-weight: 600;
            margin-right: 10px;
        }
        
        .comment-time {
            font-size: 12px;
            color: var(--text-secondary);
        }
        
        .comment-text {
            font-size: 14px;
            color: var(--text-color);
        }
        
        .notification {
            position: fixed;
            top: 20px;
            right: 20px

            .notification {
                position: fixed;
                top: 20px;
                right: 20px;
                background-color: rgba(20, 20, 31, 0.9);
                border-left: 4px solid var(--primary-color);
                padding: 15px 20px;
                border-radius: 8px;
                box-shadow: 0 5px 15px rgba(0, 0, 0, 0.5);
                z-index: 1000;
                transform: translateX(120%);
                transition: transform 0.3s ease;
                backdrop-filter: blur(10px);
            }
            
            .notification.show {
                transform: translateX(0);
            }
            
            .notification-title {
                font-weight: 600;
                margin-bottom: 5px;
            }
            
            .notification-message {
                font-size: 14px;
                color: var(--text-secondary);
            }
            
            .floating-button {
                position: fixed;
                bottom: 80px;
                right: 20px;
                width: 60px;
                height: 60px;
                border-radius: 50%;
                background: linear-gradient(135deg, var(--primary-color), var(--tertiary-color));
                display: flex;
                justify-content: center;
                align-items: center;
                box-shadow: 0 5px 15px rgba(0, 247, 255, 0.3);
                cursor: pointer;
                transition: all 0.3s ease;
                z-index: 100;
            }
            
            .floating-button:hover {
                transform: translateY(-5px);
                box-shadow: 0 10px 20px rgba(0, 247, 255, 0.5);
            }
            
            .floating-button-icon {
                width: 24px;
                height: 24px;
                background-color: white;
                mask-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24'%3E%3Cpath d='M19 13h-6v6h-2v-6H5v-2h6V5h2v6h6v2z'/%3E%3C/svg%3E");
                mask-size: contain;
                mask-repeat: no-repeat;
                mask-position: center;
                -webkit-mask-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24'%3E%3Cpath d='M19 13h-6v6h-2v-6H5v-2h6V5h2v6h6v2z'/%3E%3C/svg%3E");
                -webkit-mask-size: contain;
                -webkit-mask-repeat: no-repeat;
                -webkit-mask-position: center;
            }
            
            .status-bar {
                position: absolute;
                top: 0;
                left: 0;
                right: 0;
                height: 44px;
                background-color: rgba(10, 10, 18, 0.8);
                backdrop-filter: blur(10px);
                display: flex;
                justify-content: space-between;
                align-items: center;
                padding: 0 15px;
                z-index: 10;
            }
            
            .status-time {
                font-weight: 600;
            }
            
            .status-icons {
                display: flex;
            }
            
            .status-icon {
                width: 18px;
                height: 18px;
                margin-left: 8px;
                background-color: currentColor;
                mask-size: contain;
                mask-repeat: no-repeat;
                mask-position: center;
                -webkit-mask-size: contain;
                -webkit-mask-repeat: no-repeat;
                -webkit-mask-position: center;
            }
            
            .battery-icon {
                mask-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24'%3E%3Cpath d='M15.67 4H14V2h-4v2H8.33C7.6 4 7 4.6 7 5.33v15.33C7 21.4 7.6 22 8.33 22h7.33c.74 0 1.34-.6 1.34-1.33V5.33C17 4.6 16.4 4 15.67 4z'/%3E%3C/svg%3E");
                -webkit-mask-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24'%3E%3Cpath d='M15.67 4H14V2h-4v2H8.33C7.6 4 7 4.6 7 5.33v15.33C7 21.4 7.6 22 8.33 22h7.33c.74 0 1.34-.6 1.34-1.33V5.33C17 4.6 16.4 4 15.67 4z'/%3E%3C/svg%3E");
            }
            
            .wifi-icon {
                mask-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24'%3E%3Cpath d='M1 9l2 2c4.97-4.97 13.03-4.97 18 0l2-2C16.93 2.93 7.08 2.93 1 9zm8 8l3 3 3-3c-1.65-1.66-4.34-1.66-6 0zm-4-4l2 2c2.76-2.76 7.24-2.76 10 0l2-2C15.14 9.14 8.87 9.14 5 13z'/%3E%3C/svg%3E");
                -webkit-mask-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24'%3E%3Cpath d='M1 9l2 2c4.97-4.97 13.03-4.97 18 0l2-2C16.93 2.93 7.08 2.93 1 9zm8 8l3 3 3-3c-1.65-1.66-4.34-1.66-6 0zm-4-4l2 2c2.76-2.76 7.24-2.76 10 0l2-2C15.14 9.14 8.87 9.14 5 13z'/%3E%3C/svg%3E");
            }
            
            .signal-icon {
                mask-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24'%3E%3Cpath d='M2 22h20V2z'/%3E%3C/svg%3E");
                -webkit-mask-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24'%3E%3Cpath d='M2 22h20V2z'/%3E%3C/svg%3E");
            }
            
            .screen-content {
                padding-top: 44px;
                padding-bottom: 60px;
                height: 100%;
                overflow-y: auto;
            }
            
            @media (max-width: 768px) {
                .app-container {
                    grid-template-columns: 1fr;
                }
            }
        </style>
    </head>
    <body>
        <h1>TSG交互式艺术空间 - iOS应用原型</h1>
        
        <div class="app-container">
            <!-- 主页 -->
            <div class="screen">
                <div class="status-bar">
                    <div class="status-time">9:41</div>
                    <div class="status-icons">
                        <div class="status-icon signal-icon"></div>
                        <div class="status-icon wifi-icon"></div>
                        <div class="status-icon battery-icon"></div>
                    </div>
                </div>
                
                <div class="screen-content">
                    <h3 class="screen-title">主页</h3>
                    
                    <div class="exhibition-card">
                        <div class="exhibition-image" style="background-image: url('https://via.placeholder.com/400x200/1a1a2e/00f7ff?text=当前展览')"></div>
                        <div class="exhibition-content">
                            <div class="badge badge-current">当前展览</div>
                            <h4 class="exhibition-title">记忆之光：数字艺术与人类情感</h4>
                            <div class="exhibition-date">2023年10月15日 - 2023年12月30日</div>
                            <div class="exhibition-description">探索AI如何将人类记忆转化为视觉艺术，展示科技与情感的交融。</div>
                            <button class="button">了解更多</button>
                        </div>
                    </div>
                    
                    <div style="margin: 30px 0 20px 0; display: flex; justify-content: space-between; align-items: center;">
                        <h4>上传你的记忆</h4>
                        <button class="button">开始创作</button>
                    </div>
                    
                    <div style="margin: 30px 0 20px 0;">
                        <h4>热门AI艺术作品</h4>
                    </div>
                    
                    <div class="artwork-card">
                        <div class="artwork-image" style="background-image: url('https://via.placeholder.com/400x200/1a1a2e/ff00e6?text=AI艺术作品1')"></div>
                        <h4 class="artwork-title">夏日回忆</h4>
                        <div class="artwork-author">由 张明 创作</div>
                        <div class="artwork-stats">
                            <div class="artwork-stat">
                                <div class="artwork-stat-icon like-icon"></div>
                                <span>128</span>
                            </div>
                            <div class="artwork-stat">
                                <div class="artwork-stat-icon comment-icon"></div>
                                <span>32</span>
                            </div>
                            <div class="artwork-stat">
                                <div class="artwork-stat-icon share-icon"></div>
                                <span>14</span>
                            </div>
                        </div>
                    </div>
                    
                    <div class="artwork-card">
                        <div class="artwork-image" style="background-image: url('https://via.placeholder.com/400x200/1a1a2e/7b00ff?text=AI艺术作品2')"></div>
                        <h4 class="artwork-title">城市之梦</h4>
                        <div class="artwork-author">由 李华 创作</div>
                        <div class="artwork-stats">
                            <div class="artwork-stat">
                                <div class="artwork-stat-icon like-icon"></div>
                                <span>96</span>
                            </div>
                            <div class="artwork-stat">
                                <div class="artwork-stat-icon comment-icon"></div>
                                <span>18</span>
                            </div>
                            <div class="artwork-stat">
                                <div class="artwork-stat-icon share-icon"></div>
                                <span>7</span>
                            </div>
                        </div>
                    </div>
                </div>
                
                <div class="nav-bar">
                    <div class="nav-item active">
                        <div class="nav-icon home-icon"></div>
                        <span>主页</span>
                    </div>
                    <div class="nav-item">
                        <div class="nav-icon explore-icon"></div>
                        <span>探索</span>
                    </div>
                    <div class="nav-item">
                        <div class="nav-icon upload-icon"></div>
                        <span>上传</span>
                    </div>
                    <div class="nav-item">
                        <div class="nav-icon archive-icon"></div>
                        <span>档案</span>
                    </div>
                    <div class="nav-item">
                        <div class="nav-icon profile-icon"></div>
                        <span>我的</span>
                    </div>
                </div>
            </div>
            
            <!-- 记忆上传流程 - 步骤1 -->
            <div class="screen">
                <div class="status-bar">
                    <div class="status-time">9:41</div>
                    <div class="status-icons">
                        <div class="status-icon signal-icon"></div>
                        <div class="status-icon wifi-icon"></div>
                        <div class="status-icon battery-icon"></div>
                    </div>
                </div>
                
                <div class="screen-content">
                    <h3 class="screen-title">上传记忆</h3>
                    
                    <div style="margin-bottom: 20px;">
                        <h4>选择上传方式</h4>
                        <p style="color: var(--text-secondary); margin-bottom: 20px;">选择一种方式分享你的记忆，AI将为你创作独特的艺术作品</p>
                    </div>
                    
                    <div class="upload-option">
                        <div class="upload-option-icon">
                            <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                                <path d="M21 15a2 2 0 0 1-2 2H7l-4 4V5a2 2 0 0 1 2-2h14a2 2 0 0 1 2 2z"></path>
                            </svg>
                        </div>
                        <div class="upload-option-text">
                            <div class="upload-option-title">文字记忆</div>
                            <div class="upload-option-description">描述你的记忆，AI将根据你的文字创作艺术作品</div>
                        </div>
                    </div>
                    
                    <div class="upload-option">
                        <div class="upload-option-icon">
                            <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                                <rect x="3" y="3" width="18" height="18" rx="2" ry="2"></rect>
                                <circle cx="8.5" cy="8.5" r="1.5"></circle>
                                <polyline points="21 15 16 10 5 21"></polyline>
                            </svg>
                        </div>
                        <div class="upload-option-text">
                            <div class="upload-option-title">图片记忆</div>
                            <div class="upload-option-description">上传照片，AI将分析并创作相关艺术作品</div>
                        </div>
                    </div>
                    
                    <div class="upload-option">
                        <div class="upload-option-icon">
                            <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                                <path d="M12 1a3 3 0 0 0-3 3v8a3 3 0 0 0 6 0V4a3 3 0 0 0-3-3z"></path>
                                <path d="M19 10v2a7 7 0 0 1-14 0v-2"></path>
                                <line x1="12" y1="19" x2="12" y2="23"></line>
                                <line x1="8" y1="23" x2="16" y2="23"></line>
                            </svg>
                        </div>
                        <div class="upload-option-text">
                            <div class="upload-option-title">语音记忆</div>
                            <div class="upload-option-description">录制你的声音，AI将转换并创作艺术作品</div>
                        </div>
                    </div>
                </div>
                
                <div class="nav-bar">
                    <div class="nav-item">
                        <div class="nav-icon home-icon"></div>
                        <span>主页</span>
                    </div>
                    <div class="nav-item">
                        <div class="nav-icon explore-icon"></div>
                        <span>探索</span>
                    </div>
                    <div class="nav-item active">
                        <div class="nav-icon upload-icon"></div>
                        <span>上传</span>
                    </div>
                    <div class="nav-item">
                        <div class="nav-icon archive-icon"></div>
                        <span>档案</span>
                    </div>
                    <div class="nav-item">
                        <div class="nav-icon profile-icon"></div>
                        <span>我的</span>
                    </div>
                </div>
            </div>
            
            <!-- 记忆上传流程 - 步骤2（文字记忆） -->
            <div class="screen">
                <div class="status-bar">
                    <div class="status-time">9:41</div>
                    <div class="status-icons">
                        <div class="status-icon signal-icon"></div>
                        <div class="status-icon wifi-icon"></div>
                        <div class="status-icon battery-icon"></div>
                    </div>
                </div>
                
                <div class="screen-content">
                    <h3 class="screen-title">文字记忆</h3>
                    
                    <div style="margin-bottom: 20px;">
                        <h4>描述你的记忆</h4>
                        <p style="color: var(--text-secondary); margin-bottom: 20px;">详细描述你的记忆，包括情感、场景和重要细节</p>
                    </div>
                    
                    <textarea class="input-field" style="height: 200px; resize: none;" placeholder="例如：那是我童年最美好的夏天，阳光透过树叶洒在小溪上，我和朋友们在水中嬉戏，空气中弥漫着青草和野花的香气..."></textarea>
                    
                    <div style="margin-bottom: 20px;">
                        <h4>记忆标题</h4>
                    </div>
                    
                    <input type="text" class="input-field" placeholder="给你的记忆起个名字">
                    
                    <div style="margin-bottom: 20px;">
                        <h4>艺术风格偏好（可选）</h4>
                    </div>
                    
                    <select class="input-field">
                        <option value="">选择艺术风格（可选）</option>
                        <option value="impressionism">印象派</option>
                        <option value="surrealism">超现实主义</option>
                        <option value="abstract">抽象艺术</option>
                        <option value="digital">数字艺术</option>
                        <option value="cyberpunk">赛博朋克</option>
                    </select>
                    
                    <button class="button" style="width: 100%;">创建艺术作品</button>
                </div>
                
                <div class="nav-bar">
                    <div class="nav-item">
                        <div class="nav-icon home-icon"></div>
                        <span>主页</span>
                    </div>
                    <div class="nav-item">
                        <div class="nav-icon explore-icon"></div>
                        <span>探索</span>
                    </div>
                    <div class="nav-item active">
                        <div class="nav-icon upload-icon"></div>
                        <span>上传</span>
                    </div>
                    <div class="nav-item">
                        <div class="nav-icon archive-icon"></div>
                        <span>档案</span>
                    </div>
                    <div class="nav-item">
                        <div class="nav-icon profile-icon"></div>
                        <span>我的</span>
                    </div>
                </div>
            </div>
            
            <!-- 记忆上传流程 - 步骤3（生成中） -->
            <div class="screen">
                <div class="status-bar">
                    <div class="status-time">9:41</div>
                    <div class="status-icons">
                        <div class="status-icon signal-icon"></div>
                        <div class="status-icon wifi-icon"></div>
                        <div class="status-icon battery-icon"></div>
                    </div>
                </div>
                
                <div class="screen-content" style="display: flex; flex-direction: column; justify-content: center; align-items: center; text-align: center;">
                    <h3 class="screen-title">AI正在创作中</h3>
                    
                    <div style="width: 150px; height: 150px; margin: 30px 0; position: relative;">
                        <svg viewBox="0 0 100 100" xmlns="http://www.w3.org/2000/svg" style="width: 100%; height: 100%;">
                            <circle cx="50" cy="50" r="45" fill="none" stroke="rgba(123, 0, 255, 0.2)" stroke-width="8" />
                            <circle cx="50" cy="50" r="45" fill="none" stroke="url(#gradient)" stroke-width="8" stroke-dasharray="283" stroke-dashoffset="100" transform="rotate(-90 50 50)">
                                <animate attributeName="stroke-dashoffset" from="283" to="0" dur="3s" repeatCount="indefinite" />
                            </circle>
                            <defs>
                                <linearGradient id="gradient" x1="0%" y1="0%" x2="100%" y2="0%">
                                    <stop offset="0%" stop-color="var(--primary-color)" />
                                    <stop offset="100%" stop-color="var(--tertiary-color)" />
                                </linearGradient>
                            </defs>
                        </svg>
                    </div>
                    
                    <p style="margin-bottom: 30px; max-width: 80%;">AI正在分析你的记忆并创作独特的艺术作品，这可能需要几分钟时间</p>
                    
                    <div class="progress-bar">
                        <div class="progress-bar-fill"></div>
                    </div>
                    
                    <p style="color: var(--text-secondary); font-size: 14px;">75% 完成</p>
                </div>
                
                <div class="nav-bar">
                    <div class="nav-item">
                        <div class="nav-icon home-icon"></div>
                        <span>主页</span>
                    </div>
                    <div class="nav-item">
                        <div class="nav-icon explore-icon"></div>
                        <span>探索</span>
                    </div>
                    <div class="nav-item active">
                        <div class="nav-icon upload-icon"></div>
                        <span>上传</span>
                    </div>
                    <div class="nav-item">
                        <div class="nav-icon archive-icon"></div>
                        <span>档案</span>
                    </div>
                    <div class="nav-item">
                        <div class="nav-icon profile-icon"></div>
                        <span>我的</span>
                    </div>
                </div>
            </div>
            
            <!-- 记忆上传流程 - 步骤4（结果） -->
            <div class="screen">
                <div class="status-bar">
                    <div class="status-time">9:41</div>
                    <div class="status-icons">
                        <div class="status-icon signal-icon"></div>
                        <div class="status-icon wifi-icon"></div>
                        <div class="status-icon battery-icon"></div>
                    </div>
                </div>
                
                <div class="screen-content">
                    <h3 class="screen-title">艺术作品已创建</h3>
                    
                    <div style="margin-bottom: 20px;">
                        <h4>夏日回忆</h4>
                        <p style="color: var(--text-secondary); margin-bottom: 20px;">基于你的记忆创作的艺术作品</p>
                    </div>
                    
                    <div style="width: 100%; height: 300px; background-color: #1a1a2e; border-radius: 12px; margin-bottom: 20px; background-image: url('https://via.placeholder.com/400x300/1a1a2e/00f7ff?text=AI生成的艺术作品'); background-size: cover; background-position: center;"></div>
                    
                    <div style="margin-bottom: 20px;">
                        <h4>AI解读</h4>
                        <p style="color: var(--text-color); margin-bottom: 20px; font-size: 14px;">这幅作品捕捉了童年夏日的纯真与欢乐，通过明亮的色彩和流动的线条表现了阳光、小溪和嬉戏的场景。作品中融入了自然元素和情感记忆，创造出一种怀旧而温暖的氛围。</p>
                    </div>
                    
                    <div style="display: flex; gap: 10px; margin-bottom: 20px;">
                        <button class="button" style="flex: 1;">分享作品</button>
                        <button class="button button-secondary" style="flex: 1;">编辑描述</button>
                    </div>
                    
                    <div style="display: flex; gap: 10px;">
                        <button class="button button-secondary" style="flex: 1;">重新生成</button>
                        <button class="button" style="flex: 1;">保存到我的记忆</button>
                    </div>
                </div>
                
                <div class="nav-bar">
                    <div class="nav-item">
                        <div class="nav-icon home-icon"></div>
                        <span>主页</span>
                    </div>
                    <div class="nav-item">
                        <div class="nav-icon explore-icon"></div>
                        <span>探索</span>
                    </div>
                    <div class="nav-item active">
                        <div class="nav-icon upload-icon"></div>
                        <span>上传</span>
                    </div>
                    <div class="nav-item">
                        <div class="nav-icon archive-icon"></div>
                        <span>档案</span>
                    </div>
                    <div class="nav-item">
                        <div class="nav-icon profile-icon"></div>
                        <span>我的</span>
                    </div>
                </div>
            </div>
            
            <!-- 社区互动 -->
            <div class="screen">
                <div class="status-bar">
                    <div class="status-time">9:41</div>
                    <div class="status-icons">
                        <div class="status-icon signal-icon"></div>
                        <div class="status-icon wifi-icon"></div>
                        <div class="status-icon battery-icon"></div>
                    </div>
                </div>
                
                <div class="screen-content">
                    <h3 class="screen-title">探索</h3>
                    
                    <div class="tab-container">
                        <div class="tab active">热门</div>
                        <div class="tab">最新</div>
                        <div class="tab">关注</div>
                    </div>
                    
                    <div class="artwork-card">
                        <div class="artwork-image" style="background-image: url('https://via.placeholder.com/400x200/1a1a2e/ff00e6?text=AI艺术作品1')"></div>
                        <h4 class="artwork-title">夏日回忆</h4>
                        <div class="artwork-author">由 张明 创作</div>
                        <div class="artwork-stats">
                            <div class="artwork-stat">
                                <div class="artwork-stat-icon like-icon"></div>
                                <span>128</span>
                            </div>
                            <div class="artwork-stat">
                                <div class="artwork-stat-icon comment-icon"></div>
                                <span>32</span>
                            </div>
                            <div class="artwork-stat">
                                <div class="artwork-stat-icon share-icon"></div>
                                <span>14</span>
                            </div>
                        </div>
                    </div>
                    
                    <div class="artwork-card">
                        <div class="artwork-image" style="background-image: url('https://via.placeholder.com/400x200/1a1a2e/7b00ff?text=AI艺术作品2')"></div>
                        <h4 class="artwork-title">城市
                        <h4 class="artwork-title">城市之梦</h4>
                        <div class="artwork-author">由 李华 创作</div>
                        <div class="artwork-stats">
                            <div class="artwork-stat">
                                <div class="artwork-stat-icon like-icon"></div>
                                <span>96</span>
                            </div>
                            <div class="artwork-stat">
                                <div class="artwork-stat-icon comment-icon"></div>
                                <span>18</span>
                            </div>
                            <div class="artwork-stat">
                                <div class="artwork-stat-icon share-icon"></div>
                                <span>7</span>
                            </div>
                        </div>
                    </div>
                    
                    <div class="artwork-card">
                        <div class="artwork-image" style="background-image: url('https://via.placeholder.com/400x200/1a1a2e/00f7ff?text=AI艺术作品3')"></div>
                        <h4 class="artwork-title">星空下的约定</h4>
                        <div class="artwork-author">由 王芳 创作</div>
                        <div class="artwork-stats">
                            <div class="artwork-stat">
                                <div class="artwork-stat-icon like-icon"></div>
                                <span>76</span>
                            </div>
                            <div class="artwork-stat">
                                <div class="artwork-stat-icon comment-icon"></div>
                                <span>12</span>
                            </div>
                            <div class="artwork-stat">
                                <div class="artwork-stat-icon share-icon"></div>
                                <span>5</span>
                            </div>
                        </div>
                    </div>
                </div>
                
                <div class="nav-bar">
                    <div class="nav-item">
                        <div class="nav-icon home-icon"></div>
                        <span>主页</span>
                    </div>
                    <div class="nav-item active">
                        <div class="nav-icon explore-icon"></div>
                        <span>探索</span>
                    </div>
                    <div class="nav-item">
                        <div class="nav-icon upload-icon"></div>
                        <span>上传</span>
                    </div>
                    <div class="nav-item">
                        <div class="nav-icon archive-icon"></div>
                        <span>档案</span>
                    </div>
                    <div class="nav-item">
                        <div class="nav-icon profile-icon"></div>
                        <span>我的</span>
                    </div>
                </div>
            </div>
            
            <!-- 艺术作品详情 -->
            <div class="screen">
                <div class="status-bar">
                    <div class="status-time">9:41</div>
                    <div class="status-icons">
                        <div class="status-icon signal-icon"></div>
                        <div class="status-icon wifi-icon"></div>
                        <div class="status-icon battery-icon"></div>
                    </div>
                </div>
                
                <div class="screen-content">
                    <h3 class="screen-title">作品详情</h3>
                    
                    <div style="width: 100%; height: 250px; background-color: #1a1a2e; border-radius: 12px; margin-bottom: 20px; background-image: url('https://via.placeholder.com/400x250/1a1a2e/ff00e6?text=AI艺术作品1'); background-size: cover; background-position: center;"></div>
                    
                    <div style="margin-bottom: 20px;">
                        <h4 class="artwork-title">夏日回忆</h4>
                        <div class="artwork-author">由 张明 创作 · 2023年10月18日</div>
                    </div>
                    
                    <div style="margin-bottom: 20px;">
                        <p style="color: var(--text-color); font-size: 14px;">这是我童年最美好的夏天记忆，阳光透过树叶洒在小溪上，我和朋友们在水中嬉戏，空气中弥漫着青草和野花的香气。那时的快乐是如此纯粹，没有任何烦恼和忧愁。</p>
                    </div>
                    
                    <div style="display: flex; gap: 10px; margin-bottom: 30px;">
                        <button class="button button-secondary" style="flex: 1;">
                            <div style="display: flex; align-items: center; justify-content: center;">
                                <div class="artwork-stat-icon like-icon" style="margin-right: 5px;"></div>
                                <span>喜欢</span>
                            </div>
                        </button>
                        <button class="button button-secondary" style="flex: 1;">
                            <div style="display: flex; align-items: center; justify-content: center;">
                                <div class="artwork-stat-icon share-icon" style="margin-right: 5px;"></div>
                                <span>分享</span>
                            </div>
                        </button>
                    </div>
                    
                    <div class="comment-section">
                        <h4 style="margin-bottom: 15px;">评论 (32)</h4>
                        
                        <div class="comment">
                            <div class="comment-header">
                                <div class="comment-avatar" style="background-image: url('https://via.placeholder.com/50/1a1a2e/00f7ff?text=L')"></div>
                                <div class="comment-author">刘小明</div>
                                <div class="comment-time">2小时前</div>
                            </div>
                            <div class="comment-text">这幅作品真的很打动人，让我想起了自己的童年。色彩运用非常巧妙！</div>
                        </div>
                        
                        <div class="comment">
                            <div class="comment-header">
                                <div class="comment-avatar" style="background-image: url('https://via.placeholder.com/50/1a1a2e/ff00e6?text=Z')"></div>
                                <div class="comment-author">赵雪</div>
                                <div class="comment-time">5小时前</div>
                            </div>
                            <div class="comment-text">AI的创作能力真是惊人，能够如此准确地捕捉情感。期待看到更多类似的作品！</div>
                        </div>
                        
                        <div class="comment">
                            <div class="comment-header">
                                <div class="comment-avatar" style="background-image: url('https://via.placeholder.com/50/1a1a2e/7b00ff?text=W')"></div>
                                <div class="comment-author">王大力</div>
                                <div class="comment-time">昨天</div>
                            </div>
                            <div class="comment-text">这种风格很特别，光影效果处理得非常好。能分享一下你选择的是什么艺术风格吗？</div>
                        </div>
                        
                        <div style="margin-top: 20px;">
                            <input type="text" class="input-field" placeholder="添加评论...">
                            <button class="button" style="width: 100%;">发送</button>
                        </div>
                    </div>
                </div>
                
                <div class="nav-bar">
                    <div class="nav-item">
                        <div class="nav-icon home-icon"></div>
                        <span>主页</span>
                    </div>
                    <div class="nav-item active">
                        <div class="nav-icon explore-icon"></div>
                        <span>探索</span>
                    </div>
                    <div class="nav-item">
                        <div class="nav-icon upload-icon"></div>
                        <span>上传</span>
                    </div>
                    <div class="nav-item">
                        <div class="nav-icon archive-icon"></div>
                        <span>档案</span>
                    </div>
                    <div class="nav-item">
                        <div class="nav-icon profile-icon"></div>
                        <span>我的</span>
                    </div>
                </div>
            </div>
            
            <!-- 个人记忆档案 -->
            <div class="screen">
                <div class="status-bar">
                    <div class="status-time">9:41</div>
                    <div class="status-icons">
                        <div class="status-icon signal-icon"></div>
                        <div class="status-icon wifi-icon"></div>
                        <div class="status-icon battery-icon"></div>
                    </div>
                </div>
                
                <div class="screen-content">
                    <h3 class="screen-title">我的记忆</h3>
                    
                    <div class="tab-container">
                        <div class="tab active">我的作品</div>
                        <div class="tab">收藏</div>
                        <div class="tab">草稿</div>
                    </div>
                    
                    <div class="memory-item">
                        <div class="memory-thumbnail" style="background-image: url('https://via.placeholder.com/100/1a1a2e/00f7ff?text=记忆1')"></div>
                        <div class="memory-content">
                            <div class="memory-title">夏日回忆</div>
                            <div class="memory-date">2023年10月18日</div>
                            <div class="memory-description">童年夏天的美好记忆，阳光、小溪和嬉戏...</div>
                            <div class="memory-actions">
                                <div class="memory-action">编辑</div>
                                <div class="memory-action">隐藏</div>
                                <div class="memory-action">删除</div>
                            </div>
                        </div>
                    </div>
                    
                    <div class="memory-item">
                        <div class="memory-thumbnail" style="background-image: url('https://via.placeholder.com/100/1a1a2e/ff00e6?text=记忆2')"></div>
                        <div class="memory-content">
                            <div class="memory-title">毕业旅行</div>
                            <div class="memory-date">2023年9月5日</div>
                            <div class="memory-description">大学毕业后与朋友们的最后一次旅行，充满欢笑和不舍...</div>
                            <div class="memory-actions">
                                <div class="memory-action">编辑</div>
                                <div class="memory-action">隐藏</div>
                                <div class="memory-action">删除</div>
                            </div>
                        </div>
                    </div>
                    
                    <div class="memory-item">
                        <div class="memory-thumbnail" style="background-image: url('https://via.placeholder.com/100/1a1a2e/7b00ff?text=记忆3')"></div>
                        <div class="memory-content">
                            <div class="memory-title">初雪</div>
                            <div class="memory-date">2023年8月12日</div>
                            <div class="memory-description">第一次看到雪的兴奋和惊喜，白色的世界如此纯净...</div>
                            <div class="memory-actions">
                                <div class="memory-action">编辑</div>
                                <div class="memory-action">隐藏</div>
                                <div class="memory-action">删除</div>
                            </div>
                        </div>
                    </div>
                    
                    <div class="memory-item">
                        <div class="memory-thumbnail" style="background-image: url('https://via.placeholder.com/100/1a1a2e/00f7ff?text=记忆4')"></div>
                        <div class="memory-content">
                            <div class="memory-title">家庭聚会</div>
                            <div class="memory-date">2023年7月30日</div>
                            <div class="memory-description">久违的家庭团聚，餐桌上的欢声笑语和美食香气...</div>
                            <div class="memory-actions">
                                <div class="memory-action">编辑</div>
                                <div class="memory-action">隐藏</div>
                                <div class="memory-action">删除</div>
                            </div>
                        </div>
                    </div>
                </div>
                
                <div class="floating-button">
                    <div class="floating-button-icon"></div>
                </div>
                
                <div class="nav-bar">
                    <div class="nav-item">
                        <div class="nav-icon home-icon"></div>
                        <span>主页</span>
                    </div>
                    <div class="nav-item">
                        <div class="nav-icon explore-icon"></div>
                        <span>探索</span>
                    </div>
                    <div class="nav-item">
                        <div class="nav-icon upload-icon"></div>
                        <span>上传</span>
                    </div>
                    <div class="nav-item active">
                        <div class="nav-icon archive-icon"></div>
                        <span>档案</span>
                    </div>
                    <div class="nav-item">
                        <div class="nav-icon profile-icon"></div>
                        <span>我的</span>
                    </div>
                </div>
            </div>
            
            <!-- TSG历史档案 -->
            <div class="screen">
                <div class="status-bar">
                    <div class="status-time">9:41</div>
                    <div class="status-icons">
                        <div class="status-icon signal-icon"></div>
                        <div class="status-icon wifi-icon"></div>
                        <div class="status-icon battery-icon"></div>
                    </div>
                </div>
                
                <div class="screen-content">
                    <h3 class="screen-title">TSG历史档案</h3>
                    
                    <div style="margin-bottom: 20px;">
                        <p style="color: var(--text-secondary);">探索TSG过去的展览和艺术活动，了解更多艺术历史</p>
                    </div>
                    
                    <div class="exhibition-card">
                        <div class="exhibition-image" style="background-image: url('https://via.placeholder.com/400x200/1a1a2e/00f7ff?text=展览1')"></div>
                        <div class="exhibition-content">
                            <div class="badge badge-past">过往展览</div>
                            <h4 class="exhibition-title">数字时代的艺术革命</h4>
                            <div class="exhibition-date">2023年5月10日 - 2023年8月20日</div>
                            <div class="exhibition-description">探索数字技术如何改变艺术创作和欣赏方式，展示了来自全球的数字艺术先锋作品。</div>
                            <button class="button">查看详情</button>
                        </div>
                    </div>
                    
                    <div class="exhibition-card">
                        <div class="exhibition-image" style="background-image: url('https://via.placeholder.com/400x200/1a1a2e/ff00e6?text=展览2')"></div>
                        <div class="exhibition-content">
                            <div class="badge badge-past">过往展览</div>
                            <h4 class="exhibition-title">情感与算法：AI艺术的崛起</h4>
                            <div class="exhibition-date">2023年1月15日 - 2023年4月30日</div>
                            <div class="exhibition-description">这个展览探讨了AI如何理解和表达人类情感，展示了AI与人类艺术家合作创作的作品。</div>
                            <button class="button">查看详情</button>
                        </div>
                    </div>
                    
                    <div class="exhibition-card">
                        <div class="exhibition-image" style="background-image: url('https://via.placeholder.com/400x200/1a1a2e/7b00ff?text=展览3')"></div>
                        <div class="exhibition-content">
                            <div class="badge badge-past">过往展览</div>
                            <h4 class="exhibition-title">记忆的形状：个人历史与集体记忆</h4>
                            <div class="exhibition-date">2022年9月5日 - 2022年12月20日</div>
                            <div class="exhibition-description">通过艺术作品探索个人记忆如何塑造我们的身份，以及集体记忆如何影响社会文化。</div>
                            <button class="button">查看详情</button>
                        </div>
                    </div>
                </div>
                
                <div class="nav-bar">
                    <div class="nav-item">
                        <div class="nav-icon home-icon"></div>
                        <span>主页</span>
                    </div>
                    <div class="nav-item">
                        <div class="nav-icon explore-icon"></div>
                        <span>探索</span>
                    </div>
                    <div class="nav-item">
                        <div class="nav-icon upload-icon"></div>
                        <span>上传</span>
                    </div>
                    <div class="nav-item active">
                        <div class="nav-icon archive-icon"></div>
                        <span>档案</span>
                    </div>
                    <div class="nav-item">
                        <div class="nav-icon profile-icon"></div>
                        <span>我的</span>
                    </div>
                </div>
            </div>
            
            <!-- 远程互动功能 -->
            <div class="screen">
                <div class="status-bar">
                    <div class="status-time">9:41</div>
                    <div class="status-icons">
                        <div class="status-icon signal-icon"></div>
                        <div class="status-icon wifi-icon"></div>
                        <div class="status-icon battery-icon"></div>
                    </div>
                </div>
                
                <div class="screen-content">
                    <h3 class="screen-title">远程互动</h3>
                    
                    <div style="margin-bottom: 20px;">
                        <p style="color: var(--text-secondary);">无法亲临现场？通过远程互动功能参与TSG展览和活动</p>
                    </div>
                    
                    <div style="background-color: rgba(20, 20, 31, 0.8); border-radius: 12px; padding: 20px; margin-bottom: 20px; border: 1px solid rgba(123, 0, 255, 0.3);">
                        <h4 style="margin-bottom: 10px;">虚拟展厅</h4>
                        <p style="color: var(--text-color); font-size: 14px; margin-bottom: 15px;">通过360°全景体验当前展览"记忆之光：数字艺术与人类情感"</p>
                        <div style="width: 100%; height: 180px; background-color: #1a1a2e; border-radius: 8px; margin-bottom: 15px; background-image: url('https://via.placeholder.com/400x180/1a1a2e/00f7ff?text=虚拟展厅'); background-size: cover; background-position: center;"></div>
                        <button class="button" style="width: 100%;">进入虚拟展厅</button>
                    </div>
                    
                    <div style="background-color: rgba(20, 20, 31, 0.8); border-radius: 12px; padding: 20px; margin-bottom: 20px; border: 1px solid rgba(123, 0, 255, 0.3);">
                        <h4 style="margin-bottom: 10px;">在线工作坊</h4>
                        <p style="color: var(--text-color); font-size: 14px; margin-bottom: 15px;">参加由艺术家和AI专家主持的在线工作坊，学习如何创作AI艺术</p>
                        <div style="margin-bottom: 15px;">
                            <div style="display: flex; justify-content: space-between; margin-bottom: 10px;">
                                <div>
                                    <div style="font-weight: 600;">记忆与艺术：AI创作工作坊</div>
                                    <div style="font-size: 12px; color: var(--text-secondary);">10月25日 19:00-21:00</div>
                                </div>
                                <button class="button button-secondary" style="padding: 5px 10px; font-size: 12px;">报名</button>
                            </div>
                            <div style="display: flex; justify-content: space-between; margin-bottom: 10px;">
                                <div>
                                    <div style="font-weight: 600;">数字艺术与情感表达</div>
                                    <div style="font-size: 12px; color: var(--text-secondary);">11月5日 14:00-16:00</div>
                                </div>
                                <button class="button button-secondary" style="padding: 5px 10px; font-size: 12px;">报名</button>
                            </div>
                            <div style="display: flex; justify-content: space-between;">
                                <div>
                                    <div style="font-weight: 600;">AI艺术创作进阶技巧</div>
                                    <div style="font-size: 12px; color: var(--text-secondary);">11月15日 19:00-21:00</div>
                                </div>
                                <button class="button button-secondary" style="padding: 5px 10px; font-size: 12px;">报名</button>
                            </div>
                        </div>
                    </div>
                    
                    <div style="background-color: rgba(20, 20, 31, 0.8); border-radius: 12px; padding: 20px; margin-bottom: 20px; border: 1px solid rgba(123, 0, 255, 0.3);">
                        <h4 style="margin-bottom: 10px;">在线讨论</h4>
                        <p style="color: var(--text-color); font-size: 14px; margin-bottom: 15px;">加入关于AI艺术和记忆主题的在线讨论，与艺术家和其他参观者交流</p>
                        <div style="margin-bottom: 15px;">
                            <div class="comment">
                                <div class="comment-header">
                                    <div class="comment-avatar" style="background-image: url('https://via.placeholder.com/50/1a1a2e/00f7ff?text=L')"></div>
                                    <div class="comment-author">刘教授</div>
                                    <div class="comment-time">1小时前</div>
                                </div>
                                <div class="comment-text">AI艺术是否能真正理解人类情感，还是仅仅在模仿？这是我们需要深入探讨的问题。</div>
                            </div>
                            <div class="comment">
                                <div class="comment-header">
                                    <div class="comment-avatar" style="background-image: url('https://via.placeholder.com/50/1a1a2e/ff00e6?text=Z')"></div>
                                    <div class="comment-author">赵艺术家</div>
                                    <div class="comment-time">2小时前</div>
                                </div>
                                <div class="comment-text">我认为AI是一种新的创作工具，它扩展了艺术家的表达能力，但最终的情感理解还是来自人类。</div>
                            </div>
                        </div>
                        <input type="text" class="input-field" placeholder="加入讨论...">
                        <button class="button" style="width: 100%;">发送</button>
                    </div>
                </div>
                
                <div class="nav-bar">
                    <div class="nav-item">
                        <div class="nav-icon home-icon"></div>
                        <span>主页</span>
                    </div>
                    <div class="nav-item">
                        <div class="nav-icon explore-icon"></div>
                        <span>探索</span>
                    </div>
                    <div class="nav-item">
                        <div class="nav-icon upload-icon"></div>
                        <span>上传</span>
                    </div>
                    <div class="nav-item">
                        <div class="nav-icon archive-icon"></div>
                        <span>档案</span>
                    </div>
                    <div class="nav-item active">
                        <div class="nav-icon profile-icon"></div>
                        <span>我的</span>
                    </div>
                </div>
            </div>
            
            <!-- 个人资料 -->
            <div class="screen">
                <div class="status-bar">
                    <div class="status-time">9:41</div>
                    <div class="status-icons">
                        <div class="status-icon signal-icon"></div>
                        <div class="status-icon wifi-icon"></div>
                        <div class="status-icon battery-icon"></div>
                    </div>
                </div>
                
                <div class="screen-content">
                    <h3 class="screen-title">个人资料</h3>
                    
                    <div style="display: flex; flex-direction: column; align-items: center; margin: 20px 0 30px 0;">
                        <div style="width: 100px; height: 100px; border-radius: 50%; background-color: #1a1a2e; margin-bottom: 15px; background-image: url('https://via.placeholder.com/200/1a1a2e/00f7ff?text=头像'); background-size: cover; background-position: center;"></div>
                        <h4 style="margin-bottom: 5px;">张明</h4>
                        <p style="color: var(--text-secondary); margin-bottom: 15px;">艺术爱好者 · 北京</p>
                        <div style="display: flex; gap: 15px; margin-bottom: 10px;">
                            <div style="text-align: center;">
                                <div style="font-weight: 600;">12</div>
                                <div style="font-size: 12px; color: var(--text-secondary);">作品</div>
                            </div>
                            <div style="text-align: center;">
                                <div style="font-weight: 600;">256</div>
                                <div style="font-size: 12px; color: var(--text-secondary);">获赞</div>
                            </div>
                            <div style="text-align: center;">
                                <div style="font-weight: 600;">45</div>
                                <div style="font-size: 12px; color: var(--text-secondary);">关注者</div>
                            </div>
                        </div>
                        <button class="button button-secondary">编辑资料</button>
                    </div>
                    
                    <div style="margin-bottom: 20px;">
                        <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 15px;">
                            <h4>我的活动</h4>
                            <div style="font-size: 14px; color: var(--primary-color);">查看全部</div>
                        </div>
                        
                        <div style="background-color: rgba(20, 20, 31, 0.8); border-radius: 12px; padding: 15px; margin-bottom: 15px; border: 1px solid rgba(123, 0, 255, 0.3);">
                            <div style="display: flex; align-items: center; margin-bottom: 10px;">
                                <div style="width: 40px; height: 40px; border-radius: 50%; background-color: var(--primary-color); display: flex; justify-content: center; align-items: center; margin-right: 15px;">
                                    <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                                        <path d="M12 20v-6M6 20V10M18 20V4"></path>
                                    </svg>
                                </div>
                                <div>
                                    <div style="font-weight: 600;">你的作品"夏日回忆"获得了32条评论</div>
                                    <div style="font-size: 12px; color: var(--text-secondary);">2小时前</div>
                                </div>
                            </div>
                        </div>
                        
                        <div style="background-color: rgba(20, 20, 31, 0.8); border-radius: 12px; padding: 15px; margin-bottom: 15px; border: 1px solid rgba(123, 0, 255, 0.3);">
                            <div style="display: flex; align-items: center; margin-bottom: 10
                            <div style="display: flex; align-items: center; margin-bottom: 10px;">
                            <div style="width: 40px; height: 40px; border-radius: 50%; background-color: var(--tertiary-color); display: flex; justify-content: center; align-items: center; margin-right: 15px;">
                                <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                                    <path d="M20.84 4.61a5.5 5.5 0 0 0-7.78 0L12 5.67l-1.06-1.06a5.5 5.5 0 0 0-7.78 7.78l1.06 1.06L12 21.23l7.78-7.78 1.06-1.06a5.5 5.5 0 0 0 0-7.78z"></path>
                                </svg>
                            </div>
                            <div>
                                <div style="font-weight: 600;">李华喜欢了你的作品"城市之梦"</div>
                                <div style="font-size: 12px; color: var(--text-secondary);">5小时前</div>
                            </div>
                        </div>
                    </div>
                    
                    <div style="background-color: rgba(20, 20, 31, 0.8); border-radius: 12px; padding: 15px; border: 1px solid rgba(123, 0, 255, 0.3);">
                        <div style="display: flex; align-items: center; margin-bottom: 10px;">
                            <div style="width: 40px; height: 40px; border-radius: 50%; background-color: var(--secondary-color); display: flex; justify-content: center; align-items: center; margin-right: 15px;">
                                <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                                    <rect x="3" y="4" width="18" height="18" rx="2" ry="2"></rect>
                                    <line x1="16" y1="2" x2="16" y2="6"></line>
                                    <line x1="8" y1="2" x2="8" y2="6"></line>
                                    <line x1="3" y1="10" x2="21" y2="10"></line>
                                </svg>
                            </div>
                            <div>
                                <div style="font-weight: 600;">你已报名参加"记忆与艺术：AI创作工作坊"</div>
                                <div style="font-size: 12px; color: var(--text-secondary);">昨天</div>
                            </div>
                        </div>
                    </div>
                </div>
                
                <div style="margin-bottom: 20px;">
                    <h4 style="margin-bottom: 15px;">设置</h4>
                    
                    <div style="background-color: rgba(20, 20, 31, 0.8); border-radius: 12px; overflow: hidden; border: 1px solid rgba(123, 0, 255, 0.3);">
                        <div style="padding: 15px; border-bottom: 1px solid rgba(123, 0, 255, 0.3); display: flex; justify-content: space-between; align-items: center;">
                            <div>账号设置</div>
                            <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                                <polyline points="9 18 15 12 9 6"></polyline>
                            </svg>
                        </div>
                        <div style="padding: 15px; border-bottom: 1px solid rgba(123, 0, 255, 0.3); display: flex; justify-content: space-between; align-items: center;">
                            <div>隐私设置</div>
                            <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                                <polyline points="9 18 15 12 9 6"></polyline>
                            </svg>
                        </div>
                        <div style="padding: 15px; border-bottom: 1px solid rgba(123, 0, 255, 0.3); display: flex; justify-content: space-between; align-items: center;">
                            <div>通知设置</div>
                            <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                                <polyline points="9 18 15 12 9 6"></polyline>
                            </svg>
                        </div>
                        <div style="padding: 15px; display: flex; justify-content: space-between; align-items: center;">
                            <div>帮助与反馈</div>
                            <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                                <polyline points="9 18 15 12 9 6"></polyline>
                            </svg>
                        </div>
                    </div>
                </div>
                
                <button class="button button-secondary" style="width: 100%; margin-bottom: 20px;">退出登录</button>
            </div>
            
            <div class="nav-bar">
                <div class="nav-item">
                    <div class="nav-icon home-icon"></div>
                    <span>主页</span>
                </div>
                <div class="nav-item">
                    <div class="nav-icon explore-icon"></div>
                    <span>探索</span>
                </div>
                <div class="nav-item">
                    <div class="nav-icon upload-icon"></div>
                    <span>上传</span>
                </div>
                <div class="nav-item">
                    <div class="nav-icon archive-icon"></div>
                    <span>档案</span>
                </div>
                <div class="nav-item active">
                    <div class="nav-icon profile-icon"></div>
                    <span>我的</span>
                </div>
            </div>
        </div>
    </div>
    
    <div class="notification">
        <div class="notification-title">上传成功</div>
        <div class="notification-message">你的记忆已成功转化为艺术作品</div>
    </div>
</body>
</html>
