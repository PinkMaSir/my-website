
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>假时间计算器</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://cdn.jsdelivr.net/npm/font-awesome@4.7.0/css/font-awesome.min.css" rel="stylesheet">
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        primary: '#3B82F6',
                        secondary: '#10B981',
                        accent: '#8B5CF6',
                        neutral: '#64748B',
                        light: '#F1F5F9',
                        light: '#F1F5F9',
                        dark: '#1E293B'
                    },
                    fontFamily: {
                        sans: ['Inter', 'system-ui', 'sans-serif'],
                    },
                }
            }
        }
    </script>
    <style type="text/tailwindcss">
        @layer utilities {
            .content-auto {
                content-visibility: auto;
            }
            .card-shadow {
                box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1), 0 4px 6px -2px rgba(0, 0, 0, 0.05);
            }
            .input-focus {
                @apply focus:ring-2 focus:ring-primary/50 focus:border-primary focus:outline-none;
            }
            .btn-hover {
                @apply hover:shadow-lg hover:-translate-y-0.5 transition-all duration-300;
            }
            .chart-grid {
                stroke: #e5e7eb;
                stroke-width: 1;
            }
            .chart-line {
                stroke: #3B82F6;
                stroke-width: 2;
                fill: none;
            }
            .chart-area {
                fill: rgba(59, 130, 246, 0.1);
            }
            .chart-point {
                fill: #3B82F6;
            }
            .chart-axis {
                stroke: #9ca3af;
                stroke-width: 1;
            }
            .chart-label {
                font-size: 12px;
                fill: #6b7280;
                text-anchor: middle;
            }
            .chart-title {
                font-size: 14px;
                font-weight: 600;
                fill: #1f2937;
            }
        }
    </style>
</head>
<body class="bg-gray-50 min-h-screen font-sans text-dark">
    <!-- 顶部导航 -->
    <header class="bg-white shadow-md sticky top-0 z-50 transition-all duration-300">
        <div class="container mx-auto px-4 py-4 flex justify-between items-center">
            <div class="flex items-center space-x-2">
                <i class="fa fa-calculator text-primary text-2xl"></i>
                <h1 class="text-xl md:text-2xl font-bold text-dark">假时间计算器</h1>
            </div>
            <div class="hidden md:flex items-center space-x-4">
                <button id="themeToggle" class="p-2 rounded-full hover:bg-gray-100 transition-colors">
                    <i class="fa fa-moon-o text-neutral"></i>
                </button>
                <button id="helpBtn" class="p-2 rounded-full hover:bg-gray-100 transition-colors">
                    <i class="fa fa-question text-neutral"></i>
                </button>
            </div>
            <button class="md:hidden p-2 rounded-full hover:bg-gray-100 transition-colors" id="mobileMenuBtn">
                <i class="fa fa-bars text-neutral"></i>
            </button>
        </div>
        <!-- 移动端菜单 -->
        <div id="mobileMenu" class="hidden md:hidden bg-white border-t px-4 py-2">
            <button id="mobileThemeToggle" class="w-full text-left p-2 hover:bg-gray-100 rounded">
                <i class="fa fa-moon-o mr-2 text-neutral"></i> 切换主题
            </button>
            <button id="mobileHelpBtn" class="w-full text-left p-2 hover:bg-gray-100 rounded">
                <i class="fa fa-question mr-2 text-neutral"></i> 帮助
            </button>
        </div>
    </header>

    <!-- 主内容区 -->
    <main class="container mx-auto px-4 py-8 max-w-6xl">
        <!-- 介绍卡片 -->
        <div class="bg-white rounded-xl p-6 mb-8 card-shadow transform hover:shadow-xl transition-shadow">
            <h2 class="text-xl font-semibold mb-3 text-primary">关于假时间计算</h2>
            <p class="text-neutral mb-4">
                假时间计算是用于工程领域的重要计算，特别是在涉及气体流动和储层分析的应用中。
                本工具可以根据输入的时间点、气体粘度和压缩系数，计算出相应的假时间值。
            </p>
            <div class="flex items-center text-sm text-neutral">
                <i class="fa fa-info-circle mr-2"></i>
                <span>请输入空格分隔的数值序列，例如：0.00 0.25 0.50 0.75</span>
            </div>
        </div>

        <div class="grid grid-cols-1 lg:grid-cols-3 gap-8">
            <!-- 输入区域 -->
            <div class="lg:col-span-1">
                <div class="bg-white rounded-xl p-6 card-shadow h-full">
                    <h2 class="text-xl font-semibold mb-6 text-primary flex items-center">
                        <i class="fa fa-pencil-square-o mr-2"></i>输入参数
                    </h2>
                    
                    <form id="calculatorForm" class="space-y-6">
                        <div>
                            <label for="timePoints" class="block text-sm font-medium text-gray-700 mb-1">
                                时间点（小时）
                            </label>
                            <input 
                                type="text" 
                                id="timePoints" 
                                class="w-full px-4 py-2 border border-gray-300 rounded-lg input-focus"
                                placeholder="例如：0.00 0.25 0.50 0.75"
                                required
                            >
                        </div>
                        
                        <div>
                            <label for="gasViscosity" class="block text-sm font-medium text-gray-700 mb-1">
                                气体粘度（mPa·s）
                            </label>
                            <input 
                                type="text" 
                                id="gasViscosity" 
                                class="w-full px-4 py-2 border border-gray-300 rounded-lg input-focus"
                                placeholder="例如：0.02 0.021 0.022 0.023"
                                required
                            >
                        </div>
                        
                        <div>
                            <label for="compressibility" class="block text-sm font-medium text-gray-700 mb-1">
                                压缩系数（MPa⁻¹）
                            </label>
                            <input 
                                type="text" 
                                id="compressibility" 
                                class="w-full px-4 py-2 border border-gray-300 rounded-lg input-focus"
                                placeholder="例如：0.001 0.0011 0.0012 0.0013"
                                required
                            >
                        </div>
                        
                        <button 
                            type="submit" 
                            class="w-full bg-primary text-white py-3 px-4 rounded-lg font-medium btn-hover flex items-center justify-center"
                        >
                            <i class="fa fa-calculator mr-2"></i> 计算假时间
                        </button>
                    </form>
                </div>
            </div>
            
            <!-- 结果和查询区域 -->
            <div class="lg:col-span-2 space-y-8">
                <!-- 结果展示 -->
                <div class="bg-white rounded-xl p-6 card-shadow">
                    <h2 class="text-xl font-semibold mb-6 text-primary flex items-center">
                        <i class="fa fa-table mr-2"></i>计算结果
                    </h2>
                    
                    <div id="resultsContainer" class="hidden">
                        <div class="overflow-x-auto">
                            <table class="min-w-full divide-y divide-gray-200">
                                <thead>
                                    <tr>
                                        <th class="px-4 py-3 bg-gray-50 text-left text-xs font-medium text-gray-500 uppercase tracking-wider rounded-tl-lg">时间点 (h)</th>
                                        <th class="px-4 py-3 bg-gray-50 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">时间间隔 (h)</th>
                                        <th class="px-4 py-3 bg-gray-50 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">时间间隔 (s)</th>
                                        <th class="px-4 py-3 bg-gray-50 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">A值</th>
                                        <th class="px-4 py-3 bg-gray-50 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">B值</th>
                                        <th class="px-4 py-3 bg-gray-50 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">C'值</th>
                                        <th class="px-4 py-3 bg-gray-50 text-left text-xs font-medium text-gray-500 uppercase tracking-wider rounded-tr-lg">假时间</th>
                                    </tr>
                                </thead>
                                <tbody id="resultsTableBody" class="bg-white divide-y divide-gray-200">
                                    <!-- 结果将通过JavaScript动态填充 -->
                                </tbody>
                            </table>
                        </div>
                        
                        <!-- 图表展示 (使用原生SVG) -->
                        <div class="mt-8">
                            <h3 class="text-lg font-medium mb-4">假时间与实际时间关系图</h3>
                            <div class="h-64 md:h-80 border border-gray-200 rounded-lg overflow-hidden">
                                <svg id="timeChart" class="w-full h-full" xmlns="http://www.w3.org/2000/svg">
                                    <!-- 图表内容将通过JavaScript动态生成 -->
                                    <text x="50%" y="50%" text-anchor="middle" class="chart-title">请计算数据以显示图表</text>
                                </svg>
                            </div>
                        </div>
                    </div>
                    
                    <div id="noResultsMessage" class="py-12 text-center">
                        <i class="fa fa-bar-chart text-gray-300 text-5xl mb-4"></i>
                        <p class="text-gray-500">请输入参数并点击计算按钮</p>
                    </div>
                    
                    <div id="errorMessage" class="hidden py-8 text-center text-red-500">
                        <i class="fa fa-exclamation-triangle mb-2"></i>
                        <p id="errorText"></p>
                    </div>
                </div>
                
                <!-- 查询区域 -->
                <div class="bg-white rounded-xl p-6 card-shadow">
                    <h2 class="text-xl font-semibold mb-6 text-primary flex items-center">
                        <i class="fa fa-search mr-2"></i>任意时间查询
                    </h2>
                    
                    <div id="queryContainer" class="space-y-4">
                        <div class="flex flex-col md:flex-row gap-4">
                            <input 
                                type="number" 
                                id="queryTime" 
                                step="any"
                                class="flex-1 px-4 py-2 border border-gray-300 rounded-lg input-focus"
                                placeholder="输入要查询的时间点（小时）"
                                disabled
                            >
                            <button 
                                id="queryButton" 
                                class="bg-secondary text-white py-3 px-6 rounded-lg font-medium btn-hover flex items-center justify-center"
                                disabled
                            >
                                <i class="fa fa-search mr-2"></i> 查询假时间
                            </button>
                        </div>
                        
                        <div id="queryResult" class="hidden p-4 rounded-lg bg-light">
                            <p class="font-medium">查询结果：</p>
                            <p id="queryResultText" class="mt-1"></p>
                        </div>
                        
                        <div id="queryError" class="hidden p-4 rounded-lg bg-red-50 border border-red-100">
                            <p class="text-red-600 flex items-center">
                                <i class="fa fa-exclamation-circle mr-2"></i>
                                <span id="queryErrorText"></span>
                            </p>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </main>

    <!-- 帮助模态框 -->
    <div id="helpModal" class="fixed inset-0 bg-black bg-opacity-50 z-50 hidden items-center justify-center">
        <div class="bg-white rounded-xl max-w-2xl w-full max-h-[80vh] overflow-y-auto m-4">
            <div class="p-6 border-b">
                <div class="flex justify-between items-center">
                    <h2 class="text-xl font-bold text-primary">使用帮助</h2>
                    <button id="closeHelpBtn" class="text-gray-500 hover:text-gray-700">
                        <i class="fa fa-times text-xl"></i>
                    </button>
                </div>
            </div>
            <div class="p-6 space-y-4">
                <div>
                    <h3 class="font-semibold text-lg mb-2">输入格式</h3>
                    <p>请输入空格分隔的数值序列，三个输入框的数值数量必须相同。</p>
                    <p class="mt-1 text-sm text-gray-600">例如：时间点输入 "0 0.5 1 1.5"，表示4个时间点</p>
                </div>
                <div>
                    <h3 class="font-semibold text-lg mb-2">计算公式</h3>
                    <ul class="list-disc pl-5 space-y-2">
                        <li>A = 1/(μ_g * C_t)</li>
                        <li>计算时间间隔并转换为秒</li>
                        <li>B值为相邻A值的平均值（第一个B值为A(1)/2）</li>
                        <li>C' = B * 时间间隔(秒)</li>
                        <li>假时间为C'值的累加</li>
                    </ul>
                </div>
                <div>
                    <h3 class="font-semibold text-lg mb-2">查询功能</h3>
                    <p>计算完成后，可以在查询区域输入任意时间点，系统会通过插值计算出对应的假时间。</p>
                    <p class="mt-1 text-sm text-gray-600">注意：查询时间必须在输入的时间点范围内</p>
                </div>
            </div>
        </div>
    </div>

    <footer class="bg-white border-t mt-12 py-6">
        <div class="container mx-auto px-4 text-center text-neutral">
            <p>假时间计算器 &copy; 2023</p>
            <p class="text-sm mt-1">一个用于工程计算的工具</p>
        </div>
    </footer>

    <script>
        // 全局变量存储计算结果
        let calculationResults = null;
        
        // DOM元素
        const form = document.getElementById('calculatorForm');
        const resultsContainer = document.getElementById('resultsContainer');
        const noResultsMessage = document.getElementById('noResultsMessage');
        const errorMessage = document.getElementById('errorMessage');
        const errorText = document.getElementById('errorText');
        const resultsTableBody = document.getElementById('resultsTableBody');
        const queryTimeInput = document.getElementById('queryTime');
        const queryButton = document.getElementById('queryButton');
        const queryResult = document.getElementById('queryResult');
        const queryResultText = document.getElementById('queryResultText');
        const queryError = document.getElementById('queryError');
        const queryErrorText = document.getElementById('queryErrorText');
        const mobileMenuBtn = document.getElementById('mobileMenuBtn');
        const mobileMenu = document.getElementById('mobileMenu');
        const helpBtn = document.getElementById('helpBtn');
        const mobileHelpBtn = document.getElementById('mobileHelpBtn');
        const helpModal = document.getElementById('helpModal');
        const closeHelpBtn = document.getElementById('closeHelpBtn');
        const themeToggle = document.getElementById('themeToggle');
        const mobileThemeToggle = document.getElementById('mobileThemeToggle');
        
        // 移动端菜单切换
        mobileMenuBtn.addEventListener('click', () => {
            mobileMenu.classList.toggle('hidden');
        });
        
        // 帮助模态框控制
        function openHelpModal() {
            helpModal.classList.remove('hidden');
            helpModal.classList.add('flex');
            document.body.style.overflow = 'hidden';
        }
        
        function closeHelpModal() {
            helpModal.classList.add('hidden');
            helpModal.classList.remove('flex');
            document.body.style.overflow = '';
        }
        
        helpBtn.addEventListener('click', openHelpModal);
        mobileHelpBtn.addEventListener('click', () => {
            openHelpModal();
            mobileMenu.classList.add('hidden');
        });
        closeHelpBtn.addEventListener('click', closeHelpModal);
        
        // 点击模态框外部关闭
        helpModal.addEventListener('click', (e) => {
            if (e.target === helpModal) {
                closeHelpModal();
            }
        });
        
        // 主题切换
        themeToggle.addEventListener('click', toggleTheme);
        mobileThemeToggle.addEventListener('click', () => {
            toggleTheme();
            mobileMenu.classList.add('hidden');
        });
        
        function toggleTheme() {
            document.body.classList.toggle('bg-gray-900');
            document.body.classList.toggle('bg-gray-50');
            document.body.classList.toggle('text-white');
            document.body.classList.toggle('text-dark');
            
            const isDark = document.body.classList.contains('bg-gray-900');
            const themeIcon = themeToggle.querySelector('i');
            
            if (isDark) {
                themeIcon.classList.remove('fa-moon-o');
                themeIcon.classList.add('fa-sun-o');
            } else {
                themeIcon.classList.remove('fa-sun-o');
                themeIcon.classList.add('fa-moon-o');
            }
            
            // 如果图表已存在，更新主题
            if (calculationResults) {
                createChart(calculationResults.t, calculationResults.t_a);
            }
        }
        
        // 表单提交处理
        form.addEventListener('submit', (e) => {
            e.preventDefault();
            calculatePseudoTime();
        });
        
        // 查询按钮点击处理
        queryButton.addEventListener('click', () => {
            queryPseudoTime();
        });
        
        // 按Enter键查询
        queryTimeInput.addEventListener('keypress', (e) => {
            if (e.key === 'Enter') {
                queryPseudoTime();
            }
        });
        
        // 假时间计算函数
        function calculatePseudoTime() {
            // 获取输入值
            const timePointsStr = document.getElementById('timePoints').value.trim();
            const gasViscosityStr = document.getElementById('gasViscosity').value.trim();
            const compressibilityStr = document.getElementById('compressibility').value.trim();
            
            // 解析输入为数组
            try {
                const t = timePointsStr.split(/\s+/).map(Number);
                const mu_g = gasViscosityStr.split(/\s+/).map(Number);
                const C_t = compressibilityStr.split(/\s+/).map(Number);
                
                // 验证输入
                if (t.length === 0 || mu_g.length === 0 || C_t.length === 0) {
                    showError('请输入有效的数值');
                    return;
                }
                
                if (!(t.length === mu_g.length && mu_g.length === C_t.length)) {
                    showError('时间点、粘度和压缩系数的数值数量必须相同');
                    return;
                }
                
                // 检查是否有非数值
                if (t.some(isNaN) || mu_g.some(isNaN) || C_t.some(isNaN)) {
                    showError('输入包含非数值，请检查');
                    return;
                }
                
                // 检查是否有负值
                if (t.some(v => v < 0) || mu_g.some(v => v <= 0) || C_t.some(v => v <= 0)) {
                    showError('时间点不能为负，粘度和压缩系数必须为正数');
                    return;
                }
                
                // 检查时间点是否递增
                for (let i = 1; i < t.length; i++) {
                    if (t[i] <= t[i-1]) {
                        showError('时间点必须按递增顺序输入');
                        return;
                    }
                }
                
                // 执行计算
                const results = performCalculation(t, mu_g, C_t);
                
                // 存储计算结果
                calculationResults = results;
                
                // 显示结果
                displayResults(results);
                
                // 启用查询功能
                queryTimeInput.disabled = false;
                queryButton.disabled = false;
                queryTimeInput.focus();
                
            } catch (error) {
                showError('计算过程中发生错误: ' + error.message);
            }
        }
        
        // 执行具体计算
        function performCalculation(t, mu_g, C_t) {
            const n = t.length;
            
            // 步骤1：计算A = 1/(μ_g * C_t)
            const A = mu_g.map((mu, i) => 1 / (mu * C_t[i]));
            
            // 步骤2：计算时间间隔（小时）并转换为秒
            const dt_h = [];
            for (let i = 0; i < n - 1; i++) {
                dt_h.push(t[i + 1] - t[i]);
            }
            // 最后一个间隔使用前一个间隔的值
            dt_h.push(dt_h.length > 0 ? dt_h[dt_h.length - 1] : 0);
            
            const dt_sec = dt_h.map(dt => dt * 3600);
            
            // 步骤3：计算B值
            const B = new Array(n).fill(0);
            B[0] = A[0] / 2;  // 第一个B值：A(1)/2
            
            for (let i = 1; i < n - 1; i++) {
                B[i] = (A[i - 1] + A[i]) / 2;  // 中间B值：相邻A的平均值
            }
            
            if (n > 1) {
                B[n - 1] = (A[n - 2] + A[n - 1]) / 2;  // 最后一个B值
            }
            
            // 步骤4：计算C'值
            const C_prime = B.map((b, i) => b * dt_sec[i]);
            
            // 步骤5：计算假时间t_a
            const t_a = new Array(n).fill(0);
            t_a[0] = C_prime[0];  // 第一个假时间
            
            for (let i = 1; i < n; i++) {
                t_a[i] = t_a[i - 1] + C_prime[i];  // 累加计算
            }
            
            // 整理结果
            const results = [];
            for (let i = 0; i < n; i++) {
                results.push({
                    t: t[i],
                    dt_h: dt_h[i],
                    dt_sec: dt_sec[i],
                    A: A[i],
                    B: B[i],
                    C_prime: C_prime[i],
                    t_a: t_a[i]
                });
            }
            
            return {
                results,
                t,
                t_a
            };
        }
        
        // 显示计算结果
        function displayResults(results) {
            // 隐藏错误信息和无结果信息
            errorMessage.classList.add('hidden');
            noResultsMessage.classList.add('hidden');
            // 显示结果容器
            resultsContainer.classList.remove('hidden');
            
            // 清空表格
            resultsTableBody.innerHTML = '';
            
            // 填充表格
            results.results.forEach((item, index) => {
                const row = document.createElement('tr');
                row.className = index % 2 === 0 ? 'bg-white' : 'bg-gray-50';
                row.innerHTML = `
                    <td class="px-4 py-3 whitespace-nowrap">${item.t.toFixed(4)}</td>
                    <td class="px-4 py-3 whitespace-nowrap">${item.dt_h.toFixed(4)}</td>
                    <td class="px-4 py-3 whitespace-nowrap">${Math.round(item.dt_sec)}</td>
                    <td class="px-4 py-3 whitespace-nowrap">${item.A.toFixed(6)}</td>
                    <td class="px-4 py-3 whitespace-nowrap">${item.B.toFixed(6)}</td>
                    <td class="px-4 py-3 whitespace-nowrap">${item.C_prime.toFixed(4)}</td>
                    <td class="px-4 py-3 whitespace-nowrap font-medium text-primary">${item.t_a.toFixed(4)}</td>
                `;
                resultsTableBody.appendChild(row);
            });
            
            // 创建图表（使用原生SVG）
            createChart(results.t, results.t_a);
            
            // 平滑滚动到结果区域
            resultsContainer.scrollIntoView({ behavior: 'smooth', block: 'start' });
        }
        
        // 使用原生SVG创建假时间与实际时间关系图 - 横纵坐标都从0开始
        function createChart(t, t_a) {
            // 获取SVG元素并清空
            const svg = document.getElementById('timeChart');
            svg.innerHTML = '';
            
            // 获取SVG尺寸
            const svgWidth = svg.clientWidth;
            const svgHeight = svg.clientHeight;
            
            // 边距
            const margin = { top: 30, right: 30, bottom: 40, left: 60 };
            const chartWidth = svgWidth - margin.left - margin.right;
            const chartHeight = svgHeight - margin.top - margin.bottom;
            
            // 计算数据范围 - 确保从0开始
            const tMin = 0; // X轴从0开始
            const tMax = Math.max(...t);
            const tRange = tMax - tMin;
            
            const t_aMin = 0; // Y轴从0开始
            const t_aMax = Math.max(...t_a);
            const t_aRange = t_aMax - t_aMin;
            
            // 添加10%的额外空间，使数据不紧贴坐标轴
            const tPadding = tRange > 0 ? tRange * 0.1 : 1;
            const t_aPadding = t_aRange > 0 ? t_aRange * 0.1 : 1;
            
            // 比例尺函数 - 将数据值映射到图表坐标
            const xScale = (value) => {
                return margin.left + (value - tMin) / 
                    ((tMax + tPadding) - tMin) * chartWidth;
            };
            
            const yScale = (value) => {
                return margin.top + chartHeight - (value - t_aMin) / 
                    ((t_aMax + t_aPadding) - t_aMin) * chartHeight;
            };
            
            // 创建坐标轴
            // X轴
            const xAxis = document.createElementNS("http://www.w3.org/2000/svg", "line");
            xAxis.setAttribute('x1', margin.left);
            xAxis.setAttribute('y1', margin.top + chartHeight);
            xAxis.setAttribute('x2', margin.left + chartWidth);
            xAxis.setAttribute('y2', margin.top + chartHeight);
            xAxis.setAttribute('class', 'chart-axis');
            svg.appendChild(xAxis);
            
            // Y轴
            const yAxis = document.createElementNS("http://www.w3.org/2000/svg", "line");
            yAxis.setAttribute('x1', margin.left);
            yAxis.setAttribute('y1', margin.top);
            yAxis.setAttribute('x2', margin.left);
            yAxis.setAttribute('y2', margin.top + chartHeight);
            yAxis.setAttribute('class', 'chart-axis');
            svg.appendChild(yAxis);
            
            // X轴标签
            const xLabel = document.createElementNS("http://www.w3.org/2000/svg", "text");
            xLabel.setAttribute('x', margin.left + chartWidth / 2);
            xLabel.setAttribute('y', svgHeight - 10);
            xLabel.textContent = '实际时间 (小时)';
            xLabel.setAttribute('class', 'chart-label');
            svg.appendChild(xLabel);
            
            // Y轴标签
            const yLabel = document.createElementNS("http://www.w3.org/2000/svg", "text");
            yLabel.setAttribute('x', -svgHeight / 2);
            yLabel.setAttribute('y', 20);
            yLabel.setAttribute('transform', 'rotate(-90)');
            yLabel.textContent = '假时间';
            yLabel.setAttribute('class', 'chart-label');
            svg.appendChild(yLabel);
            
            // 绘制网格线 - X方向
            const xGridCount = 5;
            for (let i = 0; i <= xGridCount; i++) {
                const x = margin.left + (i / xGridCount) * chartWidth;
                const gridLine = document.createElementNS("http://www.w3.org/2000/svg", "line");
                gridLine.setAttribute('x1', x);
                gridLine.setAttribute('y1', margin.top);
                gridLine.setAttribute('x2', x);
                gridLine.setAttribute('y2', margin.top + chartHeight);
                gridLine.setAttribute('class', 'chart-grid');
                svg.appendChild(gridLine);
                
                // 网格标签
                const gridValue = tMin + (i / xGridCount) * (tMax + tPadding);
                const label = document.createElementNS("http://www.w3.org/2000/svg", "text");
                label.setAttribute('x', x);
                label.setAttribute('y', margin.top + chartHeight + 20);
                label.textContent = gridValue.toFixed(2);
                label.setAttribute('class', 'chart-label');
                svg.appendChild(label);
            }
            
            // 绘制网格线 - Y方向
            const yGridCount = 5;
            for (let i = 0; i <= yGridCount; i++) {
                const y = margin.top + (i / yGridCount) * chartHeight;
                const gridLine = document.createElementNS("http://www.w3.org/2000/svg", "line");
                gridLine.setAttribute('x1', margin.left);
                gridLine.setAttribute('y1', y);
                gridLine.setAttribute('x2', margin.left + chartWidth);
                gridLine.setAttribute('y2', y);
                gridLine.setAttribute('class', 'chart-grid');
                svg.appendChild(gridLine);
                
                // 网格标签
                const gridValue = t_aMin + (yGridCount - i) / yGridCount * (t_aMax + t_aPadding);
                const label = document.createElementNS("http://www.w3.org/2000/svg", "text");
                label.setAttribute('x', margin.left - 10);
                label.setAttribute('y', y + 4);
                label.setAttribute('text-anchor', 'end');
                label.textContent = gridValue.toFixed(2);
                label.setAttribute('class', 'chart-label');
                svg.appendChild(label);
            }
            
            // 准备路径数据
            let pathData = `M ${xScale(t[0])} ${yScale(t_a[0])}`;
            let areaData = `M ${xScale(t[0])} ${yScale(t_a[0])}`;
            
            for (let i = 1; i < t.length; i++) {
                pathData += ` L ${xScale(t[i])} ${yScale(t_a[i])}`;
                areaData += ` L ${xScale(t[i])} ${yScale(t_a[i])}`;
            }
            
            // 闭合面积路径 - 连接到底部X轴
            areaData += ` L ${xScale(t[t.length - 1])} ${yScale(t_aMin)}`;
            areaData += ` L ${xScale(t[0])} ${yScale(t_aMin)} Z`;
            
            // 绘制面积
            const area = document.createElementNS("http://www.w3.org/2000/svg", "path");
            area.setAttribute('d', areaData);
            area.setAttribute('class', 'chart-area');
            svg.appendChild(area);
            
            // 绘制线
            const line = document.createElementNS("http://www.w3.org/2000/svg", "path");
            line.setAttribute('d', pathData);
            line.setAttribute('class', 'chart-line');
            svg.appendChild(line);
            
            // 绘制数据点
            t.forEach((time, i) => {
                const point = document.createElementNS("http://www.w3.org/2000/svg", "circle");
                point.setAttribute('cx', xScale(time));
                point.setAttribute('cy', yScale(t_a[i]));
                point.setAttribute('r', 4);
                point.setAttribute('class', 'chart-point');
                
                // 添加悬停提示
                point.setAttribute('title', `时间: ${time.toFixed(4)}, 假时间: ${t_a[i].toFixed(4)}`);
                svg.appendChild(point);
            });
            
            // 响应深色模式
            if (document.body.classList.contains('bg-gray-900')) {
                document.querySelectorAll('.chart-grid').forEach(el => {
                    el.setAttribute('stroke', '#374151');
                });
                document.querySelectorAll('.chart-axis').forEach(el => {
                    el.setAttribute('stroke', '#6b7280');
                });
                document.querySelectorAll('.chart-label, .chart-title').forEach(el => {
                    el.setAttribute('fill', '#d1d5db');
                });
            }
        }
        
        // 响应窗口大小变化重绘图表
        window.addEventListener('resize', () => {
            if (calculationResults) {
                createChart(calculationResults.t, calculationResults.t_a);
            }
        });
        
        // 查询任意时间点的假时间
        function queryPseudoTime() {
            if (!calculationResults) return;
            
            // 隐藏之前的结果和错误
            queryResult.classList.add('hidden');
            queryError.classList.add('hidden');
            
            // 获取查询时间
            const t_query = parseFloat(queryTimeInput.value);
            
            // 验证输入
            if (isNaN(t_query)) {
                showQueryError('请输入有效的时间值');
                return;
            }
            
            const t = calculationResults.t;
            const t_a = calculationResults.t_a;
            
            // 检查时间范围
            if (t_query < t[0]) {
                showQueryError(`查询时间必须大于等于 ${t[0].toFixed(4)} 小时`);
                return;
            }
            
            if (t_query > t[t.length - 1]) {
                showQueryError(`查询时间必须小于等于 ${t[t.length - 1].toFixed(4)} 小时`);
                return;
            }
            
            // 找到查询时间所在的区间
            let t_a_query;
            if (t_query === t[0]) {
                t_a_query = t_a[0];
            } else {
                // 查找查询时间所在的区间索引
                let idx = 0;
                for (let i = 0; i < t.length; i++) {
                    if (t[i] < t_query) {
                        idx = i;
                    } else {
                        break;
                    }
                }
                
                // 计算该区间内的时间比例
                const t_start = t[idx];
                const t_end = t[idx + 1];
                const ratio = (t_query - t_start) / (t_end - t_start);
                
                // 计算对应的假时间
                const t_a_start = t_a[idx];
                const t_a_end = t_a[idx + 1];
                t_a_query = t_a_start + ratio * (t_a_end - t_a_start);
            }
            
            // 显示查询结果
            queryResultText.textContent = `时间 ${t_query.toFixed(4)} 小时对应的假时间为: ${t_a_query.toFixed(4)}`;
            queryResult.classList.remove('hidden');
            
            // 添加结果高亮动画
            queryResult.classList.add('animate-pulse');
            setTimeout(() => {
                queryResult.classList.remove('animate-pulse');
            }, 1000);
        }
        
        // 显示错误信息
        function showError(message) {
            errorText.textContent = message;
            errorMessage.classList.remove('hidden');
            resultsContainer.classList.add('hidden');
            noResultsMessage.classList.add('hidden');
            
            // 禁用查询功能
            queryTimeInput.disabled = true;
            queryButton.disabled = true;
        }
        
        // 显示查询错误
        function showQueryError(message) {
            queryErrorText.textContent = message;
            queryError.classList.remove('hidden');
        }
        
        // 页面滚动效果 - 导航栏样式变化
        window.addEventListener('scroll', () => {
            const header = document.querySelector('header');
            if (window.scrollY > 10) {
                header.classList.add('py-2');
                header.classList.remove('py-4');
            } else {
                header.classList.add('py-4');
                header.classList.remove('py-2');
            }
        });
    </script>
</body>
</html>
    
