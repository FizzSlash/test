<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Content Calendar Creation</title>
    <style>
        /* Dark theme colors from screenshot with increased transparency */
        :root {
            --bg-color: rgba(20, 19, 28, 0.7);
            --card-bg: rgba(36, 35, 49, 0.7);
            --accent-yellow: #FFDC00;
            --text-color: #FFFFFF;
            --input-bg: rgba(48, 47, 66, 0.8);
            --border-color: rgba(58, 57, 82, 0.5);
        }
        
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }
        
        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
            line-height: 1.6;
            color: var(--text-color);
            background-color: transparent;
            min-height: 100vh;
            padding: 20px;
        }
        
        .container {
            max-width: 1200px;
            margin: 0 auto;
            background-color: var(--bg-color);
            border-radius: 10px;
            overflow: hidden;
            box-shadow: 0 10px 20px rgba(0, 0, 0, 0.3);
        }
        
        header {
            padding: 20px;
            text-align: center;
            border-bottom: 1px solid var(--border-color);
        }
        
        h1 {
            font-size: 32px;
            font-weight: 700;
            letter-spacing: 1px;
            text-transform: uppercase;
        }
        
        .tab-container {
            display: flex;
            justify-content: center;
            padding: 10px 0;
        }
        
        .tab {
            padding: 8px 30px;
            font-size: 16px;
            font-weight: 500;
            border: 1px solid var(--border-color);
            border-radius: 5px;
            background: transparent;
            color: var(--text-color);
            cursor: pointer;
            margin: 0 5px;
            transition: all 0.3s;
        }
        
        .tab.active {
            background-color: var(--accent-yellow);
            color: #000;
            border-color: var(--accent-yellow);
        }
        
        .form-container {
            padding: 20px;
            background-color: var(--card-bg);
            margin: 15px;
            border-radius: 8px;
        }
        
        .form-row {
            display: flex;
            flex-wrap: wrap;
            margin-bottom: 15px;
            gap: 15px;
        }
        
        .form-group {
            flex: 1;
            min-width: 200px;
        }
        
        label {
            display: block;
            margin-bottom: 5px;
            font-size: 14px;
        }
        
        input, select, textarea {
            width: 100%;
            padding: 10px;
            border-radius: 5px;
            border: none;
            background-color: var(--input-bg);
            color: var(--text-color);
            font-size: 15px;
        }
        
        textarea {
            resize: vertical;
            min-height: 80px;
        }
        
        .submit-btn {
            background-color: rgba(255, 255, 255, 0.2);
            color: #FFF;
            border: 1px solid rgba(255, 255, 255, 0.4);
            padding: 12px;
            border-radius: 5px;
            cursor: pointer;
            font-size: 16px;
            font-weight: 600;
            text-align: center;
            display: block;
            width: 100%;
            transition: all 0.3s;
            margin-top: 15px;
            backdrop-filter: blur(5px);
        }
        
        .submit-btn:hover {
            background-color: rgba(255, 255, 255, 0.3);
        }
        
        .submit-btn:disabled {
            opacity: 0.5;
            cursor: not-allowed;
        }
        
        #export-btn {
            background-color: rgba(46, 204, 113, 0.3);
            border-color: rgba(46, 204, 113, 0.6);
        }
        
        #export-btn:hover {
            background-color: rgba(46, 204, 113, 0.4);
        }
        
        .calendar-container {
            padding: 20px;
            margin: 15px;
            background-color: var(--card-bg);
            border-radius: 8px;
        }
        
        .month-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 15px;
            padding-bottom: 10px;
            border-bottom: 1px solid var(--border-color);
        }
        
        .month-title {
            font-size: 20px;
            font-weight: 600;
        }
        
        .calendar-controls {
            display: flex;
            gap: 10px;
        }
        
        .calendar-control-btn {
            background-color: var(--input-bg);
            color: var(--text-color);
            border: none;
            border-radius: 5px;
            padding: 8px 12px;
            cursor: pointer;
            font-size: 14px;
        }
        
        .days-header {
            display: grid;
            grid-template-columns: repeat(7, 1fr);
            text-align: center;
            font-weight: 500;
            padding: 10px 0;
            border-bottom: 1px solid var(--border-color);
        }
        
        .calendar-grid {
            display: grid;
            grid-template-columns: repeat(7, 1fr);
            grid-auto-rows: minmax(100px, auto);
            gap: 4px;
            background-color: rgba(20, 19, 28, 0.4);
            border-radius: 5px;
            overflow: hidden;
            margin-top: 4px;
        }
        
        .calendar-day {
            background-color: rgba(48, 47, 66, 0.5);
            padding: 6px;
            position: relative;
            min-height: 100px;
            overflow: hidden;
            border-radius: 4px;
        }
        
        .calendar-day:hover {
            background-color: rgba(60, 60, 80, 0.95);
        }
        
        .date-number {
            position: absolute;
            top: 5px;
            right: 8px;
            font-size: 12px;
            opacity: 0.8;
        }
        
        .empty-day {
            background-color: rgba(36, 35, 49, 0.3);
        }
        
        .campaign {
            margin-top: 20px;
            padding: 6px;
            border-radius: 4px;
            font-size: 11px;
            background-color: rgba(255, 220, 0, 0.2);
            border-left: 3px solid var(--accent-yellow);
            margin-bottom: 5px;
            overflow: hidden;
            word-break: break-word;
            max-height: calc(100% - 30px);
            cursor: grab;
        }
        
        .campaign:hover {
            background-color: rgba(255, 220, 0, 0.3);
        }
        
        .campaign.sms {
            background-color: rgba(136, 84, 208, 0.2);
            border-left: 3px solid rgba(136, 84, 208, 0.8);
        }
        
        .campaign.sms:hover {
            background-color: rgba(136, 84, 208, 0.3);
        }
        
        .campaign.dragging {
            opacity: 0.4;
        }
        
        .drop-hover {
            background-color: rgba(80, 80, 120, 0.7) !important;
        }
        
        .campaign-title {
            font-weight: 600;
            margin-bottom: 2px;
            font-size: 10px;
            white-space: nowrap;
            overflow: hidden;
            text-overflow: ellipsis;
        }
        
        .campaign-offer {
            font-size: 9px;
            opacity: 0.8;
            white-space: nowrap;
            overflow: hidden;
            text-overflow: ellipsis;
        }
        
        .loading-overlay {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.7);
            z-index: 1000;
            justify-content: center;
            align-items: center;
        }
        
        .loading-spinner {
            border: 4px solid rgba(255, 255, 255, 0.3);
            border-radius: 50%;
            border-top: 4px solid var(--accent-yellow);
            width: 40px;
            height: 40px;
            animation: spin 1s linear infinite;
        }
        
        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }
        
        .status-message {
            display: none;
            margin-top: 15px;
            padding: 10px;
            border-radius: 5px;
            text-align: center;
        }
        
        .success-message {
            background-color: rgba(46, 204, 113, 0.2);
            border: 1px solid #2ecc71;
        }
        
        .error-message {
            background-color: rgba(231, 76, 60, 0.2);
            border: 1px solid #e74c3c;
        }
        
        /* Modal styles for campaign details */
        .modal-overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.7);
            display: flex;
            justify-content: center;
            align-items: center;
            z-index: 1000;
        }
        
        .modal-content {
            background-color: var(--card-bg);
            padding: 20px;
            border-radius: 8px;
            width: 90%;
            max-width: 500px;
            position: relative;
            box-shadow: 0 4px 20px rgba(0, 0, 0, 0.5);
        }
        
        .modal-close {
            position: absolute;
            top: 10px;
            right: 15px;
            font-size: 24px;
            cursor: pointer;
            color: var(--text-color);
        }
        
        .campaign-details-form .form-group {
            margin-bottom: 15px;
        }
        
        #add-camp {
            background-color: rgba(46, 204, 113, 0.5);
        }
        
        @media (max-width: 768px) {
            .form-row {
                flex-direction: column;
            }
            
            .calendar-grid {
                grid-template-columns: repeat(1, 1fr);
            }
            
            .days-header {
                display: none;
            }
            
            .calendar-day {
                margin-bottom: 4px;
            }
            
            .date-number {
                position: static;
                margin-bottom: 10px;
                display: inline-block;
                padding-right: 5px;
            }
            
            .mobile-day {
                display: inline-block;
                font-weight: 500;
                margin-bottom: 10px;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <h1>CONTENT CALENDAR CREATION</h1>
        </header>
        
        <div class="form-container">
            <div class="form-row">
                <div class="form-group">
                    <label for="website-url">Website URL</label>
                    <input type="text" id="website-url" placeholder="https://example.com">
                </div>
                
                <div class="form-group">
                    <label for="campaigns-per-week">Email Campaigns Per Week</label>
                    <select id="campaigns-per-week">
                        <option value="1">1 email campaign per week</option>
                        <option value="2">2 email campaigns per week</option>
                        <option value="3">3 email campaigns per week</option>
                        <option value="4">4 email campaigns per week</option>
                        <option value="5">5 email campaigns per week</option>
                    </select>
                </div>
            </div>
            
            <div class="form-row">
                <div class="form-group">
                    <label for="sms-count">SMS Count This Month</label>
                    <input type="number" id="sms-count" placeholder="Number of SMS messages" min="0">
                </div>
                
                <div class="form-group">
                    <label for="custom-requests">Custom Client Requests</label>
                    <input type="text" id="custom-requests" placeholder="Special requests or notes">
                </div>
            </div>
            
            <div class="button-container" style="display: flex; gap: 10px; margin-top: 15px;">
                <button id="populate-btn" class="submit-btn" style="flex: 1;">Generate Calendar</button>
                <button id="export-btn" class="submit-btn" style="flex: 1; background-color: #2ecc71;">Export to Airtable</button>
            </div>
        </div>
        
        <!-- Calendar always visible -->
        <div class="calendar-container">
            <div class="month-header">
                <h2 id="month-title" class="month-title">April 2025</h2>
                <div class="calendar-controls">
                    <button class="calendar-control-btn" id="prev-month">&lt; Previous</button>
                    <button class="calendar-control-btn" id="next-month">Next &gt;</button>
                    <button class="calendar-control-btn" id="add-camp">+ Add</button>
                </div>
            </div>
            
            <div class="days-header">
                <div>Sun</div>
                <div>Mon</div>
                <div>Tue</div>
                <div>Wed</div>
                <div>Thu</div>
                <div>Fri</div>
                <div>Sat</div>
            </div>
            
            <div id="calendar-grid" class="calendar-grid">
                <!-- Calendar days will be generated here -->
            </div>
        </div>
        
        <div id="status-message" class="status-message"></div>
    </div>
    
    <div id="loading-overlay" class="loading-overlay">
        <div class="loading-spinner"></div>
    </div>
    
    <script>
        document.addEventListener('DOMContentLoaded', function() {
            let currentDate = new Date();
            let currentMonth = currentDate.getMonth();
            let currentYear = currentDate.getFullYear();
            
            // Initial calendar render
            renderCalendar(currentMonth, currentYear);
            
            // Previous month button
            document.getElementById('prev-month').addEventListener('click', function() {
                currentMonth--;
                if (currentMonth < 0) {
                    currentMonth = 11;
                    currentYear--;
                }
                renderCalendar(currentMonth, currentYear);
            });
            
            // Next month button
            document.getElementById('next-month').addEventListener('click', function() {
                currentMonth++;
                if (currentMonth > 11) {
                    currentMonth = 0;
                    currentYear++;
                }
                renderCalendar(currentMonth, currentYear);
            });
            
            // Add campaign button
            document.getElementById('add-camp').addEventListener('click', createNewCampaign);
            
            // Populate button
            document.getElementById('populate-btn').addEventListener('click', function() {
                populateCalendar(currentMonth, currentYear);
            });
        });
        
        function renderCalendar(month, year) {
            const calendarGrid = document.getElementById('calendar-grid');
            calendarGrid.innerHTML = '';
            
            const monthTitle = document.getElementById('month-title');
            const months = [
                'January', 'February', 'March', 'April', 'May', 'June',
                'July', 'August', 'September', 'October', 'November', 'December'
            ];
            monthTitle.textContent = `${months[month]} ${year}`;
            
            const firstDay = new Date(year, month, 1);
            const lastDay = new Date(year, month + 1, 0);
            const daysInMonth = lastDay.getDate();
            
            // Fill in empty days from previous month
            const firstDayOfWeek = firstDay.getDay();
            for (let i = 0; i < firstDayOfWeek; i++) {
                const emptyDay = document.createElement('div');
                emptyDay.className = 'calendar-day empty-day';
                calendarGrid.appendChild(emptyDay);
            }
            
            // Fill in days of the month
            for (let day = 1; day <= daysInMonth; day++) {
                const currentDate = new Date(year, month, day);
                const dayOfWeek = currentDate.getDay();
                
                const dayDiv = document.createElement('div');
                dayDiv.className = 'calendar-day';
                dayDiv.setAttribute('data-date', `${year}-${month+1}-${day}`);
                
                const dateNumber = document.createElement('div');
                dateNumber.className = 'date-number';
                dateNumber.textContent = day;
                dayDiv.appendChild(dateNumber);
                
                // For mobile, add day of week
                const mobileDayName = document.createElement('div');
                mobileDayName.className = 'mobile-day';
                mobileDayName.textContent = ['Sun', 'Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat'][dayOfWeek];
                if (window.innerWidth <= 768) {
                    dayDiv.appendChild(mobileDayName);
                }
                
                calendarGrid.appendChild(dayDiv);
            }
            
            // Fill in empty days for next month to complete the grid
            const lastDayOfWeek = lastDay.getDay();
            if (lastDayOfWeek < 6) {
                for (let i = lastDayOfWeek + 1; i <= 6; i++) {
                    const emptyDay = document.createElement('div');
                    emptyDay.className = 'calendar-day empty-day';
                    calendarGrid.appendChild(emptyDay);
                }
            }
        }
        
        async function populateCalendar(month, year) {
            const websiteUrl = document.getElementById('website-url').value;
            const campaignsPerWeek = document.getElementById('campaigns-per-week').value;
            const smsCount = document.getElementById('sms-count').value;
            const customRequests = document.getElementById('custom-requests').value;
            
            if (!websiteUrl) {
                showStatusMessage('Please enter your website URL', 'error');
                return;
            }
            
            document.getElementById('loading-overlay').style.display = 'flex';
            
            try {
                const webhookData = {
                    websiteUrl,
                    campaignsPerWeek: parseInt(campaignsPerWeek) || 3,
                    smsCount: parseInt(smsCount) || 0,
                    customRequests,
                    month: month + 1,
                    year,
                    campaignType: 'email' // Default campaign type
                };
                
                // Get webhook URL
                const webhookUrl = 'https://hook.us2.make.com/dj8zoi4i6hvhr7liqtveco34h07bo3e9';
                
                console.log('Sending data to webhook:', webhookData);
                
                // Make sure to wait for the fetch to complete
                const response = await fetch(webhookUrl, {
                    method: 'POST',
                    headers: {
                        'Content-Type': 'application/json'
                    },
                    body: JSON.stringify(webhookData)
                });
                
                console.log('Response status:', response.status);
                
                // Clear any existing campaigns
                clearExistingCampaigns();
                
                // Try to get response content
                const responseText = await response.text();
                console.log('Raw response text:', responseText);
                
                // For debugging - show exact response in console
                console.log('Response text length:', responseText.length);
                console.log('Response text first 100 chars:', responseText.substring(0, 100));
                
                // Handle empty response
                if (!responseText || !responseText.trim()) {
                    console.warn('Empty response from webhook');
                    showStatusMessage('Empty response received from webhook', 'error');
                    return;
                }
                
                // Try parsing the response as JSON
                let campaignData;
                try {
                    // Try parsing with extra safety
                    const cleanedResponse = responseText.trim();
                    console.log('Cleaned response first 100 chars:', cleanedResponse.substring(0, 100));
                    
                    // Check if response starts with markdown code block and extract JSON
                    if (cleanedResponse.startsWith('```json')) {
                        const jsonContent = cleanedResponse.replace(/^```json/, '').replace(/```$/, '').trim();
                        console.log('Extracted JSON from markdown:', jsonContent.substring(0, 100));
                        campaignData = JSON.parse(jsonContent);
                    } else {
                        // Regular JSON parsing
                        campaignData = JSON.parse(cleanedResponse);
                    }
                    
                    console.log('Parsed campaign data:', campaignData);
                    
                    // Validate that it's an array
                    if (!Array.isArray(campaignData)) {
                        console.warn('Webhook response is not an array:', campaignData);
                        
                        // Check if it might be wrapped in another object
                        if (campaignData && typeof campaignData === 'object') {
                            // Look for array properties
                            const possibleArrays = Object.values(campaignData).filter(val => Array.isArray(val));
                            if (possibleArrays.length > 0) {
                                campaignData = possibleArrays[0];
                                console.log('Found array inside response object:', campaignData);
                            } else {
                                showStatusMessage('Webhook response format is incorrect (not an array)', 'error');
                                return;
                            }
                        } else {
                            showStatusMessage('Webhook response format is incorrect (not an array)', 'error');
                            return;
                        }
                    }
                    
                    // Ensure campaigns have campaignType set
                    campaignData = campaignData.map(campaign => {
                        if (!campaign.campaignType) {
                            // Set default type based on name
                            campaign.campaignType = (campaign.campaignName || '').toLowerCase().includes('sms') ? 'sms' : 'email';
                        }
                        return campaign;
                    });
                } catch (e) {
                    console.error('Failed to parse response as JSON:', e);
                    showStatusMessage('Cannot parse webhook response as JSON', 'error');
                    
                    // If webhook fails, generate sample data based on campaignsPerWeek
                    console.log('Using generated sample data');
                    campaignData = generateSampleCampaigns(parseInt(campaignsPerWeek) || 2, month, year);
                }
                
                // Check for empty array
                if (campaignData.length === 0) {
                    console.warn('Webhook returned empty array');
                    showStatusMessage('No campaigns in webhook response', 'error');
                    return;
                }
                
                // Add campaigns to calendar
                addCampaignsToCalendar(campaignData);
                
                // Enable draggable functionality
                enableDraggable();
                
                showStatusMessage(`Calendar populated with ${campaignData.length} campaigns`, 'success');
            } catch (error) {
                console.error('Error connecting to webhook:', error);
                showStatusMessage('Error connecting to webhook', 'error');
            } finally {
                // Hide loading overlay
                document.getElementById('loading-overlay').style.display = 'none';
                
                // Enable export button if we have campaigns
                if (document.querySelectorAll('.campaign').length > 0) {
                    document.getElementById('export-btn').removeAttribute('disabled');
                } else {
                    document.getElementById('export-btn').setAttribute('disabled', 'disabled');
                }
            }
        }
        
        // Make campaign items draggable
        function enableDraggable() {
            const campaignItems = document.querySelectorAll('.campaign');
            
            campaignItems.forEach(item => {
                // Add draggable attribute
                item.setAttribute('draggable', 'true');
                
                // Add draggable styling
                item.style.cursor = 'grab';
                
                // Add drag start event
                item.addEventListener('dragstart', function(e) {
                    e.dataTransfer.setData('text/plain', item.getAttribute('data-campaign'));
                    e.dataTransfer.setData('sourceId', item.id || Date.now().toString());
                    setTimeout(() => {
                        item.style.opacity = '0.4';
                    }, 0);
                });
                
                // Add drag end event
                item.addEventListener('dragend', function() {
                    item.style.opacity = '1';
                });
            });
            
            // Make calendar days accept drops
            const calendarDays = document.querySelectorAll('.calendar-day:not(.empty-day)');
            
            calendarDays.forEach(day => {
                // Allow drop
                day.addEventListener('dragover', function(e) {
                    e.preventDefault();
                    day.style.backgroundColor = 'rgba(80, 80, 120, 0.7)';
                });
                
                // Reset highlight on drag leave
                day.addEventListener('dragleave', function() {
                    day.style.backgroundColor = 'rgba(48, 47, 66, 0.5)';
                });
                
                // Handle drop
                day.addEventListener('drop', function(e) {
                    e.preventDefault();
                    day.style.backgroundColor = 'rgba(48, 47, 66, 0.5)';
                    
                    // Get dropped campaign data
                    const campaignData = e.dataTransfer.getData('text/plain');
                    const sourceId = e.dataTransfer.getData('sourceId');
                    
                    // Find source element to remove it
                    const sourceElement = document.getElementById(sourceId);
                    if (sourceElement) {
                        sourceElement.remove();
                    } else {
                        // If ID wasn't set, find by data attribute (less reliable)
                        const possibleSources = document.querySelectorAll('.campaign');
                        possibleSources.forEach(source => {
                            if (source.getAttribute('data-campaign') === campaignData) {
                                source.remove();
                            }
                        });
                    }
                    
                    // Create new campaign element
                    const campaignDiv = document.createElement('div');
                    campaignDiv.className = 'campaign';
                    campaignDiv.id = 'campaign-' + Date.now();
                    
                    let campaign;
                    try {
                        campaign = JSON.parse(campaignData);
                    } catch (e) {
                        console.error('Error parsing campaign data:', e);
                        return;
                    }
                    
                    // Check if it's an SMS campaign
                    const isSMS = (campaign.campaignType === 'sms' || e.dataTransfer.getData('text/plain').toLowerCase().includes('"campaignType":"sms"'));
                    if (isSMS) {
                        campaignDiv.classList.add('sms');
                    }
                    
                    const titleDiv = document.createElement('div');
                    titleDiv.className = 'campaign-title';
                    titleDiv.textContent = campaign.title || campaign.campaignName || 'Campaign';
                    campaignDiv.appendChild(titleDiv);
                    
                    if (campaign.offer) {
                        const offerDiv = document.createElement('div');
                        offerDiv.className = 'campaign-offer';
                        offerDiv.textContent = campaign.offer;
                        campaignDiv.appendChild(offerDiv);
                    }
                    
                    // Set data attribute for future moves
                    campaignDiv.setAttribute('data-campaign', campaignData);
                    
                    // Add click event to show campaign details
                    campaignDiv.addEventListener('click', function(e) {
                        // Don't open details if clicking on editable content
                        if (e.target.contentEditable === 'true') {
                            return;
                        }
                        
                        // Get updated campaign data
                        const campaignData = JSON.parse(campaignDiv.getAttribute('data-campaign'));
                        
                        // Show dialog with all campaign details
                        showCampaignDetails(campaignData, campaignDiv);
                    });
                    
                    // Add to target day
                    day.appendChild(campaignDiv);
                    
                    // Re-enable draggable on the new element
                    campaignDiv.setAttribute('draggable', 'true');
                    campaignDiv.style.cursor = 'grab';
                    
                    campaignDiv.addEventListener('dragstart', function(e) {
                        e.dataTransfer.setData('text/plain', campaignDiv.getAttribute('data-campaign'));
                        e.dataTransfer.setData('sourceId', campaignDiv.id);
                        setTimeout(() => {
                            campaignDiv.style.opacity = '0.4';
                        }, 0);
                    });
                    
                    campaignDiv.addEventListener('dragend', function() {
                        campaignDiv.style.opacity = '1';
                    });
                });
            });
        }
        
        function clearExistingCampaigns() {
            const campaigns = document.querySelectorAll('.campaign');
            campaigns.forEach(campaign => campaign.remove());
        }
        
        function addCampaignsToCalendar(campaignData) {
            campaignData.forEach((campaign, index) => {
                const dateStr = campaign.date; // Expected format: "YYYY-MM-DD" or "YYYY-M-D"
                if (!dateStr) {
                    console.error('Campaign missing date:', campaign);
                    return;
                }
                
                const dateParts = dateStr.split('-');
                if (dateParts.length !== 3) {
                    console.error('Invalid date format:', dateStr);
                    return;
                }
                
                const year = parseInt(dateParts[0]);
                const month = parseInt(dateParts[1]);
                const day = parseInt(dateParts[2]);
                
                // Find the day element
                const dayElement = document.querySelector(`.calendar-day[data-date="${year}-${month}-${day}"]`);
                
                if (dayElement) {
                    const campaignDiv = document.createElement('div');
                    campaignDiv.className = 'campaign';
                    campaignDiv.id = `campaign-${index}-${Date.now()}`;
                    
                    // Check if it's an SMS campaign and add appropriate class
                    const isSMS = (campaign.campaignType === 'sms' || (campaign.title || campaign.campaignName || '').toLowerCase().includes('sms'));
                    if (isSMS) {
                        campaignDiv.classList.add('sms');
                    }
                    
                    // Make title editable
                    const titleDiv = document.createElement('div');
                    titleDiv.className = 'campaign-title';
                    titleDiv.contentEditable = true;
                    titleDiv.textContent = campaign.title || campaign.campaignName || 'Campaign';
                    titleDiv.addEventListener('blur', function() {
                        // Update the data attribute when edited
                        const campaignData = JSON.parse(campaignDiv.getAttribute('data-campaign'));
                        campaignData.campaignName = titleDiv.textContent;
                        campaignDiv.setAttribute('data-campaign', JSON.stringify(campaignData));
                    });
                    campaignDiv.appendChild(titleDiv);
                    
                    // Make offer editable
                    if (campaign.offer || campaign.offer === null) {
                        const offerDiv = document.createElement('div');
                        offerDiv.className = 'campaign-offer';
                        offerDiv.contentEditable = true;
                        offerDiv.textContent = campaign.offer || 'Add offer details';
                        offerDiv.addEventListener('blur', function() {
                            // Update the data attribute when edited
                            const campaignData = JSON.parse(campaignDiv.getAttribute('data-campaign'));
                            campaignData.offer = offerDiv.textContent;
                            campaignDiv.setAttribute('data-campaign', JSON.stringify(campaignData));
                        });
                        campaignDiv.appendChild(offerDiv);
                    }
                    
                    // Add delete button
                    const deleteButton = document.createElement('span');
                    deleteButton.className = 'delete-button';
                    deleteButton.innerHTML = '&times;';
                    deleteButton.title = 'Delete this campaign';
                    deleteButton.addEventListener('click', function(e) {
                        e.stopPropagation();
                        if (confirm('Delete this campaign?')) {
                            campaignDiv.remove();
                        }
                    });
                    campaignDiv.appendChild(deleteButton);
                    
                    // Add data attribute with all campaign details
                    campaignDiv.setAttribute('data-campaign', JSON.stringify(campaign));
                    
                    // Add click event to show more details
                    campaignDiv.addEventListener('click', function(e) {
                        // Don't open details if clicking on editable content
                        if (e.target.contentEditable === 'true') {
                            return;
                        }
                        
                        // Get updated campaign data
                        const campaignData = JSON.parse(campaignDiv.getAttribute('data-campaign'));
                        
                        // Show dialog with all campaign details
                        showCampaignDetails(campaignData, campaignDiv);
                    });
                    
                    dayElement.appendChild(campaignDiv);
                } else {
                    console.warn(`Day element not found for date: ${dateStr}`);
                }
            });
        }
        
        // Show campaign details in a popup
        // Create a new campaign
        function createNewCampaign() {
            const campaign = {
                title: "New Campaign",
                campaignName: "New Campaign",
                offer: "",
                campaignIdea: "",
                websiteLink: ""
            };
            
            const overlay = document.createElement('div');
            overlay.className = 'modal-overlay';
            
            const modal = document.createElement('div');
            modal.className = 'modal-content';
            
            const closeBtn = document.createElement('span');
            closeBtn.className = 'modal-close';
            closeBtn.innerHTML = '&times;';
            closeBtn.onclick = () => document.body.removeChild(overlay);
            
            // Date picker
            const dateGroup = document.createElement('div');
            dateGroup.className = 'form-group';
            
            const dateLabel = document.createElement('label');
            dateLabel.textContent = 'Date:';
            
            const dateInput = document.createElement('input');
            dateInput.type = 'date';
            
            // Default date
            const title = document.getElementById('month-title').textContent;
            const [monthName, year] = title.split(' ');
            const months = ['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December'];
            const monthIndex = months.indexOf(monthName);
            dateInput.value = new Date(parseInt(year), monthIndex, 1).toISOString().split('T')[0];
            
            dateGroup.appendChild(dateLabel);
            dateGroup.appendChild(dateInput);
            
            const btn = document.createElement('button');
            btn.className = 'submit-btn';
            btn.textContent = 'Continue';
            btn.onclick = function() {
                const date = new Date(dateInput.value);
                // Add a day to fix the off-by-one date issue
                date.setDate(date.getDate() + 1);
                campaign.date = `${date.getFullYear()}-${date.getMonth() + 1}-${date.getDate()}`;
                document.body.removeChild(overlay);
                showCampaignDetails(campaign, null, true);
            };
            
            modal.appendChild(closeBtn);
            modal.appendChild(dateGroup);
            modal.appendChild(btn);
            
            overlay.appendChild(modal);
            document.body.appendChild(overlay);
        }
        
        function showCampaignDetails(campaign, campaignElement, isNew = false) {
            const overlay = document.createElement('div');
            overlay.className = 'modal-overlay';
            
            const modal = document.createElement('div');
            modal.className = 'modal-content';
            
            const closeButton = document.createElement('span');
            closeButton.className = 'modal-close';
            closeButton.innerHTML = '&times;';
            closeButton.onclick = () => document.body.removeChild(overlay);
            
            // Create form for campaign details
            const form = document.createElement('div');
            form.className = 'campaign-details-form';
            
            // Title
            const titleGroup = document.createElement('div');
            titleGroup.className = 'form-group';
            const titleLabel = document.createElement('label');
            titleLabel.textContent = 'Campaign Name:';
            const titleInput = document.createElement('input');
            titleInput.type = 'text';
            titleInput.value = campaign.title || campaign.campaignName || '';
            titleGroup.appendChild(titleLabel);
            titleGroup.appendChild(titleInput);
            
            // Offer
            const offerGroup = document.createElement('div');
            offerGroup.className = 'form-group';
            const offerLabel = document.createElement('label');
            offerLabel.textContent = 'Offer:';
            const offerInput = document.createElement('input');
            offerInput.type = 'text';
            offerInput.value = campaign.offer || '';
            offerGroup.appendChild(offerLabel);
            offerGroup.appendChild(offerInput);
            
            // Campaign Idea
            const ideaGroup = document.createElement('div');
            ideaGroup.className = 'form-group';
            const ideaLabel = document.createElement('label');
            ideaLabel.textContent = 'Campaign Idea:';
            const ideaTextarea = document.createElement('textarea');
            ideaTextarea.value = campaign.campaignIdea || campaign.idea || '';
            ideaGroup.appendChild(ideaLabel);
            ideaGroup.appendChild(ideaTextarea);
            
            // Website Link
            const linkGroup = document.createElement('div');
            linkGroup.className = 'form-group';
            const linkLabel = document.createElement('label');
            linkLabel.textContent = 'Website Link:';
            const linkInput = document.createElement('input');
            linkInput.type = 'text';
            linkInput.value = campaign.websiteLink || '';
            linkGroup.appendChild(linkLabel);
            linkGroup.appendChild(linkInput);
            
            // Save button
            const saveButton = document.createElement('button');
            saveButton.className = 'submit-btn';
            saveButton.textContent = isNew ? 'Create Campaign' : 'Save Changes';
            saveButton.onclick = function() {
                // Update campaign data
                campaign.campaignName = titleInput.value;
                campaign.title = titleInput.value;
                campaign.offer = offerInput.value;
                campaign.campaignIdea = ideaTextarea.value;
                campaign.websiteLink = linkInput.value;
                campaign.campaignType = campaign.campaignName.toLowerCase().includes('sms') ? 'sms' : 'email';
                
                if (isNew) {
                    // Create new campaign
                    const [year, month, day] = campaign.date.split('-').map(Number);
                    const dayEl = document.querySelector(`.calendar-day[data-date="${year}-${month}-${day}"]`);
                    
                    if (dayEl) {
                        // Create element
                        const newCamp = document.createElement('div');
                        newCamp.className = 'campaign';
                        // Check if it's an SMS campaign
                        const isSMS = campaign.campaignType === 'sms' || campaign.campaignName.toLowerCase().includes('sms');
                        if (isSMS) {
                            newCamp.classList.add('sms');
                        }
                        
                        const title = document.createElement('div');
                        title.className = 'campaign-title';
                        title.contentEditable = true;
                        title.textContent = campaign.campaignName;
                        
                        const offer = document.createElement('div');
                        offer.className = 'campaign-offer';
                        offer.contentEditable = true;
                        offer.textContent = campaign.offer || 'Add offer';
                        
                        // Set data and events
                        newCamp.setAttribute('data-campaign', JSON.stringify(campaign));
                        newCamp.setAttribute('draggable', 'true');
                        
                        newCamp.addEventListener('click', e => {
                            if (e.target.contentEditable !== 'true') {
                                showCampaignDetails(JSON.parse(newCamp.getAttribute('data-campaign')), newCamp);
                            }
                        });
                        
                        newCamp.addEventListener('dragstart', e => {
                            e.dataTransfer.setData('text/plain', newCamp.getAttribute('data-campaign'));
                            e.dataTransfer.setData('sourceId', newCamp.id);
                            setTimeout(() => newCamp.style.opacity = '0.4', 0);
                        });
                        
                        newCamp.addEventListener('dragend', () => newCamp.style.opacity = '1');
                        
                        // Append elements
                        newCamp.appendChild(title);
                        newCamp.appendChild(offer);
                        dayEl.appendChild(newCamp);
                        
                        showStatusMessage('Campaign created', 'success');
                    } else {
                        showStatusMessage('Date not in view', 'error');
                    }
                } else {
                    // Update existing campaign
                    campaignElement.setAttribute('data-campaign', JSON.stringify(campaign));
                    
                    // Update visible content
                    const titleEl = campaignElement.querySelector('.campaign-title');
                    if (titleEl) titleEl.textContent = campaign.campaignName;
                    
                    const offerEl = campaignElement.querySelector('.campaign-offer');
                    if (offerEl) offerEl.textContent = campaign.offer;
                    
                    if (campaign.campaignName.toLowerCase().includes('sms')) {
                        campaignElement.classList.add('sms');
                    } else {
                        campaignElement.classList.remove('sms');
                    }
                }
                
                document.body.removeChild(overlay);
            };
            
            // Add all elements to form
            form.appendChild(titleGroup);
            form.appendChild(offerGroup);
            form.appendChild(ideaGroup);
            form.appendChild(linkGroup);
            form.appendChild(saveButton);
            
            // Add elements to modal
            modal.appendChild(closeButton);
            modal.appendChild(form);
            
            // Add modal to overlay and overlay to body
            overlay.appendChild(modal);
            document.body.appendChild(overlay);
        }
        
        function showStatusMessage(message, type) {
            const statusElement = document.getElementById('status-message');
            statusElement.textContent = message;
            statusElement.className = `status-message ${type === 'error' ? 'error-message' : 'success-message'}`;
            statusElement.style.display = 'block';
            
            // Auto-hide after 5 seconds
            setTimeout(() => {
                statusElement.style.display = 'none';
            }, 5000);
        }
        
        function generateSampleCampaigns(campaignsPerWeek, month, year) {
            // Get number of days in the displayed month
            const daysInMonth = new Date(year, month + 1, 0).getDate();
            
            // Simply multiply by 4 to get total monthly campaigns as requested
            const totalCampaigns = campaignsPerWeek * 4;
            
            // Create evenly spaced days for campaigns
            const campaigns = [];
            
            // Campaign types
            const campaignTypes = [
                "Email Newsletter", "Product Announcement", "Promotional Email", 
                "Blog Post", "Social Media Campaign", "Seasonal Promotion", 
                "Event Announcement", "Customer Spotlight", "Educational Content"
            ];
            
            // Distribute campaigns evenly throughout the month
            for (let i = 0; i < totalCampaigns; i++) {
                // Calculate day position (1 to daysInMonth)
                // This spreads campaigns evenly across the month
                const day = Math.floor(1 + (i * daysInMonth / totalCampaigns));
                const typeIndex = i % campaignTypes.length;
                
                campaigns.push({
                    date: `${year}-${month + 1}-${day}`,
                    campaignName: campaignTypes[typeIndex],
                    offer: typeIndex % 3 === 0 ? "15% discount" : typeIndex % 3 === 1 ? "Free shipping" : "Buy one get one free",
                    campaignIdea: `${campaignTypes[typeIndex]} featuring newest products and customer testimonials`,
                    websiteLink: `https://example.com/${campaignTypes[typeIndex].toLowerCase().replace(/\s+/g, '-')}`,
                    campaignType: 'email'
                });
            }
            
            // Add SMS campaigns if specified
            const smsCount = document.getElementById('sms-count').value;
            if (smsCount && parseInt(smsCount) > 0) {
                for (let i = 0; i < parseInt(smsCount); i++) {
                    // Offset SMS campaigns slightly from regular campaigns
                    const day = Math.floor(3 + (i * daysInMonth / parseInt(smsCount)));
                    
                    campaigns.push({
                        date: `${year}-${month + 1}-${Math.min(day, daysInMonth)}`,
                        campaignName: "SMS Campaign",
                        offer: "SMS-exclusive offer",
                        campaignIdea: "Short-term flash sale announcement via SMS",
                        websiteLink: "https://example.com/sms-promo",
                        campaignType: 'sms'
                    });
                }
            }
            
            return campaigns;
        }
    </script>
</body>
</html>
