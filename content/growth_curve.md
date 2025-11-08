---
title: "成长曲线 · 体重"
date: 2025-11-08
menu:
  main:
    name: 成长曲线
    weight: 20
noindex: true  # 防止搜索引擎收录，保护隐私
---

<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

<div style="width:100%; max-width:900px; margin:0 auto;">
  <canvas id="growthChart" height="400"></canvas>
</div>

<script>
// === 中国男宝体重标准数据（0-24月）===
const STANDARD_DATA = {
  age_months: [0, 1, 2, 3, 4, 5, 6, 8, 10, 12, 15, 18, 21, 24],
  p3:  [2.57, 3.59, 4.42, 5.16, 5.74, 6.22, 6.64, 7.36, 7.92, 8.37, 8.86, 9.27, 9.62, 9.91],
  p10: [2.82, 3.91, 4.79, 5.55, 6.15, 6.65, 7.09, 7.88, 8.49, 8.99, 9.51, 9.95, 10.32, 10.63],
  p25: [3.07, 4.23, 5.14, 5.91, 6.54, 7.06, 7.52, 8.36, 9.01, 9.55, 10.12, 10.60, 11.00, 11.33],
  p50: [3.33, 4.58, 5.50, 6.29, 6.95, 7.49, 7.96, 8.85, 9.56, 10.12, 10.70, 11.20, 11.64, 12.00],
  p75: [3.60, 4.96, 5.90, 6.72, 7.40, 7.97, 8.47, 9.41, 10.19, 10.81, 11.45, 11.99, 12.46, 12.86],
  p90: [3.88, 5.37, 6.34, 7.20, 7.93, 8.52, 9.04, 10.03, 10.88, 11.55, 12.26, 12.84, 13.33, 13.73],
  p97: [4.16, 5.80, 6.80, 7.71, 8.50, 9.13, 9.68, 10.73, 11.66, 12.40, 13.17, 13.79, 14.28, 14.68]
};

// === 请在这里输入你儿子的实际体重数据 ===
// 格式：{ 月龄: 体重kg }
// 示例：出生0月3.2kg，1个月4.6kg，2个月5.8kg...
const BABY_DATA = {
  0: 3.42,
  1: 4.4,
  // 2: 5.8,
  // 3: 6.5,
  // ... 每次体检后添加一行即可
};

// 转换为 Chart.js 所需格式
const babyPoints = Object.entries(BABY_DATA).map(([age, weight]) => ({
  x: parseFloat(age),
  y: parseFloat(weight)
}));

// 渲染图表
const ctx = document.getElementById('growthChart').getContext('2d');
new Chart(ctx, {
  type: 'line',
   {
    labels: STANDARD_DATA.age_months,
    datasets: [
      { label: 'P3',   data: STANDARD_DATA.p3,  borderColor: '#cccccc', borderDash: [4,2], pointRadius: 0, fill: false },
      { label: 'P10',  data: STANDARD_DATA.p10, borderColor: '#bbbbbb', borderDash: [4,2], pointRadius: 0, fill: false },
      { label: 'P25',  data: STANDARD_DATA.p25, borderColor: '#999999', borderDash: [4,2], pointRadius: 0, fill: false },
      { label: '中位数 (P50)', data: STANDARD_DATA.p50, borderColor: '#000000', borderWidth: 2, pointRadius: 0, fill: false },
      { label: 'P75',  data: STANDARD_DATA.p75, borderColor: '#999999', borderDash: [4,2], pointRadius: 0, fill: false },
      { label: 'P90',  data: STANDARD_DATA.p90, borderColor: '#bbbbbb', borderDash: [4,2], pointRadius: 0, fill: false },
      { label: 'P97',  data: STANDARD_DATA.p97, borderColor: '#cccccc', borderDash: [4,2], pointRadius: 0, fill: false },
      {
        label: '宝宝实际体重',
         babyPoints,
        borderColor: '#e74c3c',
        backgroundColor: '#e74c3c',
        borderWidth: 3,
        pointRadius: 5,
        pointHoverRadius: 7,
        showLine: true,
        fill: false
      }
    ]
  },
  options: {
    responsive: true,
    maintainAspectRatio: false,
    interaction: { mode: 'index', intersect: false },
    scales: {
      x: {
        title: { display: true, text: '月龄（月）' },
        grid: { display: false }
      },
      y: {
        title: { display: true, text: '体重（kg）' },
        min: 0,
        ticks: { stepSize: 1 }
      }
    },
    plugins: {
      legend: { position: 'top' },
      tooltip: { callbacks: { label: (item) => `${item.dataset.label}: ${item.parsed.y} kg` } }
    }
  }
});
</script>

<div style="text-align:center; margin-top:20px; font-size:0.9em; color:#666;">
  <p>数据来源：中国7岁以下儿童生长发育参照标准（2009）</p>
  <p>建议定期体检，由专业医生评估生长发育情况</p>
</div>
