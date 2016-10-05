# interactive_google_chart
A prototype that I keep on re using in several charting projects now. Time to reference in a single versioned place.

## Dependencies
This requires google charts API in order to work
``` HTML
    <script type="text/javascript" src="https://www.gstatic.com/charts/loader.js"></script>
```

Knockout is also used, however, is **not** required. it would be easy to choose another two way binding.
``` HTML
    <script src="https://code.jquery.com/jquery-2.2.4.min.js" integrity="sha256-BbhdlvQf/xTY9gja0Dq3HiwQF8LaCRTXxZKRutelT44=" crossorigin="anonymous"></script>
    <script src="js/libs/knockout-3.4.0.js"></script>
```

## Include instructions
Since I am using [rawgit](http://rawgit.com/) as a CDN, specific versions can be selected by visiting [repository commit history](https://github.com/Wambosa/interactive_google_chart/commits/master).

**Production**: Using the _tag_ (or commit hash), it is easy to reference an exact version.
```HTML
    <script type="text/javascript" src="https://cdn.rawgit.com/wambosa/interactive_google_chart/787e1ba/InteractiveGoogleChart.js"></script>
    <!--                                                                                        ^^^^^^^ this is the tag/hash             -->
```
**Development**: always updated can also be used with the same CDN service.
```HTML
    <script type="text/javascript" src="https://cdn.rawgit.com/wambosa/interactive_google_chart/master/InteractiveGoogleChart.js"></script>
```


## Example
``` HTML
<head>
    <script type="text/javascript" src="https://www.gstatic.com/charts/loader.js"></script>
    <script type="text/javascript" src="https://cdn.rawgit.com/wambosa/interactive_google_chart/787e1ba/InteractiveGoogleChart.js"></script>
    <script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/knockout/3.4.0/knockout-min.js"></script>
</head>

<body>

    <div id="chart_div"></div>
    
    <script type="text/javascript">
    
        function MyChart(initData) {
            var self = this;
            
            if(self == window)
                console.error("Do not forget to call **new** MyChart(...) !!!");
        
            this.table = initData;
            this.divId = 'chartDiv';
        
            this.myInput = ko.observable(1);
        
            this.myInput.subscribe(function(newVal){
                self.deltaUpdate({x: 2, y: 1}, newVal);
            });
        
            return this;
        }

        MyChart.prototype = Object.create(InteractiveGoogleChart.prototype);
        MyChart.prototype.constructor = MyChart;
    
    
        var myData = [
            ["Element", "Density", { role: "style" } ],
            ["Copper", 8.94, "#b87333"],
            ["Silver", 10.49, "silver"],
            ["Gold", 19.30, "gold"],
            ["Platinum", 21.45, "color: #e5e4e2"]
        ];
    
        var chartOptions = {
            title: "Element Density",
            width: window.screen.width,
            height: window.screen.height,
            legend: { position: 'top', maxLines: 3 },
            vAxis: {minValue: 0},
            animation:{
                duration: 333,
                easing: 'out',
            }
        };
        
        // Load the Visualization API and the corechart package.
        google.charts.load('current', {'packages':['corechart']});
        google.charts.setOnLoadCallback(function(){
            
            var testChart = new MyChart(myData);
        
            testChart.draw(
                'ColumnChart',
                'chart_div',
                chartOptions
            );
            
            window.setTimeout(function(){testChart.myInput(50);}, 1500);
            window.setTimeout(function(){testChart.myInput(100);}, 2500);
            window.setTimeout(function(){testChart.myInput(10.49);}, 4000);
        });
        
    </script>
</body>
```