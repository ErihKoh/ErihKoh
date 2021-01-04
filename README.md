### Hello there 👋 I'm Yurii
![Anurag's github stats](https://github-readme-stats.vercel.app/api?username=ErihKoh&show_icons=true&theme=radical)

<!DOCTYPE html>
<!-- saved from url=(0051)https://profile-summary-for-github.com/user/ErihKoh -->
<html lang="en"><head><meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
        <title>Profile Summary For GitHub</title>
        <link rel="icon" href="https://profile-summary-for-github.com/favicon.png">
        
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <meta name="description" content="Github Profile Summary is a GitHub visualization tool written in Kotlin">
        <meta property="og:title" content="Github Profile Summary - Visualize your GitHub profile">
        <meta property="og:site_name" content="Github Profile Summary">
        <meta property="og:url" content="https://profile-summary-for-github.com">
        <meta property="og:description" content="Github Profile Summary is a GitHub visualization tool written in Kotlin">
        <meta property="og:image" content="https://user-images.githubusercontent.com/1521451/33957306-8e1d8af0-e041-11e7-8e04-3de9e32868ba.PNG">
        <link rel="stylesheet" href="./Profile Summary For GitHub_files/font-awesome.min.css">
        <link rel="stylesheet" href="./Profile Summary For GitHub_files/css">
        <link rel="stylesheet" href="./Profile Summary For GitHub_files/square-jelly-box.min.css">
        <script type="text/javascript" async="" src="./Profile Summary For GitHub_files/analytics.js.Без названия"></script><script async="" src="./Profile Summary For GitHub_files/gtm.js.Без названия"></script><script src="./Profile Summary For GitHub_files/Chart.min.js.Без названия"></script>
        <script src="./Profile Summary For GitHub_files/axios.min.js.Без названия"></script>
        <script src="./Profile Summary For GitHub_files/vue.js.Без названия"></script>
        <script src="./Profile Summary For GitHub_files/moment.min.js.Без названия"></script>
        <script src="./Profile Summary For GitHub_files/js.cookie.min.js.Без названия"></script>
        
<!-- search-view.vue -->
<template id="search-view"></template>
<script>
    Vue.component("search-view", {
        template: "#search-view",
        data: () => ({
            error: null,
            failedQuery: "",
            query: ""
        }),
        methods: {
            search() {
                this.error = null;
                this.failedQuery = null;
                axios.get("/api/can-load?user=" + this.query)
                    .then(() => window.location = "/user/" + this.query)
                    .catch(error => {
                        this.error = error;
                        this.failedQuery = this.query
                    });
            }
        },
    });
</script>
<style>
    .search-screen {
        display: flex;
        flex-direction: column;
        align-items: center;
    }

    .search-term {
        border: 1px solid rgba(0, 0, 0, 0.2);
        background: rgba(0, 0, 0, 0.025);
        padding: 1px 2px;
        font-family: monospace;
        font-size: 80%;
    }

    .search-screen input {
        height: 40px;
        font-size: 18px;
        padding: 0 15px;
        border: 0;
    }
</style>

<!-- share-bar.vue -->
<template id="share-bar"></template>
<script>
    Vue.component("share-bar", {
        template: "#share-bar",
        props: ["user"],
        computed: {
            profileUrl: function () {
                return "https://profile-summary-for-github.com/user/" + this.user.login;
            },
            shareText: function () {
                return this.user.login + "'s GitHub profile - Visualized:";
            },
            twitterUrl: function () {
                return "https://twitter.com/intent/tweet?url=" + this.profileUrl + "&text=" + this.shareText + "&via=javalin_io&related=javalin_io";
            },
            facebookUrl: function () {
                return "https://facebook.com/sharer.php?u=" + this.profileUrl + "&quote=" + this.shareText
            }
        }
    });
</script>
<style>
    .share-bar {
        position: absolute;
        top: 0;
        left: 50%;
        transform: translateX(-50%);
        background: rgba(0, 0, 0, .04);
        font-size: 14px;
        text-align: center;
    }

    .share-bar a {
        white-space: nowrap;
        margin: 5px 8px;
        display: inline-block;
    }

    .share-bar a i {
        color: #0082c8;
    }

    @media (max-width: 480px) {
        .share-bar {
            width: 100%;
        }
    }
</style>

<!-- user-info.vue -->
<template id="user-info"></template>
<script>
    Vue.component("user-info", {
        template: "#user-info",
        props: ["user", "data"],
        computed: {
            timeAgo() {
                return moment(this.user.createdAt).fromNow()
            }
        },
        mounted() {
            lineChart("quarterCommitCount", this.data)
        }
    });
</script>
<style>
    .user-info {
        display: flex;
        padding-bottom: 40px;
    }

    .user-info img {
        align-self: center;
        border-radius: 3px;
        width: 175px;
        margin-right: 20px;
    }

    .user-info .details {
        display: flex;
        flex-direction: column;
        justify-content: space-between;
        margin-right: 20px;
        flex-shrink: 0;
    }

    .user-info i.fa {
        color: rgba(0, 0, 0, 0.67);
        margin-right: 5px;
    }

    .user-info .commits-per-quarter {
        flex-grow: 1;
        flex-shrink: 1;
        position: relative;
    }

    .user-info .commits-per-quarter::after {
        content: "Commits per quarter";
        position: absolute;
        right: 40px;
        bottom: -15px;
        font-size: 13px;
    }

    @media (max-width: 480px) {
        .user-info img,
        .user-info .commits-per-quarter{
            display: none;
        }
    }
</style>

<!-- donut-charts.vue -->
<template id="donut-charts"></template>
<script>
    Vue.component("donut-charts", {
        template: "#donut-charts",
        props: ["data"],
        mounted() {
            donutChart("langRepoCount", this.data);
            donutChart("langStarCount", this.data);
            donutChart("langCommitCount", this.data);
            donutChart("repoCommitCount", this.data);
            donutChart("repoStarCount", this.data);
        }
    });
</script>
<style>
    canvas {
        user-select: none;
    }

    .charts,
    .chart-row {
        overflow: auto;
    }

    .chart-row {
        padding-bottom: 40px;
    }

    .chart-row {
        border-top: 1px solid rgba(0, 0, 0, 0.1);
        display: flex;
        justify-content: space-around;
    }

    .chart-container--third {
        width: 33%;
    }

    .chart-container--half {
        width: 50%;
    }

    @media (max-width: 900px) {
        .chart-container--third,
        .chart-container--half {
            width: 100%;
        }

        .chart-row {
            display: block;
        }

    }

    @media (max-width: 480px) {
        footer {
            display: none;
        }
    }
</style>

<!-- _main-styles.vue -->
<style>
    * {
        font-family: 'Barlow Semi Condensed', sans-serif;
        outline: 0;
        box-sizing: border-box;
    }

    [v-cloak] {
        display: none;
    }

    html {
        font-size: 18px;
        background: #eee9df;
        padding: 60px 30px;
        overflow-y: scroll;
    }

    body {
        margin: 0;
    }

    h1, h2, h3, h4 {
        font-weight: 400;
    }

    a {
        color: #0082c8;
        text-decoration: none;
    }

    .content {
        max-width: 1200px;
        margin: 0 auto;
    }

    .fade-in {
        opacity: 0;
        animation: fade-in .2s linear forwards;
    }

    @keyframes fade-in {
        from {
            opacity: 0;
        }
        to {
            opacity: 1;
        }
    }
</style>

<!-- loading-bouncer.vue -->
<template id="loading-bouncer"></template>
<script>
    Vue.component("loading-bouncer", {template: "#loading-bouncer"});
</script>
<style>
    .loading-bouncer {
        margin-top: 50px;
        display: flex;
        flex-direction: column;
        align-items: center;
    }

    .la-square-jelly-box {
        color: #38abe2;
    }
</style>

<!-- _charts.vue -->
<script>
    function donutChart(objectName, data) {
        let canvas = document.getElementById(objectName);
        if (canvas === null) {
            return;
        }
        let userId = data.user.login;
        let labels = Object.keys(data[objectName]);
        let values = Object.values(data[objectName]);
        let colors = createColorArray(labels.length);
        let tooltipInfo = null;
        window.languageColors = window.languageColors || {};
        if ("langRepoCount" === objectName) {
            // when the first language-set is loaded, set a color-profile for all languages
            labels.forEach((language, i) => languageColors[language] = colors[i]);
        }
        if (["langRepoCount", "langStarCount", "langCommitCount"].indexOf(objectName) > -1) {
            // if the dataset is language-related, load color-profile
            labels.forEach((language, i) => colors[i] = languageColors[language]);
        }
        if (objectName === "repoCommitCount") {
            tooltipInfo = data[objectName + "Descriptions"]; // high quality programming
            arrayRotate(colors, 4); // change starting color
        }
        if (objectName === "repoStarCount") {
            tooltipInfo = data[objectName + "Descriptions"]; // high quality programming
            arrayRotate(colors, 2); // change starting color
        }
        new Chart(canvas.getContext("2d"), {
            type: "doughnut",
            data: {
                labels: labels,
                datasets: [{
                    data: values,
                    backgroundColor: colors
                }]
            },
            options: {
                animation: false,
                rotation: (-0.40 * Math.PI),
                legend: { // todo: fix duplication ?
                    position: window.innerWidth < 600 ? "bottom" : "left",
                    labels: {
                        fontSize: window.innerWidth < 600 ? 10 : 12,
                        padding: window.innerWidth < 600 ? 8 : 10,
                        boxWidth: window.innerWidth < 600 ? 10 : 12
                    }
                },
                tooltips: {
                    callbacks: {
                        afterLabel: function (tooltipItem, data) {
                            if (tooltipInfo !== null) {
                                return wordWrap(tooltipInfo[data["labels"][tooltipItem["index"]]], 45);
                            }
                        }
                    },
                },
                onClick: function (e, data) {
                    try {
                        let label = labels[data[0]._index];
                        let canvas = data[0]._chart.canvas.id;
                        if (canvas === "repoStarCount" || canvas === "repoCommitCount") {
                            window.open("https://github.com/" + userId + "/" + label, "_blank");
                            window.focus();
                        } else {
                            window.open("https://github.com/" + userId + "?utf8=%E2%9C%93&tab=repositories&q=&type=source&language=" + encodeURIComponent(label), "_blank");
                            window.focus();
                        }
                    } catch (ignored) {
                    }
                },
                onResize: function (instance) { // todo: fix duplication ?
                    instance.chart.options.legend.position = window.innerWidth < 600 ? "bottom" : "left";
                    instance.chart.options.legend.labels.fontSize = window.innerWidth < 600 ? 10 : 12;
                    instance.chart.options.legend.labels.padding = window.innerWidth < 600 ? 8 : 10;
                    instance.chart.options.legend.labels.boxWidth = window.innerWidth < 600 ? 10 : 12;
                }
            }
        });

        function createColorArray(length) {
            const colors = ["#54ca76", "#f5c452", "#f2637f", "#9261f3", "#31a4e6", "#55cbcb"];
            let array = [...Array(length).keys()].map(i => colors[i % colors.length]);
            // avoid first and last colors being the same
            if (length % colors.length === 1) {
                array[length - 1] = colors[1];
            }
            return array;
        }

        function arrayRotate(arr, n) {
            for (let i = 0; i < n; i++) {
                arr.push(arr.shift());
            }
            return arr
        }

        function wordWrap(str, n) {
            if (str === null) {
                return null;
            }
            let currentLine = [];
            let resultLines = [];
            str.split(" ").forEach(word => {
                currentLine.push(word);
                if (currentLine.join(" ").length > n) {
                    resultLines.push(currentLine.join(" "));
                    currentLine = [];
                }
            });
            if (currentLine.length > 0) {
                resultLines.push(currentLine.join(" "));
            }
            return resultLines
        }
    }

    function lineChart(objectName, data) {
        new Chart(document.getElementById(objectName).getContext("2d"), {
            type: "line",
            data: {
                labels: Object.keys(data[objectName]),
                datasets: [{
                    label: "Commits",
                    data: Object.values(data[objectName]),
                    backgroundColor: "rgba(67, 142, 233, 0.2)",
                    borderColor: "rgba(67, 142, 233, 1)",
                    lineTension: 0
                }]
            },
            options: {
                maintainAspectRatio: false,
                animation: false,
                scales: {
                    xAxes: [{
                        display: false
                    }],
                    yAxes: [{
                        position: "right",
                        beginAtZero: true
                    }]
                },
                legend: {
                    display: false
                },
                tooltips: {
                    intersect: false
                }
            }
        });
    }
</script>

<!-- app-frame.vue -->
<template id="app-frame"></template>
<script>
    Vue.component("app-frame", {
        template: "#app-frame",
        data: () => ({
            requestsLeft: 9876,
        }),
        created() {
            let wsProtocol = window.location.protocol.indexOf("https") > -1 ? "wss" : "ws";
            let ws = new WebSocket(wsProtocol + "://" + location.hostname + ":" + location.port + "/rate-limit-status");
            ws.onmessage = msg => this.requestsLeft = msg.data;
        },
    });
</script>
<style>
    footer {
        font-size: 17px;
        position: fixed;
        left: 0;
        bottom: 0;
        width: 100%;
        text-align: center;
        padding: 10px 30px;
        border-top: 1px solid rgba(0, 0, 0, 0.1);
        background: #eee9df;
    }

    .rate-limit {
        position: fixed;
        right: 20px;
        bottom: 60px;
        background: #fff;
        padding: 10px;
        box-shadow: 0 1px 1px rgba(0, 0, 0, 0.3);
        font-size: 16px;
    }

    @media (max-width: 480px) {
        .rate-limit {
            padding: 5px 8px;
            font-size: 13px;
            bottom: 0;
            right: 0;
        }
    }
</style>

<!-- _gtm.vue -->
<script>
    (function(w,d,s,l,i){w[l]=w[l]||[];w[l].push({'gtm.start':
    new Date().getTime(),event:'gtm.js'});var f=d.getElementsByTagName(s)[0],
    j=d.createElement(s),dl=l!='dataLayer'?'&l='+l:'';j.async=true;j.src=
    '//www.googletagmanager.com/gtm.js?id='+i+dl;f.parentNode.insertBefore(j,f);
    })(window,document,'script','dataLayer',Cookies.get("gtm-id"));
</script>

<!-- user-view.vue -->
<template id="user-view"></template>
<script>
    Vue.component("user-view", {
        template: "#user-view",
        data: () => ({
            data: null,
            user: null,
            error: null,
        }),
        created() {
            let userId = this.$javalin.pathParams["user"];
            axios.get("/api/user/" + userId).then(response => {
                this.data = response.data;
                this.user = response.data.user;
            }).catch(error => this.error = error);
        },
    });
</script>

<script>
    Vue.prototype.$javalin = {
        pathParams: {"user":"ErihKoh"},
        queryParams: {},
        state: {}
    }
</script>

    <style type="text/css">/* Chart.js */
@keyframes chartjs-render-animation{from{opacity:.99}to{opacity:1}}.chartjs-render-monitor{animation:chartjs-render-animation 1ms}.chartjs-size-monitor,.chartjs-size-monitor-expand,.chartjs-size-monitor-shrink{position:absolute;direction:ltr;left:0;top:0;right:0;bottom:0;overflow:hidden;pointer-events:none;visibility:hidden;z-index:-1}.chartjs-size-monitor-expand>div{position:absolute;width:1000000px;height:1000000px;left:0;top:0}.chartjs-size-monitor-shrink>div{position:absolute;width:200%;height:200%;left:0;top:0}</style></head>
    <body>
        <main id="main-vue" class="content"><div><main class="main-content"><!----> <div class="fade-in"><div class="share-bar"><a href="https://twitter.com/intent/tweet?url=https://profile-summary-for-github.com/user/ErihKoh&amp;text=ErihKoh%27s%20GitHub%20profile%20-%20Visualized:&amp;via=javalin_io&amp;related=javalin_io" rel="nofollow" title="Share on Twitter" class="social-btn"><i class="fa fa-fw fa-twitter"></i>Share on Twitter</a> <a href="https://facebook.com/sharer.php?u=https://profile-summary-for-github.com/user/ErihKoh&amp;quote=ErihKoh%27s%20GitHub%20profile%20-%20Visualized:" rel="nofollow" title="Share on Facebook" class="social-btn"><i class="fa fa-fw fa-facebook"></i>Share on Facebook</a></div> <div class="user-info"><img src="./Profile Summary For GitHub_files/63535943" alt="ErihKoh"> <div class="details"><div><i class="fa fa-fw fa-user"></i>ErihKoh
                <small>(Yurii Zubar)</small></div> <div><i class="fa fa-fw fa-database"></i>45 public repos</div> <div><i class="fa fa-fw fa-clock-o"></i>Joined GitHub 9 months ago</div> <!----> <!----> <div><i class="fa fa-fw fa-external-link"></i><a href="https://github.com/ErihKoh" target="_blank">View profile on GitHub</a></div></div> <div class="chart-container commits-per-quarter"><div class="chartjs-size-monitor"><div class="chartjs-size-monitor-expand"><div class=""></div></div><div class="chartjs-size-monitor-shrink"><div class=""></div></div></div><canvas id="quarterCommitCount" width="952" height="218" class="chartjs-render-monitor" style="display: block; height: 175px; width: 762px;"></canvas></div></div> <div class="charts"><div class="chart-row"><div class="chart-container chart-container--third"><div class="chartjs-size-monitor"><div class="chartjs-size-monitor-expand"><div class=""></div></div><div class="chartjs-size-monitor-shrink"><div class=""></div></div></div><h2>Repos per Language</h2> <canvas id="langRepoCount" width="495" height="247" class="chartjs-render-monitor" style="display: block; height: 198px; width: 396px;"></canvas></div> <!----> <div class="chart-container chart-container--third"><div class="chartjs-size-monitor"><div class="chartjs-size-monitor-expand"><div class=""></div></div><div class="chartjs-size-monitor-shrink"><div class=""></div></div></div><h2>Commits per Language</h2> <canvas id="langCommitCount" width="495" height="247" class="chartjs-render-monitor" style="display: block; height: 198px; width: 396px;"></canvas></div></div> <div class="chart-row"><div class="chart-container chart-container--half"><div class="chartjs-size-monitor"><div class="chartjs-size-monitor-expand"><div class=""></div></div><div class="chartjs-size-monitor-shrink"><div class=""></div></div></div><h2>Commits per Repo
                    <small>(top 10)</small></h2> <canvas id="repoCommitCount" width="563" height="281" class="chartjs-render-monitor" style="display: block; height: 300px; width: 601px;"></canvas></div> <!----></div></div></div> <!----></main> <footer>
            GitHub profile summary is built with <a href="https://javalin.io/">javalin</a> <small>(kotlin web framework)</small> and
            <a href="http://www.chartjs.org/docs/latest/" target="_blank">chart.js</a> <small>(visualization)</small>.
            Source is on <a href="https://github.com/tipsy/profile-summary-for-github" target="_blank">GitHub</a>.
        </footer> <span class="rate-limit"><!----> <span><strong>9920</strong> requests left <br> before rate-limit</span></span></div></main>
        <script>
            new Vue({el: "#main-vue"});
        </script>
    

</body></html>

<!--
**ErihKoh/ErihKoh** is a ✨ _special_ ✨ repository because its `README.md` (this file) appears on your GitHub profile.

Here are some ideas to get you started:

- 🔭 I’m currently working on ...
- 🌱 I’m currently learning ...
- 👯 I’m looking to collaborate on ...
- 🤔 I’m looking for help with ...
- 💬 Ask me about ...
- 📫 How to reach me: ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...
-->
