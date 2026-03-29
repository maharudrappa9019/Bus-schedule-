# Bus-schedule-
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Bus Time Schedule</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            line-height: 1.6;
            color: #333;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
        }

        .container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 20px;
        }

        header {
            text-align: center;
            color: white;
            margin-bottom: 30px;
        }

        h1 {
            font-size: 2.5rem;
            margin-bottom: 10px;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.3);
        }

        .subtitle {
            font-size: 1.2rem;
            opacity: 0.9;
        }

        .search-section {
            background: white;
            border-radius: 15px;
            padding: 30px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.2);
            margin-bottom: 30px;
        }

        .search-form {
            display: flex;
            gap: 15px;
            flex-wrap: wrap;
            justify-content: center;
            align-items: center;
        }

        .form-group {
            display: flex;
            flex-direction: column;
            min-width: 200px;
        }

        label {
            font-weight: 600;
            margin-bottom: 5px;
            color: #555;
        }

        select, input {
            padding: 12px;
            border: 2px solid #e1e5e9;
            border-radius: 8px;
            font-size: 16px;
            transition: border-color 0.3s;
        }

        select:focus, input:focus {
            outline: none;
            border-color: #667eea;
        }

        .btn {
            background: linear-gradient(45deg, #667eea, #764ba2);
            color: white;
            border: none;
            padding: 12px 30px;
            border-radius: 8px;
            font-size: 16px;
            cursor: pointer;
            transition: transform 0.3s, box-shadow 0.3s;
        }

        .btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(0,0,0,0.2);
        }

        .schedule-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(350px, 1fr));
            gap: 20px;
            margin-bottom: 30px;
        }

        .route-card {
            background: white;
            border-radius: 15px;
            padding: 25px;
            box-shadow: 0 8px 25px rgba(0,0,0,0.1);
            transition: transform 0.3s, box-shadow 0.3s;
        }

        .route-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 15px 35px rgba(0,0,0,0.15);
        }

        .route-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
            padding-bottom: 15px;
            border-bottom: 2px solid #f0f0f0;
        }

        .route-number {
            font-size: 1.8rem;
            font-weight: bold;
            background: linear-gradient(45deg, #667eea, #764ba2);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
        }

        .route-direction {
            background: #f8f9fa;
            padding: 8px 15px;
            border-radius: 20px;
            font-size: 0.9rem;
            font-weight: 600;
        }

        .time-table {
            display: flex;
            flex-direction: column;
            gap: 10px;
        }

        .time-row {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 12px;
            background: #f8f9fa;
            border-radius: 8px;
            transition: background 0.3s;
        }

        .time-row:hover {
            background: #e3f2fd;
        }

        .time {
            font-size: 1.1rem;
            font-weight: 600;
            color: #2c3e50;
        }

        .status {
            padding: 5px 12px;
            border-radius: 15px;
            font-size: 0.8rem;
            font-weight: 600;
        }

        .status.on-time {
            background: #d4edda;
            color: #155724;
        }

        .status.delayed {
            background: #f8d7da;
            color: #721c24;
        }

        .status.cancelled {
            background: #f5c6cb;
            color: #721c24;
        }

        .no-results {
            text-align: center;
            padding: 60px 20px;
            color: #666;
            grid-column: 1 / -1;
        }

        .legend {
            background: white;
            border-radius: 15px;
            padding: 20px;
            box-shadow: 0 8px 25px rgba(0,0,0,0.1);
            display: flex;
            justify-content: center;
            gap: 30px;
            flex-wrap: wrap;
        }

        .legend-item {
            display: flex;
            align-items: center;
            gap: 8px;
        }

        @media (max-width: 768px) {
            h1 {
                font-size: 2rem;
            }
            
            .search-form {
                flex-direction: column;
                align-items: stretch;
            }
            
            .form-group {
                min-width: auto;
            }
            
            .schedule-grid {
                grid-template-columns: 1fr;
            }
        }

        .current-time {
            text-align: center;
            color: white;
            font-size: 1.1rem;
            margin-bottom: 10px;
            font-weight: 500;
        }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <div class="current-time" id="currentTime"></div>
            <h1>🚌 Bus Time Schedule</h1>
            <p class="subtitle">Real-time bus schedules and tracking</p>
        </header>

        <div class="search-section">
            <form class="search-form" id="searchForm">
                <div class="form-group">
                    <label for="routeSelect">Select Route</label>
                    <select id="routeSelect">
                        <option value="">All Routes</option>
                        <option value="101">101 - Downtown</option>
                        <option value="205">205 - Airport</option>
                        <option value="315">315 - University</option>
                        <option value="425">425 - Shopping Mall</option>
                        <option value="530">530 - Train Station</option>
                    </select>
                </div>
                <div class="form-group">
                    <label for="directionSelect">Direction</label>
                    <select id="directionSelect">
                        <option value="">All Directions</option>
                        <option value="outbound">Outbound</option>
                        <option value="inbound">Inbound</option>
                    </select>
                </div>
                <div class="form-group">
                    <label for="stopSelect">Bus Stop</label>
                    <select id="stopSelect">
                        <option value="">All Stops</option>
                        <option value="stop1">Main Street</option>
                        <option value="stop2">Central Park</option>
                        <option value="stop3">City Hall</option>
                        <option value="stop4">Railway Station</option>
                    </select>
                </div>
                <button type="submit" class="btn">Search Buses</button>
            </form>
        </div>

        <div class="schedule-grid" id="scheduleGrid">
            <!-- Dynamic content will be populated here -->
        </div>

        <div class="legend">
            <div class="legend-item">
                <span class="status on-time">●</span>
                <span>On Time</span>
            </div>
            <div class="legend-item">
                <span class="status delayed">●</span>
                <span>Delayed</span>
            </div>
            <div class="legend-item">
                <span class="status cancelled">●</span>
                <span>Cancelled</span>
            </div>
        </div>
    </div>

    <script>
        // Sample bus data
        const busData = [
            {
                route: '101',
                direction: 'outbound',
                stop: 'stop1',
                times: [
                    { time: '08:15', status: 'on-time' },
                    { time: '08:45', status: 'on-time' },
                    { time: '09:15', status: 'delayed' },
                    { time: '09:45', status: 'on-time' }
                ]
            },
            {
                route: '205',
                direction: 'inbound',
                stop: 'stop2',
                times: [
                    { time: '08:30', status: 'on-time' },
                    { time: '09:00', status: 'on-time' },
                    { time: '09:30', status: 'cancelled' },
                    { time: '10:00', status: 'on-time' }
                ]
            },
            {
                route: '315',
                direction: 'outbound',
                stop: 'stop3',
                times: [
                    { time: '08:20', status: 'on-time' },
                    { time: '08:50', status: 'on-time' },
                    { time: '09:20', status: 'on-time' },
                    { time: '09:50', status: 'delayed' }
                ]
            },
            {
                route: '425',
                direction: 'inbound',
                stop: 'stop4',
                times: [
                    { time: '08:25', status: 'on-time' },
                    { time: '08:55', status: 'on-time' },
                    { time: '09:25', status: 'on-time' },
                    { time: '09:55', status: 'on-time' }
                ]
            },
            {
                route: '530',
                direction: 'outbound',
                stop: 'stop1',
                times: [
                    { time: '08:35', status: 'delayed' },
                    { time: '09:05', status: 'on-time' },
                    { time: '09:35', status: 'on-time' },
                    { time: '10:05', status: 'on-time' }
                ]
            }
        ];

        // Update current time
        function updateCurrentTime() {
            const now = new Date();
            document.getElementById('currentTime').textContent = 
                `Updated: ${now.toLocaleTimeString()}`;
        }

        // Filter and display buses
        function displayBuses(routeFilter = '', directionFilter = '', stopFilter = '') {
            const grid = document.getElementById('scheduleGrid');
            const filteredData = busData.filter(bus => {
                return (!routeFilter || bus.route === routeFilter) &&
                       (!directionFilter || bus.direction === directionFilter) &&
                       (!stopFilter || bus.stop === stopFilter);
            });

            if (filteredData.length === 0) {
                grid.innerHTML = `
                    <div class="no-results">
                        <h3>🕒 No buses found</h3>
                        <p>Try adjusting your search filters</p>
                    </div>
                `;
                return;
            }

            grid.innerHTML = filteredData.map(bus => `
                <div class="route-card">
                    <div class="route-header">
                        <div class="route-number">Route ${bus.route}</div>
                        <div class="route-direction">${bus.direction.toUpperCase()}</div>
                    </div>
                    <div class="time-table">
                        ${bus.times.map(time => `
                            <div class="time-row">
                                <span class="time">${time.time}</span>
                                <span class="status ${time.status}">${time.status.replace('-', ' ').toUpperCase()}</span>
                            </div>
                        `).join('')}
                    </div>
                </div>
            `).join('');
        }

        // Event listeners
        document.getElementById('searchForm').addEventListener('submit', (e) => {
            e.preventDefault();
            const route = document.getElementById('routeSelect').value;
            const direction = document.getElementById('directionSelect').value;
            const stop = document.getElementById('stopSelect').value;
            displayBuses(route, direction, stop);
        });

        // Initialize
        updateCurrentTime();
        displayBuses(); // Show all buses initially

        // Update time every minute
        setInterval(updateCurrentTime, 60000);

        // Auto-refresh every 2 minutes
        setInterval(() => {
            const route = document.getElementById('routeSelect').value;
            const direction = document.getElementById('directionSelect').value;
            const stop = document.getElementById('stopSelect').value;
            displayBuses(route, direction, stop);
        }, 120000);
    </script>
</body>
</html>
