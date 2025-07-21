# AT-Functions-Analysis
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AT-AC Integration Deep Analysis Dashboard</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        
        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
            background: #0a0e1a;
            color: #e0e6ed;
            line-height: 1.6;
        }
        
        .container {
            max-width: 1800px;
            margin: 0 auto;
            padding: 20px;
        }
        
        header {
            background: linear-gradient(135deg, #1e3a8a 0%, #dc2626 100%);
            padding: 40px 0;
            margin-bottom: 40px;
            box-shadow: 0 4px 20px rgba(0, 0, 0, 0.5);
            text-align: center;
        }
        
        h1 {
            font-size: 2.8em;
            font-weight: 700;
            text-shadow: 0 2px 10px rgba(0,0,0,0.3);
            margin-bottom: 10px;
        }
        
        .subtitle {
            font-size: 1.2em;
            opacity: 0.9;
        }
        
        .controls {
            display: flex;
            gap: 20px;
            margin-bottom: 30px;
            flex-wrap: wrap;
            justify-content: center;
        }
        
        .control-group {
            background: #1e293b;
            padding: 15px 20px;
            border-radius: 12px;
            border: 1px solid #334155;
        }
        
        select {
            background: #0f172a;
            border: 2px solid #334155;
            border-radius: 8px;
            color: #e0e6ed;
            padding: 8px 12px;
            font-size: 1em;
            margin-left: 10px;
            cursor: pointer;
        }
        
        .tabs {
            display: flex;
            gap: 10px;
            margin-bottom: 30px;
            flex-wrap: wrap;
            justify-content: center;
        }
        
        .tab {
            padding: 12px 24px;
            background: #1e293b;
            border: 2px solid #334155;
            border-radius: 10px;
            cursor: pointer;
            transition: all 0.3s ease;
            font-weight: 500;
        }
        
        .tab:hover {
            background: #334155;
            transform: translateY(-2px);
        }
        
        .tab.active {
            background: linear-gradient(135deg, #8b5cf6 0%, #7c3aed 100%);
            border-color: transparent;
            color: white;
        }
        
        .tab-content {
            display: none;
        }
        
        .tab-content.active {
            display: block;
            animation: fadeIn 0.5s ease;
        }
        
        @keyframes fadeIn {
            from { 
                opacity: 0; 
                transform: translateY(20px); 
            }
            to { 
                opacity: 1; 
                transform: translateY(0); 
            }
        }
        
        .card {
            background: #1e293b;
            border-radius: 15px;
            padding: 25px;
            margin-bottom: 25px;
            border: 1px solid #334155;
            box-shadow: 0 4px 15px rgba(0,0,0,0.2);
        }
        
        h2 {
            margin-bottom: 20px;
            font-size: 1.8em;
            color: #a78bfa;
        }
        
        h3 {
            margin-bottom: 15px;
            color: #c4b5fd;
        }
        
        table {
            width: 100%;
            border-collapse: collapse;
        }
        
        th {
            background: #0f172a;
            padding: 12px;
            text-align: left;
            font-weight: 600;
            border-bottom: 2px solid #334155;
            color: #a78bfa;
        }
        
        td {
            padding: 12px;
            border-bottom: 1px solid #334155;
        }
        
        tr:hover {
            background: rgba(139, 92, 246, 0.05);
        }
        
        .impact-high { color: #ef4444; }
        .impact-medium { color: #fbbf24; }
        .impact-low { color: #22c55e; }
        
        .dependency-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
            gap: 20px;
            margin-top: 20px;
        }
        
        .dependency-card {
            background: #0f172a;
            border: 2px solid #334155;
            border-radius: 12px;
            padding: 20px;
            transition: all 0.3s ease;
        }
        
        .dependency-card:hover {
            border-color: #8b5cf6;
            transform: translateY(-2px);
        }
        
        .button {
            background: linear-gradient(135deg, #8b5cf6 0%, #7c3aed 100%);
            color: white;
            border: none;
            padding: 12px 25px;
            border-radius: 8px;
            cursor: pointer;
            font-weight: 600;
            transition: all 0.3s ease;
            font-size: 1em;
        }
        
        .button:hover {
            transform: translateY(-2px);
            box-shadow: 0 5px 20px rgba(139, 92, 246, 0.4);
        }
        
        .insight-box {
            background: rgba(139, 92, 246, 0.1);
            border-left: 4px solid #8b5cf6;
            padding: 15px;
            margin: 15px 0;
            border-radius: 0 8px 8px 0;
        }
        
        .modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.8);
            z-index: 1000;
        }
        
        .modal.active {
            display: flex;
            align-items: center;
            justify-content: center;
        }
        
        .modal-content {
            background: #1e293b;
            border-radius: 15px;
            padding: 30px;
            max-width: 800px;
            width: 90%;
            max-height: 90vh;
            overflow-y: auto;
            box-shadow: 0 10px 40px rgba(0, 0, 0, 0.5);
        }
        
        .close-btn {
            background: none;
            border: none;
            color: #94a3b8;
            font-size: 1.5em;
            cursor: pointer;
            float: right;
        }
        
        .close-btn:hover {
            color: #e0e6ed;
        }
        
        .function-matrix {
            display: grid;
            grid-template-columns: repeat(5, 1fr);
            gap: 2px;
            margin-top: 20px;
            background: #334155;
            padding: 2px;
            border-radius: 8px;
        }
        
        .matrix-cell {
            background: #0f172a;
            padding: 15px;
            text-align: center;
            font-size: 0.9em;
            transition: all 0.3s ease;
            cursor: pointer;
            position: relative;
        }
        
        .matrix-cell:hover {
            background: #1e293b;
            z-index: 1;
            transform: scale(1.05);
        }
        
        .matrix-header {
            background: #1e293b;
            font-weight: 600;
            color: #a78bfa;
            cursor: default;
        }
        
        .transfer-ac {
            background: rgba(59, 130, 246, 0.2);
            border: 1px solid #3b82f6;
        }
        
        .retain-at {
            background: rgba(220, 38, 38, 0.2);
            border: 1px solid #dc2626;
        }
        
        .split-function {
            background: linear-gradient(135deg, rgba(59, 130, 246, 0.2) 50%, rgba(220, 38, 38, 0.2) 50%);
            border: 1px solid #8b5cf6;
        }
        
        .timeline-container {
            position: relative;
            padding: 40px 0;
            margin-top: 20px;
        }
        
        .timeline-track {
            height: 6px;
            background: #334155;
            position: relative;
            border-radius: 3px;
        }
        
        .timeline-milestone {
            position: absolute;
            top: -20px;
            transform: translateX(-50%);
            background: #8b5cf6;
            color: white;
            padding: 8px 16px;
            border-radius: 20px;
            font-size: 0.85em;
            white-space: nowrap;
            box-shadow: 0 4px 15px rgba(139, 92, 246, 0.4);
        }
        
        .heatmap-cell {
            text-align: center;
            font-weight: 600;
        }
        
        .complexity-indicator {
            display: inline-block;
            width: 12px;
            height: 12px;
            border-radius: 50%;
            margin-right: 8px;
        }
        
        .complexity-low { background: #22c55e; }
        .complexity-medium { background: #fbbf24; }
        .complexity-high { background: #ef4444; }
        
        .progress-bar {
            width: 100%;
            height: 20px;
            background: #334155;
            border-radius: 10px;
            overflow: hidden;
            margin: 10px 0;
        }
        
        .progress-fill {
            height: 100%;
            background: linear-gradient(90deg, #8b5cf6 0%, #7c3aed 100%);
            transition: width 0.3s ease;
        }
        
        .risk-score {
            font-size: 2em;
            font-weight: bold;
            margin: 10px 0;
        }
        
        .workforce-card {
            background: #0f172a;
            border: 2px solid #334155;
            border-radius: 12px;
            padding: 20px;
            text-align: center;
        }
        
        .workforce-card h3 {
            color: #a78bfa;
            margin-bottom: 10px;
        }
        
        .workforce-card .number {
            font-size: 2.5em;
            font-weight: bold;
            margin: 10px 0;
        }
        
        @media (max-width: 768px) {
            .function-matrix {
                grid-template-columns: 1fr;
            }
            .dependency-grid {
                grid-template-columns: 1fr;
            }
        }
    </style>
</head>
<body>
    <header>
        <div class="container">
            <h1>AT-AC Integration Deep Analysis</h1>
            <p class="subtitle">Advanced Functional Dependencies & Impact Assessment</p>
        </div>
    </header>
    
    <div class="container">
        <!-- Notice for GitHub users -->
        <div class="card" style="background: rgba(139, 92, 246, 0.2); border-color: #8b5cf6;">
            <h3>📌 GitHub Version Notice</h3>
            <p>This is a static version optimized for GitHub. For the full interactive experience, please download and open this file locally in your browser, or visit the hosted version.</p>
        </div>

        <!-- Controls -->
        <div class="controls">
            <div class="control-group">
                <label>Primary Scenario:</label>
                <select id="primaryScenario">
                    <option value="1">Pure Bus Company</option>
                    <option value="2" selected>Operations Plus</option>
                    <option value="3">Integrated Delivery</option>
                    <option value="4">Strategic Alignment</option>
                    <option value="5">Status Quo Plus</option>
                </select>
            </div>
            <div class="control-group">
                <label>Compare With:</label>
                <select id="compareScenario">
                    <option value="none">None</option>
                    <option value="1">Pure Bus Company</option>
                    <option value="2">Operations Plus</option>
                    <option value="3" selected>Integrated Delivery</option>
                    <option value="4">Strategic Alignment</option>
                    <option value="5">Status Quo Plus</option>
                </select>
            </div>
        </div>

        <!-- Static Content for GitHub -->
        <div class="card">
            <h2>📊 Scenario Overview: Operations Plus</h2>
            <p>This dashboard provides comprehensive analysis of the AT-AC integration scenarios. Below is a snapshot of the key metrics for the default "Operations Plus" scenario.</p>
            
            <div class="dependency-grid" style="margin-top: 20px;">
                <div class="dependency-card">
                    <h3>Total Staff Movement</h3>
                    <div class="number impact-high" style="font-size: 2em; text-align: center; margin: 15px 0;">1,050 FTE</div>
                    <p style="text-align: center; color: #94a3b8;">Transferring to Auckland Council</p>
                </div>
                
                <div class="dependency-card">
                    <h3>Implementation Timeline</h3>
                    <div class="number impact-medium" style="font-size: 2em; text-align: center; margin: 15px 0;">18 Months</div>
                    <p style="text-align: center; color: #94a3b8;">Phased approach</p>
                </div>
                
                <div class="dependency-card">
                    <h3>Overall Risk Level</h3>
                    <div class="number impact-medium" style="font-size: 2em; text-align: center; margin: 15px 0;">MEDIUM-HIGH</div>
                    <p style="text-align: center; color: #94a3b8;">Manageable with mitigation</p>
                </div>
            </div>
        </div>

        <!-- Function Matrix -->
        <div class="card">
            <h2>🔗 Functional Dependencies Map</h2>
            <p style="margin-bottom: 20px;">Understanding how functions depend on each other and the impact of transfers</p>
            
            <div class="function-matrix">
                <div class="matrix-cell matrix-header">Public Transport</div>
                <div class="matrix-cell matrix-header">Strategy</div>
                <div class="matrix-cell matrix-header">Operations</div>
                <div class="matrix-cell matrix-header">Capital Delivery</div>
                <div class="matrix-cell matrix-header">Support Functions</div>
                
                <div class="matrix-cell retain-at">PT Operations</div>
                <div class="matrix-cell transfer-ac">Policy Development</div>
                <div class="matrix-cell transfer-ac">Road Network Ops</div>
                <div class="matrix-cell transfer-ac">Project Delivery</div>
                <div class="matrix-cell transfer-ac">Finance</div>
                
                <div class="matrix-cell split-function">PT Infrastructure Mgmt</div>
                <div class="matrix-cell transfer-ac">Strategic Planning</div>
                <div class="matrix-cell transfer-ac">Enforcement</div>
                <div class="matrix-cell transfer-ac">Road Maintenance</div>
                <div class="matrix-cell split-function">Customer Service</div>
                
                <div class="matrix-cell split-function">PT Infrastructure Delivery</div>
                <div class="matrix-cell transfer-ac">Project Development</div>
                <div class="matrix-cell transfer-ac">Road Corridor Access</div>
                <div class="matrix-cell split-function">Property</div>
                <div class="matrix-cell split-function">Communications</div>
                
                <div class="matrix-cell split-function">PT Network Planning</div>
                <div class="matrix-cell transfer-ac">Funding</div>
                <div class="matrix-cell transfer-ac">Parking</div>
                <div class="matrix-cell split-function">Design</div>
                <div class="matrix-cell split-function">Legal</div>
            </div>
            
            <div style="margin-top: 20px; display: flex; gap: 20px; justify-content: center;">
                <div><span class="complexity-indicator complexity-low"></span>AT Retain</div>
                <div><span class="complexity-indicator complexity-high"></span>AC Transfer</div>
                <div><span class="complexity-indicator complexity-medium"></span>Split Function</div>
            </div>
        </div>

        <!-- Impact Assessment -->
        <div class="card">
            <h2>💥 Impact Assessment Matrix</h2>
            <p style="margin-bottom: 20px;">Analyzing the cascading effects of functional transfers</p>
            
            <table>
                <thead>
                    <tr>
                        <th>Function Area</th>
                        <th>Service Impact</th>
                        <th>Staff Impact</th>
                        <th>System Impact</th>
                        <th>Customer Impact</th>
                        <th>Overall Risk</th>
                    </tr>
                </thead>
                <tbody>
                    <tr>
                        <td><strong>Public Transport</strong></td>
                        <td class="heatmap-cell impact-high">85%</td>
                        <td class="heatmap-cell impact-high">90%</td>
                        <td class="heatmap-cell impact-medium">60%</td>
                        <td class="heatmap-cell impact-high">95%</td>
                        <td class="heatmap-cell impact-high">80%</td>
                    </tr>
                    <tr>
                        <td><strong>Strategy</strong></td>
                        <td class="heatmap-cell impact-low">30%</td>
                        <td class="heatmap-cell impact-medium">50%</td>
                        <td class="heatmap-cell impact-medium">65%</td>
                        <td class="heatmap-cell impact-low">20%</td>
                        <td class="heatmap-cell impact-medium">48%</td>
                    </tr>
                    <tr>
                        <td><strong>Operations</strong></td>
                        <td class="heatmap-cell impact-medium">65%</td>
                        <td class="heatmap-cell impact-medium">55%</td>
                        <td class="heatmap-cell impact-high">75%</td>
                        <td class="heatmap-cell impact-medium">60%</td>
                        <td class="heatmap-cell impact-medium">62%</td>
                    </tr>
                    <tr>
                        <td><strong>Capital Delivery</strong></td>
                        <td class="heatmap-cell impact-medium">55%</td>
                        <td class="heatmap-cell impact-high">75%</td>
                        <td class="heatmap-cell impact-medium">60%</td>
                        <td class="heatmap-cell impact-medium">45%</td>
                        <td class="heatmap-cell impact-medium">64%</td>
                    </tr>
                    <tr>
                        <td><strong>Support Functions</strong></td>
                        <td class="heatmap-cell impact-low">35%</td>
                        <td class="heatmap-cell impact-medium">60%</td>
                        <td class="heatmap-cell impact-high">80%</td>
                        <td class="heatmap-cell impact-medium">50%</td>
                        <td class="heatmap-cell impact-medium">58%</td>
                    </tr>
                </tbody>
            </table>
            
            <div class="insight-box">
                <strong>Key Insight:</strong> Public Transport functions show the highest impact across all dimensions, particularly customer impact (95%). This suggests any scenario affecting PT operations requires careful transition planning.
            </div>
        </div>

        <!-- Critical Dependencies -->
        <div class="card">
            <h2>🕸️ Critical Dependencies</h2>
            <div class="dependency-grid">
                <div class="dependency-card">
                    <h3>PT Operations → PT Infrastructure</h3>
                    <p style="color: #94a3b8; margin-bottom: 10px;">Strong operational dependency</p>
                    <div style="display: flex; justify-content: space-between;">
                        <span>Dependency Strength:</span>
                        <span class="impact-high">HIGH</span>
                    </div>
                    <div style="display: flex; justify-content: space-between; margin-top: 10px;">
                        <span>Risk if Split:</span>
                        <span class="impact-high">85%</span>
                    </div>
                </div>
                
                <div class="dependency-card">
                    <h3>Strategic Planning → Funding</h3>
                    <p style="color: #94a3b8; margin-bottom: 10px;">Financial planning alignment</p>
                    <div style="display: flex; justify-content: space-between;">
                        <span>Dependency Strength:</span>
                        <span class="impact-high">HIGH</span>
                    </div>
                    <div style="display: flex; justify-content: space-between; margin-top: 10px;">
                        <span>Risk if Split:</span>
                        <span class="impact-medium">60%</span>
                    </div>
                </div>
                
                <div class="dependency-card">
                    <h3>Project Delivery → Design</h3>
                    <p style="color: #94a3b8; margin-bottom: 10px;">Technical integration required</p>
                    <div style="display: flex; justify-content: space-between;">
                        <span>Dependency Strength:</span>
                        <span class="impact-medium">MEDIUM</span>
                    </div>
                    <div style="display: flex; justify-content: space-between; margin-top: 10px;">
                        <span>Risk if Split:</span>
                        <span class="impact-medium">45%</span>
                    </div>
                </div>
            </div>
        </div>

        <!-- Timeline -->
        <div class="card">
            <h2>📅 Implementation Timeline</h2>
            <div class="timeline-container">
                <div class="timeline-track"></div>
                <div class="timeline-milestone" style="left: 5%;">Month 0: Approval</div>
                <div class="timeline-milestone" style="left: 20%;">Month 3: Design Phase</div>
                <div class="timeline-milestone" style="left: 35%;">Month 6: Phase 1 Transfer</div>
                <div class="timeline-milestone" style="left: 55%;">Month 12: Phase 2 Transfer</div>
                <div class="timeline-milestone" style="left: 75%;">Month 15: Integration</div>
                <div class="timeline-milestone" style="left: 90%;">Month 18: Optimization</div>
            </div>
            <div class="insight-box" style="margin-top: 60px;">
                <strong>Phased Approach:</strong> Lower risk through staged implementation, but longer overall timeline
            </div>
        </div>

        <!-- Risk Analysis -->
        <div class="card">
            <h2>⚠️ Risk Analysis Summary</h2>
            <table>
                <thead>
                    <tr>
                        <th>Risk Category</th>
                        <th>Likelihood</th>
                        <th>Impact</th>
                        <th>Risk Score</th>
                        <th>Mitigation Status</th>
                    </tr>
                </thead>
                <tbody>
                    <tr>
                        <td><strong>Service Continuity</strong></td>
                        <td class="impact-medium">Medium</td>
                        <td class="impact-high">High</td>
                        <td class="impact-medium">65</td>
                        <td>Mitigation Planned</td>
                    </tr>
                    <tr>
                        <td><strong>Staff Retention</strong></td>
                        <td class="impact-medium">Medium</td>
                        <td class="impact-medium">Medium</td>
                        <td class="impact-medium">55</td>
                        <td>Active Management</td>
                    </tr>
                    <tr>
                        <td><strong>System Integration</strong></td>
                        <td class="impact-high">High</td>
                        <td class="impact-medium">Medium</td>
                        <td class="impact-medium">70</td>
                        <td>Technical Review</td>
                    </tr>
                    <tr>
                        <td><strong>Customer Impact</strong></td>
                        <td class="impact-low">Low</td>
                        <td class="impact-medium">Medium</td>
                        <td class="impact-low">40</td>
                        <td>Communication Plan</td>
                    </tr>
                    <tr>
                        <td><strong>Financial</strong></td>
                        <td class="impact-medium">Medium</td>
                        <td class="impact-medium">Medium</td>
                        <td class="impact-medium">50</td>
                        <td>Budget Allocated</td>
                    </tr>
                </tbody>
            </table>
        </div>

        <!-- Workforce Summary -->
        <div class="card">
            <h2>👥 Workforce Transition Summary</h2>
            <div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 20px; margin: 20px 0;">
                <div class="workforce-card">
                    <h3>Total FTE</h3>
                    <div class="number">2,300</div>
                    <p>Current Workforce</p>
                </div>
                <div class="workforce-card">
                    <h3>To AT</h3>
                    <div class="number impact-low">1,250</div>
                    <p>Remaining with AT</p>
                </div>
                <div class="workforce-card">
                    <h3>To AC</h3>
                    <div class="number impact-high">1,050</div>
                    <p>Transferring to AC</p>
                </div>
                <div class="workforce-card">
                    <h3>At Risk</h3>
                    <div class="number impact-medium">65</div>
                    <p>Potential Redundancies</p>
                </div>
            </div>
        </div>

        <!-- Download Instructions -->
        <div class="card" style="background: rgba(34, 197, 94, 0.1); border-color: #22c55e;">
            <h2>💡 Full Interactive Version</h2>
            <p>To access the full interactive features of this dashboard:</p>
            <ol style="margin: 15px 0 15px 30px;">
                <li>Download this HTML file to your computer</li>
                <li>Open it in a modern web browser (Chrome, Firefox, Safari, or Edge)</li>
                <li>All interactive features will be enabled, including:
                    <ul style="margin-top: 10px;">
                        <li>Dynamic scenario comparisons</li>
                        <li>Interactive tabs and modals</li>
                        <li>Real-time data updates</li>
                        <li>Export functionality</li>
                    </ul>
                </li>
            </ol>
            <p>This static version shows the default "Operations Plus" scenario for reference.</p>
        </div>
    </div>

    <!-- Limited JavaScript for basic functionality -->
    <script>
        // Basic tab switching functionality
        document.addEventListener('DOMContentLoaded', function() {
            const tabs = document.querySelectorAll('.tab');
            tabs.forEach(tab => {
                tab.addEventListener('click', function() {
                    alert('Interactive features are disabled in the GitHub version. Please download and open locally for full functionality.');
                });
            });
            
            // Basic hover effects are handled by CSS
            // No dynamic content generation for GitHub compatibility
        });
    </script>
</body>
</html>
