# Overview
The extensions of NPOI, which provides IEnumerable&lt;T&gt; save to and load from excel functionality.

![NPOI.Extension demo](images/demo.PNG)

# Features
- [x] Support POCO, so that if your mother langurage is Engilish, none any configuration;
- [x] Support attribute base configuration, this feature will be very useful for English not their mother language developers
- [ ] Decouple the configuration from the POCO model, like `FluentValidation`

# Get Started
## Using Package Manager Console to install NPOI.Extension

        PM> Install-Package NPOI.Extension
    
## Reference NPOI.Extension in code

        using NPOI.Extension;
    
## Apply attribute to the model

```csharp
        [Filter(FirstCol = 0, FirstRow = 0, LastCol = 2)]
        [Freeze(ColSplit = 2, RowSplit = 1, LeftMostColumn = 2, TopRow = 1)]
        [Statistics(Name = "合计", Formula = "SUM", Columns = new[] { 6, 7 })]
        public class Report {
            [Column(Index = 0, Title = "城市", AllowMerge = true)]
            public string City { get; set; }
            [Column(Index = 1, Title = "楼盘", AllowMerge = true)]
            public string Building { get; set; }
            [Column(Index = 2, Title = "成交时间")]
            public DateTime HandleTime { get; set; }
            [Column(Index = 3, Title = "经纪人")]
            public string Broker { get; set; }
            [Column(Index = 4, Title = "客户")]
            public string Customer { get; set; }
            [Column(Index = 5, Title = "房源")]
            public string Room { get; set; }
            [Column(Index = 6, Title = "佣金(元)")]
            public decimal Brokerage { get; set; }
            [Column(Index = 7, Title = "收益(元)")]
            public decimal Profits { get; set; }
        }
```

## Export POCO to excel.

```csharp
        var len = 1000;
        var reports = new Report[len];
        for (int i = 0; i < len; i++) {
            reports[i] = new Report {
                    City = "ningbo",
                    Building = "世茂首府",
                    HandleTime = new DateTime(2015, 11, 23),
                    Broker = "RigoFunc 18957139**7",
                    Customer = "RigoFunc 18957139**7",
                    Room = "2#1703",
                    Brokerage = 125M,
                    Profits = 25m
            };

            // other data here...
        }

        // save the excel file
        reports.ToExcel(@"C:\demo.xlsx");
 ```       
## Load IEnumerable&lt;T&gt; from excel

```csharp
        // load from excel
        var loadFromExcel = Excel.Load<Report>(@"C:\demo.xlsx");
```

## Custom excel export setting

The POCO export use following setting, so, the end user can costomize the setting like `Excel.Setting.DateFormatter = "yyyy-MM-dd";`

```csharp
    public class ExcelSetting
    {
        public string Company { get; set; } = "rigofunc (xuyingting)";

        public string Author { get; set; } = "rigofunc (xuyingting)";

        public string Subject { get; set; } = "The extensions of NPOI, which provides IEnumerable<T>; save to and load from excel.";

        public bool UserXlsx { get; set; } = true;

        public string DateFormatter { get; set; } = "yyyy-MM-dd HH:mm:ss";
    }
```