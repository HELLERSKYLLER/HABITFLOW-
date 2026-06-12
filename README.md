 # HABITFLOW-
HABIT TRACKING 
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>HabitFlow | Interactive Habit Tracker Dashboard</title>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        :root {
            --primary: #6366f1;
            --primary-dark: #4f46e5;
            --secondary: #8b5cf6;
            --accent: #06b6d4;
            --success: #10b981;
            --warning: #f59e0b;
            --danger: #ef4444;
            --bg: #0f172a;
            --surface: #1e293b;
            --surface-light: #334155;
            --text: #f8fafc;
            --text-muted: #94a3b8;
            --border: #334155;
            --shadow: 0 10px 40px rgba(0,0,0,0.4);
            --gradient-1: linear-gradient(135deg, #6366f1, #8b5cf6);
            --gradient-2: linear-gradient(135deg, #06b6d4, #6366f1);
            --gradient-3: linear-gradient(135deg, #10b981, #06b6d4);
        }

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Inter', sans-serif;
            background: var(--bg);
            color: var(--text);
            min-height: 100vh;
            overflow-x: hidden;
        }

        /* Background Animation */
        .bg-shapes {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
            z-index: 0;
            overflow: hidden;
        }

        .bg-shapes .shape {
            position: absolute;
            border-radius: 50%;
            filter: blur(80px);
            opacity: 0.15;
            animation: float 20s infinite ease-in-out;
        }

        .shape-1 {
            width: 400px;
            height: 400px;
            background: var(--primary);
            top: -100px;
            right: -100px;
            animation-delay: 0s;
        }

        .shape-2 {
            width: 300px;
            height: 300px;
            background: var(--accent);
            bottom: -50px;
            left: -50px;
            animation-delay: -5s;
        }

        .shape-3 {
            width: 250px;
            height: 250px;
            background: var(--secondary);
            top: 50%;
            left: 50%;
            animation-delay: -10s;
        }

        @keyframes float {
            0%, 100% { transform: translate(0, 0) scale(1); }
            33% { transform: translate(30px, -30px) scale(1.1); }
            66% { transform: translate(-20px, 20px) scale(0.9); }
        }

        /* Layout */
        .app-container {
            position: relative;
            z-index: 1;
            max-width: 1400px;
            margin: 0 auto;
            padding: 2rem;
        }

        /* Header */
        .header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 2.5rem;
            padding-bottom: 1.5rem;
            border-bottom: 1px solid var(--border);
        }

        .brand {
            display: flex;
            align-items: center;
            gap: 1rem;
        }

        .brand-icon {
            width: 50px;
            height: 50px;
            background: var(--gradient-1);
            border-radius: 14px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 1.5rem;
            box-shadow: 0 4px 15px rgba(99, 102, 241, 0.4);
        }

        .brand-text h1 {
            font-size: 1.75rem;
            font-weight: 800;
            background: var(--gradient-1);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
        }

        .brand-text p {
            color: var(--text-muted);
            font-size: 0.875rem;
            margin-top: 2px;
        }

        .header-actions {
            display: flex;
            gap: 1rem;
            align-items: center;
        }

        .date-display {
            background: var(--surface);
            padding: 0.75rem 1.25rem;
            border-radius: 12px;
            border: 1px solid var(--border);
            font-weight: 600;
            display: flex;
            align-items: center;
            gap: 0.5rem;
        }

        .btn {
            padding: 0.75rem 1.5rem;
            border-radius: 12px;
            border: none;
            font-family: inherit;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
            display: flex;
            align-items: center;
            gap: 0.5rem;
            font-size: 0.9rem;
        }

        .btn-primary {
            background: var(--gradient-1);
            color: white;
            box-shadow: 0 4px 15px rgba(99, 102, 241, 0.3);
        }

        .btn-primary:hover {
            transform: translateY(-2px);
            box-shadow: 0 6px 20px rgba(99, 102, 241, 0.4);
        }

        .btn-outline {
            background: transparent;
            color: var(--text);
            border: 1px solid var(--border);
        }

        .btn-outline:hover {
            background: var(--surface);
            border-color: var(--primary);
        }

        /* Stats Grid */
        .stats-grid {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 1.5rem;
            margin-bottom: 2.5rem;
        }

        .stat-card {
            background: var(--surface);
            border: 1px solid var(--border);
            border-radius: 20px;
            padding: 1.5rem;
            position: relative;
            overflow: hidden;
            transition: all 0.3s ease;
        }

        .stat-card:hover {
            transform: translateY(-4px);
            border-color: var(--primary);
            box-shadow: var(--shadow);
        }

        .stat-card::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 4px;
            background: var(--gradient-1);
        }

        .stat-card:nth-child(2)::before { background: var(--gradient-2); }
        .stat-card:nth-child(3)::before { background: var(--gradient-3); }
        .stat-card:nth-child(4)::before { background: linear-gradient(135deg, #f59e0b, #ef4444); }

        .stat-header {
            display: flex;
            justify-content: space-between;
            align-items: flex-start;
            margin-bottom: 1rem;
        }

        .stat-icon {
            width: 44px;
            height: 44px;
            border-radius: 12px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 1.1rem;
        }

        .stat-icon.purple { background: rgba(99, 102, 241, 0.15); color: #818cf8; }
        .stat-icon.cyan { background: rgba(6, 182, 212, 0.15); color: #22d3ee; }
        .stat-icon.green { background: rgba(16, 185, 129, 0.15); color: #34d399; }
        .stat-icon.orange { background: rgba(245, 158, 11, 0.15); color: #fbbf24; }

        .stat-trend {
            font-size: 0.75rem;
            font-weight: 600;
            padding: 0.25rem 0.5rem;
            border-radius: 6px;
        }

        .trend-up {
            background: rgba(16, 185, 129, 0.15);
            color: #34d399;
        }

        .trend-down {
            background: rgba(239, 68, 68, 0.15);
            color: #f87171;
        }

        .stat-value {
            font-size: 2rem;
            font-weight: 800;
            margin-bottom: 0.25rem;
        }

        .stat-label {
            color: var(--text-muted);
            font-size: 0.875rem;
        }

        /* Main Content Grid */
        .main-grid {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 1.5rem;
            margin-bottom: 2.5rem;
        }

        .card {
            background: var(--surface);
            border: 1px solid var(--border);
            border-radius: 20px;
            padding: 1.5rem;
            transition: all 0.3s ease;
        }

        .card:hover {
            border-color: var(--primary);
        }

        .card-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 1.5rem;
        }

        .card-title {
            font-size: 1.1rem;
            font-weight: 700;
            display: flex;
            align-items: center;
            gap: 0.5rem;
        }

        .card-title i {
            color: var(--primary);
        }

        /* Habit List */
        .habit-list {
            display: flex;
            flex-direction: column;
            gap: 0.75rem;
        }

        .habit-item {
            display: flex;
            align-items: center;
            gap: 1rem;
            padding: 1rem 1.25rem;
            background: var(--bg);
            border-radius: 14px;
            border: 1px solid transparent;
            transition: all 0.3s ease;
            cursor: pointer;
        }

        .habit-item:hover {
            border-color: var(--border);
            transform: translateX(4px);
        }

        .habit-item.completed {
            border-color: rgba(16, 185, 129, 0.3);
            background: rgba(16, 185, 129, 0.05);
        }

        .habit-checkbox {
            width: 28px;
            height: 28px;
            border-radius: 8px;
            border: 2px solid var(--border);
            display: flex;
            align-items: center;
            justify-content: center;
            transition: all 0.3s ease;
            flex-shrink: 0;
            cursor: pointer;
        }

        .habit-item.completed .habit-checkbox {
            background: var(--success);
            border-color: var(--success);
        }

        .habit-checkbox i {
            opacity: 0;
            transform: scale(0.5);
            transition: all 0.3s ease;
            color: white;
            font-size: 0.8rem;
        }

        .habit-item.completed .habit-checkbox i {
            opacity: 1;
            transform: scale(1);
        }

        .habit-info {
            flex: 1;
        }

        .habit-name {
            font-weight: 600;
            font-size: 0.95rem;
            margin-bottom: 0.25rem;
        }

        .habit-meta {
            display: flex;
            align-items: center;
            gap: 0.75rem;
            font-size: 0.8rem;
            color: var(--text-muted);
        }

        .habit-streak {
            display: flex;
            align-items: center;
            gap: 0.25rem;
            color: var(--warning);
            font-weight: 600;
        }

        .habit-category {
            padding: 0.2rem 0.6rem;
            border-radius: 6px;
            font-size: 0.7rem;
            font-weight: 600;
            text-transform: uppercase;
            letter-spacing: 0.5px;
        }

        .cat-health { background: rgba(16, 185, 129, 0.15); color: #34d399; }
        .cat-productivity { background: rgba(99, 102, 241, 0.15); color: #818cf8; }
        .cat-learning { background: rgba(6, 182, 212, 0.15); color: #22d3ee; }
        .cat-wellness { background: rgba(245, 158, 11, 0.15); color: #fbbf24; }

        .habit-progress {
            width: 50px;
            height: 50px;
            position: relative;
            flex-shrink: 0;
        }

        .habit-progress svg {
            transform: rotate(-90deg);
        }

        .habit-progress circle {
            fill: none;
            stroke-width: 3;
        }

        .habit-progress .bg-circle {
            stroke: var(--border);
        }

        .habit-progress .progress-circle {
            stroke: var(--primary);
            stroke-linecap: round;
            transition: stroke-dashoffset 0.5s ease;
        }

        .habit-progress-text {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            font-size: 0.75rem;
            font-weight: 700;
        }

        /* Calendar Heatmap */
        .heatmap-container {
            display: grid;
            grid-template-columns: repeat(7, 1fr);
            gap: 6px;
        }

        .heatmap-day {
            aspect-ratio: 1;
            border-radius: 8px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 0.7rem;
            font-weight: 600;
            transition: all 0.3s ease;
            cursor: pointer;
            position: relative;
        }

        .heatmap-day:hover {
            transform: scale(1.15);
            z-index: 2;
        }

        .heatmap-day.level-0 { background: var(--bg); color: var(--text-muted); }
        .heatmap-day.level-1 { background: rgba(99, 102, 241, 0.2); color: #818cf8; }
        .heatmap-day.level-2 { background: rgba(99, 102, 241, 0.4); color: #a5b4fc; }
        .heatmap-day.level-3 { background: rgba(99, 102, 241, 0.6); color: #c7d2fe; }
        .heatmap-day.level-4 { background: rgba(99, 102, 241, 0.85); color: white; }

        .heatmap-day.today {
            box-shadow: 0 0 0 2px var(--primary);
        }

        .heatmap-legend {
            display: flex;
            align-items: center;
            justify-content: flex-end;
            gap: 0.5rem;
            margin-top: 1rem;
            font-size: 0.75rem;
            color: var(--text-muted);
        }

        .heatmap-legend-boxes {
            display: flex;
            gap: 3px;
        }

        .legend-box {
            width: 16px;
            height: 16px;
            border-radius: 4px;
        }

        /* Charts Section */
        .charts-grid {
            display: grid;
            grid-template-columns: 2fr 1fr;
            gap: 1.5rem;
            margin-bottom: 2.5rem;
        }

        .chart-container {
            position: relative;
            height: 280px;
        }

        /* Weekly Overview */
        .weekly-grid {
            display: grid;
            grid-template-columns: repeat(7, 1fr);
            gap: 0.75rem;
        }

        .week-day {
            text-align: center;
            padding: 1rem 0.5rem;
            background: var(--bg);
            border-radius: 14px;
            border: 1px solid transparent;
            transition: all 0.3s ease;
        }

        .week-day.active {
            border-color: var(--primary);
            background: rgba(99, 102, 241, 0.1);
        }

        .week-day-name {
            font-size: 0.75rem;
            color: var(--text-muted);
            text-transform: uppercase;
            letter-spacing: 1px;
            margin-bottom: 0.5rem;
        }

        .week-day-number {
            font-size: 1.25rem;
            font-weight: 700;
            margin-bottom: 0.75rem;
        }

        .week-day-bars {
            display: flex;
            flex-direction: column;
            gap: 4px;
            align-items: center;
        }

        .mini-bar {
            width: 6px;
            border-radius: 3px;
            transition: all 0.3s ease;
        }

        /* Add Habit Modal */
        .modal-overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(15, 23, 42, 0.8);
            backdrop-filter: blur(8px);
            z-index: 1000;
            display: none;
            align-items: center;
            justify-content: center;
            opacity: 0;
            transition: opacity 0.3s ease;
        }

        .modal-overlay.active {
            display: flex;
            opacity: 1;
        }

        .modal {
            background: var(--surface);
            border: 1px solid var(--border);
            border-radius: 24px;
            padding: 2rem;
            width: 90%;
            max-width: 500px;
            transform: translateY(20px);
            transition: transform 0.3s ease;
            box-shadow: var(--shadow);
        }

        .modal-overlay.active .modal {
            transform: translateY(0);
        }

        .modal-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 1.5rem;
        }

        .modal-title {
            font-size: 1.25rem;
            font-weight: 700;
        }

        .modal-close {
            width: 36px;
            height: 36px;
            border-radius: 10px;
            border: 1px solid var(--border);
            background: transparent;
            color: var(--text);
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            transition: all 0.3s ease;
        }

        .modal-close:hover {
            background: var(--bg);
            border-color: var(--danger);
            color: var(--danger);
        }

        .form-group {
            margin-bottom: 1.25rem;
        }

        .form-label {
            display: block;
            font-size: 0.875rem;
            font-weight: 600;
            margin-bottom: 0.5rem;
            color: var(--text-muted);
        }

        .form-input {
            width: 100%;
            padding: 0.875rem 1rem;
            background: var(--bg);
            border: 1px solid var(--border);
            border-radius: 12px;
            color: var(--text);
            font-family: inherit;
            font-size: 0.95rem;
            transition: all 0.3s ease;
        }

        .form-input:focus {
            outline: none;
            border-color: var(--primary);
            box-shadow: 0 0 0 3px rgba(99, 102, 241, 0.1);
        }

        .form-select {
            width: 100%;
            padding: 0.875rem 1rem;
            background: var(--bg);
            border: 1px solid var(--border);
            border-radius: 12px;
            color: var(--text);
            font-family: inherit;
            font-size: 0.95rem;
            cursor: pointer;
            transition: all 0.3s ease;
        }

        .form-select:focus {
            outline: none;
            border-color: var(--primary);
        }

        .category-options {
            display: flex;
            gap: 0.5rem;
            flex-wrap: wrap;
        }

        .category-option {
            padding: 0.5rem 1rem;
            border-radius: 10px;
            border: 2px solid var(--border);
            background: transparent;
            color: var(--text);
            font-family: inherit;
            font-size: 0.85rem;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
        }

        .category-option:hover,
        .category-option.active {
            border-color: var(--primary);
            background: rgba(99, 102, 241, 0.1);
        }

        .modal-footer {
            display: flex;
            gap: 1rem;
            margin-top: 1.5rem;
        }

        .modal-footer .btn {
            flex: 1;
            justify-content: center;
        }

        /* Toast Notification */
        .toast-container {
            position: fixed;
            bottom: 2rem;
            right: 2rem;
            z-index: 2000;
            display: flex;
            flex-direction: column;
            gap: 0.75rem;
        }

        .toast {
            background: var(--surface);
            border: 1px solid var(--border);
            border-radius: 14px;
            padding: 1rem 1.5rem;
            display: flex;
            align-items: center;
     